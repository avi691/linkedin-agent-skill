---
name: li
description: >-
  Menu for the LinkedIn skill pack - lists the eleven li-* commands with one
  line each and says which to start with. Use when the user types /li, asks
  "what LinkedIn commands are there", "what can the LinkedIn skills do", or
  does not know where to start.
---

# li

Print this table, then one line on where to start. Do not run any of the
skills from here.

| command | what it does |
| --- | --- |
| `/li-post` | One idea into a post: three hooks, one draft, humanized |
| `/li-carousel` | A document post, slide by slide, plus the PDF |
| `/li-repurpose` | One long piece into a week of posts |
| `/li-plan` | The week: what to post, when, and who to engage with |
| `/li-comment` | Comments on other people's posts |
| `/li-reply` | Replies under your own posts |
| `/li-dm` | Connection notes and follow-ups |
| `/li-inbox` | Sort the inbox and draft the replies worth sending |
| `/li-profile` | Score the profile out of 100 and fix what loses points |
| `/li-audit` | What already worked, and what to stop doing |
| `/li-human` | Clean any draft and score it on five checks |

Where to start: if the voice file does not exist yet (see
`~/.claude/linkedin/config.json`, else `~/.claude/linkedin/voice.md`), start
there, because every other skill reads it. Otherwise `/li-plan` once a week,
then `/li-post` for each slot.

If the config lists `products`, say so in one line and list them, since every
writing command will ask which one.

Nothing here posts to LinkedIn. Every command produces text the user pastes.
