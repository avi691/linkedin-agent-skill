# The LinkedIn Agent Skill — Install & Field Guide (corrected)

2026-10-08 · Fattah Labs

## What changed in this edition

This is the original free guide (github.com/avi691/linkedin-agent-skill) with the errors found in the 2026-10-08 audit fixed. Content and structure are otherwise unchanged.

| Where | Was | Now |
|---|---|---|
| Every command block | `-version`, `-install`, `-report` (double hyphens collapsed) | `--version`, `--install`, `--report` |
| Every URL | `https: /github.com/...` | `https://github.com/...` |
| Section 02, What you need | Git listed for Paths B and C only | Git listed for Paths A, B and C |
| Section 02 | No mention of claude.ai skill sync | Note that skills uploaded to claude.ai also sync into Claude Code |
| Section 03, Path C | `/plugin install linkedin-agent@linkedin-agent-skill`, inconsistent with README | One-line `--marketplace` form, two-step fallback, `/reload-plugins`, placeholder for the marketplace name to verify |
| Section 04, check 2 | Implied a good draft before voice.md exists | Warns the draft is generic until Section 05; new troubleshooting row |
| Section 05, Option 1 | Prompt only worked in Claude Code | claude.ai variant added |
| Section 06, /li-dm | "Max 20 invites/day" with no tier note | Free-account note limit (200 chars, about 5 notes/month) vs Premium (300 chars) |
| Section 07 | PASS threshold only in routine text | Stated with the five checks |

The marketplace name is confirmed: the `name` field in `.claude-plugin/marketplace.json` is `linkedin-agent-skill`, so the full install id is `linkedin-agent@linkedin-agent-skill`.

## Section 01: What this is, and the one rule

A skill is a folder of instructions (and sometimes small scripts) that Claude loads when the job calls for it. This pack has eleven, and together they cover the day-to-day work of running a LinkedIn account.

| Writes | Plans | Checks |
|---|---|---|
| Posts from 21 hook formulas, carousels, comments on other people's posts, replies under yours, connection notes and DMs. | The week: what to post, when, and the ten people to engage with. Repurposes long content into a week of posts. | Scores your profile out of 100, audits what you already published, sorts your inbox, and humanizes every draft. |

**The one rule: it writes, you post.** None of these skills post to LinkedIn, send a message or log into your account, and that's deliberate. Posting from a personal profile through automation or a browser bot breaks LinkedIn's User Agreement and gets accounts restricted. Every skill ends the same way: a copy-ready block of text that you read, approve and paste yourself.

**How the pieces fit.** Three small files live in a folder called `~/.claude/linkedin/` in your home folder:

| File | Who writes it | What it's for |
|---|---|---|
| `voice.md` | You, once (Claude can draft it) | How you talk, who you're writing for, what you never say, and real numbers you're happy to use. |
| `plan.md` | `/li-plan` | This week's posting schedule and engagement list. |
| `log.md` | `/li-post`, `/li-comment` | Every post you approved and the hook it used, so `/li-audit` can learn from your history. |

**Nothing is made up.** The skills never invent metrics, clients or results under your name. If a draft needs a number you haven't given, it comes back with `{{your number}}` in it and a flag, so you fill in the real one.

## Section 02: Pick your install path

The right install depends on which Claude you use. Find your row, then follow that path in Section 03.

| If you use... | Use path | What you get |
|---|---|---|
| Claude Code (terminal, VS Code, or the Code tab in the Claude desktop app) | A (easiest) or B / C | Everything, including the two humanizer scripts. Recommended. |
| Claude.ai in the browser or the Claude desktop/mobile chat | D | Skills upload. The humanizer scripts run if code execution is switched on in your settings. Skills you upload to claude.ai also sync into Claude Code when you're signed in with the same account, so if you use both, Path D may be all you need. |
| Any chat assistant, no install allowed | E | Paste one skill at a time. Works, but you lose the humanizer scripts. |

**What you need**

