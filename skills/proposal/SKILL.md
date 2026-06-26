---
name: waleed-proposal
triggers:
  - proposal
  - partnership proposal
  - offer letter
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/sow`. Reads the Business Opportunity Profile and Scope of Work, then
produces a formal bilingual partnership proposal letter to send to the founder.

This is the founder-facing document that formalizes Dreamy's interest and proposed terms.

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
for F in "03-bop.md" "05-sow.md"; do
  if [ ! -f "$SESSION/$F" ]; then
    echo "ERROR: $F not found — run the preceding skill first"; exit 1
  fi
done
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/proposal` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read inputs

Read from the current session:
1. `03-bop.md` — Business Opportunity Profile (company context, founder, strategic upside)
2. `05-sow.md` — Scope of Work (deliverables, timeline, equity terms)

### Step 2: Write 06-proposal.md

The proposal is a formal letter — bilingual, Arabic first (RTL), English second.
Register: Modern Standard Arabic (MSA), formal business correspondence.
The Arabic section should be copy-paste ready for WhatsApp or email.

Write to `{SESSION}/06-proposal.md`:

```markdown
# Partnership Proposal

**To:** {Founder Name}
**From:** Waleed Al-Sanosi, Dreamy
**Date:** {today's date}
**Re:** Technical Co-founding Partnership — {Company Name}

---

## Arabic Proposal Letter / خطاب عرض الشراكة

<div dir="rtl">

السيد/السيدة {Founder Name}،

يسعدني التواصل معكم بعد لقائنا الأخير الذي أتاح لنا فرصة الاطلاع على مشروعكم الواعد {Company Name}.

لقد درسنا بعناية ما تفضلتم بمشاركته معنا، وأجرينا تقييماً شاملاً للفرصة التي يمثلها مشروعكم في سوق {industry/vertical} بمنطقة الخليج. ويسعدنا إبلاغكم باهتمام شركة Dreamy بإقامة شراكة تأسيسية تقنية معكم.

### ما تقدمه Dreamy

تتميز Dreamy بتقديم خدمات التأسيس التقني الشاملة، التي تشمل:

- **تطوير المنتج:** تحويل رؤيتكم إلى منتج رقمي متكامل وقابل للتوسع
- **الهندسة البرمجية:** بناء البنية التقنية والكود المصدري بأعلى معايير الجودة
- **استراتيجية المنتج:** المشاركة الفعّالة في قرارات المنتج وخريطة الطريق
- **الشراكة على المدى البعيد:** التزام لمدة عامين كشريك مؤسس تقني

### نطاق العمل المقترح

{Arabic summary of the key deliverables from the SOW — 3–5 bullets}

### الجدول الزمني

{Arabic summary of key milestones and timeline from SOW}

### شروط الشراكة

تقترح Dreamy الحصول على نسبة **[___]٪** من أسهم الشركة مقابل الخدمات المقدمة، وذلك وفق جدول استحقاق مدته أربع سنوات مع فترة انتظار لمدة سنة.

### الخطوات التالية

ندعوكم للتواصل معنا لمناقشة هذا العرض والإجابة على أي استفسارات. يمكننا عقد اجتماع خلال الأسبوع القادم في الوقت الذي يناسبكم.

نتطلع إلى بناء شراكة ناجحة ومثمرة معكم.

مع خالص التحيات والتقدير،

وليد الأحمد السنوسي
مدير المنتج، Dreamy
dreamybuilders.com

</div>

---

## English Proposal Letter

Dear {Founder Name},

Thank you for sharing your vision for {Company Name} during our recent meeting. We've
completed our evaluation of the opportunity, and we're pleased to extend this partnership
proposal.

### What Dreamy Offers

Dreamy is a technical co-founding studio. We partner with early-stage founders in the
MENA/Gulf region to build their products from zero to launch — and stay embedded for
two years to see them through.

For {Company Name}, we propose to deliver:

{English numbered list of key deliverables from SOW — same content, cleaner prose}

### Proposed Timeline

{English summary of milestones — 3–4 key dates}

### Equity Terms

In exchange for our co-founding services, Dreamy proposes:

- **Equity stake:** [___]% of {Company Name}
- **Vesting:** 4-year vest, 1-year cliff, from engagement start date
- **Cash compensation:** None — this is a pure equity engagement

### Why Dreamy

{2–3 sentences that are specific to this founder and opportunity — not generic.
Reference something from their background or product that makes this a natural fit.}

### Next Steps

We'd like to schedule a follow-up meeting to discuss this proposal, answer questions,
and align on next steps. Please reply to suggest a time that works for you.

We look forward to building {Company Name} together.

Warm regards,

Waleed Al-Sanosi
Product Manager, Dreamy
dreamybuilders.com

---

*This proposal is non-binding and subject to final approval by Dreamy's leadership.
Equity terms will be formalized in a separate partnership agreement.*
```

---

### Step 3: Confirm

After writing the file, print:

```
/proposal complete.

File: {session path}/06-proposal.md

⚠ Reminder: Fill in the equity percentage ([___]%) before sending.

The Arabic letter section is copy-paste ready for WhatsApp.
```
