---
name: waleed-outreach
triggers:
  - outreach
  - first contact
  - whatsapp message
  - reach out
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/bop`. Produces a first-contact Arabic WhatsApp message to the founder at
`09-outreach.md`. This is the first formal correspondence after the internal evaluation.

The message is formal MSA Arabic, 3–4 short paragraphs, copy-paste ready for WhatsApp.
An English version follows for Waleed's reference.

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
if [ ! -f "$SESSION/03-bop.md" ]; then
  echo "ERROR: 03-bop.md not found — run /bop first"; exit 1
fi
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/outreach` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read inputs

Read from the current session:
- `03-bop.md` — Business Opportunity Profile (company, founder, product, next steps)

### Step 2: Craft the outreach

The message has two purposes:
1. Signal genuine interest from Dreamy (not generic follow-up)
2. Propose a specific next step (meeting, call, or document share)

Rules:
- Reference something specific from the founder's pitch that shows Dreamy listened
- No marketing language, no superlatives ("amazing", "exciting", "innovative")
- 3–4 short paragraphs — this is WhatsApp, not email
- Formal MSA Arabic. Salutation: "السيد/السيدة [Name]،"
- Closing: "مع خالص التحيات،\nوليد الأحمد السنوسي\nDreamy"
- Do not make equity or partnership commitments in this message — this is first contact

### Step 3: Write 09-outreach.md

Write to `{SESSION}/09-outreach.md`:

```markdown
# First Outreach Message

**To:** {Founder Name}
**From:** Waleed Al-Sanosi, Dreamy
**Date:** {today's date}
**Channel:** WhatsApp

---

## Arabic Message (Copy-Paste Ready)

<div dir="rtl">

السيد/السيدة {Founder First Name}،

{Paragraph 1 — Opening and reference to the meeting. 2–3 sentences.
Example structure: "يسعدني التواصل معكم بعد لقائنا بتاريخ [date] والاستماع إلى رؤيتكم
لمشروع [Company Name]." Then one specific observation that shows you listened —
reference the specific pain point or insight that stood out.}

{Paragraph 2 — Dreamy's interest, framed specifically for this opportunity.
2–3 sentences. What about this specific project aligns with what Dreamy does.
Do not use template language. Reference the actual product or market.}

{Paragraph 3 — Proposed next step. 1–2 sentences.
Examples:
- "نودّ تحديد موعد لمناقشة إمكانيات التعاون. هل يتسنى لكم اللقاء خلال الأسبوع القادم؟"
- "سنشارككم ملخصاً لتقييمنا الأولي خلال الأيام القليلة القادمة."
Be specific about the next step — date range, type of meeting, or document.}

مع خالص التحيات والتقدير،

وليد الأحمد السنوسي
مدير المنتج، Dreamy
dreamybuilders.com

</div>

---

## English Version (Reference)

Dear {Founder First Name},

{Paragraph 1 — English equivalent of the Arabic opening, same specificity.}

{Paragraph 2 — Dreamy's angle on this opportunity.}

{Paragraph 3 — Specific next step.}

Best regards,
Waleed Al-Sanosi
Product Manager, Dreamy
dreamybuilders.com

---

## Notes for Waleed

**Best time to send:** {weekday morning, business hours in the founder's timezone}
**Follow-up if no reply:** {3 business days}
**Tone check:** {confirm the Arabic register is formal and not overly stiff — adjust if the founder's own communication style was more casual in the intake}
```

---

### Step 4: Confirm

After writing the file, print:

```
/outreach complete.

File: {session path}/09-outreach.md

The Arabic section above is copy-paste ready for WhatsApp.
```
