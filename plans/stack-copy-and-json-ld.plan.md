---
name: Stack copy and JSON-LD
overview: Refresh site copy and JSON-LD so messaging matches your three delivery modes (lighter Webflow + Memberstack + Airtable + Make; scalable Webflow + Wized + Xano/Supabase; full-code Next.js/static on Vercel), with the databases page reframed around data and backends—not Airtable alone. Keep FAQ visible text and FAQPage schema in lockstep for SEO compliance.
todos:
  - id: copy-map
    content: Produce per-page bullet map (hero, stack ladder, FAQs, process, service bullets) from user’s three-mode narrative
    status: pending
  - id: html-index
    content: "Update index.html: meta/OG, body copy, FAQ HTML; then sync FAQPage + HowTo JSON-LD"
    status: pending
  - id: html-services
    content: Update automations.html, databases.html, websites.html body + meta (fix websites meta bug)
    status: pending
  - id: jsonld-person
    content: Update duplicated Person JSON-LD on index, websites, automations, databases (description + knowsAbout)
    status: pending
  - id: jsonld-service
    content: Refresh Service JSON-LD on websites, automations, databases (descriptions + offer catalog aligned to new positioning)
    status: pending
  - id: satellite-pages
    content: "Optional: index-test.html, tutorials.html when published, extensions.html grep cleanup"
    status: pending
isProject: true
---

# Stack messaging, site copy, and JSON-LD refresh

## Your positioning (captured for implementation)

- **Simpler / lighter builds:** Webflow + Memberstack + Airtable + Make (and similar glue).
- **More complex / scalable web apps:** Webflow + **Wized** + **Xano or Supabase** as the data/backend layer (pick per project).
- **Full-code / hosted apps:** Next.js (or similar) and static sites, deployed on **Vercel** when you are not shipping on Webflow.

_(Terminology: you wrote “Superbase”—implementation will use **Supabase**, the product name, unless you prefer different wording.)_

## Where the website should change

High impact (currently wrong or heavily outdated vs your story):

| Area                          | File(s)                              | Issue                                                                                                                                                                                                                                  |
| ----------------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Global brand + homepage story | [index.html](index.html)             | `<title>` / meta / OG / Twitter still “Webflow - Make - Airtable”; hero, services blurb, use-case cards, process paragraph, and **FAQ** are Make/Airtable-centric.                                                                     |
| Automations positioning       | [automations.html](automations.html) | Hero + body + meta are **Make-first**; JSON-LD `Service` says “using Make (Integromat)” and catalog includes “Zapier to Make Migration” as a headline offer—may still be valid but should sit inside a **broader integrations** story. |
| Databases / backends          | [databases.html](databases.html)     | Page and JSON-LD are **Airtable-first**; you chose **rebalance** to “data & backends” (Airtable + Supabase + Xano with honest use-case split).                                                                                         |
| Websites page                 | [websites.html](websites.html)       | **Meta description / OG copy is incorrect** (generic Make automation text, not websites). Body is mostly Webflow—needs **Memberstack vs Wized**, and “when we graduate to Xano/Supabase” without overclaiming.                         |

Medium / consistency:

- **[index-test.html](index-test.html)** — mirror homepage meta if you still use it for previews.
- **[tutorials.html](tutorials.html)** — optional: align meta/hero tags **when** you publish (you previously skipped JSON-LD there).

Lower priority unless you want full consistency:

- **[extensions.html](extensions.html)** — quick scan for tool mentions; update only if it references the old trio.

**CSS ([css/pierredemontalte.css](css/pierredemontalte.css))** — only if new sections/layout need styling; avoid scope creep.

## JSON-LD: what to optimize (SEO + “AI discoverability”)

Principles:

1. **Truth and alignment** — Schema should mirror what users see. In particular, [index.html](index.html) **FAQPage** JSON-LD must stay **word-aligned** (or substantively aligned) with the visible FAQ in the same file; same for **HowTo** vs the “My Process” section. Misalignment risks rich-result issues and weak trust signals for crawlers.
2. **Entity clarity (`Person`)** — Tighten `description` and expand `knowsAbout` to reflect your **three modes** (labels like Webflow, Wized, Memberstack, Airtable, Make, Xano, Supabase, Next.js, Vercel—only what you actively deliver).
3. **Service specificity** — For each `Service` block in [websites.html](websites.html), [automations.html](automations.html), [databases.html](databases.html): refresh `name` / `description` / `serviceType` and **`hasOfferCatalog`** so offer names match your real menu (e.g. databases: Airtable architecture, Supabase setup/migrations, Xano APIs, sync/automation—exact list to be drafted from your preferences in implementation).
4. **Avoid schema bloat** — Skip extra types (`WebSite` + `SearchAction`, multiple redundant `ProfessionalService` nodes) unless you add a real site search or a distinct business need; gains are usually marginal vs clear Person + Service + aligned FAQ/HowTo.

**Implementation note:** `Person` JSON-LD is duplicated in four files today; the plan keeps that pattern but updates all four identically so nothing drifts.

## Implementation sequence (after you approve)

1. **Draft copy map** — Short bullet list per page: hero, one “stack ladder” paragraph (simple → complex → full-code), 2–3 FAQ rewrites, process step tweaks, Service catalog items.
2. **Edit visible HTML first** ([index.html](index.html), [automations.html](automations.html), [databases.html](databases.html), [websites.html](websites.html)).
3. **Update JSON-LD in the same PR** — Person (×4), Service (×3), FAQPage + HowTo on homepage only; match FAQ/process text.
4. **Meta / social** — Fix [websites.html](websites.html) meta mismatch; refresh [index.html](index.html) titles and descriptions to include your broader stack without keyword stuffing.
5. **Quick pass** — [index-test.html](index-test.html) + optional [tutorials.html](tutorials.html) + [extensions.html](extensions.html) if needed.

## Optional phase: expand JSON-LD only (no visible copy yet)

Goal: reflect new tools in schema **without** editing body HTML, while keeping mismatch / “markup not representative of page” risk low.

| Schema | Safe to extend without matching copy? | Notes |
|--------|--------------------------------------|--------|
| **Person → `knowsAbout`** | **Usually yes (lowest risk)** | Add tools you **actually** deliver. Stronger if the tool appears **somewhere** on the published site (e.g. homepage FAQ already mentions Wized, Xano, Memberstack, Firebase). For tools with **no** on-site mention yet (e.g. Supabase, Vercel, Next.js), adding only here is a judgment call: still true if you sell that work, but weaker “supported by this page” alignment until copy or case studies mention them. |
| **Person → `description`** | **Careful** | Broaden only with honest, non-superlative wording (e.g. “low-code and code-forward stacks”) without claiming the site’s **primary** focus is something the hero still contradicts. |
| **Service** (per URL) | **Risky without copy** | `description` and `hasOfferCatalog` should describe what that **same page** communicates. If the automations page still reads Make-first but schema lists Xano/Supabase as core offers, crawlers can treat that as inconsistency. Prefer **minimal** Service tweaks (e.g. one neutral phrase like “APIs and third-party integrations”) until body copy is updated. |
| **FAQPage** | **No** | Must stay aligned with visible FAQ text on [index.html](index.html). Do not add tools or claims in FAQ JSON-LD that the visible answers do not support. |
| **HowTo** | **No** | Must stay aligned with visible “My Process” copy. |

**Bottom line:** The practical “no copy change” path is **expand `Person.knowsAbout` (and optionally a modest `Person.description` tweak)** on all four pages where Person appears, in sync. Defer **Service** / **FAQ** / **HowTo** changes until visible content matches, or do only **neutral** Service wording that does not introduce tool names the page never mentions.

## Open detail to resolve during implementation (small)

- Exact **offer catalog** names under each `Service` (especially automations: keep “Zapier → Make migration” as one line item vs rename to “automation platform migration”).
- Whether **Memberstack** and **Wized** should both appear in `knowsAbout` and in FAQs as the default membership split (simple vs advanced).

No dependency on restoring the deleted `json-ld-schemas-*.html` reference file.
