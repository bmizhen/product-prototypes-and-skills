# Prototype workflow

## Always build prototypes via a subagent

When the user asks to build, design, mock up, or iterate on a prototype (UI mockup, wireframe, clickable screen, HTML prototype, etc.), **delegate the implementation to a subagent via the Agent tool** rather than writing files directly in the main thread.

- Prefer a specialized skill-agent when one fits the task (e.g. `taxdome-prototype` for TaxDome-style UIs, `web-artifacts-builder` for multi-component React artifacts).
- Otherwise use `subagent_type: general-purpose` with a self-contained prompt.
- Pass the target output path (see below) to the subagent so it writes the file in the right place.
- After the subagent returns, relay a concise summary and the file path to the user.

No Exceptions, ALLWAYS ALLWAYS write code in a subagent.

## Save all prototypes to `~/Prototypes/`

All prototypes live in a single permanent location so they're easy to find later:

```
~/Prototypes/
  product/                 ← product feature prototypes (things shipping in TaxDome)
    <feature-slug>/
      README.md            ← feature description, version log, source chats
      v1-<short-desc>.html ← prototype files (HTML, JSX, TSX) at root
      v2-<short-desc>.html
      diagrams/            ← .mermaid, .svg flow diagrams (visual, lives next to prototypes)
      docs/                ← specs, briefs, decision logs, business rules (.md, .docx)
      archive/             ← zips, old exports, superseded versions
  marketing/               ← marketing prototypes (reports, landing pages, campaigns, data viz)
    <feature-slug>/
      (same subfolder structure as product/)
```

Rules:

- **Root**: `~/Prototypes/` (absolute: `/Users/ilyaradzinsky/Prototypes/`). Never save prototypes to the repo, Desktop, or `/tmp`.
- **Top-level category**: every feature folder lives under either `product/` or `marketing/`. Decide by audience and surface: if it ships inside the TaxDome app it's `product/`; if it's for external audiences (reports, landing pages, industry indices, campaigns, content viz) it's `marketing/`. When unsure, ask.
- **Subfolder per feature**: kebab-case slug describing the feature (e.g. `product/client-portal-onboarding/`, `marketing/industry-index-q1-2026/`). Reuse the existing folder when iterating on the same feature — do not create a new folder per session.
- **Prototype files at root** of the feature folder: `v<n>-<short-desc>.<ext>` where `<n>` increments per iteration. Example: `v1-initial-layout.html`, `v2-with-empty-state.html`. Do not overwrite prior versions. (Pre-existing files with other naming can keep their names.)
- **`diagrams/`**: flow diagrams and visual references (`.mermaid`, `.svg`) that illustrate the prototype. Keep with the prototype because they evolve together.
- **`docs/`**: prose artifacts — specs, product briefs, decision logs, business rules, design reviews (`.md`, `.docx`, `.pdf`). These set context for the prototype but aren't the prototype itself.
- **`archive/`**: zip files, old exports, superseded versions kept for reference but not actively worked on.
- **README.md** in each feature folder: one-paragraph description of the feature, links to any Figma/specs/tickets, source Claude.ai chat (if any), and a running log of what changed between versions.

Before starting a prototype:

1. Check if `~/Prototypes/<feature-slug>/` already exists. If yes, read the README and the latest `v<n>` file for context, then increment the version.
2. If no, create the folder and seed the README with the feature name and a one-line description before building.

When the user refers to "the [feature] prototype" without specifying a path, look it up under `~/Prototypes/` first.
