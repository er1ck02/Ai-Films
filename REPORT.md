# Content MAX: progress report

**As of:** 23 November 2026 · **Written by:** Claude (the build team) · **Status:** private drafts only, nothing published

This repository is public, so this report leaves out personal details, account details, machine paths and credentials. The full working files live in the project's private workspace.

---

## 1. Summary

Content MAX is a one-person company run by a team of AI agents. It makes 30 to 90 second vertical educational videos (YouTube Shorts and Facebook Reels) about how machines, buildings and engineering work. The first brand is **Assembly Reel**: faceless, clean 3D cutaway animations made in Blender, with an AI narration voice.

In two days we have:

- Researched and written a full architecture (13 planning documents) and had the owner approve it.
- Built the core software: the Coordinator, the agent runtime, the safety rules, the database and the job queue that sends render work to the owner's PC. About 2,700 lines of Python, 54 automated tests passing.
- Defined 9 agents with versioned prompts.
- Installed a free local voice (Chatterbox) and Blender on the production PC, and measured how fast they run.
- Studied about 950 top-performing Shorts in our niche and turned them into a house playbook.
- Created the brand, logo, cover art, Facebook page and a ready-to-paste page kit.

Next: render the first showcase clip, build the Blender templates and produce four private draft shorts for the owner to review.

---

## 2. What we worked on (timeline)

| When | Work |
|---|---|
| 22 Nov | Discovery: the owner answered questions on goals, hardware, budget, channels, publishing rules and branding. |
| 22–23 Nov | **Phase 1, research and architecture.** 13 documents: architecture, agent registry, model routing, memory design, self-improvement, a catalogue of open-source repositories with licence checks, costs and hardware, local inference plan, security, roadmap, decision log. |
| 23 Nov | Owner chose the cheapest route: **Blender animation as the main method** instead of AI-generated video. Production plan rewritten (decision ADR-016). |
| 23 Nov | Free voice model chosen and installed on the PC (Chatterbox, MIT licence). Blender 4.5 LTS installed. Render and voice speed measured. |
| 23 Nov | **Phase 2, foundation code**: configuration, database schema, safety policies, stub model runner, agent registry, owner API, tests. |
| 23 Nov | **Phase 3, Coordinator core**: the Coordinator's turn loop, a closed list of allowed actions, tasks, memory, daily reports. |
| 23 Nov | **Phase 4, workforce runtime**: agents pick up and complete tasks, automatic independent review, render job queue, PC worker client, scene spec format. |
| 23 Nov | **Format research**: 70 niche searches, 950 Shorts, 36 channels profiled. Findings folded into the researcher's prompt and the topic scorer. |
| 23 Nov | **Brand**: name "Assembly Reel", logo and cover art, Facebook page created, page kit and visual layout spec written. |
| 23 Nov (in progress) | Showcase render (a pin tumbler lock cutaway), Blender templates, four draft shorts. Waiting on PC time. |

---

## 3. The system

### 3.1 Shape

```
                    Owner (web dashboard, email alerts)
                                  │
              ┌───────────────────▼────────────────────┐
              │  Small cloud server (planned, ~$7/mo)  │
              │  Coordinator · agent runtime · API     │
              │  PostgreSQL + pgvector (memory, tasks, │
              │  budget ledger, approvals, event log)  │
              └───────────────────▲────────────────────┘
                                  │ outbound HTTPS only
                                  │ (PC asks for jobs; no open ports)
              ┌───────────────────┴────────────────────┐
              │  Owner's PC = GPU worker               │
              │  Blender renders · Chatterbox voice ·  │
              │  WhisperX timings · FFmpeg / Remotion  │
              └────────────────────────────────────────┘
```

- **The Coordinator has no memory of its own.** Everything it knows lives in the database, and it reloads that on every turn. It survives restarts and does not depend on any chat window staying open.
- **Cloud plus PC.** The Coordinator runs on a cheap server so it keeps working when the PC is off. The heavy work (rendering, voice) runs on the owner's GPU for free.
- **Multi-brand from day one.** Every row carries a brand id, enforced by database row-level security, with separate secrets and storage per brand. One brand is live.

### 3.2 Safety rules built into the code

These are code, not prompt instructions, so an agent cannot talk its way past them.