- A Claude account. Custom skills work on Pro, Max, Team and Enterprise plans.
- For paths A, B and C: Git (Macs usually have it; Windows users install "Git for Windows"). Path A needs it because Claude clones the repo for you.
- For the humanizer scripts: Python 3. Nothing else to install, since the scripts have no dependencies. Check with `python3 --version` (Mac) or `python --version` (Windows).
- No API key, no LinkedIn login, and no third-party account.

**Never used a terminal?** Use Path A. You paste one message into Claude Code and it runs every command for you, asking before it does anything.

## Section 03: Install, step by step

### Path A · Claude Code · easiest: let Claude install it for you

1. Open Claude Code. In a terminal type `claude`, or open the Code tab in the Claude desktop app.
2. Paste this message:

```
https://github.com/avi691/linkedin-agent-skill

Install this skill, then confirm /li-post works.
```

3. Approve the commands. Claude clones the repo and copies the eleven `li-*` folders into `~/.claude/skills/`. Approve each step when it asks.
4. Restart if needed. If `/li-post` isn't recognised straight away, start a new Claude Code session so it picks up the new skills.

### Path B · Claude Code · manual: copy the skills yourself

Mac / Linux (Terminal):

```bash
# 1. download the repo
git clone https://github.com/avi691/linkedin-agent-skill.git

# 2. make sure the skills folder exists, then copy all eleven in
mkdir -p ~/.claude/skills
cp -r linkedin-agent-skill/skills/li-* ~/.claude/skills/

# 3. check: you should see eleven li-* folders
ls ~/.claude/skills
```

Windows (PowerShell):

```powershell
git clone https://github.com/avi691/linkedin-agent-skill.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse linkedin-agent-skill\skills\li-* "$HOME\.claude\skills\"
Get-ChildItem "$HOME\.claude\skills"
```

Just for one project? Copy the same folders into that project's `.claude/skills/` instead of your home folder, so the skills only load inside that project.

### Path C · Claude Code · plugin (easy updates)

The repo doubles as a Claude Code plugin marketplace. Inside Claude Code, run:

```
/plugin install linkedin-agent --marketplace avi691/linkedin-agent-skill
```

Claude Code asks you to confirm adding the marketplace, then opens the plugin's details so you can pick a scope: just you (user), everyone on this repo (project), or you in this repo only (local). This one-line form needs Claude Code v2.1.275 or later. On older versions, do it in two steps:

```
/plugin marketplace add avi691/linkedin-agent-skill
/plugin install linkedin-agent@linkedin-agent-skill
```

`linkedin-agent-skill` is the `name` field in the repo's `.claude-plugin/marketplace.json`. If the install summary says `Run /reload-plugins to activate`, run that, or start a new session.

With a plugin install, commands may appear as `/linkedin-agent:li-post`. They work the same way, and you can also describe what you want in plain English.

Updating later: open `/plugin`, go to the Installed tab and choose Update now, or from your shell run `claude plugin update linkedin-agent@linkedin-agent-skill`.

### Path D · Claude.ai (web, desktop or mobile chat): upload the skills to your account

1. Download the repo. On the GitHub page, click the green Code button, then Download ZIP, then unzip it.
2. Zip each skill folder on its own. Open `skills/`. Each skill is uploaded separately, so make a ZIP of each folder you want (right-click, Compress on Mac; Send to, Compressed folder on Windows). The ZIP must contain the folder as its root, with `SKILL.md` inside it.
3. Turn on code execution. In Claude's Settings, find the capabilities section and switch on code execution / file creation. Skills need it, and it lets `/li-human` run its scripts.
4. Upload. In the same settings area, open Skills and upload each ZIP. Start with `li-human` and `li-post`, since the other skills hand drafts to `li-human`.

Heads up: Claude's settings menus get rearranged from time to time. If you can't find Skills, search Claude's help centre for "upload a skill". Claude.ai can't see your computer's `~/.claude/linkedin/` folder, so paste your `voice.md` at the top of a chat or save it in a Claude Project as project knowledge.

### Path E · any chat · no install: paste a skill as a "mode"

