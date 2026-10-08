---
name: li-post
description: >-
  Write a LinkedIn post from a raw idea using 21 proven hook formulas, in the
  user's own voice, humanized so it does not read as AI. Use whenever the user
  wants a LinkedIn post, a hook, a draft for the feed, "post about X", "turn
  this into a LinkedIn post", or asks for hook options. Produces three hook
  options, one full draft, and a copy-ready block that is never published
  without an explicit yes.
---

# li-post

Turns one raw idea into a LinkedIn post that sounds like the person who
posted it.

## Before you write

1. Find the user's files first. If `~/.claude/linkedin/config.json` exists, it
   says where they live: `voice`, `plan` and `log` are paths, and it may also
   set `timezone` and `products`. Use those paths wherever this pack says
   `voice.md`, `plan.md` or `log.md`. Without a config, the files are
   `~/.claude/linkedin/voice.md`, `plan.md` and `log.md`.
   Then read the voice file if it exists. That file is the user's
   voice profile: how they talk, what they never say, who they are talking to.
   If it does not exist, ask for **three of their own past posts**, infer the
   voice from those, and write the file. Do not skip this and do not invent a
   voice. A post in the wrong voice is worse than no post.
2. Read `hooks.json` in this folder. All 21 formulas, with templates, filled
   examples, what each is for, and how each one usually gets ruined.
3. If the idea is thin - "post about AI" - do not pad it. Ask one batched
   question: what happened, to whom, and what did it cost or return. A post
   needs one specific true thing. Get it before writing.

## Which product

Some users post for more than one product or brand from the same profile.
If `~/.claude/linkedin/config.json` lists `products`, every draft is for
exactly one of them:

1. **Take it from the request** - "for the app", "for the agency". If the
   request does not name one, **ask which before writing anything.** Do not
   guess from the topic.
2. **Write from that product's section of the voice file** (its
   `voice_section`): its audience, tone and proof points. The shared sections
   still apply. Never borrow a proof point from another product.
3. **Read every file in `checks`** (top level, for every product) **and in
   that product's own `checks`** before drafting. They are the user's rules:
   banned claims, verified figures, disclosure. A draft that breaks one is not
   shown. Say which rule it broke and redraft.
4. **Name the product in the receipt** under the draft, so the log records it.

No `products` in the config means one voice and one audience, as before.

## The shape

LinkedIn rewards dwell time, saves and comments, in that order. So:

```
Line 1     the hook. Alone. It has to survive truncation at ~140 chars mobile.
Line 2     the payoff of line 1, not setup for line 3.
Body       short paragraphs, 1-3 lines each, blank line between every one.
           No wall. The white space is the format.
The turn   one line that reframes what came before.
Close      one specific question, or one instruction. Never both.
```

Length: 900-1,300 characters is the working range for a text post. Under 400
reads as a thought, not a post. Over 2,000 needs to earn every line, and the
"see more" tap has to be paid for by line 2.

## The loop

**1. Pick three hooks, not one.** Run the idea through `hooks.json` and choose
the three formulas that genuinely fit it. Different formulas, not three
variations of one. Show them as three numbered lines and say which you would
ship and why, in one sentence.

**2. Draft the full post** on the strongest hook.

**3. Humanize it.** Run the draft through `/li-human` before showing it. Every
post from this skill ships humanized. That is not an optional extra step, it
is the reason the draft is worth reading.

**4. Print the block.** Copy-ready, in a fenced block, exactly as it should be
pasted. Then, underneath:

```
POST READY
product:   (from config, if it lists products)
hook:      #17 Time Anchor
length:    1,140 characters
humanizer: 6 artefacts stripped, human score 84 PASS
post at:   Tuesday 8:15am ET (from your plan)

Reply "yes" to log it, or tell me what to change.
```

**5. Never publish.** This skill produces text. The user posts it. On "yes",
append the post to the log file with the date, the hook used
and the first line, so `/li-audit` has a history to work from later.

## Rules that make the difference

- **One idea per post.** If the draft has two, you have two posts. Say so.
- **Numbers over adjectives.** "$4,200" beats "a lot". If the user has not
  given you a number, ask for one rather than writing around the hole.
- **No engagement bait.** "Thoughts?" and "Agree?" are dead. The closing
  question has to be one only this post could ask.
- **Three hashtags maximum**, at the bottom, and only if they are real
  categories someone follows.
- **No links in the post body.** LinkedIn suppresses posts with outbound
  links. Put the link in the first comment and say so in the receipt.
- **Never fabricate.** No invented metrics, clients, revenue figures or
  outcomes under the user's name, even as a placeholder. If a number is
  needed and unknown, leave `{{your number}}` in the draft and flag it.

## Example

```
/li-post we built an internal tool that cut proposal time from 5 hours to 20 min
```

```
HOOKS
1. #17 Time Anchor    Writing a proposal used to take me 5 hours. It now takes 20 minutes.
2. #12 Comparison     A $12,000 proposal writer vs a weekend and a template. The weekend won.
3. #3  Mistake        For two years I charged clients for hours I was wasting on formatting.

Shipping #17: the ratio is believable and the number is yours.
```
