# Technical Handoff

**Product:** Munaqis — Contractor Qualification SaaS
**Company:** Munaqis (مناقص)
**Prepared by:** Waleed Al-Sanosi, PM — Dreamy
**Date:** 2026-06-26
**Status:** Partnership confirmed — development starting TBD (pending incorporation + equity sign-off)

---

## Overview

Munaqis is a web SaaS platform for Saudi SME contractors (5–50 employees) that lets them store company profile data, documents, and project history once, then generate professional tender qualification files on demand for any government or corporate bid. The v1 MVP covers six features: contractor profile, document vault, project history log, qualification file generator, tender deadline tracker, and subscription/billing. Target market is 112,000+ registered SME contractors in KSA. The build is Arabic RTL-first, web-only, with a Saudi payment gateway. Target: v1 live in approximately 7 months from engagement start.

---

## Platform & Stack

**Target platform:** Web (browser-based SaaS, responsive — desktop and tablet primary)
**Frontend:** React / Next.js (TBD — finalize in technical discovery call)
**Backend:** Node.js or Python (TBD — finalize in discovery call)
**Database:** PostgreSQL
**Auth:** Supabase Auth or equivalent (email + password + OTP; 30-min session timeout)
**Hosting / Infra:** AWS Bahrain region (me-south-1) preferred for KSA data residency; Vercel for frontend if Next.js
**Key third-party integrations:**
- Payment: Moyasar (preferred, Saudi-native, supports mada/Visa/Mastercard) or HyperPay as fallback
- Email notifications: SendGrid
- SMS notifications: Twilio (or local KSA SMS provider for better delivery)
- PDF generation: Puppeteer or equivalent with Arabic font embedding support

*Note: Stack decisions marked TBD are to be finalized in the technical discovery call with the founder. Do not assume defaults.*

---

## Language & Localization

- Primary language: **Arabic (RTL)** + English bilingual (required for all user-facing text)
- RTL support required: **yes — from day one, not a post-launch pass**
- Arabic character encoding: UTF-8
- Date format: DD/MM/YYYY (Gregorian); Hijri display optional for future iteration
- Generated PDF files must embed Arabic fonts — do not rely on system fonts for PDF output
- All form validation messages, error states, and empty states must exist in both Arabic and English

---

## Epics & Features

Organized by build sequence. Each epic = one deployable unit.

---

### Epic 1: Contractor Company Profile

**Priority:** P0 (MVP blocker — all other features depend on this data)
**Estimate:** 2 weeks (1 developer)

**User story:**
> As a bid coordinator, I want to enter my company's registration, classification, and license data once so that I never have to re-enter this information for a new tender.

**Acceptance criteria:**
- [ ] User can enter and save all required fields: CR number, VAT registration number, GOSI number, Zakat certificate number, MoC classification tier, specializations (multi-select), and license expiry dates
- [ ] Profile persists across sessions and is tied to the company account
- [ ] User can edit any field; updates reflect immediately in all future generated files
- [ ] Fields with expiry dates display a warning badge when within 30 days of expiry
- [ ] Arabic and English labels on all fields; form validates in both languages
- [ ] Profile completeness indicator shows % of fields filled (motivates completion)

**Technical notes:**
MoC contractor classification tiers must be mapped to the actual Saudi Ministry of Commerce classification codes — get the full list from the founder before building the field. This list changes occasionally; store as a configurable enum, not hardcoded.

---

### Epic 2: Document Vault

**Priority:** P0 (MVP blocker)
**Estimate:** 2 weeks (1 developer)

**User story:**
> As a bid coordinator, I want to upload my qualification documents and have them stored with expiry dates so that I always know what's current and can attach the right version to any tender.