Open any `SKILL.md` on GitHub (for example `skills/li-post/SKILL.md`), copy the whole thing, and paste it at the top of a new chat followed by your request. Paste `hooks.json` along with it for `li-post`, or `rubric.json` for `li-profile`.

The trade-off: without the two Python tools you lose the real humanizer, which is most of the point of `/li-human`. Everything else works.

## Section 04: Check it works

Three quick checks. If all three pass, you're installed.

1. **Claude can see the skills.** In Claude Code, type `/li`. The eleven commands should appear in the list. Or ask: "Which LinkedIn skills do you have?"
2. **A real draft comes back.** Run:

```
/li-post we built an internal tool that cut proposal time from 5 hours to 20 min
```

You should get three hook options (each tagged with a formula number like `#17 Time Anchor`), one full draft, and a POST READY receipt with a human score. The draft will sound generic at this point, because you haven't set up `voice.md` yet. That's expected; Section 05 fixes it.

3. **The humanizer scripts run.** From inside the `li-human` folder:

```bash
cd ~/.claude/skills/li-human
python3 detect.py SKILL.md
```

You should see five bars (BURSTINESS, SPECIFICITY, SLOP DENSITY, FINGERPRINT, VOICE) and a HUMAN SCORE.

### Troubleshooting

| What you see | Fix |
|---|---|
| `/li-post` isn't recognised | Start a new Claude Code session. Check the folders sit directly in `~/.claude/skills/li-post/SKILL.md`, not one level deeper (e.g. `skills/skills/li-post`). |
| `cp: ~/.claude/skills: No such file or directory` | Run `mkdir -p ~/.claude/skills` first, then copy again. |
| `git: command not found` | Install Git (Mac: run `xcode-select --install`; Windows: Git for Windows). Or use Download ZIP and copy the folders by hand. |
| `python3: command not found` | Windows: use `python` instead of `python3`. Mac: run `xcode-select --install`, or install Python from python.org. |
| Drafts from `/li-post` are generic in check 2 | Expected. You haven't set up `voice.md` yet. Do Section 05, then run it again. |
| Drafts still sound generic after Section 05 | Check `voice.md` is at `~/.claude/linkedin/voice.md` (Claude Code) or in your Project knowledge (claude.ai), and that the Proof section has real numbers. |
| Commands show as `/linkedin-agent:li-post` | Normal for a plugin install (Path C) and works the same way. |
| Claude.ai upload rejected | Each ZIP must hold one skill folder with `SKILL.md` inside. Don't zip the whole repo. |
| Hidden folder `.claude` not visible | Mac Finder: press Cmd + Shift + . to show hidden files. Windows: View, Show, Hidden items. |

**Updating later.** Path B: run `git pull` inside your `linkedin-agent-skill` folder, then copy the `li-*` folders again. Path C: Update now from the `/plugin` Installed tab. Path D: re-upload the changed skill ZIPs.

**Before you update:** if you've edited `slop.json` (your own banned and allowed words), keep a copy, because copying the folders again overwrites it.

**The repo at a glance**

```
linkedin-agent-skill/
├── skills/
│   ├── li-post/      SKILL.md + hooks.json          # 21 hook formulas
│   ├── li-human/     SKILL.md + humanize.py + detect.py + slop.json
│   ├── li-profile/   SKILL.md + rubric.json         # 100-point score
│   └── li-comment, li-reply, li-plan, li-carousel,
│       li-repurpose, li-dm, li-inbox, li-audit/   # one SKILL.md each
├── templates/voice.md                               # your voice profile
├── .claude-plugin/                                  # lets it install as a plugin
├── README.md
└── LICENSE                                          # MIT
```

## Section 05: Set up your voice (do not skip)

This is the ten minutes that decides whether you post the drafts or rewrite them. Every skill reads your voice profile. If you skip it, everything comes out sounding like everyone else on LinkedIn.

### Option 1: let Claude write it (recommended)

Pick three of your own posts that sound most like you. Paste them into Claude and say:

In Claude Code:

```
Write my voice.md from these three posts, using the template in
templates/voice.md, and save it to ~/.claude/linkedin/voice.md.

[post 1]
[post 2]
[post 3]
```

