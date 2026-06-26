---
name: waleed-bop
triggers:
  - bop
  - business opportunity
  - opportunity profile
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/evaluate`. Reads the Founder Brief and Evaluation Report, then produces
a Business Opportunity Profile (BOP) — Dreamy's internal one-pager summarizing the
opportunity and the strategic fit. Used as the basis for `/proposal` and `/ceo-review`.

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
for F in "01-intake.md" "03-evaluate.md"; do
  if [ ! -f "$SESSION/$F" ]; then
    echo "ERROR: $F not found — run the preceding skill first"; exit 1
  fi
done
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/bop` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read inputs

Read both files from the current session:
1. `01-intake.md` — the Founder Brief
2. `03-evaluate.md` — the Evaluation Report

### Step 2: Write 04-bop.md

The BOP is a tight, decision-ready document. No padding. It answers:
what is this opportunity, why does it matter for MENA, what are the risks, and
what is Dreamy's role and upside.

Write to `{SESSION}/04-bop.md`:

```markdown
# Business Opportunity Profile

**Company:** {company name}
**Founder:** {founder name}
**Date:** {today's date}
**Prepared by:** Waleed Al-Sanosi, Dreamy

---

## The Opportunity in One Paragraph

{3–5 sentences. State: what the startup does, who the customer is, why the market
exists now in MENA/Gulf, and what makes this worth Dreamy's 2-year co-founding bet.
This paragraph should work as a standalone pitch to Samir. Write it last.}

---

## Market Context

**Vertical:** {industry / sector}
**Geography:** {target countries in MENA/Gulf}
**Market size:** {figure and methodology, or "Not established in intake"}
**Timing signal:** {what's changed recently that makes this viable now}
**Key regional dynamic:** {the Gulf-specific factor that matters most}

---

## The Founder

**Name:** {founder name}
**Credibility signal:** {the one thing that makes this founder credible in this space}
**Risk:** {the one thing about the founder that is a concern}
**Coachability:** {High / Medium / Low — with one-line evidence}

---

## Product & Technical Fit

**What's being built:** {1–2 sentences on the product}
**Build complexity:** {Low / Medium / High — with rationale}
**Dreamy's technical angle:** {what kind of engineering this requires — mobile, web, AI, data, etc.}
**MVP scope estimate:** {rough: 3-month / 6-month / 9-month build for a 2-person team}

---

## Business Model

**Revenue model:** {how they make money}
**Equity model fit:** {whether Dreamy can take equity; any concerns}
**Path to unit economics:** {1–2 sentences on the monetization thesis}

---

## Traction Summary

**Current state:** {pre-revenue / early revenue / growing revenue}
**Strongest signal:** {the single most credible traction data point}
**What's missing:** {what would make this a stronger investment}

---

## Risk Register

| Risk | Severity | Mitigation |
|------|----------|------------|
| {risk 1} | High / Medium / Low | {what Dreamy or the founder can do} |
| {risk 2} | High / Medium / Low | {mitigation} |
| {risk 3} | High / Medium / Low | {mitigation} |

---

## Dreamy's Strategic Upside

**Equity stake rationale:** {why this is a good equity bet for Dreamy}
**Portfolio synergy:** {any connection to existing Dreamy work or expertise}
**Reputational angle:** {does this build Dreamy's brand in a valuable vertical?}

---

## Evaluation Score

**Score:** {X}/100 | **Recommendation:** {Proceed / Conditional / Watch / Pass}

{Copy the Recommendation paragraph verbatim from 03-evaluate.md.}

---

## Recommended Next Steps

{Bullet list of 2–4 specific actions Waleed or Dreamy should take next.
Be concrete: "Schedule technical discovery call to scope the backend", not "do more research."}
```

---

### Step 3: Confirm

After writing the file, print:

```
/bop complete.

File: {session path}/04-bop.md

Next: run /prd (if Proceed or Conditional) or /ceo-review (to brief Samir)
```