**Acceptance criteria:**
- [ ] User can upload PDF files up to 20MB per document
- [ ] Each uploaded document requires a "document type" tag (CR certificate, VAT certificate, GOSI certificate, Zakat certificate, professional license, ISO certification, other)
- [ ] Optional expiry date field per document
- [ ] Dashboard widget shows count of: expired documents, expiring within 30 days, and valid documents
- [ ] Documents can be previewed in-browser (PDF viewer) without downloading
- [ ] Documents can be downloaded and deleted
- [ ] File storage scoped strictly to the company account — no cross-account access

**Technical notes:**
Store uploaded files in S3 (or equivalent object storage) with pre-signed URLs for access — never serve documents directly from the filesystem. Enforce account-level bucket prefixes to prevent IDOR. 20MB per file; set a total vault storage limit per subscription tier (e.g. 500MB small, 2GB medium).

---

### Epic 3: Project History Log

**Priority:** P0 (MVP blocker — required by Qualification File Generator)
**Estimate:** 1.5 weeks (1 developer)

**User story:**
> As a bid coordinator, I want to log our completed projects so that I can quickly pull a list of relevant experience when a tender asks for "similar projects."

**Acceptance criteria:**
- [ ] User can add a project with: client name, contract value (SAR), project type (dropdown), start date, end date, reference contact name, reference contact phone
- [ ] Projects are searchable by type, client name, and date range
- [ ] Project list exports to PDF as a formatted "Project Experience" table, Arabic RTL, with company name and logo in header
- [ ] Minimum 50 project entries supported per account in v1
- [ ] Project types are a configurable list; get the taxonomy from the founder (construction subtypes, facilities management, subcontracting, etc.)

---

### Epic 4: Qualification File Generator

**Priority:** P0 (core value proposition)
**Estimate:** 4 weeks (1 developer)

**User story:**
> As a bid coordinator, I want to describe what a tender requires and have the platform assemble the right documents for me so that I submit a complete, professional packet without manually collating files each time.

**Acceptance criteria:**
- [ ] User can create a new "Tender" record with: tender name, issuing entity, deadline date, and a checklist of required items (manually entered line by line)
- [ ] System maps each checklist item to the closest available asset: profile data field, vault document by type, or project history entry
- [ ] "Readiness view" shows each required item as: Satisfied (green), Missing (red), or Needs attention (amber — document exists but expired)
- [ ] User can generate a single PDF export containing: cover page (company name, tender name, date), table of contents, then all satisfied items assembled in order
- [ ] Generated PDF renders Arabic RTL correctly throughout, including right-aligned text and Arabic numerals where appropriate
- [ ] PDF file name format: `[CompanyName]_[TenderName]_[YYYY-MM-DD].pdf`
- [ ] PDF generation completes in < 10 seconds for a standard 10-document packet
- [ ] User can save a tender as a draft and return to it later

**Technical notes:**
This is the highest-complexity feature. PDF generation with Arabic text requires a library that supports RTL and Arabic font embedding — Puppeteer with a custom HTML template is the recommended approach. Test Arabic rendering thoroughly before committing to the approach. The checklist-to-asset mapping in v1 is manual (user confirms which asset satisfies which requirement) — do not attempt auto-matching in v1.

---

### Epic 5: Tender Deadline Tracker

**Priority:** P1 (important but not a launch blocker if Epic 4 ships)
**Estimate:** 1.5 weeks (1 developer)

**User story:**
> As a bid coordinator, I want to see all my active tender deadlines in one place so that I never miss a submission date.

**Acceptance criteria:**
- [ ] Calendar view and list view of upcoming tender deadlines (tenders created in Epic 4 appear automatically)
- [ ] User can add standalone deadlines not tied to a full tender (e.g., document renewal dates)
- [ ] Deadlines within 7 days shown in amber; overdue deadlines shown in red
- [ ] Automated email reminder sent at 7 days before deadline and 1 day before deadline (user-configurable: on/off per tender)
- [ ] Automated SMS reminder at 1 day before deadline (user-configurable)
- [ ] Reminder emails and SMS render in Arabic

