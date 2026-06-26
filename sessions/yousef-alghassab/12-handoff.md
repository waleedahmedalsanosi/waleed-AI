# Technical Handoff

> ⚠ **DRAFT — DO NOT USE YET**
> CEO Review outcome is **Watch**, not Proceed. This handoff is generated for planning purposes only.
> Do not share with the engineering team until Samir confirms Proceed and the three Watch conditions are met:
> (1) Yousef's professional background, (2) co-founder identity, (3) minimum demand signal from providers.

---

**Product:** أمين (Amin)
**Company:** أمين
**Prepared by:** Waleed Al-Sanosi, PM — Dreamy
**Date:** 2026-06-26
**Status:** DRAFT — partnership pending; development starts TBD after Outcome: Proceed

---

## Overview

أمين is a multi-sided healthcare platform for the Saudi market. Patients search for and review doctors and healthcare facilities using verified, AI-filtered reviews. Healthcare providers get an analytics dashboard showing demand patterns and patient feedback. The system uses a coded doctor profile approach — doctors are identified by unique codes within their facility, not by public name — to navigate Saudi defamation law constraints. v1 scope is mobile-first (iOS + Android) with a web provider dashboard and an internal admin panel. Target build: 2-person Dreamy team over 9 months.

---

## Platform & Stack

**Target platform:** iOS + Android (React Native) + Web (provider dashboard, responsive)
**Frontend:** React Native (mobile); React/Next.js (provider dashboard)
**Backend:** TBD — Node.js or Python recommended; to be confirmed in technical discovery
**Database:** PostgreSQL recommended (relational structure needed for multi-role auth, coded profiles, review verification)
**Auth:** Supabase or custom — patient auth via SMS OTP; provider auth via document verification queue; admin auth via internal accounts
**Hosting / Infra:** AWS or GCP with KSA region or compliant data residency (PDPL requirement)
**Key third-party integrations:**
- SMS/OTP: Unifonic or similar KSA provider (patient registration)
- Push notifications: Firebase Cloud Messaging (post-visit review reminders)
- AI/NLP: LLM API (Arabic + English review filtering) — provider TBD
- Payment: Moyasar or HyperPay (KSA; for v2 provider subscriptions — API integrated in v1, not activated)
- Geolocation: Device GPS (review verification signal)

*Stack decisions marked TBD to be finalized in technical discovery call with founder. Do not assume defaults.*

---

## Language & Localization

- Primary language: **Arabic (RTL)** + English bilingual (required for all user-facing text from v1)
- RTL support required: yes — Arabic-first layout
- Arabic character encoding: UTF-8
- Date format: DD/MM/YYYY (Gregorian); Hijri date display optional for v2

---

## Epics & Features

### Epic 1: Authentication & Onboarding

**Priority:** P0
**Estimate:** 2 weeks for 1 developer

**User story:**
> As a new user (patient or provider), I want to register and log in securely so that my data and activity are tied to my verified identity.

**Acceptance criteria:**
- [ ] Patient registration: phone number entry → OTP verification → profile creation (name, age optional)
- [ ] Provider registration: facility name, CR number, contact email, document upload → enters admin review queue; provider notified when approved
- [ ] Admin login: internal credentials, role-based access control (admin sees all; provider sees own data; patient sees own reviews)
- [ ] Forgot password / OTP resend flow functional
- [ ] Session management: JWT with appropriate expiry; refresh token pattern

**Technical notes:**
- Patient OTP must route through a KSA-compliant SMS provider (Unifonic preferred for local delivery rates)
- Provider document upload must be stored encrypted with access logging (PDPL requirement)

---

### Epic 2: Healthcare Provider Search & Browse

**Priority:** P0
**Estimate:** 3 weeks for 1 developer

**User story:**
> As a patient, I want to search for healthcare providers by specialty and location so that I can find the right facility for my condition.

**Acceptance criteria:**
- [ ] Search by specialty (free text + category select), facility type, and city/area
- [ ] Results list shows: facility name, specialty tags, overall rating (star + numeric), review count, distance (if location permission granted)
- [ ] Facility profile page: name, specialty, address, photos (if uploaded), rating breakdown, review list, doctor code list
- [ ] Search results load within 3 seconds on 4G
- [ ] Arabic and English content display correctly in RTL layout

---

### Epic 3: Coded Doctor Profile System

**Priority:** P0
**Estimate:** 2 weeks for 1 developer

**User story:**
> As a patient, I want to identify the doctor I saw using a code and request their name privately, so I can reference the right physician in my review without exposing them publicly.

