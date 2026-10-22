# SignpostMe landing pages — complete build guide

Written 2026-10-22 for Haseeb. Assumes no prior web-development knowledge and optimises for
learning. Companion files: `design.md` (what it must look like), `PIPELINE.md` (the agent council).

Decisions already made:

| Decision | Choice |
|---|---|
| Scope | Paid-ads landing pages first at `/lp/[slug]`, structured so the full Wellness site can move in later |
| Framework | Next.js 16 (App Router) + TypeScript |
| Styling | Tailwind CSS v4 with `design.md` tokens, shadcn/ui for interactive primitives |
| Build mode | Hybrid: you hand-build the foundation and Hero, agents build the rest, you review |
| Content | One JSON file per landing page, in git |
| Analytics | Deferred. A no-op `track()` stub is left in place so wiring it later touches one file |
| Hosting | Vercel, preview deploy per pull request. Launch on the Vercel URL; custom domain later |
| Repository | This one, `haseeb-phlo/marketing-landing-pages`. App at the root, docs in `docs/` |

How to use this guide: read Part A once, then work through Part B phase by phase. Every phase ends
with a "you should now be able to" check. Do not skip the checks; they are the learning.

---

## Part A — Tooling: options, pros and cons, recommendations

Each row is a decision you would otherwise make by accident. The recommendation is what this guide uses.

### A1. Package manager (installs and runs JavaScript libraries)

| Option | Pros | Cons |
|---|---|---|
| **pnpm** (recommended) | Fast, disk-efficient, strict about dependencies so bugs surface early. Vercel's default. Most docs and agents assume it | One-time install via `corepack` |
| npm | Already installed with Node, zero setup | Slower, looser dependency rules hide mistakes |
| bun (already on your machine, v1.4.3) | Fastest installs and test runner, can replace Node | Occasional incompatibilities with Next.js tooling; fewer agents trained on it |

Why pnpm: strictness teaches you what depends on what, and the Vercel build uses it natively.

### A2. Framework (how pages are built and served)

| Option | Pros | Cons |
|---|---|---|
| **Next.js 16** (chosen) | Matches `signpostme.com`, static generation per LP, server components, best agent support, Vercel-native | Larger mental model than a static site generator |
| Astro | Zero JS by default, fastest LPs, islands for the calculator | Second framework in the estate, weaker for the later full-site app features |
| Plain HTML + Vite | Smallest possible | No routing, no image pipeline, you rebuild everything Next gives you |

### A3. Styling

| Option | Pros | Cons |
|---|---|---|
| **Tailwind v4** (chosen) | Tokens live in CSS (`@theme`), utilities generated from them, agents produce consistent output, no runtime | Class strings get long; discipline needed to use tokens not arbitrary values |
| CSS Modules | Plain CSS, scoped per component | Tokens need manual plumbing; agents drift more |
| MUI (what the core site uses) | Ready-made components | Heavy runtime, emotion CSS-in-JS, fights with Tailwind, worst Lighthouse of the three |

### A4. Component primitives

| Option | Pros | Cons |
|---|---|---|
| **shadcn/ui** (chosen, selectively) | Accessible accordion, slider, switch, dialog copied into your repo as source you own | Its defaults are product-UI (zinc, dark). You override tokens and ignore its page-composition advice for marketing |
| Radix UI directly | Same primitives, no styling | More wiring |
| Headless UI | Smaller set | Fewer primitives (no slider) |

Use shadcn only for: `accordion` (FAQ), `slider` and `switch` (calculator), `dialog` (future). Everything else is hand-written from `design.md` §7.

### A5. Animation

| Option | Pros | Cons |
|---|---|---|
| **CSS first + Motion** (recommended) | Marquee and transitions are pure CSS; `motion` (the successor to framer-motion) for fade-up reveals, tiny API, React-native | Two mental models (CSS and JS) |
| GSAP | Most powerful, timelines, scroll-trigger | Heavier, imperative, overkill for a brand that barely animates |
| None | Simplest | The live sites do have a marquee and subtle reveals |

### A6. Images and assets

Use `next/image` for every photo (automatic AVIF/WebP, sizing, lazy loading). Export source art from
Figma with the script in Phase 2. Logos stay SVG.

### A7. Testing and quality gates

| Tool | What it gives you | Verdict |
|---|---|---|
| **Playwright** | Real-browser tests, screenshots at 390 and 1440, CTA href assertions. Also the judge agent's "eyes" | Required |
| **Lighthouse CI** | Performance, accessibility, SEO scores in CI with thresholds | Required |
| **@axe-core/playwright** | Accessibility rule checks inside Playwright | Required |
| Vitest | Unit tests for pure functions (UTM forwarding, calculator maths) | Nice to have, add in Phase 4 |
| Storybook | Component gallery | Skip; the `/dev/tokens` page and Playwright screenshots cover it |