**Technical notes:**
Schedule reminders via a cron job or message queue (not synchronous at send time). Reminder delivery must be idempotent — if the job runs twice, only one message is sent. Use SendGrid for email; Twilio for SMS. Keep reminder templates in a translations file for easy Arabic/English maintenance.

---

### Epic 6: Subscription & Billing

**Priority:** P0 (required for paid launch)
**Estimate:** 3 weeks (1 developer)

**User story:**
> As a company owner, I want to subscribe to the appropriate plan and manage my billing so that I have uninterrupted access to the platform.

**Acceptance criteria:**
- [ ] Two tiers displayed clearly at signup and on upgrade screen: Small (SAR 500/month, up to 20 employees, up to 5 active tenders) and Medium (SAR 1,500/month, up to 50 employees, unlimited tenders)
- [ ] 14-day free trial: no credit card required; full feature access; banner showing days remaining
- [ ] Payment processed via Moyasar (mada, Visa, Mastercard); HyperPay as fallback if Moyasar integration blocks
- [ ] After each successful payment: system generates a VAT-compliant tax invoice (required by ZATCA for KSA) and emails it to the account owner
- [ ] User can cancel subscription from account settings; access continues until end of billing period
- [ ] Failed payment triggers: 3-day grace period with banner warning, then account downgraded to read-only until payment method updated
- [ ] Tier limits enforced: Small tier cannot add a 6th active tender until they upgrade

**Technical notes:**
KSA VAT compliance (ZATCA) requires specific invoice fields: seller name, seller VAT number, buyer name, invoice date, line items with VAT breakdown, QR code (ZATCA Phase 2 e-invoicing). Do not skip this — it is legally required for B2B SaaS in KSA. Investigate ZATCA Phase 2 e-invoicing requirements early; this may require integration with a ZATCA-certified provider.

---

## Out of Scope (v1)

Do not build the following in v1. These are explicitly deferred:

1. **Government portal API integration (Etimad, Monafasat)** — API access unconfirmed; v1 is manual-input/manual-submission. If the founder requests this mid-build, escalate to Waleed immediately.
2. **AI-based tender recommendation engine** — Requires live tender data feed and ML infrastructure; Phase 2.
3. **Mobile applications (iOS / Android)** — Web-first; mobile deferred post-PMF.
4. **Gulf market expansion (Kuwait, UAE)** — Different portal requirements per country; deferred post-KSA PMF.
5. **Multi-user / team accounts** — Role-based access within a company account; Phase 2.
6. **Automated tender discovery / portal monitoring** — Scraping government portals for new tenders; deferred pending API policy clarity.

If the founder requests any of these mid-development, escalate to Waleed. Do not scope-creep without a revised SOW.

---

## Milestones

| Milestone | Target | What "done" means |
|-----------|--------|-------------------|
| Technical discovery complete | Engagement +1 week | API feasibility confirmed or ruled out; stack finalized; schema drafted; Moyasar merchant account status known |
| Architecture sign-off | Engagement +1 week | ERD approved, API contracts defined, hosting environment provisioned |
| Design complete | Engagement +4 weeks | All screens mocked in Figma (Arabic RTL), all edge cases covered, founder has reviewed and approved every screen before build starts |
| Epic 1 + 2 shipped (Profile + Vault) | Engagement +8 weeks | Features live in staging, all acceptance criteria passing, founder has tested with real company data |
| Epic 3 shipped (Project History) | Engagement +12 weeks | Feature live in staging, PDF export generates correctly in Arabic RTL |
| Epic 4 shipped (File Generator) | Engagement +16 weeks | PDF generation working end-to-end, Arabic rendering verified, tested with 3 real tender checklists from founder |
| Epic 5 + 6 shipped (Tracker + Billing) | Engagement +22 weeks | Billing integrated with Moyasar, VAT invoices generating correctly, ZATCA compliance verified |
| MVP feature-complete | Engagement +22 weeks | All epics integrated in production environment, smoke-tested end-to-end |
| Beta testing complete | Engagement +26 weeks | 3 rounds of sessions with minimum 3 pilot contractors; all P0 bugs resolved; P1 bugs triaged |
| v1 public launch | Engagement +28 weeks | DNS live, monitoring active, first paying customer onboarded, founder trained on basic ops |

