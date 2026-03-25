# The Genius Tap Content System - Master Instructions

## System Overview

The Genius Tap is a productized content service that extracts expertise from professionals through guided interviews and transforms it into high-quality, voice-matched content using AI agents and a swipe file of 9 master copywriters.

### The Expert Extraction Method (3 Layers)

**Layer 1: Guided Excavation**
Raw expertise is extracted through structured interview sessions that mine for stories, frameworks, contrarian opinions, and hard-won lessons. Every piece of content must trace back to something the client actually said, did, or believes. Nothing is fabricated. Nothing is generic.

**Layer 2: Copywriter DNA Match**
The client's natural voice is analyzed and matched to 2-3 primary copywriter DNA styles from the swipe file. This creates a structural backbone: the client's authentic voice fills the words, while proven copywriting architecture shapes the delivery.

**Layer 3: 3-Agent Quality Gauntlet**
Every piece passes through Creator > Critic > Approver. The Creator drafts using voice profile + story bank + copywriter DNA. The Critic scores against 10 quality criteria and hunts for AI slop. The Approver runs final QA for cross-piece consistency, platform compliance, and the pride test. Nothing ships until all three agents sign off.

---

## Client Identity

```yaml
CLIENT_NAME: "[CLIENT_NAME]"
BACKGROUND: "[BACKGROUND - professional history, career arc, key milestones]"
EXPERTISE: "[EXPERTISE - primary domain, sub-specialties, unique angles]"
AUDIENCE: "[AUDIENCE - who they serve, demographics, psychographics, pain points]"
VOICE_PROFILE: "[VOICE_PROFILE_REFERENCE - path to voice_profile.md]"
STORY_BANK: "[STORY_BANK_REFERENCE - path to story_bank.md]"
PRIMARY_DNA_STYLES:
  - "[STYLE_1 - e.g., Alex Hormozi]"
  - "[STYLE_2 - e.g., Bill Mueller]"
SECONDARY_DNA_STYLE: "[STYLE_3 - e.g., Jon Buchan]"
CONTENT_GOALS: "[What the content should achieve - leads, authority, speaking gigs, etc.]"
INDUSTRY: "[INDUSTRY]"
TABOO_TOPICS: "[Topics to never touch]"
COMPETITOR_DIFFERENTIATION: "[What makes this client different from everyone else in their space]"
```

When setting up a new client, populate every field above. Reference the voice profile and story bank by relative path from the client repo root.

---

## Content Production Rules

### Default Cadence

| Content Type | Frequency | Platform |
|---|---|---|
| LinkedIn Posts | 2 per day (10/week) | LinkedIn |
| Long-form Articles | 3 per week | LinkedIn Articles / Blog |
| Newsletters | 2 per week | Email (ConvertKit/Beehiiv) |
| Email Nurture Sequences | As needed per campaign | Email |
| Social Captions | 5 per week | Twitter/X, Instagram, TikTok |

### Platform Formatting Rules