In claude.ai (web, desktop or mobile chat): drop the "save it to" clause and say "...and give it to me as a file." Then add the result to a Claude Project as project knowledge, since claude.ai can't see your computer's `~/.claude/linkedin/` folder.

Then read what it wrote and correct anything that's off. In particular, fill in Proof I can use with real numbers yourself. The skills never invent a number, so an empty Proof section means every draft comes back with `{{your number}}` placeholders. Five real figures (revenue, time saved, clients won, a mistake that cost you) unlock most of the strong hook formulas.

### Option 2: fill in the template by hand

```bash
# Mac / Linux
mkdir -p ~/.claude/linkedin
cp linkedin-agent-skill/templates/voice.md ~/.claude/linkedin/voice.md
```

```powershell
# Windows (PowerShell)
New-Item -ItemType Directory -Force "$HOME\.claude\linkedin" | Out-Null
Copy-Item linkedin-agent-skill\templates\voice.md "$HOME\.claude\linkedin\voice.md"
```

### What the template asks for

- **Who I am:** what you do in one sentence, who you write for (as specific as "agency owners doing $1–5M", not "professionals"), and what you sell.
- **What I sound like:** words you use and words you'd never use, sentence length, swearing, emoji, contractions (almost always yes).
- **My positions:** three to five things you believe that part of your audience doesn't. This is where the good posts come from.
- **Off limits and proof:** topics, clients and claims you can't use publicly, plus real numbers and stories you're happy to put your name on.

## Section 06: The eleven skills

You can type the slash command, or just say what you want in plain English. Claude picks the right skill from your request. The "Say" column shows example prompts you can copy.