---

## Open Technical Questions

These are blockers or near-blockers that need answers before or during the first week.

| # | Question | Owner | Needed by |
|---|----------|-------|-----------|
| 1 | Do Etimad and Monafasat offer any public or private API access for third-party applications? | Founder + Dreamy (parallel tracks) | End of week 1 |
| 2 | What exact qualification file formats are required by different Saudi government bodies — are they standardized or per-entity? Founder to provide 3–5 real tender checklists. | Founder | End of week 1 |
| 3 | Does the founder have a Moyasar merchant account, or does one need to be set up? (Requires registered company entity.) | Founder | Week 2 |
| 4 | Is KSA data residency (AWS Bahrain or local) a requirement for the target customers? | Founder | Week 2 |
| 5 | What is the full MoC contractor classification tier list with official codes? | Founder | Before Epic 1 build |
| 6 | What project type taxonomy does the founder want for the Project History log? | Founder | Before Epic 3 build |
| 7 | ZATCA Phase 2 e-invoicing: does the startup need a ZATCA-certified ERP integration, or does a compliant invoice format with QR code suffice at this revenue scale? | Dreamy (legal/compliance check) | Before Epic 6 build |

---

## Constraints & Non-negotiables

- **Arabic RTL is required from day one** — not a post-launch localization pass. Every component must support RTL at build time. Test with real Arabic content, not placeholder text.
- **KSA data residency preferred** — host in AWS Bahrain (me-south-1) or a KSA-local provider. Do not default to EU-West or US regions without explicit founder approval.
- **ZATCA-compliant invoicing** — KSA B2B SaaS requires VAT invoices meeting ZATCA Phase 2 standards. This is a legal requirement, not optional.
- **Moyasar (or HyperPay) payment gateway** — no international-only gateways; mada card support is required for the KSA market.
- **PDF Arabic rendering** — generated PDFs must embed Arabic fonts. Test early with a real Arabic PDF before committing to the generation approach.
- **Account-level data isolation** — strict. No contractor should ever be able to see another contractor's data. Enforce at the query level, not just the UI.

---

## Working with the Founder

- **Waleed's role:** PM and primary liaison. All commercial discussions, scope changes, and timeline negotiations go through Waleed — not directly to the engineering team.
- **Weekly sync:** 30-minute call with Waleed present every week during active build phases. Day and time to be agreed at engagement start.
- **Direct founder contact:** Allowed for technical clarifications (e.g., "what does this classification code mean?"). Loop Waleed in on anything that affects scope, timeline, or the product roadmap.
- **Design approvals:** Founder signs off on each epic's Figma screens before build starts. Do not begin coding an epic until the founder has confirmed the design. Build-to-spec, not build-then-show.
- **Language:** Founder is Arabic-speaking. Written updates to Waleed can be in English; any written communication that goes to the founder should be bilingual or Arabic-first.

---

## Reference Documents

| Document | Location | What it contains |
|----------|----------|-----------------|
| Founder Brief | `sessions/faisal-alharbi/01-intake.md` | Raw context from the founder meeting — product vision, market claims, traction data |
| Evaluation Report | `sessions/faisal-alharbi/02-evaluate.md` | Dreamy's 100-pt assessment: 68/100, Conditional |
| PRD | `sessions/faisal-alharbi/04-prd.md` | Full product requirements with user stories, technical requirements, and open questions |
| SOW | `sessions/faisal-alharbi/05-sow.md` | Commercial scope, deliverables table, equity terms, and assumptions |

---

*v1 handoff — 2026-06-26. Update this document after each major scope change or after the technical discovery call resolves open questions.*
