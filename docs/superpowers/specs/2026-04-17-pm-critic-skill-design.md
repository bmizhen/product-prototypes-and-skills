# pm-critic skill — design

## Purpose

A Claude skill that stress-tests product/feature ideas by surfacing hidden assumptions, ranking them by risk, and proposing the cheapest way to validate the riskiest ones.

Target user: a product manager (the owner of this repo) who wants fast, disciplined pushback on their own ideas before committing to them.

## Invocation

User-invoked. The skill's `description` frontmatter makes its trigger unambiguous: product or feature ideas only — not architecture reviews, code reviews, or pure strategy questions.

Typical trigger phrasings:
- "critique this idea: …"
- "what am I missing on …"
- "run pm-critic on …"
- explicit `/pm-critic` invocation

## Input

Freeform text describing a product or feature idea, at any level of detail.

If the input is too thin to produce a real critique, the critic does **not** ask clarifying questions. It treats vagueness itself as the first assumption, names the specific gaps, and still critiques what is present.

## The lens: 8 assumption categories

Every critique walks through these:

1. **User need** — the idea assumes users want this / have this problem.
2. **Adoption & discovery** — assumes users will find it, understand it, and change habits to use it.
3. **Effort estimation** — assumes the build is small; assumes the hard part is the visible part.
4. **Business impact** — assumes the metric it moves is the one that matters.
5. **Competitive / market** — assumes nobody else solves this; assumes the market is ready.
6. **Technical feasibility** — assumes the tech works at scale, the data exists, the integration is possible.
7. **Second-order effects** — assumes it won't cannibalize, confuse, or break adjacent features.
8. **Success definition** — assumes success will be recognizable; assumes the definition is agreed.

## Process

1. Extract the idea's one-line thesis.
2. Walk each of the 8 categories and surface implicit assumptions.
3. Bucket each assumption into one of three risk tiers:
   - **Likely fatal** — if wrong, the idea doesn't work.
   - **Worth validating** — real risk, worth spending cycles on.
   - **Minor** — flag and move on.
4. For every *Likely fatal* and *Worth validating* assumption, propose the **cheapest concrete validation** (e.g., "5 user interviews next sprint", "check X dashboard", "1-day prototype"). Never "do research."
5. Explicitly flag what the critic could not evaluate due to gaps in the idea.

## Output format

```markdown
## PM Critic: <one-line thesis>

### 🔴 Likely fatal
- **[Category] Assumption:** …
  - **Why risky:** …
  - **Cheapest validation:** …

### 🟡 Worth validating
- **[Category] Assumption:** …
  - **Why risky:** …
  - **Cheapest validation:** …

### ⚪ Minor
- [Category] — one-liner

### What the critic could not evaluate
- Gaps in the idea that blocked deeper analysis
```

Every category must either appear with at least one assumption or carry an explicit note that it doesn't apply and why.

## Behavior rules

- **No sycophancy.** No "great idea, but…". Skip straight to the critique.
- **Concrete validations only.** Cheap, specific, actionable. Never vague advice.
- **The critic does not prescribe what to build.** It only names what to validate and what evidence would shift the picture.
- **Tone:** direct senior-PM peer review. Skeptical, not hostile. Challenges assumptions without dismissing the idea.
- **No clarifying questions up front.** If the idea is thin, say so as a finding and critique what's present.

## File location

`.claude/skills/pm-critic/SKILL.md` in this repo. Can be promoted to `~/.claude/skills/` later if the PM wants it available globally.

## Out of scope

- Technical architecture critique (code, systems, infra).
- Strategy/market-entry critique at the company level.
- Writing PRDs, specs, or counter-proposals on the PM's behalf.
- Multi-turn interview flows. This skill produces one critique per invocation.