| Skill | What it does | Say | House rules |
|---|---|---|---|
| `/li-post` One idea into a finished post | Reads your voice and picks three different hook formulas from the 21, says which it would use. Writes the full post on the strongest one: hook alone on line 1, short paragraphs, one turn, one close. Humanizes it, prints a copy-ready block and a POST READY receipt. Reply "yes" and it logs the post to `log.md`. | `/li-post we lost a $40k deal because our proposal took 9 days` · `Give me hook options for: I stopped doing discovery calls` | 900–1,300 characters. No links in the body (link goes in the first comment). Max 3 hashtags, no "Thoughts?" |
| `/li-comment` Comments on other people's posts | Picks from nine comment types based on what the post says: add a datum, add the missing case, respectful disagree, extend one line, ask the real question, the receipt, the correction, the reframe, the one-liner. Gives two options plus which to post and why. Batch mode: paste 5–10 posts for your daily round. | `/li-comment [paste the post + author's name and role]` · `Here are 6 posts for my engagement round: ...` | 2–4 sentences, one idea. Never "Great post!" and no emoji openers. |
| `/li-reply` The thread under your own post | Sorts every comment into LEAD, SUBSTANCE, PEER, SUPPORT, NOISE and shows the counts. Writes replies in that order. Handles critics (concede the true part, then hold your ground) and ignores pitches. | `/li-reply [paste the comments, or a screenshot]` · `Someone disagreed with my post, help me reply: ...` | Run it within the first hour after you post. That's when replies do the most for reach. |
| `/li-profile` Profile score out of 100 + rewrites | Scores your profile on the 12-part, 100-point rubric (Appendix B). Expect 30s–40s on the first pass. Rewrites in fix-first order: headline (3 options), about (first two lines), about body, featured, experience, banner. Re-scores at the end. | `/li-profile [paste headline, about, current role, last two roles]` · `Score my LinkedIn from this screenshot` | Headline formula: {what you do for whom} \| {proof} \| {how to start} |
| `/li-plan` The week, decided | Plans four posts a week, mixing Proof, Opinion, Teach, Story and Offer, with a specific angle and hook number for each. Posting times built around when your audience is at a desk (B2B default: Tue–Thu, 7:30–9:30am their timezone). Engagement list of 10 people: 5 reach, 3 peers, 2 buyers. Saves to `plan.md`. Say "write Tuesday" to draft a slot. | `/li-plan this week I closed 2 clients, lost one to a cheaper agency, and rebuilt our onboarding` | Give it what actually happened this week. Posts come from real events, not topics. |
| `/li-human` The humanizer (Section 07) | Removes invisible characters, em dashes and curly quotes, and swaps 113 stock "AI" words and phrases for plain ones. Flags sentence patterns that need a human rewrite. Scores the draft on five checks: PASS, REVIEW or FLAGGED. | `/li-human [paste any text]` · `Does this sound like AI?` · `Remove the em dashes and de-slop this` | The other skills run it automatically before showing you a draft. |
| `/li-carousel` Document posts (PDF slides) | 8–12 slides: cover (six words or fewer), the stakes, one idea per slide, a recap, a CTA. Writes the 2–3 lines of post text above the PDF. After you approve the copy, builds the 1080×1350 PDF. | `/li-carousel the 6 steps we use to onboard a client in 48 hours` | If the idea is one claim and not a sequence, it sends you to `/li-post`. |
| `/li-repurpose` One long asset into a week of posts | Pulls claims, numbers, stories, how-it-works explanations, mistakes and quotable lines from a transcript, newsletter, podcast or call. Builds a week with a different hook formula per post. Drafts one at a time so they don't all sound the same. | `/li-repurpose [paste transcript or article]` · `Turn my latest newsletter into posts` | It doesn't summarise. |
| `/li-dm` Connection notes and follow-ups | Asks who, why now, and what you want, and pushes back if there's no real reason to reach out. Writes the invite note (under 200 characters, with a count), the first message, and two follow-ups (+4 and +10 days). | `/li-dm Jane Doe, Head of Growth at Acme, she posted about killing their SDR team` | No calendar link in message one. Note under 200 characters: LinkedIn allows 200 on free accounts, 300 on Premium, and free accounts get only about five personalised notes a month. Cap yourself around 20 invites a day, sent by hand. |
| `/li-inbox` Inbox triage | Sorts pasted messages into LEAD, RECRUITER, PEER, ASK, SPAM, counts first. Spots automated outreach sequences and names the giveaway. Drafts replies only for messages worth answering, including polite final declines. | `/li-inbox [paste messages or screenshots]` · `Should I reply to this?` |  |
| `/li-audit` What's actually working | Ranks posts by engagement rate and reach multiple (impressions ÷ followers), not raw impressions. Compares top 5 and bottom 5 by hook, format, length and topic; checks posting day last. Tells you what to stop and do more of, then passes that to `/li-plan`. | `/li-audit [attach the LinkedIn analytics CSV]` · `Why did this post flop?` | Get the data: LinkedIn, Analytics, Content, Export. |

## Section 07: The humanizer, in depth

`/li-human` comes with two small Python scripts that need nothing else installed. They run on your own computer, on your own text, and nothing is uploaded anywhere.

```bash
# run from ~/.claude/skills/li-human
python3 humanize.py draft.txt --report        # clean it, list every change
python3 humanize.py draft.txt -o clean.txt    # save the cleaned version
python3 detect.py draft.txt                   # score it on five checks
python3 detect.py draft.txt clean.txt         # before vs after
```

**What it fixes automatically**

1. **Invisible characters.** Zero-width spaces and joiners, soft hyphens, byte-order marks, Unicode tag characters, odd spaces. Your keyboard doesn't type these, and they survive copy-paste.
2. **Typography.** Em dash to comma, en dash to hyphen, curly quotes to straight, ellipsis to three dots, bullet character to hyphen.
3. **The slop lexicon.** 113 stock words and phrases (delve, leverage, robust, seamless, "let that sink in") swapped for plain ones, keeping capitals and leaving URLs alone.

**Flagged, not fixed.** These need judgement, so the script points them out and you rewrite them: "It's not just X, it's Y", rule-of-three lists, one-word rhetorical questions ("The result?"), rocket/fire/sparkle emoji, hashtag walls, "Thoughts?"-style bait, and sentences or bullets that are all the same length.

**The five checks** (0–100, higher = more human)

| Check | What it measures | What reads as machine-written |
|---|---|---|
| BURSTINESS | How much sentence length varies | Every sentence about the same length |
| SPECIFICITY | Numbers, names and concrete details per 100 words | Abstract nouns, no figures |
| SLOP DENSITY | Lexicon hits per 100 words | Stock vocabulary |
| FINGERPRINT | Invisible characters, em dashes and curly quotes per 1,000 characters | Typographically perfect |
| VOICE | Contractions, first/second person, structural tells | No contractions, staged reveals |

The final score is 60% the average and 40% the weakest single check, because a detector only needs one signal to fire. PASS needs 70+ overall with no check below 55. 55–69 is REVIEW; below that is FLAGGED.

**The routine**

1. `python3 humanize.py draft.txt -o clean.txt --report`
2. Rewrite the flagged lines by hand, keeping the meaning.
3. `python3 detect.py draft.txt clean.txt` to see the before and after.
4. Not PASS? Fix the weakest check it names and run it again. Two rounds is normal. If it takes five, the draft itself is the problem, so start a new draft rather than doing more passes.

**Make it yours.** The lexicon lives in `skills/li-human/slop.json` and is meant to be edited. If it strips a word you genuinely use, delete that entry. If you have a pet phrase you want gone, add it.

**An honest limit.** These are local checks modelled on what public AI detectors look for. They're not GPTZero, Originality, Copyleaks, Winston or Turnitin, and they don't call those services. Fixing what they measure tends to improve those scores too, but nobody can honestly promise "undetectable".

## Section 08: Your weekly routine

The skills work best as a loop. Here's a simple week that takes about 20 minutes a day.

| When | Run | Why |
|---|---|---|
| Monday | `/li-plan` | Tell it what happened last week. You get four post slots, times and your ten people. |
| Every day, before posting | `/li-comment` (batch) | 20 minutes commenting on your list. Comment early, before a post has 20 comments, or nobody sees yours. |
| Post days | "write Tuesday" then `/li-post` | Draft the planned slot, check the human score, paste it into LinkedIn yourself. |
| First hour after posting | `/li-reply` | Reply to leads and real discussion first. The first hour does most of the work for reach. |
| Friday | `/li-inbox`, `/li-dm` | Clear the inbox, then send a few targeted connection notes by hand. |
| Monthly | `/li-audit`, `/li-profile` | See what actually worked and feed it into next month's plans. Re-score your profile. |
| Whenever you make long content | `/li-repurpose`, `/li-carousel` | One video, newsletter or call becomes 4–6 posts. List-shaped ideas become carousels. |

**Your first 30 minutes**

1. **Install.** Path A, B, C or D from Section 03.
2. **Write your voice.md.** Paste three of your posts and let Claude draft it. Add five real numbers to Proof.
3. **Score your profile.** `/li-profile`. Fix the headline and about first, since that's where most of the points are.
4. **Plan the week.** `/li-plan`, then "write Tuesday".

## Appendix A: The 21 hook formulas

Each one is in `skills/li-post/hooks.json` with a template, a filled-in example and the common way it goes wrong. `/li-plan` and `/li-post` refer to them by number.

| # | Formula | Best for |
|---|---|---|
| 1 | Contrarian Take | Establishing a point of view. Highest comment rate of any formula. |
| 2 | Number Reveal | Experiments, challenges, anything with a countable result. |
| 3 | Mistake Confession | Trust. People forward the posts where you look bad. |
| 4 | Before / After | Proof you've travelled the distance your reader wants to travel. |
| 5 | The List Promise | Saves. Highest save rate of the 21, which LinkedIn weights heavily. |
| 6 | Insider Secret | Credentialed takes. Works only if the years are real. |
| 7 | The Callout | Short posts with one instruction. Fastest to write. |
| 8 | Question Trap | Comment volume. People answer scenarios and ignore "thoughts?". |
| 9 | Story Cold Open | Long-form storytelling posts. Highest dwell time. |
| 10 | The Receipt | Anything you can prove. Pair it with an image. |
| 11 | Myth Bust | Reframing. Sets you up to deliver the real cause in line 3. |
| 12 | The Comparison | Tool and process posts. Very high share rate. |
| 13 | Permission Slip | Reaching people outside your niche. Travels further than tactical posts. |
| 14 | Pattern Interrupt | Stopping the scroll cold. The white space does the work. |
| 15 | The Warning | Readers who know the problem but haven't linked it to what it costs them. |
| 16 | Good vs Great | Quotable one-liners. People screenshot these. |
| 17 | Time Anchor | Tool, system and automation posts. Strongest shape for a how-to. |
| 18 | The Unpopular Rule | Positioning. Attracts the right clients by putting off the wrong ones. |
| 19 | Curiosity Gap | Openers that make people tap "see more". |
| 20 | The Walk-Away | Values posts with a business result attached. |
| 21 | The Direct Value | Lead magnets and comment-for-link posts. Highest conversion of the 21. |

## Appendix B: The 100-point profile rubric

This is what `/li-profile` scores you against (from `skills/li-profile/rubric.json`). Most profiles land in the 30s and 40s on the first pass. The Activity item is the only time-sensitive one, so a quiet week costs points that come back as soon as you post.

| Item | Points | What full marks looks like |
|---|---|---|
| Headline | 12 | Says who you help and what changes for them, includes one piece of proof, and isn't just your job title. Uses the full 220 characters. |
| About, first 2 lines | 10 | Names the reader's problem before "see more". No "passionate about", no third-person bio, doesn't open with your name. |
| About body | 10 | Written to one person: their problem, what you do, one number as proof, what to do next. Under 1,400 characters (LinkedIn allows 2,600; shorter reads better). |
| Featured | 8 | Three items: your best post, a proof asset, and how to contact you. |
| Banner | 6 | One line of positioning and one way to reach you. Not the default gradient. |
| Photo | 6 | Face fills about 60% of the frame, eyes visible, taken in the last two years. |
| Current role | 10 | One line of scope, then 2–3 results with numbers. Duties aren't results. |
| Experience depth | 8 | Last two roles detailed; anything older than ten years cut to one line. |
| Skills | 6 | Your top three skills are the ones you want to be hired for, and they're endorsed. |
| Recommendations | 8 | At least three from the last two years, each naming a specific result. |
| Activity | 10 | Posted or commented in the last seven days. |
| Contact | 6 | Custom URL, a working email or booking link, and the same handle as your other platforms. |
| **Total** | **100** |  |

## Appendix C: The fine print

**These skills don't post for you, on purpose.** There's no official way to post to a personal LinkedIn profile without an approved partner app, and automating the site with a browser or a third-party tool breaks LinkedIn's User Agreement and gets accounts restricted. Every skill ends with text you copy and paste. That's why you approve everything before it goes out.

**The humanizer is honest about what it is.** The five checks are heuristics that run on your machine. They don't call any detection service and can't promise a verdict from one. The invisible-character cleanup is real but limited: it removes stray formatting characters, and it doesn't claim to beat any cryptographic watermarking scheme.

**Your data stays with you.** The scripts read and write files on your own computer only. Your voice profile, plan and log live in `~/.claude/linkedin/`. Whatever you paste into Claude is handled under your own Claude account's terms.

**Credits.** This pack was originally created by Jake Schincariol ([opusjake.ai](https://opusjake.ai)) and released under the MIT License. It's shared by Avi Grondin at [github.com/avi691/linkedin-agent-skill](https://github.com/avi691/linkedin-agent-skill).

**License.** MIT. You're free to use it, change it and share it. Keep the LICENSE file with any copy you pass on.

**Sources checked for this edition:** [Claude Code: install and manage plugins](https://code.claude.com/docs/en/plugins/install.md) · [Agent Skills overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) · [Using skills in Claude (support)](https://support.claude.com/en/articles/12512180-using-skills-in-claude) · [How to create custom skills (support)](https://support.claude.com/en/articles/12512198) · [Repo README](https://github.com/avi691/linkedin-agent-skill)
