# The Genius Tap Content System

## What Is This Repo?

The Genius Tap is a productized content service that extracts expertise from professionals through guided interviews and transforms it into high-quality, voice-matched content at scale.

The core product is **The Expert Extraction Method**, which operates through three layers:

**Layer 1: Guided Excavation** -- A structured 90-minute interview that mines the client's career for stories, frameworks, contrarian opinions, and hard-won lessons. Every piece of content traces back to something the client actually said, did, or believes. Nothing is fabricated.

**Layer 2: Copywriter DNA Match** -- The client's natural voice is analyzed and matched to 2-3 copywriter DNA styles from a swipe file of 5,716 real emails across 9 master copywriters. The client's authentic voice fills the words; proven copywriting architecture shapes the delivery.

**Layer 3: 3-Agent Quality Gauntlet** -- Every piece passes through Creator > Critic > Approver. Nothing ships until all three agents sign off. The Critic scores against 10 quality criteria and hunts for AI slop. The Approver runs final QA and the pride test.

**The result:** One 90-minute call produces a Voice Profile, a Story Bank, and a Copywriter DNA match that fuels 60+ content pieces per month -- LinkedIn posts, articles, newsletters, email sequences, social captions -- all in the client's voice.

---

## Repo Structure

```
genius-tap-content-system/
|
|-- README.md                          <-- You are here. Documentation + launch ops guide.
|-- CLAUDE.md                          <-- Master instructions for Claude Code sessions.
|                                          Contains client identity template, production rules,
|                                          anti-AI-slop rules, swipe file integration,
|                                          agent pipeline flow, and workflow commands.
|
|-- agents/                            <-- Agent prompt definitions (system-level, not templates)
|   |-- AGENT_SYSTEM.md                    Pipeline orchestration, scoring system, handoff protocol
|   |-- style_matcher.md                   Selects DNA styles, extracts structural patterns
|   |-- creator.md                         Drafts content using voice + stories + DNA structure
|   |-- critic.md                          Scores 0-100, flags AI slop, checks voice match
|   |-- approver.md                        Final QA, cross-piece coherence, pride test
|
|-- client/                            <-- LIVE client files (Tony D'Agostino is the first client)
|   |-- voice_profile.md                   Tony's voice analysis (vocabulary, rhythm, tone)
|   |-- story_bank.md                      Tony's extracted stories with IDs
|   |-- content_themes.md                  Tony's recurring themes and topic clusters
|
|-- templates/                         <-- TEMPLATES for new clients (blank/example versions)
|   |-- linkedin_post.md                   LinkedIn post template with hook patterns per DNA style
|   |-- article.md                         Long-form article template
|   |-- newsletter.md                      Newsletter template
|   |-- email_nurture.md                   Email nurture sequence template
|   |-- social_caption.md                  Social caption template (Twitter/X, IG, TikTok)
|
|-- sales-copy/                        <-- All sales and marketing materials for The Genius Tap
|   |-- GENIUS-TAP-SALES-PAGE.md           Full long-form sales page (multiple headline options)
|   |-- LANDING-PAGE.md                    Lead magnet opt-in landing page copy
|   |-- LEAD-MAGNET.md                     "The Expert's Content Cheat Sheet" PDF content
|   |-- LINKEDIN-POSTS-12.md              12 LinkedIn posts for launching The Genius Tap
|   |-- NEWSLETTERS-4.md                  4 newsletter editions for launch sequence
|   |-- EMAIL-NURTURE-5.md                5-email nurture sequence (lead magnet to sales page)
|   |-- EMAIL-AUTORESPONDER-7.md          7-email autoresponder (post-purchase / long nurture)
|   |-- COLD-OUTREACH-20-ANGLES.md        20 cold outreach email angles
|
|-- swipe-file/                        <-- Swipe file metadata and configuration
|   |-- SWIPE_FILE_CONTEXT.md              Copywriter profiles, DNA matching process, usage guide
|   |-- config/
|       |-- authors.json                   Machine-readable copywriter profiles for agent pipeline
|
|-- output/                            <-- Generated content (organized by type)
|   |-- linkedin-posts/                    Approved LinkedIn posts
|   |-- articles/                          Approved long-form articles
|   |-- newsletters/                       Approved newsletters
|   |-- email-sequences/                   Approved email sequences
|   |-- social/                            Approved social captions
|
|-- visuals/                           <-- Design assets, diagrams, and images
```

### Templates vs. Live Files