**Acceptance criteria:**
- [ ] Each doctor record has a unique alphanumeric code scoped to their facility (e.g., "CLINIC-A-DR-003")
- [ ] Doctor codes are visible on facility profile pages; doctor names are never shown publicly
- [ ] Patient can submit a "name request" for a code; admin receives and fulfils within 24 hours; name delivered in-app to requesting patient only
- [ ] Admin interface: create, assign, deactivate doctor codes per facility; link code to actual doctor record (admin-only visible)
- [ ] Doctor records (name + code mapping) encrypted at rest; admin-only access; access logged

**Technical notes:**
- This is the most legally sensitive part of the system — the name-to-code mapping table must be isolated from the public-facing data model
- Review a data access pattern where the patient-facing API never returns doctor names, only codes

---

### Epic 4: Review Submission & Verification

**Priority:** P0
**Estimate:** 4 weeks for 1 developer

**User story:**
> As a patient who visited a healthcare facility, I want to submit a verified review of my experience so that future patients can benefit from accurate, trustworthy information.

**Acceptance criteria:**
- [ ] Review form is accessible after at least one verification signal is confirmed (see below)
- [ ] Review captures: overall rating (1–5 stars), category ratings (reception, wait time, doctor communication, outcome), free-text comment (Arabic + English), optional doctor code reference
- [ ] Verification signals (modular — implement in order of priority):
  - (a) **Geolocation**: patient was within 500m of the facility in the last 48 hours
  - (b) **Manual confirmation**: patient self-confirms visit date and doctor code
  - (c) **Appointment linkage**: future — requires facility API integration
- [ ] Submitted review enters AI moderation queue before going live; not visible to public until approved
- [ ] Patient receives confirmation notification when review is published

---

### Epic 5: AI Review Filtering

**Priority:** P0
**Estimate:** 3 weeks for 1 developer

**User story:**
> As a platform admin, I want submitted reviews to be automatically screened for abusive or fabricated content so that only high-quality reviews are published.

**Acceptance criteria:**
- [ ] AI model flags reviews containing: profanity, personal attacks on named individuals, content inconsistent with genuine patient experience (e.g., marketing-sounding text)
- [ ] Flagged reviews held in admin moderation queue with AI flag reason
- [ ] Non-flagged reviews auto-publish within 30 minutes of submission
- [ ] Admin can override AI in either direction (approve flagged / flag approved)
- [ ] System supports Arabic and English review text equally

**Technical notes:**
- Start with rules-based filter for profanity (Arabic + English word lists) + LLM call for borderline content — reduces LLM cost vs. calling LLM on every review
- Arabic NLP quality varies significantly by provider; test Anthropic claude-haiku-4-5-20251001 or similar against Arabic healthcare review samples before committing to a provider

---

### Epic 6: Post-Visit Review Reminder

**Priority:** P1
**Estimate:** 1 week for 1 developer

**User story:**
> As the platform, I want to send a push notification to patients after a confirmed visit so that review collection happens at the moment of highest recall.

**Acceptance criteria:**
- [ ] Trigger fires 24–48 hours after a geolocation verification event is logged for a facility
- [ ] Notification contains a deep link directly to the review form for that facility
- [ ] Patient can snooze (7 days) or opt out permanently from notification settings
- [ ] Reminder is not sent if the patient has already submitted a review for that facility visit

---

### Epic 7: Provider Dashboard

**Priority:** P1
**Estimate:** 3 weeks for 1 developer

**User story:**
> As a clinic manager, I want to see how patients are finding and rating my facility so that I can identify areas for improvement and track demand for my specialties.

**Acceptance criteria:**
- [ ] Dashboard accessible on web (responsive; also usable on mobile browser)
- [ ] Shows: total reviews, average rating, month-on-month review volume, specialty search volume for facility's categories, top review themes (positive and negative)
- [ ] Provider can respond to individual reviews (response displayed publicly under the review)
- [ ] Provider can update their facility profile (description, photos, contact info)
- [ ] Data refreshes at least daily; near-real-time for review count

---

### Epic 8: Admin Moderation Panel

**Priority:** P1
**Estimate:** 2 weeks for 1 developer

**User story:**
> As an admin, I want to manage all review moderation, provider verification, and doctor code assignments in one internal tool.

