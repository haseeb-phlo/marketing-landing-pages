# Landing-page build pipeline (council of agents + LLM-as-judge)

Companion to `design.md`. Describes how to turn a campaign brief into a deployed, brand-checked
landing page using Claude Code, with multiple builder agents competing and a judge agent scoring
against `design.md`.

## 1. Stack

| Layer | Choice | Why |
|---|---|---|
| Framework | Next.js 16 App Router (current stable), TypeScript | Matches `signpostme.com`; static export per LP; server-side UTM handling |
| Styling | Tailwind v4 (`@theme` tokens from `design.md` §3) + shadcn/ui | Tokens as CSS vars, components as editable files agents handle well |
| Fonts | `next/font/google` Plus Jakarta Sans + Inter | Self-hosted, no layout shift |
| Content | `content/lps/<slug>.json` (headline, strap, proofs, price, mosaic) | One file per ad; agents edit content without touching layout |
| Analytics | **Deferred (decision 2026-10-22).** `lib/track.ts` no-op stub; candidates are GTM + PostHog, GTM + Vercel Flags, or PostHog only, see BUILD-GUIDE Part A | One-file change when decided |
| Flags / A/B | Deferred with analytics | |
| Hosting | Vercel, preview deploy per PR, `lp.signpostme.com` or sub-path | Vercel plugin already in this session |
| Tests | Playwright (screenshots 390/1440, CTA href assertions), Lighthouse CI, axe | Judge consumes these artefacts |

Routes: `/lp/[slug]` reads `content/lps/[slug].json`, declares `data-surface="wellness"`, sets `noindex`.

## 2. Agents (`.claude/agents/*.md`)

| Agent | Model | Role |
|---|---|---|
| `lp-builder-a`, `lp-builder-b`, `lp-builder-c` | sonnet | Each builds the same brief independently in its own worktree. Given `design.md`, the brief and the content JSON. Different seeds: A "closest to lp-wls", B "best conversion practice", C "most visually distinct within brand". |
| `brand-judge` | opus | Read-only. Scores each build on the rubric below using the Playwright screenshots, the diff, Lighthouse JSON and `design.md`. Returns JSON. |
| `copy-compliance` | sonnet | Checks claims against `design.md` §10 (GLP-1 claim rules, GPhC block, no before/after). Hard fail on any breach. |
| `fixer` | sonnet | Takes the winning build plus the judge's defect list and applies fixes. |
| `deployer` | haiku | Runs `vercel deploy`, posts preview URL. |

Each agent file has frontmatter `tools:` limited to what it needs (judge: Read, Grep, Bash for screenshots only).

## 3. Workflow

```
brief.md ─► content JSON ─► parallel(builder A, B, C) ─► playwright + lighthouse per build
        ─► parallel(brand-judge × 3 builds, copy-compliance × 3)
        ─► pick winner (weighted score, compliance must pass)
        ─► fixer loop (max 2 rounds, judge re-scores)
        ─► PR + Vercel preview ─► human sign-off ─► promote
```

Implement with the Claude Code `Workflow` tool (opt in with "use a workflow") or as a plain script
calling `claude -p` per stage. Keep under 10 agents per run: 3 builders, 3 judges, 3 compliance, 1 fixer.

Pseudo-script for the Workflow tool:

```js
export const meta = { name: 'lp-council', description: 'Build 3 LP variants, judge, fix, deploy',
  phases: [{ title: 'Build' }, { title: 'Judge' }, { title: 'Fix' }] }
const brief = args.brief
const builds = await parallel(['a','b','c'].map(k => () =>
  agent(`Build ${brief} per design.md. Seed: ${SEEDS[k]}. Output branch lp/${brief.slug}-${k}.`,
        { label: `build:${k}`, phase: 'Build', isolation: 'worktree' })))
const scored = await parallel(builds.map(b => () =>
  agent(`Score branch ${b.branch} against design.md using the rubric in PIPELINE.md §4. Return JSON.`,
        { label: `judge:${b.branch}`, phase: 'Judge', schema: SCORE_SCHEMA })))
const winner = scored.filter(s => s.compliance.pass).sort((x,y) => y.total - x.total)[0]
await agent(`Apply these fixes on ${winner.branch}: ${JSON.stringify(winner.defects)}`, { phase: 'Fix' })
return winner
```

## 4. Judge rubric (returns JSON)

| Dimension | Weight | How it is measured |
|---|---|---|
| Token fidelity | 25 | No hex outside `globals.css`; colours, radii, fonts match `design.md` §3–5. Grep + screenshot inspection |
| Layout fidelity | 20 | Section order matches §11; spacing and container widths within ±8px of §6 |
| Visual quality | 15 | Judge compares 1440px screenshot with `lp-wls` reference; penalise generic "AI template" look (see `design-taste-frontend` skill) |
| Conversion hygiene | 15 | Price in hero, one primary CTA colour, sticky bar, trust strip, ≥4 CTAs to the §12 contract with UTMs forwarded |
| Performance | 10 | Lighthouse mobile ≥95, LCP <2.0s, CLS <0.05 |
| Accessibility | 10 | axe 0 serious, contrast per §4, focus rings, alt text |
| Copy + compliance | 5 (gate) | `copy-compliance` must pass; any breach = disqualified regardless of score |

Output shape:

```json
{ "branch": "lp/wls-oct-a", "total": 86,
  "scores": { "tokens": 23, "layout": 18, "visual": 12, "conversion": 14, "perf": 9, "a11y": 10 },
  "compliance": { "pass": true, "notes": [] },
  "defects": [ { "severity": "high", "file": "components/Hero.tsx", "line": 41, "issue": "button radius 999px, spec is 10px" } ] }
```

Judge rules: evidence-only (quote file and line, or name the screenshot region); never rewrite code;
score each dimension before summing; disagree with builders by default (adversarial stance).

## 5. Why a council works here, and where it does not

- Works: three independent interpretations of the same `design.md` expose ambiguities in the spec.
  Every ambiguity the judge trips over becomes a line in `design.md`. The spec gets sharper each run.
- Works: the judge has an objective reference (live `lp-wls`, exact tokens), so scores are not vibes.
- Does not work: judging "which converts better". Only Meta and Vercel Flags data can do that.
  Ship the top two builds as a flag split and let the pixel decide.
- Cost control: builders on sonnet, one opus judge pass per build, haiku for deploy. Cache
  `design.md` in the system prompt so each agent reads it once.

## 6. Learning path (do these in order)

1. Scaffold Next.js + Tailwind v4 by hand, paste the `@theme` block, build the Hero only. Compare at 1440px with the live LP.
2. Add Playwright screenshot test. Now you have the judge's eyes.
3. Write `brand-judge.md` and run it manually on your own Hero. Fix what it finds.
4. Add one builder agent, then two more with different seeds. Watch where they diverge.
5. Wire the Workflow script. Add Lighthouse CI and axe.
6. Add Vercel Flags for a hero A/B and connect Meta conversions API via GTM.