| Path | Type | Purpose |
|---|---|---|
| `client/voice_profile.md` | **LIVE** | Tony D'Agostino's actual voice profile |
| `client/story_bank.md` | **LIVE** | Tony's extracted stories |
| `client/content_themes.md` | **LIVE** | Tony's content themes |
| `templates/*.md` | **TEMPLATE** | Blank structures for new clients |
| `agents/*.md` | **SYSTEM** | Shared across all clients (do not modify per-client) |
| `sales-copy/*.md` | **LIVE** | Ready-to-deploy marketing materials for The Genius Tap |
| `swipe-file/` | **SYSTEM** | Shared reference library (do not modify per-client) |

---

## How to Use This Repo

### For Justin (Delivering to Clients)

**Setting up a new client:**

1. Clone the repo into a new directory named for the client.
2. Conduct the Genius Tap extraction call (60-90 minutes, using the Guided Excavation protocol).
3. Process the transcript:
   - Fill in `client/voice_profile.md` with the client's vocabulary patterns, sentence rhythms, humor style, signature phrases, and words they never use.
   - Fill in `client/story_bank.md` with every story extracted from the call, each assigned an ID (`#001`, `#002`, etc.) and tagged by theme.
   - Fill in `client/content_themes.md` with recurring topics, contrarian positions, and content pillars.
4. Update `CLAUDE.md` with the client's identity block: name, background, expertise, audience, DNA style matches, content goals, industry, taboo topics, and differentiators.
5. Run the agent pipeline to generate the first content batch: `Generate weekly content batch for [WEEK]`
6. Share samples with the client for calibration. Iterate on voice profile based on feedback. Adjust DNA style weighting if needed.
7. Launch ongoing production once the client confirms voice match.

**Monthly production cycle:**

1. Run a monthly optimization call with the client (30 minutes). Extract new stories, refine themes.
2. Update story bank and content themes.
3. Generate 4 weekly batches.
4. Client reviews flagged pieces only (auto-approved pieces ship directly).

---

### For Audrey / Another Claude Code Session (Executing the Launch)

**Step 1: Open this repo in Claude Code.**

**Step 2: Read `CLAUDE.md`.** It contains the full system instructions, client identity template, production rules, swipe file integration, and workflow commands.

**Step 3: Deploy the sales materials.** Everything in `sales-copy/` is written and ready:

| File | Deploy To |
|---|---|
| `sales-copy/GENIUS-TAP-SALES-PAGE.md` | Website or landing page builder (Carrd, Webflow, WordPress) |
| `sales-copy/LANDING-PAGE.md` | Landing page builder (lead magnet opt-in page) |
| `sales-copy/LEAD-MAGNET.md` | Design into a PDF (use Canva, Gamma, or a designer) |
| `sales-copy/EMAIL-NURTURE-5.md` | Load into email platform (ActiveCampaign, GHL, ConvertKit) |
| `sales-copy/EMAIL-AUTORESPONDER-7.md` | Load into email platform as long-term autoresponder |
| `sales-copy/LINKEDIN-POSTS-12.md` | Schedule in LinkedIn or a scheduling tool (Buffer, Taplio) |
| `sales-copy/NEWSLETTERS-4.md` | Load into newsletter platform (Beehiiv, ConvertKit, Substack) |
| `sales-copy/COLD-OUTREACH-20-ANGLES.md` | Load into cold email tool (Instantly, Smartlead, Lemlist) |

**Step 4: Follow the 4-week launch calendar below.**

---

## 4-Week Launch Calendar

### Week -2 to 0: Tony Delivery (Pre-Launch)

- [ ] Process Tony's transcript through the agent pipeline
- [ ] Generate Tony's first 2 weeks of content (20 LinkedIn posts, 6 articles, 4 newsletters)
- [ ] Run all content through the 3-Agent Quality Gauntlet
- [ ] Deliver content to Tony for review and calibration
- [ ] Get Tony's approval on voice match quality
- [ ] Request a testimonial from Tony (before/after transformation)
- [ ] Document the before/after: zero content published --> 60+ pieces per month, all in his voice
- [ ] Photograph or screenshot Tony's reaction/quote for social proof

### Week 1: Asset Build

