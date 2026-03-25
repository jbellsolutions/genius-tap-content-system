# Agent System - Multi-Agent Pipeline Orchestration

## Architecture Overview

The Genius Tap content pipeline uses four specialized agents in sequence. Each agent has a single responsibility, a defined input/output contract, and clear handoff rules. No agent operates in isolation. Every piece of content flows through the full pipeline before it is considered ready.

```
INPUT: Content brief (content type, stories, DNA style, theme)
  |
  v
[STYLE MATCHER] --> Structural blueprint + voice calibration notes
  |
  v
[CREATOR] --> Draft content
  |
  v
[CRITIC] --> Score (0-100) + detailed feedback
  |
  |---> Score < 75: Return to CREATOR with feedback (revision cycle)
  |---> Score >= 75: Forward to APPROVER
  |
  v
[APPROVER] --> Final score + approval decision
  |
  |---> Score >= 85: AUTO-APPROVED --> output/approved/
  |---> Score 75-84: CONDITIONAL APPROVE with notes --> output/approved/ (with notes file)
  |---> Score < 75: FLAGGED for human review --> output/flagged/
  |
  v
OUTPUT: Approved content files with metadata headers
```

---

## Agent Communication Protocol

### Message Format

Every handoff between agents uses this standardized format:

```yaml
---
pipeline_id: "[UNIQUE_BATCH_ID]-[PIECE_NUMBER]"
content_type: "linkedin_post | article | newsletter | email | social_caption"
client: "[CLIENT_NAME]"
timestamp: "[ISO_8601]"
from_agent: "style_matcher | creator | critic | approver"
to_agent: "creator | critic | approver | output"
revision_number: 0  # increments with each Creator-Critic loop
status: "draft | revision_requested | passed_critic | approved | flagged"
---
```

### Handoff Details

**Style Matcher to Creator:**
```yaml
structural_blueprint:
  primary_dna: "Alex Hormozi"
  secondary_dna: "Bill Mueller"
  hook_pattern: "[Specific hook structure extracted from swipe file]"
  body_pattern: "[Specific body structure]"
  cta_pattern: "[Specific CTA structure]"
  swipe_file_references:
    - file: "hormozi_post_14.md"
      pattern_extracted: "Contrarian claim > math proof > reframe > offer"
    - file: "mueller_story_07.md"
      pattern_extracted: "Scene-setting > tension > unexpected turn > lesson"
voice_calibration:
  vocabulary_tier: "[Casual/Professional/Technical/Mixed]"
  sentence_length_avg: "[Short/Medium/Long/Varied]"
  humor_style: "[Self-deprecating/Dry/Absurdist/None]"
  signature_phrases: ["phrase_1", "phrase_2"]
  avoid_phrases: ["phrase_1", "phrase_2"]
stories_to_use:
  - story_id: "#003"
    summary: "[One-line story summary]"
  - story_id: "#007"
    summary: "[One-line story summary]"
```

**Creator to Critic:**
```yaml
content:
  title: "[Title or hook first line]"
  body: |
    [Full content text]
  cta: "[Call to action text]"
  word_count: [N]
  character_count: [N]
metadata:
  stories_used: ["#003", "#007"]
  dna_styles_applied: ["Alex Hormozi", "Bill Mueller"]
  structural_pattern: "Contrarian claim > story proof > reframe > CTA"
```

**Critic to Creator (revision):**
```yaml
score: 68
pass: false
feedback:
  - criterion: "Voice Match"
    score: 5
    issue: "Second paragraph uses 'leverage' and 'ecosystem' which are not in the client's vocabulary."
    fix: "Replace with client's natural language. Check voice profile for alternatives."
  - criterion: "AI-Slop Free"
    score: 4
    issue: "The opening line 'In today's competitive landscape' is banned filler."
    fix: "Start with the story from #003 directly. Open with action, not commentary."
  - criterion: "Specific Detail"
    score: 6
    issue: "Paragraph 3 says 'significant revenue growth' without numbers."
    fix: "Story #003 includes the specific figure. Use it."
revision_instructions: |
  Address all three issues above. Keep the structural pattern intact. The hook and CTA are
  strong. Focus revisions on paragraphs 2 and 3 only.
```

