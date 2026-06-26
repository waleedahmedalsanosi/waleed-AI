---
name: waleed-client-review
triggers:
  - client review
  - founder review
  - review package
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/prd` and `/sow`. Produces a founder-facing review package at `09-client-review.md`
that summarizes what Dreamy is proposing to build, the agreed scope, and what the founder
needs to confirm before work begins.

This is the document that goes to the founder for sign-off before development starts.

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
for F in "05-prd.md" "06-sow.md"; do
  if [ ! -f "$SESSION/$F" ]; then
    echo "ERROR: $F not found — run the preceding skill first"; exit 1
  fi
done
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/client-review` skill for Waleed Al-Sanosi, PM at Dreamy.

The audience is the founder. They may not be technical. The document must be:
- Clear enough for a non-technical founder to understand what will be built
- Specific enough that there's no ambiguity about scope
- Actionable: the founder knows exactly what to confirm and what decisions remain open

### Step 1: Read inputs

Read from the current session:
1. `05-prd.md` — Product Requirements Document
2. `06-sow.md` — Scope of Work

### Step 2: Write 09-client-review.md

Write to `{SESSION}/09-client-review.md`:

```markdown
# Client Review Package

**Company:** {Company Name}
**Prepared by:** Waleed Al-Sanosi, PM — Dreamy
**Date:** {today's date}
**Review type:** Pre-development scope confirmation

---

## Overview

This document summarizes what Dreamy proposes to build for {Company Name} as part of our
technical co-founding partnership. Please review each section carefully and confirm or
raise questions before we begin development.

---

## What We're Building

### Product Summary

{2–3 sentences describing the product in plain language — no jargon, no tech speak.
A founder's parent should be able to understand what this product does.}

### Who It's For

**Primary user:** {user description in plain language}
**How they use it:** {the core use case in one sentence}

---

## Scope: What's Included in v1

The following features will be built in the v1 MVP:

{Numbered list from PRD In Scope section — plain language, no technical details.
Each item: feature name → what the user can do with it in one sentence.}

**Expected timeline:** {key dates from SOW}

---

## Scope: What's NOT Included in v1

To ship on time, the following have been deferred to future versions:

{Numbered list from PRD Out of Scope section — with plain-language reason for each deferral.}

*These can be built in v2. We'll reprioritize after launch based on what users need most.*

---

## What Dreamy Will Deliver

| Deliverable | Description | When |
|-------------|-------------|------|
{Table from SOW deliverables — same content, plain language descriptions}

---

## What We Need from You

Before development begins, we need you to confirm or provide:

- [ ] **Decision: {open question 1 from PRD}** — We need your answer by {date}
- [ ] **Decision: {open question 2}** — We need your answer by {date}
- [ ] **Access:** {any credentials, accounts, or third-party access Dreamy needs}
- [ ] **Legal:** Company incorporation complete and equity grant ready to execute

---

## Equity Terms Reminder

As agreed, Dreamy will receive **[___]%** equity in {Company Name} in exchange for
the services described in this document.

*Please confirm you've reviewed the full Scope of Work document and are aligned on
these terms before we proceed.*

---

## Arabic Summary / ملخص للمراجعة

<div dir="rtl">

## ملخص حزمة المراجعة للعميل

**الشركة:** {Company Name}
**أعدّها:** وليد الأحمد السنوسي، Dreamy
**التاريخ:** {today's date in Arabic format}

---

### ما الذي سنبنيه؟

{Arabic summary of the product — 2–3 sentences in MSA formal register}

### المميزات المشمولة في الإصدار الأول

{Arabic numbered list of features in scope}

### المميزات المرجأة

{Arabic numbered list of deferred features}

### ما نحتاجه منكم

{Arabic bullet list of founder actions required before start}

### شروط الأسهم

ستحصل Dreamy على نسبة **[___]٪** من أسهم شركة {Company Name} مقابل الخدمات المتفق عليها.

---

*يُرجى مراجعة هذه الوثيقة والتواصل معنا بأي استفسارات قبل بدء التطوير.*

</div>

---

**To confirm:** Please reply with "Confirmed" or raise any questions.
*This document does not replace the formal Scope of Work agreement.*
```

---

### Step 3: Confirm

After writing the file, print:

```
/client-review complete.

File: {session path}/09-client-review.md

⚠ Reminder: Fill in equity percentage before sharing with founder.
```