- [ ] Design lead magnet PDF from `sales-copy/LEAD-MAGNET.md` (use Canva or Gamma)
- [ ] Build the lead magnet opt-in landing page from `sales-copy/LANDING-PAGE.md`
- [ ] Deploy the full sales page from `sales-copy/GENIUS-TAP-SALES-PAGE.md`
- [ ] Connect the funnel: landing page opt-in --> email platform --> nurture sequence --> sales page
- [ ] Load the 5-email nurture sequence (`sales-copy/EMAIL-NURTURE-5.md`) into email platform
- [ ] Load the 7-email autoresponder (`sales-copy/EMAIL-AUTORESPONDER-7.md`) as the post-nurture sequence
- [ ] Prepare cold outreach angles from `sales-copy/COLD-OUTREACH-20-ANGLES.md` in cold email tool
- [ ] Build cold outreach target list (200-500 prospects: consultants, coaches, agency owners, executives)
- [ ] Set up calendar link (Calendly or Cal.com) for booking Genius Tap Sessions
- [ ] Add Tony's testimonial to the sales page

### Week 2: Soft Launch

- [ ] Publish lead magnet with opt-in page (share link on LinkedIn profile, email signature)
- [ ] Begin posting LinkedIn content: Posts 1-4 from `sales-copy/LINKEDIN-POSTS-12.md`
- [ ] Send Newsletter Edition 1 from `sales-copy/NEWSLETTERS-4.md` to existing contacts/list
- [ ] Start cold outreach: 50-100 emails per day using the first 5 angles
- [ ] Verify the full funnel: lead magnet opt-in --> nurture emails fire --> sales page loads --> calendar link works
- [ ] Monitor email deliverability (check open rates, spam placement)
- [ ] Track: opt-in rate, email open rate, click-through to sales page, calls booked

### Week 3: Case Study Launch

- [ ] Publish Tony case study as a LinkedIn article (long-form before/after narrative)
- [ ] Send Newsletter Edition 2 featuring the Tony transformation story
- [ ] Post LinkedIn Posts 5-8 from `sales-copy/LINKEDIN-POSTS-12.md`
- [ ] Add Tony testimonial and case study link to the sales page
- [ ] Scale cold outreach to 200-300 emails per day, rotating angles
- [ ] A/B test subject lines on cold outreach (track reply rate per angle)
- [ ] Engage with every comment and reply on LinkedIn posts
- [ ] Track: calls booked, close rate, revenue

### Week 4+: Full Launch

- [ ] Send Newsletter Editions 3 and 4 from `sales-copy/NEWSLETTERS-4.md`
- [ ] Post LinkedIn Posts 9-12 from `sales-copy/LINKEDIN-POSTS-12.md`
- [ ] Activate the 7-email autoresponder for all leads who completed the nurture and did not book
- [ ] Full cold outreach at scale (300+ per day, full angle rotation)
- [ ] Begin onboarding Client #2 (replicate the Tony delivery process)
- [ ] Post Tony's content samples as social proof ("here's what we built for a client this week")
- [ ] Collect Client #2 testimonial for sales page
- [ ] Review and optimize: which outreach angles convert, which LinkedIn posts drive the most profile visits, which newsletter editions drive the most replies

---

## Pricing and Tiers

### Tier 1: The Genius Tap Session -- $500

**For the expert who wants to experience the extraction before committing.**

What they get:
- 60-minute Expert Extraction call (full Guided Excavation protocol)
- Complete Voice Profile documenting natural communication patterns
- Story Bank with all extracted stories catalogued and categorized
- 3 sample content pieces written using their Copywriter DNA match
- A clear picture of what a full content engine could produce

Most clients who do a Tier 1 session upgrade within 30 days.

### Tier 2: The Genius Tap + Content Engine -- $2,000 Setup + $750/Month

**The full system. This is where most clients land. This is the primary offer.**

Everything in Tier 1, plus:
- Custom content system built on their Voice Profile and Copywriter DNA match
- 60+ content pieces per month (LinkedIn posts, articles, newsletters, emails)
- All content run through the 3-Agent Quality Gauntlet
- Monthly optimization call to refine voice profile, add new stories, adjust strategy
- Ongoing story extraction (every optimization call surfaces more material)
- Content calendar with strategic publishing schedule

Unit economics: $750/month for 60+ pieces = roughly $12.50 per piece.

### Tier 3: The Genius Tap Enterprise -- $5,000 Setup + $1,500/Month

**Maximum output. Maximum leverage.**

Everything in Tier 2, plus:
- Higher content cadence (100+ pieces per month across all formats)
- Video content repurposing (talks, interviews, and calls turned into written content)
- Cold outreach copy written in the client's voice
- Bi-weekly strategy calls
- Priority turnaround on all content
- Dedicated content strategist

---

## The Buyer's Journey