**LinkedIn Posts**
- Hard limit: 3,000 characters (stay under 2,800 for safety)
- Hook must land in the first 2 lines (before the "see more" fold at ~210 characters)
- Use single-line paragraphs with line breaks between them for readability
- No walls of text. Max 3 lines per paragraph block
- Hashtags: 3-5 per post, placed at the bottom, mix of broad (#Leadership, #Sales) and niche (#B2BSaaS, #AgencyGrowth)
- No emojis in the hook line. Emojis sparingly in body (max 3 per post) and only if natural to client voice
- End with a clear CTA or conversation starter, never both

**LinkedIn Articles**
- 800-1,500 words ideal range
- Use H2 subheadings every 200-300 words
- Include at least one story from the story bank
- Bold key phrases for skimmability
- End with a single clear takeaway and CTA

**Newsletters**
- Subject line: under 50 characters, curiosity-driven or benefit-driven
- Preview text: separate from subject, adds context (40-90 characters)
- Body: 500-900 words
- One core idea per newsletter, not three half-baked ideas
- Personal tone, written as if to one person
- Include one story from the story bank
- CTA: single, clear, low-friction

**Email Nurture Sequences**
- 5-7 emails per sequence
- Day spacing: 0, 1, 2, 3, 5, 7, 10
- Each email escalates commitment from "read" to "click" to "reply" to "buy/book"
- Subject lines: under 45 characters, no spam trigger words
- Plain text formatting preferred (no heavy HTML templates)

**Social Captions (Twitter/X, Instagram, TikTok)**
- Twitter/X: under 280 characters, punchy, single-idea
- Instagram: 125-200 characters for feed posts, up to 2,200 for carousel captions
- TikTok: under 150 characters, hook-first, conversational

### Anti-AI-Slop Rules

These rules are non-negotiable. Every piece of content must pass every single one.

1. **No Filler Phrases**: Never use "In today's fast-paced world", "It's no secret that", "At the end of the day", "Let's dive in", "Without further ado", "In this article we'll explore", "Are you struggling with", "Imagine a world where", "What if I told you" (unless the client literally says "what if I told you" in their interviews)
2. **No Orphan Advice**: Every piece of advice must be anchored to a specific story, example, or data point from the client's experience. "Be consistent" is slop. "I posted every day for 90 days and got zero engagement until day 47 when..." is not.
3. **No Corporate Jargon** (unless natural to client): Ban "leverage", "synergy", "thought leader", "value proposition", "ecosystem", "paradigm shift", "move the needle", "circle back", "deep dive" unless the client actually talks like this in their interviews.
4. **Story Anchor Requirement**: Every LinkedIn post references at least one real story, moment, or specific detail from the story bank. Articles reference at least two. Newsletters reference at least one.
5. **The Human Test**: Read the piece aloud. Would a human think a human wrote this? If it sounds like a ChatGPT default output, it fails. Look for: unnaturally smooth transitions, perfectly parallel structures that no human would naturally produce, hedging language that adds nothing, conclusions that restate the introduction.
6. **No List Filler**: If a post uses a numbered list, every item must carry real weight. No padding items like "Stay positive" or "Believe in yourself" to fill out a list of 7.
7. **Specific Over General**: "Revenue grew" is slop. "Revenue went from $47K to $312K in 11 months" is not. Always choose the specific detail over the general claim.
8. **Voice Consistency**: Every piece must sound like the client wrote it, not like a content agency wrote it. Check against the voice profile for vocabulary, sentence length patterns, humor style, and signature phrases.
9. **No Engagement Bait**: Do not use "Agree?" or "Thoughts?" or "Drop a comment below" or "Like if you..." as standalone CTAs. CTAs must be substantive.
10. **Platform-Native Feel**: LinkedIn posts should feel like LinkedIn posts, not blog excerpts pasted into LinkedIn. Newsletters should feel like letters, not articles reformatted for email.

---

## Swipe File Integration

### The 9 Copywriter DNA Styles

Each client is matched to 2-3 primary DNA styles based on their natural voice, audience, and content goals. The swipe file (located in `../Real Swipe File/data/`) contains real examples from each copywriter.

| Copywriter | DNA Style | Best For | Signature Moves |
|---|---|---|---|
| **Jon Buchan** | Playful / Pattern-Interrupt | Cold outreach, attention-grabbing posts | Self-deprecating humor, absurd analogies, breaking the fourth wall, making the reader laugh before they learn |
| **Todd Brown** | Mechanism-Focused / Educational | Course creators, consultants, info products | Naming proprietary frameworks, "the mechanism behind", building curiosity through education, "here's why what you've been told is wrong" |
| **Bill Mueller** | Story-Driven / Curiosity | Personal brands, coaches, advisors | Open loops, nested stories, "I never told anyone this but", emotional arc from struggle to insight, delayed reveals |
| **Jay Abraham** | Strategic / Leverage | Business strategists, high-ticket B2B | Abundance thinking, "most people" framing, cross-industry pattern matching, strategic contrarianism, preeminence positioning |
| **Brian Kurtz** | Insider-Authority / Direct-Response | Newsletter operators, list builders, DR marketers | "I learned from the legends", name-dropping earned (not manufactured), insider access feel, heritage credibility, the "secret history" of |
| **Alex Hormozi** | Value-Stacking / Contrarian | Gym owners, agency owners, operators | Brutal honesty, "free > paid", value equation breakdowns, contrarian reframes, math-based arguments, "here's what I'd do if I were you" |
| **Tom Bilyeu** | Mindset / Aspirational | Mindset coaches, personal development, founders | Identity-level reframes, "the person you need to become", narrative arcs from rock bottom to breakthrough, existential urgency |
| **Lead Gen Jay** | B2B / Tactical | Lead gen agencies, B2B service providers | Tactical breakdowns, "here's the exact process", screenshot-style specificity, ROI framing, "steal this" generosity |
| **Liam Ottley** | AI / Technical | AI agencies, tech founders, SaaS builders | Demystifying complex tech, "the stack behind", build-in-public transparency, future-casting with receipts, "here's what's actually happening in AI" |

### How to Use DNA Styles

1. **Read the client voice profile** to understand their natural communication patterns
2. **Match to 2-3 DNA styles** that complement (not replace) the client's voice
3. **Use the DNA for structure, not vocabulary**: The copywriter DNA provides the architecture (hook type, story structure, CTA pattern). The client's voice provides the words, tone, and personality.
4. **Rotate styles by content type**: A client might use Hormozi DNA for LinkedIn posts (contrarian, punchy) but Mueller DNA for newsletter stories (emotional arc, curiosity). Map styles to content types in the client identity section.
5. **Pull structural patterns from actual swipe file entries**: Do not just mimic the style in theory. Open the relevant copywriter's examples in the swipe file and extract the specific structural pattern being used (hook > pattern interrupt > mechanism reveal > proof > CTA, etc.)

### Content Type to DNA Style Mapping (Defaults)

| Content Type | Recommended Primary DNA | Why |
|---|---|---|
| LinkedIn Posts (authority) | Hormozi, Jay Abraham | Punchy, contrarian, positions expertise |
| LinkedIn Posts (storytelling) | Bill Mueller, Tom Bilyeu | Emotional arc, curiosity hooks |
| LinkedIn Posts (tactical) | Lead Gen Jay, Todd Brown | Specificity, frameworks, "steal this" energy |
| Articles | Bill Mueller, Brian Kurtz | Long-form story capacity, authority building |
| Newsletters | Jon Buchan, Brian Kurtz | Personality-forward, relationship building |
| Cold Outreach / Email | Jon Buchan, Hormozi | Pattern-interrupt, value-forward |
| Social Captions | Hormozi, Liam Ottley | Punchy, single-idea, scroll-stopping |

Override these defaults based on client voice match. A naturally funny client should use Buchan DNA more broadly. A data-driven client should lean toward Lead Gen Jay across formats.

---

## Agent Pipeline

### Flow

```
[Story Bank + Voice Profile + Swipe File]
              |
              v
    +-------------------+
    |   STYLE MATCHER   |  <-- Selects DNA styles, extracts structural patterns
    +-------------------+
              |
              v
    +-------------------+
    |     CREATOR       |  <-- Drafts content using voice + stories + DNA structure
    +-------------------+
              |
              v
    +-------------------+
    |      CRITIC       |  <-- Scores 0-100, flags AI slop, checks voice match
    +-------------------+
         |         |
     [<75: REVISE] [>=75: PASS]
         |         |
         v         v
    +----------+  +-------------------+
    | CREATOR  |  |     APPROVER      |  <-- Final QA, cross-piece coherence, pride test
    | (Revise) |  +-------------------+
    +----------+         |
         |          [>=85: AUTO-APPROVE]
         |          [<85: MANUAL REVIEW]
         v               |
    (max 3 loops)        v
         |          [PUBLISHED or FLAGGED]
         v
    [HUMAN REVIEW]
```

### Agent Instructions

- **Style Matcher** (`agents/style_matcher.md`): Runs first. Analyzes client voice profile, selects DNA styles for the content batch, and extracts structural patterns from the swipe file.
- **Creator** (`agents/creator.md`): Produces draft content. Uses voice profile for tone/vocabulary, story bank for real material, and DNA structural patterns for architecture.
- **Critic** (`agents/critic.md`): Scores each piece on 10 criteria (0-100 scale). Anything below 75 goes back to Creator with specific feedback. Anything 75+ goes to Approver.
- **Approver** (`agents/approver.md`): Final QA. Checks cross-piece coherence, platform compliance, brand safety, and runs the pride test. Scores 85+ auto-approve. Below 85 gets flagged for human review.

### Loop Rules

- Maximum 3 Creator-Critic revision cycles per piece
- If a piece fails 3 times, it is flagged for human review with all Critic feedback attached
- Creator must address every specific issue raised by the Critic, not just "try again"
- Each revision must cite which Critic feedback point it addressed and how

---

## Content Types & Templates

Detailed templates are in the `templates/` directory:

| Template | File | Primary Use |
|---|---|---|
| LinkedIn Post | `templates/linkedin_post.md` | Daily LinkedIn content |
| Long-form Article | `templates/article.md` | Authority building, SEO, deep dives |
| Newsletter | `templates/newsletter.md` | Email list engagement, relationship building |
| Email Nurture Sequence | `templates/email_nurture.md` | Lead conversion sequences |
| Social Caption | `templates/social_caption.md` | Twitter/X, Instagram, TikTok |

Each template includes structural guidance, hook patterns mapped to DNA styles, and formatting rules. Do not deviate from template structures without explicit instruction.

---

## Quality Standards: 10-Point Checklist

Every piece of content must pass ALL 10 checks before publishing. A failure on any single item means the piece goes back to the Creator.

| # | Check | What It Means | Pass Criteria |
|---|---|---|---|
| 1 | **Story Anchor** | Contains at least one real story/example from the story bank | Specific story bank entry referenced by ID |
| 2 | **Voice Match** | Sounds like the client, not like a content mill | Passes voice profile comparison on vocabulary, rhythm, tone |
| 3 | **AI-Slop Free** | No banned phrases, no filler, no generic advice | Zero hits on the banned phrases list; no orphan advice |
| 4 | **Specific Detail** | Contains concrete numbers, names, timeframes, or outcomes | At least one specific data point or named detail |
| 5 | **DNA Structure** | Uses the assigned copywriter DNA structural pattern | Hook, body, and CTA follow the DNA blueprint |
| 6 | **Platform Format** | Meets all platform-specific formatting rules | Character limits, spacing, hashtags, CTA format correct |
| 7 | **Single Core Idea** | One clear takeaway per piece (not three half-baked ideas) | Can be summarized in one sentence |
| 8 | **Human Test** | Would pass if read aloud to a stranger | No robotic transitions, no artificial parallelism, sounds conversational |
| 9 | **CTA Clarity** | Clear, single call-to-action that fits the piece | Reader knows exactly what to do next |
| 10 | **No Contradictions** | Does not contradict other published content from this client | Cross-referenced against recent content batch |

---

## Workflow Commands

Use these commands to operate the content pipeline. Run them from the root of the client's content repo.

### Generate Content

```
Generate [N] LinkedIn posts for [DATE_RANGE] using stories [#ID, #ID] from the story bank.
Primary DNA: [STYLE]. Themes: [THEME_1, THEME_2].
```

```
Generate 1 article on [TOPIC] using stories [#ID, #ID, #ID].
Target length: [WORD_COUNT]. DNA style: [STYLE].
```

```
Generate 1 newsletter on [TOPIC]. Subject line angle: [ANGLE].
Story anchor: [#ID]. DNA style: [STYLE].
```

```
Generate a [N]-email nurture sequence for [CAMPAIGN_GOAL].
Stories to weave in: [#ID, #ID, #ID]. DNA style: [STYLE].
```

### Run the Pipeline

```
Run pipeline on [CONTENT_FILE or BATCH_DIR].
```

This triggers: Style Matcher > Creator > Critic > (loop if needed) > Approver. Output is placed in the `output/` directory with status tags.

### Review Flagged Content

```
Show flagged content with Critic feedback.
```

Returns all pieces that failed 3 Critic cycles or scored below 85 at Approver stage, with full feedback history.

### Batch Operations

```
Generate weekly content batch for [WEEK_OF_DATE].
```

This generates the full default cadence (10 LinkedIn posts, 3 articles, 2 newsletters) for the specified week, pulling from the story bank and rotating DNA styles per the content type mapping.

### Voice Calibration

```
Calibrate voice profile against [SAMPLE_CONTENT].
```

Analyzes a sample of the client's own writing (or interview transcript) and updates the voice profile with refined vocabulary lists, sentence patterns, and tone markers.

---

## File Structure (Client Repo)

```
client-repo/
  CLAUDE.md              <-- This file (customized per client)
  voice_profile.md       <-- Client voice analysis
  story_bank.md          <-- All extracted stories with IDs
  agents/                <-- Agent prompts
    AGENT_SYSTEM.md
    creator.md
    critic.md
    approver.md
    style_matcher.md
  templates/             <-- Content templates
    linkedin_post.md
    article.md
    newsletter.md
    email_nurture.md
    social_caption.md
  output/                <-- Generated content (auto-created)
    drafts/
    approved/
    flagged/
  swipe_file/            <-- Symlink or copy of master swipe file
```