### A8. Lint and format

| Option | Pros | Cons |
|---|---|---|
| **Biome** (recommended) | One fast tool for lint + format, zero config, agents respect it | Fewer plugins than ESLint |
| ESLint + Prettier | What `create-next-app` sets up | Two tools, slower, config sprawl |

### A9. Version control and review

GitHub via the `gh` CLI (already logged in as haseeb-phlo). One branch per landing page or feature,
pull request per change, Vercel preview on every PR. Branch protection on `main` once the first page ships.

### A10. Deploy

Vercel CLI v53 is installed. Preview per PR, `vercel --prod` to promote. Domain decision in Phase 7.

### A11. Agent tooling

| Tool | Role |
|---|---|
| Claude Code (this) | Pair-builder in Phases 1–4, council orchestrator from Phase 5 |
| `.claude/agents/*.md` | Builder, judge, compliance and fixer definitions (Phase 5) |
| `Workflow` tool | Runs the council deterministically (Phase 6) |
| `npx skills add …` (skills.sh) | Installs reusable agent skills into `.claude/skills` |
| Composio Figma | Already linked; used by the asset export script |
| `superset:browser` skill | Drive a real browser to inspect the running page |
| Chrome headless | Quick full-page screenshots without Playwright |

Skills worth installing (verify each with `--list` before adding):

```bash
npx skills add vercel-labs/agent-skills --list     # react-best-practices, web-design-guidelines, composition-patterns
npx skills add shadcn/ui --list                    # shadcn CLI and registry know-how
```

You already have `design-taste-frontend`, `ui-ux-pro-max` and `web-design-guidelines` locally. Three skills (`web-design-guidelines`, `frontend-design`, `high-end-visual-design`) are switched off in `~/.claude/settings.json` under `skillOverrides`. Turn `web-design-guidelines` on for this project.

---

## Part B — Build, phase by phase

Vocabulary you will meet, defined once:

- **Repository (repo)**: the project folder tracked by git.
- **Dependency**: a library your code imports. Listed in `package.json`, installed into `node_modules`.
- **Component**: a function that returns UI. `Hero()` returns the hero section.
- **Server Component**: runs on the server at build time, ships HTML not JavaScript. Default in Next.
- **Client Component**: runs in the browser because it needs state or events. Marked with `'use client'`.
- **Token**: a named design value (`--color-teal`). Components use the name, never the raw hex.
- **Route**: a URL. In Next a folder under `app/` is a route; `app/lp/[slug]/page.tsx` serves `/lp/anything`.
- **Static generation**: Next renders the page to HTML once at build time. Fastest possible delivery.
- **Preview deploy**: Vercel builds every branch and gives it a URL for review.

### Phase 0 — Environment (30 minutes)

1. Node 24 is installed. Enable pnpm through corepack, which ships with Node:

```bash
corepack enable && corepack prepare pnpm@latest --activate && pnpm -v
```

2. Confirm the CLIs: `vercel whoami`, `gh auth status`, `git config user.name`.

3. Editor: VS Code or Cursor with the Tailwind CSS IntelliSense and Biome extensions.

4. Use this repository. One repo holds the app at the root and the specs in `docs/`, so agents, CI and
   Vercel all see the same history. (A separate repo would only make sense if a different team owned
   the site; it does not.) Work on a branch:

```bash
cd ~/.superset/projects/marketing-landing-pages    # or open a Superset workspace on main
git pull && git switch -c scaffold
```

   Note: the repo is public on GitHub. Nothing secret goes in it; `.env.local` is git-ignored by the
   scaffold and must stay that way.

You should now be able to: explain what `corepack`, `pnpm` and `gh` each do in one sentence.

### Phase 1 — Scaffold Next.js (1 hour, hand-build)

```bash
pnpm create next-app@latest . --typescript --tailwind --biome --app --import-alias "@/*" --use-pnpm --yes
```

`create-next-app` refuses to run in a folder with unexpected files, but it allows `docs/`, `.git`,
`LICENSE` and `README.md`, which is why the three specs live in `docs/`. If it still complains, it will
name the file; move it into `docs/` and rerun.