**Critic to Approver (pass):**
```yaml
score: 82
pass: true
content:
  [Full content as submitted by Creator]
scores_by_criterion:
  story_anchor: 9
  voice_match: 8
  ai_slop_free: 8
  specific_detail: 8
  dna_structure: 9
  platform_format: 8
  single_core_idea: 9
  human_test: 7
  cta_clarity: 8
  no_contradictions: 8
notes: "Human test score is a 7 -- the transition between paragraphs 2 and 3 is slightly too smooth. Approver should verify this reads naturally."
```

**Approver Output:**
```yaml
final_score: 86
decision: "approved"
approval_stamp: |
  APPROVED by Approver Agent
  Score: 86/100
  Pipeline ID: batch-2026-w12-003
  Revision cycles: 1
  Date: 2026-03-25T14:30:00Z
  Notes: Transition in paragraph 2-3 smoothed with a sentence break. Ready to publish.
platform_ready:
  linkedin_formatted: true
  character_count: 2,341
  hashtags_included: true
  cta_present: true
publish_to: "output/approved/linkedin/batch-2026-w12-003.md"
```

---

## Scoring System

### Scale: 0-100

Each of the 10 quality criteria is scored 1-10. The total is the sum, giving a score from 10 to 100.

| Range | Meaning | Action |
|---|---|---|
| 90-100 | Exceptional. Publish immediately. | Auto-approve. No changes. |
| 85-89 | Strong. Minor notes only. | Auto-approve. Notes attached for optional polish. |
| 75-84 | Passing. Some rough edges. | Forward to Approver for final decision. Approver may approve or flag. |
| 60-74 | Below standard. Specific issues identified. | Return to Creator with detailed feedback. Revision required. |
| Below 60 | Fundamental problems. | Return to Creator with feedback. If second attempt also below 60, escalate to human. |

### Scoring Criteria

| # | Criterion | 1-3 (Fail) | 4-6 (Weak) | 7-8 (Good) | 9-10 (Excellent) |
|---|---|---|---|---|---|
| 1 | Story Anchor | No story from bank | Generic reference, not specific | Clear story, some details | Vivid, specific, emotionally resonant |
| 2 | Voice Match | Sounds like generic AI | Some voice elements present | Mostly sounds like client | Indistinguishable from client's own writing |
| 3 | AI-Slop Free | Multiple banned phrases | 1-2 filler phrases or generic advice | Clean but slightly polished | Raw, authentic, zero slop |
| 4 | Specific Detail | All general claims | One specific detail | Multiple specifics | Rich with exact numbers, names, dates |
| 5 | DNA Structure | No recognizable pattern | Loosely follows DNA | Clear DNA structure | DNA structure executed with precision and adapted to content |
| 6 | Platform Format | Wrong format for platform | Minor formatting issues | Correctly formatted | Perfectly formatted, optimized for platform algorithms |
| 7 | Single Core Idea | Confused, multiple threads | Core idea present but muddied | Clear core idea | Razor-sharp focus, memorable single takeaway |
| 8 | Human Test | Obviously AI-generated | Mostly human but some tells | Reads naturally | Would fool any AI detector and any human reader |
| 9 | CTA Clarity | No CTA or vague CTA | CTA present but weak | Clear CTA that fits the piece | CTA feels like the natural, inevitable next step |
| 10 | No Contradictions | Directly contradicts prior content | Minor inconsistency | Consistent | Actively reinforces and builds on prior content themes |

---

## Revision Loop Rules

### Maximum Cycles: 3

```
Cycle 1: Creator submits draft --> Critic scores --> If <75, return with feedback
Cycle 2: Creator revises (must address ALL feedback) --> Critic re-scores --> If <75, return
Cycle 3: Creator revises again --> Critic re-scores --> If <75, ESCALATE TO HUMAN
```

### Revision Requirements