```
1. DISCOVER          LinkedIn post, cold email, newsletter, referral, podcast
       |
       v
2. OPT-IN            Download "The Expert's Content Cheat Sheet" (lead magnet)
       |
       v
3. NURTURE            5-email sequence (educates, builds trust, introduces the method)
       |
       v
4. READ               Full sales page (the Tony story, the 3 layers, the swipe file, proof)
       |
       v
5. BOOK               Calendar link to schedule a Genius Tap Session
       |
       v
6. EXPERIENCE         The call IS delivery step 1. The extraction is the product demo.
       |               They leave the call with their Voice Profile + Story Bank started.
       v
7. CLOSE              See content samples generated from THEIR stories, in THEIR voice.
       |               Pricing, guarantee, and the math ($12.50/piece vs. $200-$500/piece).
       v
8. ONBOARD            Voice Build --> Calibration --> First content batch --> Launch
       |
       v
9. RETAIN             Monthly production + optimization calls + ongoing story extraction
```

Key insight: the Genius Tap Session (Tier 1) IS the sales mechanism for Tier 2. The extraction call itself demonstrates the product. Clients experience the value before they buy the full system.

---

## File Inventory

| File | Description |
|---|---|
| `README.md` | This file. Documentation, launch ops guide, and 4-week calendar. |
| `CLAUDE.md` | Master instructions for Claude Code. Client identity, production rules, anti-AI-slop rules, swipe file integration, agent pipeline, workflow commands. |
| `agents/AGENT_SYSTEM.md` | Pipeline orchestration. Scoring system (0-100), handoff protocol between agents, revision loop rules, batch processing, error handling. |
| `agents/style_matcher.md` | Style Matcher agent. Analyzes client voice, selects DNA styles for the batch, extracts structural patterns from the swipe file. |
| `agents/creator.md` | Creator agent. Drafts content using voice profile + story bank + DNA structural patterns. |
| `agents/critic.md` | Critic agent. Scores each piece on 10 criteria. Returns revision feedback or passes to Approver. |
| `agents/approver.md` | Approver agent. Final QA: cross-piece coherence, platform compliance, pride test. Scores 85+ auto-approve. |
| `client/voice_profile.md` | Tony D'Agostino's voice profile. Vocabulary, sentence patterns, humor style, signature phrases. |
| `client/story_bank.md` | Tony's extracted stories. Each story has an ID, summary, theme tags, and emotional arc. |
| `client/content_themes.md` | Tony's content themes. Recurring topics, contrarian positions, content pillars. |
| `templates/linkedin_post.md` | LinkedIn post template. Hook patterns mapped to DNA styles, formatting rules, character limits. |
| `templates/article.md` | Long-form article template. Structure, subheading cadence, story integration points. |
| `templates/newsletter.md` | Newsletter template. Subject line rules, body structure, CTA patterns. |
| `templates/email_nurture.md` | Email nurture sequence template. Day spacing, escalation framework, subject line rules. |
| `templates/social_caption.md` | Social caption template. Platform-specific rules for Twitter/X, Instagram, TikTok. |
| `sales-copy/GENIUS-TAP-SALES-PAGE.md` | Full long-form sales page. Multiple headline options (Todd Brown, Hormozi, Mueller, Kurtz, Buchan style). Tony's story, the 3 layers, pricing, guarantee, FAQ. |
| `sales-copy/LANDING-PAGE.md` | Lead magnet opt-in landing page. Headline, bullet points, opt-in form copy. |
| `sales-copy/LEAD-MAGNET.md` | "The Expert's Content Cheat Sheet." Full content for a PDF lead magnet with self-guided extraction exercises. |
| `sales-copy/LINKEDIN-POSTS-12.md` | 12 LinkedIn posts for the Genius Tap launch. Sequenced for weeks 2-4 of the launch calendar. |
| `sales-copy/NEWSLETTERS-4.md` | 4 newsletter editions for the launch. Each builds on the previous, escalating from education to offer. |
| `sales-copy/EMAIL-NURTURE-5.md` | 5-email nurture sequence. Triggered by lead magnet download. Educates, builds trust, drives to sales page. |
| `sales-copy/EMAIL-AUTORESPONDER-7.md` | 7-email autoresponder. Long-term nurture for leads who did not convert from the 5-email sequence. |
| `sales-copy/COLD-OUTREACH-20-ANGLES.md` | 20 cold outreach email angles. Pattern-interrupt openers, value-first bodies, soft CTAs. |
| `swipe-file/SWIPE_FILE_CONTEXT.md` | Swipe file documentation. Copywriter profiles, DNA matching process, agent usage guide. |
| `swipe-file/config/authors.json` | Machine-readable copywriter profiles. Used by the Style Matcher agent for DNA matching. |