Flag by flag: `--typescript` catches mistakes before the browser does; `--tailwind` installs v4 and a
`globals.css`; `--biome` sets up Biome for lint and format instead of ESLint; `--app` uses the App
Router (the modern routing system); `--import-alias "@/*"` lets you write `import x from "@/components/x"`;
`--yes` accepts defaults for everything else, which keeps `app/` at the root with no `src/` folder (if you have run `create-next-app` before, `--yes` reuses your saved answers instead, so check the generated tree).
(Checked against `create-next-app` 16.4 on 2026-10-22.)

Then:

```bash
pnpm add motion
pnpm dev
```

Open `http://localhost:3000`. What you are looking at and where it comes from:

| File | What it does |
|---|---|
| `app/layout.tsx` | The HTML shell every page shares: `<html>`, `<body>`, fonts, global CSS |
| `app/page.tsx` | The `/` route. You will replace it with a redirect or a simple index of LPs |
| `app/globals.css` | Tailwind import plus your tokens |
| `next.config.ts` | Framework settings (image domains, redirects) |
| `package.json` | Dependencies and scripts (`pnpm dev`, `pnpm build`) |
| `tsconfig.json` | TypeScript settings, including the `@/*` alias |
| `biome.json` | Lint and format rules |

Delete the sample content in `app/page.tsx` and make it return a one-line `<main>`. Commit:

```bash
git add -A && git commit -m "Scaffold Next.js 16 + Tailwind v4 + Biome"
```

Note for later: in Next 16 the request-interception file is `proxy.ts` with a `proxy()` export and
`proxyConfig`, not `middleware.ts`. You will not need it for LPs; you may for the full site.

You should now be able to: add a new route by creating a folder with a `page.tsx` and see it in the browser.

### Phase 2 — Tokens, fonts and assets (2 hours, hand-build)

**2.1 Tokens.** Replace `app/globals.css` with the `@theme` block from `design.md` §3 (including the
`--color-flag-dark` line added after the Figma pass). Keep the `html`, `body` and heading rules under it.

What `@theme` does: every `--color-*` becomes a Tailwind utility (`bg-teal`, `text-ink`,
`border-line`), every `--text-*` becomes `text-h1` with its paired line-height, every `--radius-*`
becomes `rounded-md`. The first lines of the block, `--color-*: initial;` and friends, delete
Tailwind's built-in palette, fonts, radii and shadows. Without them `bg-zinc-900` would still compile
with no hex in sight and the "tokens only" rule would be unenforceable. With them, a component can only
leave the system by writing an arbitrary value like `bg-[#123456]`, which the judge greps for.

**2.2 Fonts.** Create `app/fonts.ts`:

```ts
import { Plus_Jakarta_Sans, Inter } from "next/font/google";

export const pjs = Plus_Jakarta_Sans({
  subsets: ["latin"],
  weight: ["700", "800"],
  variable: "--font-pjs",
  display: "swap",
});

export const inter = Inter({
  subsets: ["latin"],
  weight: ["400", "700"],
  variable: "--font-inter",
  display: "swap",
});
```

In `app/layout.tsx` put both `variable` class names on `<html>`:

```tsx
import { pjs, inter } from "./fonts";
import "./globals.css";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en-GB" data-surface="wellness" className={`${pjs.variable} ${inter.variable}`}>
      <body className="antialiased">{children}</body>
    </html>
  );
}
```

How this fits together, because it is the single most common font bug with this stack. `next/font`
downloads the font at build time, writes the `@font-face` rules, and the `variable` class on `<html>`
makes sure those rules are loaded. Tailwind v4 resolves `@theme` at build time, so the `@theme` keys
`--font-heading` and `--font-body` must contain the literal family names `"Plus Jakarta Sans"` and
`"Inter"`, which are exactly the names `next/font` registers. The `variable` names (`--font-pjs`,
`--font-inter`) are deliberately different from the `@theme` keys so nothing overrides itself at runtime.
Do not use `.className` for two fonts on the same element: both set `font-family` and only the last wins.

**2.3 Token page.** Create `app/dev/tokens/page.tsx` that renders every colour as a swatch with its
name, every text size as a sample line, and the four button variants. This page is your visual unit
test for Phase 2 and the judge's reference later. Keep it out of production with a `robots` `noindex`
and by excluding `/dev` in Phase 7.

**2.4 Assets from Figma.** Create `scripts/figma-export.mjs`:

