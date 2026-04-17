# Prototype workflow

## Delegate prototype work to a subagent

When asked to build or iterate on a prototype, use the Agent tool — don't write the files directly.

- Prefer a skill-agent when one fits (`taxdome-prototype`, `web-artifacts-builder`).
- Otherwise use `subagent_type: general-purpose`.
- Pass the target path from below.

No exceptions — always delegate, even for one-line tweaks!

## Save to `~/Prototypes/`

```
~/Prototypes/
  product/                   ← ships inside TaxDome
  marketing/                 ← external: reports, landing pages, campaigns
    <feature-slug>/
      README.md              ← description, links, version log
      v1-<short-desc>.html   ← prototypes at root
      v2-<short-desc>.html
      diagrams/              ← .mermaid, .svg
      docs/                  ← specs, briefs, decision logs (.md, .docx, .pdf)
      archive/               ← zips, superseded versions
```

- Pick `product/` vs `marketing/` by audience. Ask if unsure.
- Reuse an existing feature folder when iterating — don't make a new one per session.
- Version files as `v<n>-<desc>.<ext>`. Never overwrite a prior version.
- If the feature folder exists, read the README and latest version first.