- **Kill switch.** The company starts paused.
- **Budget guard.** Every paid call reserves money first, then settles. All paid providers are off and all spending caps are $0 until the owner sets them.
- **Approval gate.** Changes to permissions, spending or publishing need the owner's approval.
- **Draft-first publishing.** A public post is refused without a recorded owner approval.
- **Permissions.** Each agent has an allowed list of tools and memory areas.
- **Stub mode.** With no paid AI API switched on, the system runs on a labelled stub, so it can be built and tested at no cost.

### 3.3 Production pipeline (one short)

1. Research and fact check produce a research dossier.
2. The creative director writes a script with timed beats.
3. The production planner turns it into a shot list and a **scene spec** (JSON): which Blender template, which parts, how they move, camera, labels.
4. The PC renders each shot in Blender (headless, EEVEE, 1080×1920, 30 fps).
5. Chatterbox reads the narration; WhisperX gives word timings.
6. Remotion adds captions and part labels; FFmpeg does the final encode and loudness.
7. Automatic quality checks, then the reviewer agent, then the owner.
8. Upload as a private draft only.

Planned Blender templates: `assembly_sequence`, `mechanism_loop`, `process_line`, `cutaway_reveal`, `terrain_cutaway`, `versus_split`, `scale_compare`, `timeline_diorama`.

---

## 4. Agents built

Nine agents are defined, each with a role file and a versioned system prompt. They run on the stub runner today. They switch to real models once the owner turns on API billing.

| Agent | Job | Planned model |
|---|---|---|
| `coordinator` | Runs the company; the only agent that talks to the owner. Plans, delegates, reports. | Fable 5.2 (routine bookkeeping on Haiku) |
| `researcher` | Topics, trends, competitor formats. Prompt v2 is "aggressive": studies what works for other creators and borrows formats, hooks and pacing, never their footage, audio or scripts. | Sonnet 5.5 + Haiku helpers |
| `fact_checker` | Independently checks every claim in a script. | Opus 5.5 |
| `creative_director` | Concept, hook, script, titles and metadata. | Sonnet / Opus 5.5 |
| `production_planner` | Shot list and Blender scene specs. | Sonnet 5.5 |
| `reviewer` | Independent review at each stage; never reviews its own work. | Opus 5.5 |
| `growth_analyst` | Reads platform analytics and proposes experiments. | Sonnet 5.5 |
| `engineer` | Writes code fixes as pull requests for owner review; never deploys itself. | Opus 5.5, sandboxed |
| `tech_scout` | Weekly scan of new open-source tools with licence checks; never installs anything. | Haiku 5.5 |

Everything else in the original 35-role plan became a skill, a tool or a plain service. Fewer agents means lower cost and clearer responsibility.

---

## 5. Progress against the roadmap

| Phase | Scope | Status |
|---|---|---|
| 0 Discovery | Owner's answers recorded | Done |
| 1 Research | Architecture and plan documents | Done (revision 3) |
| 2 Foundation | Config, schema, safety policies, registry, API, CI | Done |
| 3 Coordinator | Turn loop, actions, tasks, memory, daily report | Core done; live-model run waits for API billing |
| 4 Workforce | Agent runtime, review queueing, render queue, PC worker, scene spec | Core done |
| 7-pre Local setup | Blender and voice installed, speed measured | Done; voice sample awaiting owner review |
| 7a Blender MVP | Templates, captions, voice, assembly, 3 finished shorts | **Next** |
| 5, 6, 8, 9, 10 | Self-improvement, repo intelligence, dashboard, publishing, validation | Not started |

**Code status:** about 2,700 lines of Python across 7 commits. 54 tests passing against a real PostgreSQL database (as a non-admin user, so the security rules are actually tested). Lint and strict type checks are clean. The code is not in this repository (see section 9).

**Measured on the owner's PC:**

| Test | Result |
|---|---|
| Blender EEVEE, 1080×1920 | 0.33 s per frame (a 10 s clip renders in about 100 s) |
| Blender Cycles (OptiX) | about 0.8 s per frame |
| Chatterbox voice | about 1.5× real time, 3.4 GB GPU memory, 5 GB RAM |

Both comfortably beat the targets in the plan.

---

## 6. Research findings (short version)

From about 950 top Shorts in manufacturing, engineering, construction and history:

1. **Faceless 3D explainers are a proven, growing lane**, and channels with only a few thousand subscribers are getting 1 to 3 million views on single videos.
2. **Clean, stylised 3D beats photoreal.** Cutaways, simple shapes, bright lighting, one hero mechanism per shot. That suits scripted Blender.
3. **Captions one or two bold words at a time**, with big part-name labels.
4. **Hook = a question or paradox title, and the first frame is already moving.** "Mystery object" and "shocking image" openings do best; a person talking to camera does worst.
5. **Two length sweet spots:** 5 to 12 s silent mechanism loops, and 22 to 60 s explainers.
6. **Winners post daily or more.** Templates make that cadence possible for us.

Ranked formats: impossible-build construction cutaway, "how X actually works" mechanism cutaway, silent mechanism loop (as daily filler), small-to-huge build-up, historic engineering, vehicle insides, versus split-screen. No modern firearms (ad safety).

---

## 7. Brand: Assembly Reel

- **Line:** "Assembly Reel shows how the built world works, one 3D cutaway at a time."
- **Pillars:** Impossible builds · How it actually works · Old genius (historic engineering) · plus silent loops.
- **Look:** navy blueprint background, orange and white accents, Montserrat and Inter fonts (open licences), colour-coded flows (fuel orange, air blue, exhaust grey). A full layout spec with safe zones for both platforms is done.
- **Facebook page:** https://www.facebook.com/assemblyreel (created; logo and cover still to be uploaded by the owner).
- **Page kit:** bio, about text, pinned "start here" plan, caption pattern, comment reply rules, Facebook + YouTube plan. Ready, not yet applied.
- **Honesty:** the page says plainly that the animation is ours and the narration voice is AI.

---

## 8. Our resources

### Hardware

| Item | Spec | Use |
|---|---|---|
| Production PC | RTX 4080 (16 GB VRAM), Intel i9-14900K, 16 GB RAM, ~179 GB free disk | Rendering, voice, all local AI |
| Recommended upgrades | 64 GB RAM first (~$150–300), then a 2 TB NVMe SSD (~$100–180) | 16 GB of system RAM is the main bottleneck |
| Cloud server | Planned: small VPS, ~$7/month | Coordinator, database, dashboard |

### Software (all free or open source)

| Area | Tool | Licence | Status |
|---|---|---|---|
| 3D animation | Blender 4.5 LTS | GPL | Installed |
| Voice | Chatterbox | MIT | Installed |
| Voice fallback | Kokoro | Apache-2.0 | Planned |
| Word timings | WhisperX | BSD | Planned |
| Captions / graphics | Remotion | Free for companies of 3 or fewer | Planned |
| Encoding | FFmpeg | LGPL/GPL | Planned |
| Database | PostgreSQL + pgvector | Open source | In use (code) |
| Optional local AI images/video | Z-Image-Turbo, FLUX.2 klein, Wan 2.2 5B | Various, checked | Optional, later |
| AI models | Claude (Fable 5.2, Opus, Sonnet, Haiku) | Paid API | Not switched on yet |

Rejected after licence checks: F5-TTS (non-commercial weights), Sora (API shut down), WanGP (no hosted use).

### Money

- Spent so far: **$0.**
- Expected running cost once live: about **$25–45 a month at 4 shorts**, **$50–120 at 12 shorts** (mostly Claude API usage; rendering and voice are free on the PC).
- Nothing paid is switched on without the owner's approval.

### Research tools

Virlo and vidIQ are connected but have no credits; research used YouTube directly.

---

## 9. What is next

1. Render the pin tumbler lock showcase clip.
2. Build the first three Blender templates (`assembly_sequence`, `mechanism_loop`, `process_line`).
3. Produce four private draft shorts: four-stroke engine loop, gasoline vs diesel, hydraulic press, bridge built at sea.
4. Owner reviews the showcase and the voice sample and says whether the standard is OK.
5. Move the Content MAX code into its own **private** repository (it is kept out of this public one).
6. When the owner is ready: turn on Claude API billing with a spending cap, set up the cloud server, create the YouTube channel.

**Waiting on the owner:** review of the voice sample and showcase, uploading the logo and cover to the Facebook page, the "go public" decision, and any paid service.

---

## 10. Working with ChatGPT

The [`chatgpt/`](chatgpt/) folder is a shared workspace. We post requests in [`chatgpt/REQUESTS.md`](chatgpt/REQUESTS.md); ChatGPT does the research and writes its findings into [`chatgpt/findings/`](chatgpt/findings/) using the template; we read them, check them, and use what holds up. Instructions for ChatGPT are in [`chatgpt/README.md`](chatgpt/README.md).
