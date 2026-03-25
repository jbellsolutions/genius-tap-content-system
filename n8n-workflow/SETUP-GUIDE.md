# The Genius Tap - n8n Workflow Setup Guide

This guide walks you through setting up the Creator > Critic > Approver content pipeline in n8n. This workflow replicates the same multi-agent system used in Claude Code, but runs entirely inside n8n so you can trigger it on a schedule, from a Google Sheet, or manually.

---

## Prerequisites

Before importing the workflow, make sure you have:

1. **n8n instance** -- either n8n Cloud (https://n8n.io) or a self-hosted n8n installation
2. **Claude API key** -- get one at https://console.anthropic.com/settings/keys (requires an Anthropic account with API access)
3. **Google account** -- for Google Sheets (content briefs) and optionally Google Docs (output)
4. **Slack workspace** -- with permission to create incoming webhooks (for content review and notifications)

### Estimated API costs per content piece

Each piece runs through 3 Claude API calls minimum (Creator + Critic + Approver). If revisions are needed, add 2 more calls per revision cycle.

- **No revisions**: ~3 API calls, roughly $0.08-0.15 per piece (using Claude Sonnet)
- **1 revision cycle**: ~5 API calls, roughly $0.12-0.25 per piece
- **Max revisions (3 cycles)**: ~9 API calls, roughly $0.25-0.45 per piece

A typical weekly batch of 20 pieces costs approximately $2-6 in API usage.

---

## Step 1: Import the Workflow

1. Open your n8n instance
2. Go to **Workflows** in the left sidebar
3. Click the **...** menu (top right) and select **Import from File**
4. Select the file `genius-tap-content-pipeline.json`
5. The workflow will appear with all nodes. It will show warnings about missing credentials -- that is expected.

---

## Step 2: Configure Credentials

### 2a. Anthropic API Key

1. In n8n, go to **Settings > Credentials**
2. Click **Add Credential**
3. Search for **Header Auth** (HTTP Header Authentication)
4. Name it: `Anthropic API Key`
5. Set the header:
   - **Name**: `x-api-key`
   - **Value**: your Claude API key (starts with `sk-ant-...`)
6. Save the credential
7. Open each of these nodes and assign the credential:
   - `Creator Agent (Claude API)`
   - `Critic Agent (Claude API)`
   - `Approver Agent (Claude API)`

### 2b. Google Sheets

1. In n8n, go to **Settings > Credentials**
2. Click **Add Credential**
3. Search for **Google Sheets OAuth2 API**
4. Follow the prompts to connect your Google account
5. Open the `Read Content Briefs` node and:
   - Assign the Google Sheets credential
   - Replace `YOUR_GOOGLE_SHEET_ID_HERE` with your actual Sheet ID (the long string in the Google Sheets URL between `/d/` and `/edit`)
   - Make sure the sheet name matches (default: `Content Briefs`)

### 2c. Slack Webhooks

You need two Slack webhooks -- one for content output and one for notifications.

1. Go to https://api.slack.com/apps
2. Create a new app (or use an existing one)
3. Go to **Incoming Webhooks** and enable them
4. Create two webhooks:
   - **Content Review channel**: where approved content gets posted for final human review
   - **Notifications channel**: where pipeline status updates are sent
5. Open these nodes and replace the webhook URLs:
   - `Send to Slack (Content Review)` -- paste the content review webhook URL
   - `Notification (Slack/Email)` -- paste the notification webhook URL
   - `Escalation Notification` -- paste the notification webhook URL

### 2d. Google Docs Output (Optional)

The `Create Google Doc` node is disabled by default. To enable it:

1. Create a Google OAuth2 credential in n8n (if you have not already)
2. Open the `Create Google Doc` node
3. Assign the Google OAuth2 credential
4. Enable the node (right-click > Enable)
5. You may want to disable the Slack output node if you prefer Google Docs only

---

## Step 3: Set Up the Google Sheet

Create a new Google Sheet with a tab named `Content Briefs`. Use these exact column headers in row 1:

| Column | Header | Description | Example Values |
|--------|--------|-------------|----------------|
| A | `content_type` | Type of content to generate | `linkedin_post`, `article`, `newsletter`, `email`, `social_caption` |
| B | `topic` | The theme or subject of the content piece | `Why your best hire will come from your worst firing` |
| C | `story_reference` | Story bank IDs to use (comma-separated) | `#003, #007` or leave blank for auto-selection |
| D | `copywriter_dna` | The DNA style(s) to use | `Alex Hormozi`, `Bill Mueller`, `Hormozi + Mueller` |
| E | `platform` | Target platform | `LinkedIn`, `Twitter`, `Instagram`, `Email`, `Blog` |
| F | `character_limit` | Max characters for the piece | `3000` (LinkedIn), `280` (Twitter), `2200` (Instagram) |

### Example rows

| content_type | topic | story_reference | copywriter_dna | platform | character_limit |
|---|---|---|---|---|---|
| linkedin_post | Why firing my top salesperson was the best decision I made | #003 | Alex Hormozi | LinkedIn | 3000 |
| linkedin_post | The 2 AM phone call that changed how I run my agency | #007, #012 | Bill Mueller | LinkedIn | 3000 |
| article | Three frameworks for retaining clients beyond the first year | #005, #009, #015 | Todd Brown | Blog | 10000 |
| newsletter | The $400K contract I turned down | #003 | Bill Mueller | Email | 5000 |
| social_caption | One sentence that saved a $50K deal | #012 | Lead Gen Jay | Twitter | 280 |

---

## Step 4: Customize the Agent Prompts

The agent prompts live inside the HTTP Request nodes. You need to add your client-specific content to each one.

### 4a. Add the Voice Profile

In each of the three agent nodes (`Creator Agent`, `Critic Agent`, `Approver Agent`), find the system prompt section. Look for the text:

```
PASTE YOUR CLIENT VOICE PROFILE HERE
```

Replace it with the contents of your client's `voice_profile.md`. This should include:
- Vocabulary tier (casual/professional/technical/mixed)
- Sentence patterns and average length
- Humor style
- Signature phrases the client actually uses
- Phrases to avoid (words the client never uses)

### 4b. Add the Story Bank

In the `Creator Agent` and `Critic Agent` nodes, find:

```
PASTE YOUR CLIENT STORY BANK HERE
```

Replace it with the contents of your client's `story_bank.md`. Each story should have:
- A story ID (e.g., #003)
- What happened (the raw events)
- Who was involved
- The outcome
- The lesson
- Quotable moments / direct quotes

### 4c. Advanced: Use n8n Variables (Recommended)

Instead of pasting content directly into the prompts, you can store your voice profile and story bank as n8n workflow variables:

1. In the workflow settings, go to **Variables**
2. Create two variables:
   - `voice_profile` -- paste the full voice profile text
   - `story_bank` -- paste the full story bank text
3. In the agent prompts, replace the placeholder text with:
   - `{{$vars.voice_profile}}`
   - `{{$vars.story_bank}}`

This makes it easier to update the profiles without editing each node individually.

---

## Step 5: Test with a Single Content Piece

1. Add one row to your Google Sheet (see example rows above)
2. Open the workflow in n8n
3. Click **Test Workflow** (the play button at the bottom)
4. The workflow will execute. Watch the execution in real time:
   - Green check = node succeeded
   - Red X = node failed (click to see the error)
5. Check each node's output by clicking on it:
   - `Creator Agent` -- should contain the draft content in the API response
   - `Parse Critic Response` -- should show the score and feedback
   - `Quality Gate` -- should route to either Approver (score >= 75) or revision loop
   - `Parse Approver Response` -- should show the final decision
6. Check your Slack channel for the output message

### What to look for in the test

- **Creator output**: Does it sound like the client? Does it use the specified story?
- **Critic score**: Is it reasonable? Are the feedback items specific?
- **Quality gate**: Did it route correctly based on the score?
- **Approver decision**: Does the final assessment make sense?
- **Slack output**: Is the message formatted correctly?

### Common first-run issues

| Symptom | Cause | Fix |
|---|---|---|
| Creator output is generic | Voice profile not pasted in | Add your voice profile to the Creator node's system prompt |
| Critic gives all 10s | Story bank is missing, so Critic cannot verify | Add your story bank to the Critic node's system prompt |
| API returns 401 | API key is wrong or not set | Check the Header Auth credential |
| API returns 429 | Rate limited | Wait 60 seconds and retry, or reduce batch size |
| Google Sheets returns empty | Wrong Sheet ID or sheet name | Verify the Sheet ID and tab name match exactly |
| Slack message not received | Webhook URL is wrong | Test the webhook URL directly with curl |

---

## Step 6: Schedule for Daily Production

Once your test run is successful:

1. Open the workflow
2. Find the `Schedule Trigger (Weekdays 8AM)` node (currently disabled)
3. Right-click the node and select **Enable**
4. Adjust the schedule if needed:
   - Default: weekdays at 8:00 AM (cron: `0 8 * * 1-5`)
   - To change: click the node, edit the cron expression
   - Examples:
     - `0 6 * * 1-5` -- weekdays at 6 AM
     - `0 8 * * 1` -- Mondays only at 8 AM
     - `0 8 1 * *` -- first of each month at 8 AM
5. **Activate the workflow** by toggling the switch in the top right corner

### Daily workflow pattern

1. Before the scheduled time, add content briefs to your Google Sheet
2. The workflow fires at the scheduled time
3. Each row in the sheet becomes one content piece running through the pipeline
4. Approved content appears in your Slack review channel
5. Escalated pieces get a separate notification

---

## Step 7: Troubleshooting

### Pipeline issues

**Content keeps failing the Critic (stuck in revision loop)**

The piece loops up to 3 times. If it still fails, it gets escalated. Common causes:
- Voice profile is too vague -- add more specific examples of the client's language
- Story bank entries lack detail -- the Critic flags missing specifics
- DNA style mismatch -- try a different DNA style that fits the topic better

**Critic scores are always very high (90+)**

The Critic may not have enough context to judge accurately. Make sure:
- The voice profile includes phrases to avoid (so the Critic can catch violations)
- The story bank has specific numbers and details (so the Critic can verify accuracy)
- You have not removed the banned phrases list from the Critic's system prompt

**Approver rejects content the Critic passed**

This is working as designed. The Approver checks different things (brand safety, cross-piece coherence, pride test). If this happens frequently, review the Approver's notes to understand what the Critic is missing.

### Technical issues

**n8n timeout errors**

Claude API calls can take 30-90 seconds. The default timeout in the HTTP Request nodes is set to 120 seconds. If you hit timeouts:
- Check your Anthropic API plan -- rate limits may be causing delays
- Reduce the `max_tokens` in the request body (e.g., from 4096 to 2048)
- Process fewer pieces per batch

**JSON parsing errors in the Code nodes**

The Critic and Approver are instructed to return JSON. Occasionally, they may include markdown code fences or extra text. The parsing code handles common cases, but if you see parsing errors:
- Check the raw API response in the HTTP Request node output
- The Code node includes fallback logic that flags unparseable responses for human review

**Google Sheets returns no data**

- Verify the sheet tab name is exactly `Content Briefs` (case-sensitive)
- Check that row 1 contains headers and data starts in row 2
- Make sure the Sheet ID in the node matches your spreadsheet

**Slack messages not appearing**

- Test your webhook URL directly:
  ```bash
  curl -X POST -H 'Content-type: application/json' \
    --data '{"text":"Test message"}' \
    YOUR_WEBHOOK_URL
  ```
- Verify the webhook is not disabled or deleted in Slack
- Check that the Slack app has permission to post to the target channel

---

## Workflow Architecture Reference

```
                         +-------------------+
                         | Manual Trigger    |--+
                         +-------------------+  |
                                                |    +------------------+    +--------------------+
                         +-------------------+  +--->| Read Content     |--->| Prepare Brief      |
                         | Schedule Trigger  |------>| Briefs (Sheets)  |    | & Init Loop        |
                         | (Weekdays 8AM)   |       +------------------+    +--------------------+
                         +-------------------+                                       |
                                                                                     v
              +------------------+                                     +-------------------+
              | Merge: Revision  |<----- (revision loop) ------------ | Creator Agent     |
              | Loop             |                                     | (Claude API)      |
              +------------------+                                     +-------------------+
                                                                                     |
                                                                                     v
                                                                       +-------------------+
                                                                       | Extract Creator   |
                                                                       | Output            |
                                                                       +-------------------+
                                                                                     |
                                                                                     v
                                                                       +-------------------+
                                                                       | Critic Agent      |
                                                                       | (Claude API)      |
                                                                       +-------------------+
                                                                                     |
                                                                                     v
                                                                       +-------------------+
                                                                       | Parse Critic      |
                                                                       | Response          |
                                                                       +-------------------+
                                                                                     |
                                                                                     v
                                                                       +-------------------+
                                                              +------->| Quality Gate      |
                                                              |  YES   | (Score >= 75?)    |
                                                              |        +-------------------+
                                                              |                  | NO
                                                              |                  v
                                                    +---------+------+  +-------------------+
                                                    | Approver Agent |  | Revisions Left?   |
                                                    | (Claude API)   |  | (< 3 cycles)      |
                                                    +----------------+  +-------------------+
                                                              |            YES |        | NO
                                                              v                v        v
                                                    +----------------+  +--------+  +-----------+
                                                    | Parse Approver |  |Prepare |  | Escalate  |
                                                    | Response       |  |Revision|  | to Human  |
                                                    +----------------+  |Brief   |  | Review    |
                                                              |        +--------+  +-----------+
                                                              v            |              |
                                                    +----------------+     |              v
                                                    | Send to Slack  |     |        +-----------+
                                                    | (Content)      |     +------> | Escalation|
                                                    +----------------+     (loops)  | Notify    |
                                                              |                     +-----------+
                                                              v
                                                    +----------------+
                                                    | Notification   |
                                                    | (Slack/Email)  |
                                                    +----------------+
```

---

## Model Selection

The workflow defaults to `claude-sonnet-4-20250514`. You can change the model in each agent's HTTP Request node by editing the `model` field in the JSON body.

| Model | Speed | Quality | Cost | Recommended For |
|-------|-------|---------|------|-----------------|
| `claude-sonnet-4-20250514` | Fast | Very Good | $$ | Default -- best balance of speed and quality |
| `claude-opus-4-20250514` | Slower | Excellent | $$$$ | High-value clients where quality justifies cost |
| `claude-haiku-3-20240307` | Very Fast | Good | $ | High-volume batches where speed matters most |

---

## Extending the Workflow

### Add email notifications

Replace or supplement the Slack notification node with an n8n **Send Email** node. Configure your SMTP credentials and route the final output data to the email body.

### Add Google Docs output

The `Create Google Doc` node is included but disabled. Enable it and configure Google OAuth2 credentials. You can extend it with a second HTTP request to insert the content into the created document using the Google Docs API.

### Add a Style Matcher agent

The full Genius Tap pipeline includes a Style Matcher agent that runs before the Creator. To add it:

1. Duplicate the Creator Agent HTTP Request node
2. Place it between `Prepare Brief & Init Loop` and `Creator Agent`
3. Replace the system prompt with the Style Matcher prompt from `agents/style_matcher.md`
4. Pass the Style Matcher's output (structural blueprint) as additional context to the Creator

### Process multiple briefs in parallel

By default, n8n processes items sequentially. To run multiple content briefs in parallel:

1. Go to workflow **Settings**
2. Under **Error Handling**, find **Execute items in parallel**
3. Set the concurrency limit (recommended: 3-5 to avoid API rate limits)

---

## Support

If you run into issues not covered here:

1. Check the n8n execution log (click on a failed execution to see detailed error output for each node)
2. Test each Claude API call independently using the Anthropic Console (https://console.anthropic.com)
3. Verify your Google Sheet has data in the expected format
4. Test Slack webhooks with a simple curl command before debugging the workflow
