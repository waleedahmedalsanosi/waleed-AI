---
name: waleed-prd
triggers:
  - prd
  - product requirements
  - product doc
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/bop`. Reads the Founder Brief, Evaluation Report, and Business Opportunity
Profile, then produces a bilingual Product Requirements Document (PRD) at `06-prd.md`.

The PRD defines what will be built — features, user stories, and acceptance criteria.
It is the contract between Dreamy and the founder on product scope.

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
for F in "01-intake.md" "03-evaluate.md" "04-bop.md"; do
  if [ ! -f "$SESSION/$F" ]; then
    echo "ERROR: $F not found — run the preceding skill first"; exit 1
  fi
done
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/prd` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read inputs

Read from the current session:
1. `01-intake.md` — original Founder Brief (product description, features mentioned)
2. `03-evaluate.md` — Evaluation Report (technical feasibility, build complexity)
3. `04-bop.md` — Business Opportunity Profile (MVP scope estimate, Dreamy's technical angle)

### Step 2: Write 06-prd.md

The PRD has two sections: English (primary, detailed) followed by Arabic (executive summary
for the founder). The Arabic section uses RTL formatting.

Write to `{SESSION}/06-prd.md`:

```markdown
# Product Requirements Document

**Company:** {company name}
**Product:** {product name / working title}
**Version:** 1.0
**Date:** {today's date}
**Authors:** Waleed Al-Sanosi (PM), Dreamy

---

## 1. Product Vision

**One-line vision:** {complete this: "[Product] enables [target user] to [outcome] by [mechanism]."}

**Problem statement:** {2–3 sentences: the specific pain, who has it, how acute it is in MENA/Gulf}

**Success metric (v1):** {the single measurable outcome that defines success at launch}

---

## 2. Users

### Primary User
- **Who:** {specific role, industry, location}
- **Pain:** {what they struggle with today}
- **Job to be done:** {what they're trying to accomplish}

### Secondary Users (if applicable)
{Additional user segments, or "None identified in intake"}

---

## 3. Scope

### In Scope (v1 MVP)

{Numbered list of features included in the MVP. Each feature should be testable.
Derive from the intake transcript + BOP MVP scope estimate.}

1. {Feature: short name} — {1-sentence description}
2. {Feature}
3. {Feature}
(aim for 4–8 features for a 6-month build)

### Out of Scope (v1)

{Numbered list of features explicitly deferred. Each should include a brief reason.}

1. {Feature} — {why deferred: complexity / not core to v1 / founder confirmed}
2. {Feature}

---

## 4. User Stories

For each in-scope feature, write one user story:

**{Feature 1 name}**
> As a {user type}, I want to {action} so that {outcome}.
> Acceptance criteria:
> - {specific, testable criterion}
> - {criterion}

{Repeat for each feature}

---

## 5. Technical Requirements

**Platform:** {Web / Mobile (iOS + Android) / Mobile (iOS only) / API / Other}
**Architecture notes:** {key technical constraints or requirements from intake/BOP}
**Integrations required:** {third-party APIs, payment providers, maps, etc.}
**Performance requirements:** {load time, uptime SLA, concurrent users — if discussed}
**Security / compliance:** {data residency, KYC requirements, PDPL (Saudi), etc.}

---

## 6. Design Requirements

**Language support:** Arabic (RTL) + English bilingual (required for Gulf market)
**Platform conventions:** {iOS HIG / Material Design / Web — per platform target}
**Accessibility:** {WCAG 2.1 AA — standard for all Dreamy products}
**Brand:** {founder has branding / Dreamy to design / TBD}

---

## 7. Timeline & Milestones

| Milestone | Target Date | Owner |
|-----------|-------------|-------|
| Design & architecture complete | +4 weeks | Dreamy |
| MVP feature-complete | +{N} months | Dreamy |
| Beta testing with first users | +{N+1} months | Founder + Dreamy |
| v1 launch | +{N+2} months | Founder + Dreamy |

*Timeline based on 2-person Dreamy team. Adjust per actual capacity.*

---

## 8. Open Questions

{Numbered list of questions that must be answered before development starts.
These are blockers, not nice-to-haves.}

1. {Question — who is responsible for answering it}
2. {Question}

---

## Arabic Summary / ملخص المتطلبات

<div dir="rtl">

## وثيقة متطلبات المنتج — ملخص تنفيذي

**الشركة:** {company name}
**المنتج:** {product name}
**التاريخ:** {today's date in Arabic format: DD/MM/YYYY}
**أعدّها:** وليد الأحمد السنوسي، Dreamy

---

### رؤية المنتج

{Arabic translation of the one-line vision and problem statement — 3–4 sentences.
Use MSA. Formal register.}

---

### النطاق الوظيفي — الإصدار الأول

المميزات المتضمنة في الإصدار الأول (MVP):

{Numbered list of features in Arabic}

المميزات المرجأة إلى إصدارات لاحقة:

{Numbered list of deferred features in Arabic}

---

### الجدول الزمني التقديري

{Arabic version of the milestone table — keep the same structure}

---

### الأسئلة المفتوحة

{Arabic version of open questions}

</div>

---

*PRD v1.0 — subject to revision based on technical discovery and founder feedback.*
```

---

### Step 3: Confirm

After writing the file, print:

```
/prd complete.

File: {session path}/06-prd.md

Next: run /sow to define the commercial scope
```