**Acceptance criteria:**
- [ ] Review queue: shows all pending reviews with AI flag status, flag reason, review text, facility, and patient ID (anonymised); approve/reject controls
- [ ] Provider onboarding queue: shows pending provider applications with uploaded documents; approve/reject with notification to provider
- [ ] Doctor code management: create/edit/deactivate codes per facility; link to doctor record
- [ ] Name request queue: shows pending patient name requests with doctor code; fulfil with in-app delivery to requesting patient only
- [ ] Basic audit log: all admin actions timestamped and attributed

---

## Out of Scope (v1)

Do not build the following in v1. These are explicitly deferred:

1. **Direct appointment booking** — Requires individual integration with each clinic's scheduling system; post-launch
2. **In-platform job listings / healthcare recruitment** — Separate legal and product scope; Phase 2
3. **Advanced provider subscription tiers** — Freemium launch; paid tier design post-launch
4. **Cross-border GCC expansion** — Saudi Arabia only in v1
5. **Hijri calendar support** — Optional for v2 depending on user feedback
6. **Delivery service / transport integration** — Discussed as verification mechanism; too complex for MVP; geolocation sufficient at launch

If the founder requests any of these mid-development, escalate to Waleed. Do not scope-creep without a revised SOW.

---

## Milestones

| Milestone | Target | What "done" means |
|-----------|--------|-------------------|
| Architecture sign-off | Week 4 | Stack confirmed, schema drafted, API contracts defined, all screens designed and founder-approved |
| Auth + patient core | Month 3 | Patient registration, search, browse, facility profiles, coded doctor system live in staging |
| Full MVP feature-complete | Month 6 | All 8 epics live in staging; AI filtering integrated; provider dashboard and admin panel functional |
| Beta with pilot facilities | Month 7 | 3–5 real facilities onboarded; real patient reviews flowing; bugs resolved |
| v1 public launch | Month 9 | Live on App Store, Google Play, and web; monitoring in place; founder has dashboard access |

---

## Open Technical Questions

| # | Question | Owner | Needed by |
|---|----------|-------|-----------|
| 1 | Has the coded profile approach been reviewed by a Saudi legal advisor? Can private in-app name disclosure be done without triggering defamation exposure? | Founder | Week 2 |
| 2 | Is there an appointment system at any pilot facility we can integrate with for verification signal, or will geolocation + manual be the v1 approach? | Founder | Week 4 |
| 3 | Which government platform requires quarterly renewal? What is the registration category for a healthcare review platform? | Founder + Dreamy | Week 3 |
| 4 | What documents are required to verify a healthcare facility at onboarding? Who manages verification operations? | Founder | Week 2 |
| 5 | Does أمين have existing branding, or does Dreamy design the visual identity from scratch? | Founder | Week 1 |
| 6 | KSA data residency: AWS (Bahrain + Riyadh region) or GCP (Dammam region)? Preferred cloud? | Dreamy | Week 2 |

---

## Constraints & Non-negotiables

- **Arabic RTL is required from day one** — not an afterthought; all screens designed and built RTL-first
- **Saudi PDPL compliance from day one** — data residency in KSA or compliant region; patient and provider data handling must be documented
- **Coded doctor profile data is classified sensitive** — encrypted at rest, admin-only access, access logged; never exposed in patient-facing API responses
- **AI review filtering must support Arabic** — English-only models will fail on the majority of content; validate Arabic quality before choosing provider

---

## Working with the Founder

- **Waleed's role:** PM and primary liaison. All founder communication goes through Waleed.
- **Weekly sync:** Day and time TBD at contract signing; Waleed present
- **Direct founder contact:** Allowed for technical clarifications about healthcare domain (facility types, doctor specialties, regulatory terms). Loop Waleed in on anything that affects scope, timeline, or the product roadmap.
- **Design approvals:** Founder signs off on each epic's design before build starts — build does not begin on an epic without design approval.
- **Pilot facility introductions:** Founder is responsible for introducing Dreamy to at least 3 pilot facilities by Month 4 for beta testing.

---

## Reference Documents

| Document | Location | What it contains |
|----------|----------|-----------------|
| Founder Brief | `{session}/01-intake.md` | Context from both founder meetings (June 4 + June 16) |
| Evaluation Report | `{session}/02-evaluate.md` | Dreamy's 48/100 Watch assessment with scoring detail |
| PRD | `{session}/05-prd.md` | Full product requirements with user stories and acceptance criteria |
| SOW | `{session}/06-sow.md` | Commercial scope, deliverables table, equity terms (30%), and assumptions |

---

*DRAFT v1 handoff — 2026-06-26. Activate only after CEO confirms Outcome: Proceed. Update this document after each major scope change.*
