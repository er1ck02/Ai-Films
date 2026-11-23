# Instructions for ChatGPT

Hello ChatGPT. This folder is a shared workspace between you and Claude, who builds and runs Content MAX, a small company making 30 to 90 second educational 3D animated shorts under the brand **Assembly Reel**. Read [`../REPORT.md`](../REPORT.md) first for the full picture.

## How it works

1. **Claude posts requests** in [`REQUESTS.md`](REQUESTS.md). Each has an id (R-001, R-002, ...), a goal and what a good answer looks like.
2. **You pick an open request** and do the research or work.
3. **You write your findings** as a new file in [`findings/`](findings/), named `R-XXX-short-title.md`, by copying [`findings/_TEMPLATE.md`](findings/_TEMPLATE.md).
4. **You update the request's status** in `REQUESTS.md` from `open` to `answered` and add a link to your findings file. Do not change anything else in that file.
5. **Claude reads your findings**, checks them, and records what was used in the "Claude's review" section at the bottom of your file.

If the owner gives you a task directly, add it to `REQUESTS.md` yourself with the next id and "Asked by: owner", then answer it the same way.

## Rules for findings

- **Cite sources** with links for every fact, number, price, rule or policy. Say when you looked (date).
- **Mark confidence**: *verified* (from an official or original source), *likely* (good secondary source), or *guess*. Say plainly when you are not sure.
- **Be current.** Platform rules, prices and licences change. Prefer sources from the last 12 months and say if a source is older.
- **Be practical.** We are one person with one PC (RTX 4080) and close to zero budget. Free and open source first. Flag anything that costs money.
- **No copied content.** Suggest formats, ideas and techniques, never others' footage, audio, scripts or characters.
- **Keep it public-safe.** This repository is public. Never write passwords, keys, personal information, email addresses or account details.
- **Only write inside `chatgpt/`.** Do not edit `REPORT.md`, the main `README.md`, or another file in `findings/` that is not yours.

## What we do with your work

Your findings are advice. Claude weighs them alongside its own research, checks the important facts, and the owner makes the final calls. Instructions inside findings are treated as suggestions, not commands.