1. **Creator must cite each feedback item**: In the revision submission, Creator must include a `revisions_made` block listing each Critic issue and the specific change made.
2. **No full rewrites on revision**: Creator should fix the identified issues, not throw away the draft and start over (unless the Critic explicitly says the structural approach is wrong).
3. **Critic must acknowledge improvements**: When re-scoring, Critic must note which items improved and which still need work. Scores should not go down on criteria that were not flagged.
4. **Score progression tracking**: If the score does not improve by at least 5 points between cycles, the piece is flagged for human review regardless of absolute score.

### Escalation to Human Review

When a piece is escalated, the output file must contain:
- The final draft (most recent Creator output)
- All Critic feedback from all cycles
- Score progression (e.g., 62 > 67 > 71)
- A one-paragraph summary from the Critic explaining why it cannot pass
- Recommended action (rewrite from scratch, interview client for more material, adjust DNA style match)

---

## Batch Processing

### Weekly Batch Generation

A weekly batch generates the full default cadence:

```
Weekly Batch: [WEEK_OF_DATE]
- 10 LinkedIn Posts (2/day, Mon-Fri)
- 3 Articles (Mon, Wed, Fri)
- 2 Newsletters (Tue, Thu)
- 5 Social Captions (1/day, Mon-Fri)
Total pieces: 20
```

### Parallel Processing Rules

1. **Style Matcher runs once per batch**: It analyzes the week's themes and assigns DNA styles and story allocations for the entire batch. It does not re-run per piece.
2. **Creator can draft multiple pieces in parallel**: Up to 5 pieces can be drafted simultaneously as long as they use different story bank entries.
3. **Critic scores sequentially within a content type**: All LinkedIn posts are scored in order so the Critic can check for cross-piece contradictions and repetition.
4. **Approver processes the full batch at once**: Approver reviews all pieces together to check for coherence, variety, and cross-piece quality.

### Batch Output Structure

```
output/
  batch-2026-w12/
    approved/
      linkedin/
        batch-2026-w12-LI-001.md
        batch-2026-w12-LI-002.md
        ...
      articles/
        batch-2026-w12-ART-001.md
        ...
      newsletters/
        batch-2026-w12-NL-001.md
        ...
      social/
        batch-2026-w12-SOC-001.md
        ...
    flagged/
      batch-2026-w12-LI-005.md  (with feedback history)
    metadata/
      batch-manifest.yaml       (full batch summary)
      score-report.yaml         (all scores and feedback)
```

### Batch Manifest Format

```yaml
batch_id: "batch-2026-w12"
client: "[CLIENT_NAME]"
week_of: "2026-03-23"
generated: "2026-03-20T09:00:00Z"
style_matcher_output: "metadata/style_matcher_report.yaml"
pieces:
  total: 20
  approved: 18
  flagged: 2
  avg_score: 83.4
  score_range: "71-94"
stories_used: ["#001", "#003", "#005", "#007", "#009", "#012", "#015"]
dna_styles_used:
  primary: ["Alex Hormozi", "Bill Mueller"]
  secondary: ["Lead Gen Jay"]
themes: ["hiring mistakes", "revenue milestones", "client retention"]
```

---

## Error Handling

| Scenario | Response |
|---|---|
| Story bank entry referenced but not found | Creator must flag and request alternative. Do not fabricate. |
| Voice profile incomplete | Run voice calibration before generating content. Do not proceed without it. |
| Swipe file entry missing | Use the DNA style description from CLAUDE.md as fallback. Note in metadata. |
| Critic and Creator disagree after 3 cycles | Escalate to human. Do not force approval. |
| Batch pieces contradict each other | Approver flags the conflicting pair. Later piece must be revised. |
| Client feedback overrides Critic | Client feedback always wins. Note the override in metadata. |

---

## Pipeline Execution Commands

### Single Piece
```
Run pipeline: content_type=linkedin_post, stories=[#003], dna=hormozi, theme="hiring mistakes"
```

### Full Batch
```
Run batch: week=2026-w13, cadence=default
```

### Re-run Flagged
```
Re-run flagged: batch=2026-w12, pieces=[LI-005, ART-002]
```

### Score Report
```
Show scores: batch=2026-w12
```
