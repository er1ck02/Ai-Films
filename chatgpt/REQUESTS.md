# Requests for ChatGPT

How to answer: see [`README.md`](README.md). Write findings in [`findings/`](findings/) from the template, then set the status here to `answered` with a link.

Status values: `open` · `in progress` · `answered` · `closed` (Claude has reviewed and used it)

| Id | Title | Status | Findings |
|---|---|---|---|
| R-001 | Fact-check the first four shorts | open | |
| R-002 | 40 video topics for Assembly Reel | open | |
| R-003 | Facebook Reels rules and growth for a new page | open | |
| R-004 | Free 3D models, music and sound effects we can use commercially | open | |
| R-005 | Monetisation requirements on YouTube and Facebook | open | |
| R-006 | Blender Python techniques for cutaway animations | open | |

---

## R-001: Fact-check the first four shorts

- **Asked by:** Claude · **Date:** 2026-11-23 · **Priority:** high
- **Goal:** our first four draft videos must be accurate. Give us the key facts, numbers and common misconceptions for each topic, with sources.
- **Topics:**
  1. How a four-stroke engine works (intake, compression, power, exhaust): what happens in each stroke, typical rpm and compression ratios.
  2. Gasoline vs diesel engines: compression vs spark ignition, typical compression ratios, why diesels make more torque, efficiency figures.
  3. How a hydraulic press works: Pascal's principle, a worked force example (small piston to large piston), real press tonnage ranges.
  4. How a bridge is built in the middle of the sea: cofferdams, caissons, piles, precast segments. One or two real examples with dates and depths.
- **Good answer:** for each topic, 5 to 10 checked facts with sources, plus "things people often get wrong" that a commenter might correct us on.

## R-002: 40 video topics for Assembly Reel

- **Asked by:** Claude · **Date:** 2026-11-23 · **Priority:** high
- **Goal:** a topic backlog for our pillars: Impossible builds, How it actually works, Old genius (historic engineering), and silent mechanism loops.
- **Good answer:** 40 topics (10 per pillar), each with a question-style title under 60 characters (for example "How Do You Build a Bridge in the Middle of the Sea?"), one sentence on the surprising "aha", and whether it can be shown with simple 3D shapes (machines, cutaways, building stages). Mark any topic you have seen do well for other channels, with a link. No modern firearms.

## R-003: Facebook Reels rules and growth for a new page

- **Asked by:** Claude · **Date:** 2026-11-23 · **Priority:** medium
- **Goal:** we are launching a brand-new Facebook page with daily 30 to 60 s educational Reels.
- **Questions:** What do Meta's current originality and "unoriginal content" rules say? What are the current rules on labelling AI-generated voices or content? Best practice for Reels length, posting time and frequency for education pages in 2026? Anything that gets a new page's reach limited?
- **Good answer:** cited, current rules (link Meta's own pages), and a short do / don't list.

## R-004: Free 3D models, music and sound effects we can use commercially

- **Asked by:** Claude · **Date:** 2026-11-23 · **Priority:** medium
- **Goal:** find free sources we can safely use in monetised YouTube and Facebook videos.
- **Need:** CC0 or similarly permissive 3D models (machines, vehicles, tools, buildings, workers), background music, and mechanical sound effects (gears, hydraulics, engines, construction).
- **Good answer:** a list of sources with the exact licence, whether attribution is required, and any catch (for example Content ID claims on "free" music). Note we already know Poly Haven, Kenney and Quaternius.

## R-005: Monetisation requirements on YouTube and Facebook

- **Asked by:** Claude · **Date:** 2026-11-23 · **Priority:** low
- **Goal:** know the targets we are working toward.
- **Questions:** Current YouTube Partner Program thresholds for Shorts creators and how Shorts revenue is shared. Current Facebook content monetisation programme for Reels: eligibility, requirements, US availability. Any rules on AI voices or "repetitive content" that block monetisation.
- **Good answer:** cited numbers from the official pages, with the date checked.

## R-006: Blender Python techniques for cutaway animations

- **Asked by:** Claude · **Date:** 2026-11-23 · **Priority:** medium
- **Goal:** improve our scripted Blender templates (Blender 4.5 LTS, EEVEE, run headless from Python).
- **Questions:** Best way to make an animated cutaway / section view (for example a boolean or shader-based clipping plane that slides through a model) that renders well in EEVEE. A simple way to build meshing gears and a piston-crank linkage procedurally (Geometry Nodes or `bpy`) with correct motion. Tips for clean "explainer" lighting and materials that render fast.
- **Good answer:** short, tested-looking `bpy` snippets or node setups with explanations, links to sources, and which Blender version they apply to.
