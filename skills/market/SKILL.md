---
name: waleed-market
triggers:
  - market
  - market analysis
  - competitor analysis
  - market research
allowed-tools:
  - Read
  - Write
  - Bash
  - WebSearch
---

## When to invoke this skill

Run after `/intake`, before `/evaluate`. Uses web search to produce an independent
market and competitor analysis at `02-market.md`.

The analysis covers: market size (TAM/SAM) for the founder's vertical in MENA/Saudi,
direct competitors (regional first, then global), market timing signals, and comparable
funding activity. `/evaluate` reads this file to calibrate the Market scoring category.

---

## Bash Preamble

```bash
SESSION_FILE="$HOME/.waleed-ai/.current-session"
if [ ! -f "$SESSION_FILE" ]; then
  echo "ERROR: no active session — run /intake first"; exit 1
fi
SESSION=$(cat "$SESSION_FILE")
if [ ! -d "$SESSION" ]; then
  echo "ERROR: session folder missing: $SESSION — re-run /intake"; exit 1
fi
if [ ! -f "$SESSION/01-intake.md" ]; then
  echo "ERROR: 01-intake.md not found — run /intake first"; exit 1
fi
FIRST_LINE=$(head -1 "$SESSION/01-intake.md" 2>/dev/null)
if [ "$FIRST_LINE" != "# Founder Brief" ]; then
  echo "ERROR: 01-intake.md is empty or malformed — re-run /intake"; exit 1
fi
echo "SESSION: $SESSION"
echo "INTAKE: OK"
```

---

## Instructions

You are running the `/market` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read intake

Read `01-intake.md` from the current session. Extract:
- **Vertical / sector** — what industry this is (e.g., healthcare, fintech, edtech)
- **Geography** — which countries (usually KSA + Gulf)
- **Target customers** — who buys (B2C patients, B2B clinics, etc.)
- **Product category** — the specific problem being solved (e.g., "doctor review platform")
- **Company name** — to search for the founder's existing brand if present

### Step 2: Run market searches

Use `WebSearch` to research each of the four areas below. Run focused, targeted searches
— do not generate figures from training data alone. If a search returns no useful result,
note that explicitly rather than fabricating data.

#### 2a. Market Size (TAM/SAM)

Search for:
- "{sector} market size Saudi Arabia {current year}"
- "{sector} market size MENA {current year}"
- "{product category} market revenue GCC"
- Vision 2030 sector-specific allocations if applicable (healthcare, education, etc.)

Extract: published TAM figures with source and year. If no Saudi-specific data exists,
use the MENA figure and caveat. Do not invent numbers.

#### 2b. Competitors — Regional

Search for:
- "{product category} app Saudi Arabia"
- "{product category} startup KSA"
- "{product category} platform UAE" (GCC market proxy)
- Any competitor names the founder mentioned in the intake

For each: company name, country, product description, funding if known, key difference
from the founder's concept.

#### 2c. Competitors — Global

Search for:
- "{product category} platform" (global)
- The best-known global player in the space

Extract: 2–3 most relevant global players and why each is (or isn't) a direct threat
in the Gulf market context given regulatory, language, or trust differences.

#### 2d. Market Timing & Investment Activity

Search for:
- "{sector} Saudi Arabia startup funding 2024 2025"
- "{product category} startup raised funding"
- Vision 2030 {sector} investments
- "{sector} regulation Saudi Arabia" for recent regulatory signals

Extract: 2–4 data points on investment activity or regulatory tailwinds that support
or challenge the timing thesis.

### Step 3: Write 02-market.md

Write to `{SESSION}/02-market.md`:

```markdown
# Market & Competitor Analysis

**Company:** {company name from intake}
**Vertical:** {sector}
**Geography:** {target countries}
**Prepared by:** Waleed AI — /market skill
**Date:** {today's date}
**Source:** Web search — all figures should be independently verified before use in investor materials

---

## Market Size

**TAM Estimate:** {figure + source + year, or "No reliable public figure found — see note"}
**SAM Estimate:** {narrowed figure for the accessible segment, or "Not established"}
**Methodology note:** {how you arrived at these numbers, or which sources were searched and returned nothing}

{1–2 sentences on data confidence level. KSA-specific figures are often unavailable
in public sources — say so clearly if that's the case.}

---

## Competitive Landscape

### Regional Competitors

| Company | Country | What they do | Funding | Key difference from {company} |
|---------|---------|--------------|---------|-------------------------------|
| {name} | {KSA/UAE/etc} | {1 sentence} | {if known, else "unknown"} | {1 sentence} |

{If no direct regional competitor found: "No direct regional competitor identified
via search. Closest proxies: [list any indirect competitors found]."}

### Global Comparables

| Company | Market | What they do | Why less relevant to GCC |
|---------|--------|--------------|--------------------------|
| {name} | {country} | {1 sentence} | {regulatory / language / trust barrier} |

### Competitive Positioning

{2–3 sentences: where the founder's concept fits in the competitive map, what white
space it occupies, and the most dangerous near-term competitive risk.}

---

## Market Timing Signals

{Bullet list of 3–5 data points — each with source and year.
If nothing relevant found for a signal type, omit rather than padding with generic text.}

- **{signal headline}:** {1 sentence with source and year}

---

## Investment Activity

{2–4 relevant funding events or investment trends in this vertical/region.
Note the stage ({company} is likely pre-seed/seed) and what comparable round sizes look like.
If no funding data found, say so.}

---

## Summary for /evaluate

**Market opportunity signal:** {Strong / Moderate / Weak / Unclear}
**Competition density:** {Low / Medium / High}
**Timing advantage:** {Yes / Partial / No / Unclear}
**Key insight for Waleed:** {One sentence — the single most useful thing this research
surfaces that changes how you'd score the Market category.}
```

---

### Step 4: Confirm

After writing the file, print:

```
/market complete.

Vertical: {sector}
Market signal: {Strong / Moderate / Weak / Unclear}
File: {session path}/02-market.md

Next: run /evaluate — it will incorporate this market analysis into Market scoring.
```