```js
// Usage: FIGMA_TOKEN=... node scripts/figma-export.mjs
const FILE = "GfxumnaSlcB9jzZ1NMRt89";
const FRAMES = {
  "604:93": "lp/LP-home-hero-1x1", "493:5037": "lp/home-hero-4x3", "493:5060": "lp/home-WL-4x3",
  "487:244": "lp/dennis-ouko", "487:250": "lp/order-delivered", "489:510": "lp/WL-50",
  "489:481": "lp/WL-20", "484:133": "brand/hero-bg-pattern", "484:136": "brand/hero-bg-pattern-mob",
  "486:196": "brand/SP_phlo-inverse", "473:167": "brand/SPW-phlo", "492:3527": "brand/SPMW-opengraph",
};
const ids = Object.keys(FRAMES).join(",");
const res = await fetch(`https://api.figma.com/v1/images/${FILE}?ids=${ids}&format=png&scale=2`,
  { headers: { "X-Figma-Token": process.env.FIGMA_TOKEN } });
const { images } = await res.json();
const { mkdir, writeFile } = await import("node:fs/promises");
for (const [id, name] of Object.entries(FRAMES)) {
  const buf = Buffer.from(await (await fetch(images[id])).arrayBuffer());
  await mkdir(`public/${name.split("/")[0]}`, { recursive: true });
  await writeFile(`public/${name}.png`, buf);
  console.log("saved", name);
}
```

Get a personal access token in Figma (Settings → Security → Personal access tokens, read-only). Put it in
`.env.local` as `FIGMA_TOKEN=…` and never commit that file. Logos: download the SVGs from the live site
(URLs in `design.md` §8) into `public/brand/` because SVG exports keep their fills.

Commit: "Add tokens, fonts, token page, Figma asset export".

You should now be able to: say what happens when you add `--color-foo: #abc` to `@theme`, and why
`next/font` removes layout shift.

### Phase 3 — Content model and the Hero (3 hours, hand-build)

**3.1 Content schema.** Create `content/lps/schema.ts` using `zod` (`pnpm add zod`):

```ts
import { z } from "zod";

export const LpSchema = z.object({
  slug: z.string(),
  title: z.string(),              // <title>, human
  description: z.string(),        // meta description
  announcement: z.object({ text: z.string(), expires: z.string() }).optional(),
  hero: z.object({
    h1: z.string(),
    price: z.string(),            // "Prices from £89"
    strap: z.string(),
    proofs: z.array(z.string()).length(3),
    mosaic: z.enum(["LP-home-hero-1x1", "home-hero-4x3"]),
    statBadge: z.enum(["WL-50", "WL-20"]),
  }),
  cta: z.object({
    condition: z.literal("weight-loss"),
    variant: z.string(),          // "starter"
    primaryLabel: z.string(),     // "From £89 – Get started"
    secondaryLabel: z.string(),   // "Explore weight loss"
  }),
  price: z.object({ from: z.string(), delivery: z.string() }),
});
export type Lp = z.infer<typeof LpSchema>;
```

Why a schema: agents will write these files. The schema rejects a malformed one at build time instead
of shipping a broken page. Create `content/lps/wls-oct.json` by copying the live `lp-wls` copy from
`design.md` §11.

**3.2 Route.** `app/lp/[slug]/page.tsx`:

```tsx
import { notFound } from "next/navigation";
import { readdir, readFile } from "node:fs/promises";
import { LpSchema } from "@/content/lps/schema";
import { Hero } from "@/components/sections/Hero";

export async function generateStaticParams() {
  const files = await readdir("content/lps");
  return files.filter(f => f.endsWith(".json")).map(f => ({ slug: f.replace(".json", "") }));
}

export async function generateMetadata({ params }: { params: Promise<{ slug: string }> }) {
  const { slug } = await params;
  const lp = await load(slug);
  return { title: lp.title, description: lp.description, robots: { index: false, follow: true } };
}

async function load(slug: string) {
  try { return LpSchema.parse(JSON.parse(await readFile(`content/lps/${slug}.json`, "utf8"))); }
  catch { notFound(); }
}

export default async function Page({ params }: { params: Promise<{ slug: string }> }) {
  const { slug } = await params;
  const lp = await load(slug);
  return (
    <main>
      <Hero lp={lp} />
    </main>
  );
}
```

Three things to understand here. `generateStaticParams` tells Next which slugs exist so it renders them
all at build time. `params` is a Promise in Next 15+ so it must be awaited. The file read happens on
the server at build; no JSON reaches the browser. The relative path `content/lps/...` works because
Next runs the build from the repo root, locally and on Vercel.

**3.3 Button.** `components/ui/Button.tsx` with `class-variance-authority` (`pnpm add class-variance-authority clsx tailwind-merge`):

