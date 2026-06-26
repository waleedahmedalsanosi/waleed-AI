---
name: waleed-sow
triggers:
  - sow
  - scope of work
  - commercial scope
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/prd`. Reads the PRD and produces a bilingual Scope of Work (SOW) —
the commercial document that defines Dreamy's deliverables, timeline, and equity terms.
This is what the founder signs.

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
if [ ! -f "$SESSION/05-prd.md" ]; then
  echo "ERROR: 05-prd.md not found — run /prd first"; exit 1
fi
FIRST_LINE=$(head -1 "$SESSION/05-prd.md" 2>/dev/null)
if [ "$FIRST_LINE" != "# Product Requirements Document" ]; then
  echo "ERROR: 05-prd.md is empty or malformed — re-run /prd"; exit 1
fi
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/sow` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read inputs

Read from the current session:
- `05-prd.md` — the Product Requirements Document (scope, features, timeline)

Also use what you know about Dreamy's standard engagement model:
- Dreamy provides: technical co-founding (product + engineering), 2-year commitment
- Compensation: equity stake (percentage to be filled in by Waleed)
- Dreamy does NOT charge cash fees in the equity-for-services model
- Typical engagement: 2 Dreamy team members embedded with the startup

### Step 2: Write 06-sow.md

Write to `{SESSION}/06-sow.md`:

```markdown
# Scope of Work

**Company:** {company name}
**Engagement Type:** Technical Co-founding Partnership
**SOW Version:** 1.0
**Date:** {today's date}
**Parties:** {Founder Name}, {Company Name} ("the Startup") and Dreamy ("the Co-founder")

---

## 1. Engagement Overview

Dreamy will serve as the technical co-founding partner for {company name}, providing
product strategy, design, and engineering services in exchange for an equity stake in
the company. This engagement covers the development of the v1 MVP as defined in the
Product Requirements Document dated {PRD date}.

---

## 2. Dreamy's Deliverables

The following deliverables will be produced by Dreamy within the engagement period:

| # | Deliverable | Description | Target Date |
|---|-------------|-------------|-------------|
{Numbered table of deliverables derived from PRD features and milestones.
Each row: deliverable name, 1-sentence description, target date from PRD timeline.}

**Deliverable quality standard:** Each deliverable is considered complete when it meets
the acceptance criteria defined in the PRD and has been tested with {N} real users.

---

## 3. Startup's Responsibilities

The Startup commits to:

1. **Product decisions:** Final product decisions rest with the Startup founder. Dreamy
   provides recommendations; the founder approves.
2. **Domain expertise access:** Providing Dreamy team access to customers, market knowledge,
   and domain expertise required to build the product correctly.
3. **Feedback cadence:** Weekly 30-minute sync with Dreamy's lead during active build phases.
4. **User testing:** Recruiting {N} beta users for testing at milestone checkpoints.
5. **Legal & compliance:** Ensuring the company is properly incorporated and that equity
   transfer is legally executable in the relevant jurisdiction.

---

## 4. Timeline

**Engagement start:** {date — to be confirmed at contract signing}
**v1 MVP target:** {date from PRD timeline}
**Co-founding term:** 2 years from engagement start

| Phase | Duration | Milestone |
|-------|----------|-----------|
| Discovery & Design | {N} weeks | Architecture, wireframes, design system approved |
| Build Phase 1 | {N} months | Core features complete |
| Build Phase 2 | {N} months | All MVP features complete |
| Beta & Launch | {N} weeks | Live product with first users |

---

## 5. Equity Terms

**Dreamy's equity stake:** [___] % (to be negotiated and confirmed by Waleed)

**Vesting schedule:** Standard 4-year vest with 1-year cliff, beginning on engagement start date.

**Anti-dilution:** {Pro-rata rights / Standard dilution / To be negotiated}

**Board representation:** Observer seat on the board of directors (standard for co-founding engagements).

*Note: These terms are indicative. Final equity percentage and terms are subject to
Samir Al-Rashidi's review and approval, and will be confirmed in the formal partnership agreement.*

---

## 6. Intellectual Property

- All code, designs, and intellectual property produced under this engagement vest in
  {company name} upon equity agreement execution.
- Dreamy retains the right to reference this engagement as a portfolio case study
  (with founder approval on specific content).
- Any pre-existing IP brought by Dreamy (reusable components, frameworks) remains
  Dreamy's property; derivative works belong to the Startup.

---

## 7. Exclusions

The following are explicitly out of scope for this SOW:

{List of features from PRD "Out of Scope" section}
- Cash payment to Dreamy (equity-only engagement)
- Ongoing maintenance post-launch beyond the 2-year term (subject to renewal)
- {Any other explicit exclusions relevant to this startup}

---

## 8. Assumptions

This SOW is based on the following assumptions. If they change, the SOW must be revised:

1. The Startup is (or will be) incorporated before development begins.
2. The founder is the primary decision-maker with authority to grant equity.
3. {Other assumptions from the intake / BOP}

---

## Arabic Summary / ملخص نطاق العمل

<div dir="rtl">

## اتفاقية نطاق العمل — ملخص تنفيذي

**الشركة:** {company name}
**نوع الشراكة:** شراكة تأسيس تقني
**التاريخ:** {today's date in Arabic format}
**الأطراف:** {Founder Name} و Dreamy

---

### ملخص الاتفاقية

ستعمل شركة Dreamy بوصفها شريكاً مؤسساً تقنياً لشركة {company name}، وستقدم خدمات تطوير المنتج والهندسة البرمجية مقابل حصة في أسهم الشركة.

---

### المخرجات الرئيسية

{Arabic numbered list of deliverables — same as English table but in bullet format}

---

### الجدول الزمني

{Arabic summary of phases and milestones}

---

### شروط المشاركة في الأسهم

تمتلك شركة Dreamy نسبة [___]٪ من أسهم الشركة، وفقاً لجدول الاستحقاق المتفق عليه على مدى أربع سنوات مع فترة انتظار لمدة سنة واحدة.

---

*يُعدّ هذا الملخص مرجعاً عاماً. تُعتمد نسخة الاتفاقية الإنجليزية المفصّلة المرجعَ القانونيَّ الرسمي.*

</div>

---

*SOW v1.0 — subject to legal review and founder approval. Equity percentage pending Samir Al-Rashidi's confirmation.*
```

---

### Step 3: Confirm

After writing the file, print:

```
/sow complete.

File: {session path}/06-sow.md

⚠ Reminder: Fill in the equity percentage before sending to founder.

Next: run /proposal (founder-facing) or /ceo-review (Samir briefing)
```
