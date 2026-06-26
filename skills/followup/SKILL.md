---
name: waleed-followup
triggers:
  - followup
  - follow up
  - follow-up
  - next steps letter
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/ceo-review`. Reads Samir's decision from `08-ceo-review.md` and produces
the appropriate follow-up correspondence to the founder at `10-followup.md`.

The message branches on Samir's outcome:
- **Proceed** → positive follow-up with next steps and meeting request
- **Conditional** → conditional offer with specific requirements before proceeding
- **Pass** → formal, respectful decline with door left open

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
if [ ! -f "$SESSION/08-ceo-review.md" ]; then
  echo "ERROR: 08-ceo-review.md not found — run /ceo-review first"; exit 1
fi
# Read the outcome field from the last line of ceo-review.md
OUTCOME=$(grep "^Outcome:" "$SESSION/08-ceo-review.md" | tail -1 | sed 's/Outcome: //')
if [ -z "$OUTCOME" ]; then
  echo "ERROR: No 'Outcome:' line found in 08-ceo-review.md — re-run /ceo-review"; exit 1
fi
echo "SESSION: $SESSION"
echo "OUTCOME: $OUTCOME"
```

---

## Instructions

You are running the `/followup` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read inputs

1. Read `08-ceo-review.md` from the current session.
2. Note the `OUTCOME` from the bash preamble — this determines the letter tone.
3. Also read `01-intake.md` for founder name and company name.

### Step 2: Branch on outcome

**If OUTCOME = "Proceed":**
Write a positive, forward-moving letter. Propose a specific next step (partnership agreement
discussion, technical scoping call). Express Dreamy's genuine interest without overpromising.
Tone: warm, professional, concrete about next steps.

**If OUTCOME = "Conditional":**
Write a conditional offer letter. State Dreamy's interest clearly, then list the specific
conditions that must be met before a formal engagement begins. Tone: encouraging but precise —
the conditions are real requirements, not soft suggestions.

**If OUTCOME = "Pass":**
Write a formal, respectful decline. Thank the founder for their time, acknowledge something
specific and genuine from the pitch, and close the door politely — leaving it open for future
re-engagement if the situation changes. Tone: warm, honest, respectful. No false encouragement.

### Step 3: Write 10-followup.md

Write to `{SESSION}/10-followup.md`:

```markdown
# Follow-up Correspondence

**To:** {Founder Name}
**From:** Waleed Al-Sanosi, Dreamy
**Date:** {today's date}
**Outcome:** {Proceed / Conditional / Pass}
**Channel:** WhatsApp / Email

---

## Arabic Letter (Copy-Paste Ready)

<div dir="rtl">

السيد/السيدة {Founder First Name}،

{--- IF PROCEED ---}

{Paragraph 1: Thank for the time spent together. Reference the specific company/product.}

{Paragraph 2: Express Dreamy's decision to move forward. 2–3 sentences on what Dreamy
sees in this opportunity — specific, not generic. No superlatives.}

{Paragraph 3: Propose next step. Specific: "نقترح عقد اجتماع خلال الأسبوع القادم لمناقشة
اتفاقية الشراكة وتفاصيل الاستحقاق." Name the action, not just "let's meet."}

{--- IF CONDITIONAL ---}

{Paragraph 1: Thank for the time. Signal interest clearly — Dreamy sees potential here.}

{Paragraph 2: State the condition(s) clearly. Each condition on its own line starting
with "•". Be precise — "تسجيل الشركة قانونياً" not "الإجراءات القانونية". 2–3 conditions max.}

{Paragraph 3: What happens after conditions are met — "عند استيفاء هذه المتطلبات، نكون
مستعدين للمضي قدماً في مناقشة الشراكة." Give a realistic timeline for reconvening.}

{--- IF PASS ---}

{Paragraph 1: Thank genuinely. Acknowledge something specific and real from the pitch.}

{Paragraph 2: The decline — direct but respectful. One clear sentence: "بعد دراسة متأنية،
وصلنا إلى أن هذا التوقيت لا يتوافق مع أولويات Dreamy الحالية." Do not give false hope
or vague reasons if the real reason is clear.}

{Paragraph 3: Door-open close. If there's a genuine reason to re-engage later
(market matures, company raises, team expands), say so specifically. If not, a warm
general close: "نتمنى لكم التوفيق والنجاح في هذه الرحلة الرائدة."}

{End all three variants with:}
مع خالص التحيات والتقدير،

وليد الأحمد السنوسي
مدير المنتج، Dreamy
dreamybuilders.com

</div>

---

## English Version (Reference)

Dear {Founder First Name},

{English equivalent — same structure, same branching. Keep it concrete and personal.
3–4 paragraphs. No template filler.}

Best regards,
Waleed Al-Sanosi
Product Manager, Dreamy
dreamybuilders.com

---

## Internal Notes

**Outcome:** {Proceed / Conditional / Pass}
**Key decision factor:** {The one thing that drove this decision — from ceo-review}
{If Conditional:}
**Conditions to track:**
1. {Condition 1 — with a realistic check-in timeline}
2. {Condition 2 if applicable}

{If Pass:}
**Re-engagement trigger:** {Under what specific circumstances would Dreamy reconsider?
Or: "Not applicable — fundamental mismatch."}

Outcome: {Proceed / Conditional / Pass}
```

**CRITICAL:** The file MUST end with the bare line `Outcome: {value}` (no bold, no markdown).
This is read by `/brief` via `grep "^Outcome:"` to determine meeting context.
Use exactly one of: `Outcome: Proceed`, `Outcome: Conditional`, or `Outcome: Pass`.

---

### Step 4: Confirm

After writing the file, print:

```
/followup complete.

Outcome: {Proceed / Conditional / Pass}
File: {session path}/10-followup.md

The Arabic section is copy-paste ready for WhatsApp.
{If Proceed or Conditional: "Next: run /brief for your meeting prep."}
{If Pass: "Cycle complete for this founder."}
```