```tsx
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/cn";

const button = cva(
  "inline-flex items-center justify-center gap-2.5 rounded-md border-2 px-8 py-3 font-heading text-body font-extrabold leading-none transition-colors duration-250 ease-out max-sm:w-full",
  {
    variants: {
      variant: {
        primary: "border-peach bg-peach text-teal-deep hover:border-peach-hover hover:bg-peach-hover",
        inverse: "border-white bg-white text-teal",
        outline: "border-teal bg-transparent text-teal",
        outlineInverse: "border-white bg-transparent text-white",
      },
      glow: { true: "shadow-glow" },
    },
    defaultVariants: { variant: "primary" },
  },
);

export function Button({ className, variant, glow, ...props }: React.ComponentProps<"a"> & VariantProps<typeof button>) {
  return <a className={cn(button({ variant, glow }), className)} {...props} />;
}
```

Every value above comes from `design.md` §7.1. If you cannot trace a class back to the spec, delete it.

**3.4 CTA link with UTM forwarding.** `components/CtaLink.tsx` is a Client Component because it reads
the browser URL:

```tsx
"use client";
import { useEffect, useState } from "react";
import { Button } from "@/components/ui/Button";
import { track } from "@/lib/track";

const APP = "https://app.wellnessbyphlo.signpostme.com/getting-started";
const KEEP = ["utm_source","utm_medium","utm_campaign","utm_content","utm_term","fbclid","gclid","ttclid"];

export function CtaLink({ condition, variant, position, children, ...rest }:
  { condition: string; variant: string; position: string } & React.ComponentProps<typeof Button>) {
  const base = `${APP}?condition=${condition}&variant=${variant}`;
  const [href, setHref] = useState(base);
  useEffect(() => {
    const url = new URL(base);
    const current = new URLSearchParams(window.location.search);
    for (const k of KEEP) { const v = current.get(k); if (v) url.searchParams.set(k, v); }
    setHref(url.toString());
  }, [base]);
  return (
    <Button href={href} data-cta={position} onClick={() => track("cta_click", { position, variant })} {...rest}>
      {children}
    </Button>
  );
}
```

And `lib/track.ts`, the analytics stub that stays a no-op until you decide on a vendor:

```ts
export function track(event: string, props: Record<string, string | number> = {}) {
  if (process.env.NODE_ENV !== "production") console.debug("[track]", event, props);
}
```

Why not read `searchParams` on the server: doing so makes the page dynamic and loses static
generation. Appending in the browser keeps the page static and still forwards attribution.

**3.5 Hero.** `components/sections/Hero.tsx` as a Server Component following `design.md` §7.9: teal
panel, pattern SVG as a positioned background, H1 / H2 price / H3 strap, primary + outline-inverse
CTAs, three tick proofs, and the mosaic image via `next/image` with `priority` (it is the LCP element).

**3.6 Check in the browser at 390 and 1440.** Compare with `lp-wls` side by side. Fix until it matches.

Commit: "Content schema, LP route, Button, CtaLink, Hero".

You should now be able to: explain the difference between a Server and a Client Component using
`Hero` and `CtaLink` as the examples, and why `params` is awaited.

### Phase 4 — Test harness (2 hours, hand-build)

This is the part most people skip and the part that makes the agent council possible.

```bash
pnpm create playwright     # choose tests/ folder, TypeScript, no GitHub Action yet
pnpm exec playwright install chromium
pnpm add -D @axe-core/playwright @lhci/cli
```

`tests/lp.spec.ts`:

```ts
import { test, expect } from "@playwright/test";
import AxeBuilder from "@axe-core/playwright";

for (const vp of [{ w: 390, h: 844, name: "mobile" }, { w: 1440, h: 900, name: "desktop" }]) {
  test(`wls-oct renders at ${vp.name}`, async ({ page }) => {
    await page.setViewportSize({ width: vp.w, height: vp.h });
    await page.goto("/lp/wls-oct?utm_source=test&fbclid=abc");
    await expect(page.getByRole("heading", { level: 1 })).toHaveText("Weight loss service");
    const cta = page.locator('[data-cta="hero"]');
    await expect(cta).toHaveAttribute("href", /condition=weight-loss&variant=starter/);
    await expect(cta).toHaveAttribute("href", /utm_source=test/);
    await expect(cta).toHaveAttribute("href", /fbclid=abc/);
    await page.screenshot({ path: `test-results/wls-oct-${vp.name}.png`, fullPage: true });
    const a11y = await new AxeBuilder({ page }).analyze();
    expect(a11y.violations.filter(v => v.impact === "serious" || v.impact === "critical")).toEqual([]);
  });
}
```

`playwright.config.ts` needs two things the generator does not add: a `baseURL` so `page.goto("/lp/…")`
resolves, and a `webServer` so the site is running when tests start:

```ts
import { defineConfig } from "@playwright/test";
export default defineConfig({
  testDir: "tests",
  use: { baseURL: "http://localhost:3000" },
  webServer: { command: "pnpm build && pnpm start", url: "http://localhost:3000", reuseExistingServer: !process.env.CI },
  projects: [{ name: "chromium", use: { browserName: "chromium" } }],
});
```

`lighthouserc.json` (no preset, so Lighthouse runs its default mobile emulation, which is the threshold
`design.md` §17 and `PIPELINE.md` §4 use):

```json
{ "ci": { "collect": { "url": ["http://localhost:3000/lp/wls-oct"], "startServerCommand": "pnpm start" },
          "assert": { "assertions": { "categories:performance": ["error", { "minScore": 0.95 }], "categories:accessibility": ["error", { "minScore": 1 }] } } } }
```

Scripts in `package.json`:

```json
"test": "playwright test",
"lhci": "pnpm build && lhci autorun",
"check": "biome check . && tsc --noEmit && pnpm test"
```

`.github/workflows/check.yml`, so every pull request is red or green before a human looks at it:

```yaml
name: check
on: [pull_request]
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
      - uses: actions/setup-node@v4
        with: { node-version: 24, cache: pnpm }
      - run: pnpm install --frozen-lockfile
      - run: pnpm exec playwright install --with-deps chromium
      - run: pnpm check
      - run: pnpm lhci
      - uses: actions/upload-artifact@v4
        if: always()
        with: { name: screenshots, path: test-results }
```

Commit: "Playwright, axe, Lighthouse CI, check workflow".

You should now be able to: break the CTA on purpose, run `pnpm test`, and watch it fail with a
message that names the exact assertion.

### Phase 5 — Agent setup (1 hour)

**5.1 `CLAUDE.md`** at the repo root, short:

```
Read docs/design.md before touching any UI. Tokens only; no hex outside app/globals.css.
Content lives in content/lps/*.json validated by content/lps/schema.ts.
Run `pnpm check` before claiming done. Screenshots land in test-results/.
CTA contract and tracking rules: docs/design.md §12. Claims rules: docs/design.md §10.
Council and judge: docs/PIPELINE.md. Build phases and timelines: docs/BUILD-GUIDE.md.
```

**5.2 Agents** in `.claude/agents/`. Minimal frontmatter plus a role prompt:

```md
---
name: lp-builder
description: Builds one landing-page section from design.md and the content JSON
tools: Read, Edit, Write, Bash, Grep, Glob
model: sonnet
---
You build exactly the section you are assigned, in components/sections/<Name>.tsx, from design.md §7 and §11.
Use only tokens. Do not touch other sections. Run `pnpm check` and fix until green. Report files changed.
```

```md
---
name: brand-judge
description: Scores a landing page build against design.md; read-only
tools: Read, Grep, Glob, Bash
model: opus
---
You are adversarial. Score the branch using the rubric in PIPELINE.md §4. Evidence only: cite file and line
or name a screenshot region in test-results/. Never edit code. Output the JSON shape in PIPELINE.md §4.
```

Add `copy-compliance` (sonnet, Read/Grep only, rules from `design.md` §10) and `fixer` (sonnet, full
tools, takes a defect list). Keep `tools:` minimal; the judge must not be able to edit.

**5.3 Skills and hooks.** Install the skills from Part A11. Then add a hook in the repo's
`.claude/settings.json` that formats every file an agent edits, so unformatted code never reaches a PR
(the `update-config` skill in this session can write this for you):

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{ "type": "command", "command": "pnpm exec biome check --write \"$CLAUDE_FILE_PATH\" >/dev/null 2>&1 || true" }]
      }
    ]
  }
}
```

You should now be able to: run `claude` in the repo, ask `@lp-builder` to build the trust marquee, and
see it finish with `pnpm check` green.

### Phase 6 — The council (first run: half a day)

1. Pick three sections for the first run so the loop is cheap: TrustMarquee, BenefitCards, PricingBlock.
2. Start the Workflow (say "use a workflow" to opt in) with the script in `PIPELINE.md` §3, or run the
   four agents by hand in order: builder A/B/C in worktrees → judge each → pick → fixer.
3. Read every judge report in full. Where the judge was wrong, the spec was ambiguous: fix `design.md`,
   not the judge. Where the judge was right, you learned a rule.
4. Merge the winner. Repeat for the next sections until all 14 in `design.md` §11 exist.
5. Human review checklist before merging any page: both screenshots match `lp-wls` in rhythm and colour;
   price appears in hero, price block and sticky bar; every CTA passes the UTM test; legal block present;
   no section uses a colour outside `/dev/tokens`.

What you are learning here is the real skill: writing a spec tight enough that three independent
builders converge, and reading output critically rather than trusting it.

### Phase 7 — Deploy (1 hour)

```bash
vercel link          # creates the Vercel project
vercel               # preview deploy; copy the URL into a PR comment
vercel --prod        # when the PR is merged
```

Set `FIGMA_TOKEN` in Vercel only if the export runs in CI; otherwise keep it local.

Domain: launch on the Vercel-provided URL (`marketing-landing-pages.vercel.app` or whatever `vercel link`
assigns). Decision 2026-10-22: custom domain comes later. When it does, the options are:

| Option | Pros | Cons |
|---|---|---|
| **`lp.signpostme.com`** (recommended when ready) | One CNAME at the DNS provider, no interaction with Webflow or the core Next site, clean analytics scope | New subdomain to get approved by whoever controls `signpostme.com` DNS (likely Regent, not Phlo) |
| `wellnessbyphlo.signpostme.com/lp/*` | Same host as today's LPs | Webflow cannot proxy a path to Vercel; you would have to move the whole subdomain, which is Phase 9 |
| Vercel-provided `*.vercel.app` (now) | Zero DNS work, fine for testing and internal review | Meta ad review can flag it and it shows the vendor; swap before scaling spend |

Add `/dev/*` to a `robots` disallow and set `noindex` on `/lp/*` (already in `generateMetadata`).

### Phase 8 — Ad to page (ongoing)

- One JSON per ad concept. Slug equals the Meta ad set name so the dashboard and the repo agree.
- Message match: the Figma OG tile style (`design.md` §15) is the ad look; the LP hero repeats its
  headline and the same stat badge. A visitor should see the same number in the ad and above the fold.
- Meta URL parameters template: `utm_source=meta&utm_medium=paid&utm_campaign={{campaign.name}}&utm_content={{ad.name}}`.
- Analytics is deferred. `lib/track.ts` exports `track(event, props)` as a no-op; `CtaLink` calls it on click.
  When you decide, implementing GTM, PostHog or both is a change to that one file plus the consent script.

### Phase 9 — Later: the full site

Everything above was chosen to make this a continuation, not a rewrite:

- Add `app/(site)/` as a route group for home, `/weight-loss-service/*`, `/faqs`, `/legal/*`.
- Reuse the same sections; the Webflow home is built from the same 14 blocks as the LP.
- Content for service pages moves to MDX in `content/pages/`; the schema pattern repeats.
- Move the DNS for `wellnessbyphlo.signpostme.com` to Vercel at cutover; keep Webflow published until then.
- At that point revisit the CMS decision (`BUILD-GUIDE` Part A, content row) with real editor needs.

---

## Part C — Becoming a strong builder with this stack

Ten concepts to actually understand, in order, each with the file in this project that demonstrates it:

1. **HTML, CSS, the box model** — `app/dev/tokens/page.tsx`. Resize the window and watch flex wrap.
2. **CSS custom properties and `@theme`** — `app/globals.css`. Rename a token and see every usage change.
3. **Components and props** — `Button.tsx`. Add a `size` variant.
4. **Server vs Client Components** — `Hero.tsx` vs `CtaLink.tsx`. Try adding `useState` to `Hero` and read the error.
5. **Static generation and routing** — `app/lp/[slug]/page.tsx`. Add a JSON file, run `pnpm build`, see the new page in the build output.
6. **Images and LCP** — the mosaic `next/image`. Remove `priority` and compare Lighthouse LCP.
7. **Accessibility** — the axe test. Remove an `alt` and watch it fail.
8. **Testing as specification** — `tests/lp.spec.ts`. Every rule in `design.md` that can be asserted should be.
9. **Git and review** — PRs with preview URLs. Review your own diff before asking anyone else.
10. **Agent orchestration** — `PIPELINE.md`. The spec is the product; the code is a derivative.

Habits that separate strong builders from everyone else:

- Write the spec before the code, and update the spec when the code teaches you something.
- Screenshot every change at two widths. Eyes catch what tests miss and tests catch what eyes miss.
- Never accept a colour, size or spacing you cannot trace to a token.
- Read agent diffs line by line. If you cannot explain a line, ask the agent to explain it, then decide.
- Keep a `DECISIONS.md` log: date, decision, why, what would change it. This file is the start of it.
- Measure before optimising: Lighthouse and the Playwright trace viewer, not intuition.

Reference material inside this session's skills (ask me to open any of them): `vercel:nextjs`
references on fonts, images, metadata, hydration errors and `proxy.ts`; `vercel:shadcn` on the CLI and
the `@theme inline` font gotcha; `vercel:deploy` and `vercel:env` for the Vercel CLI.

---

## Day-one checklist

- [ ] Phase 0 complete: `pnpm -v`, `vercel whoami`, `gh auth status` all answer
- [ ] Phase 1: `pnpm dev` shows your one-line page; first commit pushed
- [ ] Phase 2: `/dev/tokens` shows every colour and type size from `design.md` §3–5; fonts are Plus Jakarta Sans and Inter in the DevTools computed styles
- [ ] Phase 3: `/lp/wls-oct` renders the Hero and matches `lp-wls` at 390 and 1440
- [ ] Phase 4: `pnpm check` is green and `test-results/` has two screenshots
- [ ] Phase 5: `.claude/agents/` has four files and `CLAUDE.md` exists
- [ ] Phase 6: first council run merged three sections
- [ ] Phase 7: preview URL in a PR; production on the Vercel URL

---

## Part D — Timelines

Estimates are in focused working days for someone new to this stack, pairing with Claude Code,
with the three specs already written. Two calendar views: full-time (5 days a week) and part-time
(2 days a week). Add 30% if you are interrupted a lot; subtract 30% once you have shipped the first page.

### MVP: one landing page live on the Vercel URL

Included: Phases 0–7. The `wls-oct` page with all 14 sections from `design.md` §11, tokens and fonts
correct, CTA contract with UTM forwarding, legal block, `noindex`, Playwright + axe + Lighthouse green in
CI, four agents defined, one council run merged, production deploy on `*.vercel.app`. Not included:
custom domain, analytics, A/B, a second page.

| Phase | Work | Days |
|---|---|---|
| 0 Environment | pnpm, CLIs, editor, branch | 0.5 |
| 1 Scaffold | create-next-app, Biome, first commit | 0.5 |
| 2 Tokens, fonts, assets | globals.css, fonts.ts, `/dev/tokens`, Figma export | 1 |
| 3 Content + Hero | schema, route, Button, CtaLink, Hero, matching at two widths | 1.5 |
| 4 Test harness | Playwright, axe, Lighthouse CI, GitHub Action | 1 |
| 5 Agent setup | CLAUDE.md, four agents, skills, hook | 0.5 |
| 6 Council | 13 sections in 4–5 runs, judge reads, spec fixes, human review | 2.5 |
| 7 Deploy | vercel link, preview, prod | 0.5 |
| **MVP total** | | **8 days** |

Calendar: **2 weeks full-time**, or **4 weeks at 2 days a week**. The first sprint (Phases 0–4) is the
slow, learning-heavy half; Phase 6 goes faster than it looks because agents do the typing.

### Full build: the Wellness site off Webflow

Included on top of the MVP: Phase 9 in full. Home page, `/weight-loss-service` and the three treatment
pages (Wegovy, Wegovy tablets, Mounjaro), help centre, contact, FAQs, privacy and terms, global nav with
mobile drawer, footer CTA band, the calculator as a shared component, MDX content for service pages,
indexable SEO with sitemap and OG images, 301 redirects from every current Webflow URL, the analytics
decision implemented (Phase 8 stub replaced) with Cookiebot consent, one A/B experiment on a hero, three
further landing pages for ad variants, DNS cutover of `wellnessbyphlo.signpostme.com` to Vercel, and the
CMS decision revisited with real editor needs.

| Block | Work | Days |
|---|---|---|
| Site shell | Route group, nav, drawer, footer, shared layout, `proxy.ts` redirects | 2 |
| Pages | Home + 4 service pages + help centre + contact + FAQs + 2 legal, mostly council-built | 5 |
| Content + SEO | MDX pipeline, metadata, sitemap, OG images, redirect map from Webflow | 2 |
| Analytics + consent | GTM/PostHog wiring, consent gating, event audit | 2 |
| Experiments + LPs | One A/B, three more LP JSONs with Figma assets | 2 |
| Cutover | DNS, Webflow freeze, redirect verification, Lighthouse on every route | 2 |
| Buffer | Unknowns, stakeholder review, copy sign-off with Phlo | 3 |
| **Full build total** | | **18 days after MVP** |

Calendar: **about 4 more weeks full-time (6 weeks from today)**, or **about 9 more weeks at 2 days a
week (13 weeks from today)**. The cutover block is wall-clock bound by DNS approval and Phlo copy sign-off,
so start those conversations during the MVP sprint.
