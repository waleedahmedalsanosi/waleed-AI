# Business Opportunity Profile

**Company:** Munaqis (مناقص)
**Founder:** Faisal Al-Harbi
**Date:** 2026-06-26
**Prepared by:** Waleed Al-Sanosi, Dreamy

---

## The Opportunity in One Paragraph

Munaqis is a B2B SaaS platform targeting the 112,000+ SME contractors in Saudi Arabia who lose government and corporate tenders because their qualification files are incomplete, disorganized, or submitted incorrectly through portals designed for enterprise procurement teams. The founder spent 10 years inside this problem — first as a procurement officer at Saudi Aramco, then charging consulting fees to manually solve it for clients at Riyad Consulting — and brings a 40+ contractor network as a pre-built customer acquisition channel. Vision 2030's push to digitize government procurement has made this problem more acute: Etimad and Monafasat portal requirements are expanding while SME contractors lack the infrastructure to respond. Dreamy's co-founding bet is a 2-year technical build alongside a credible domain expert in a market with no dedicated SaaS solution.

---

## Market Context

**Vertical:** B2B SaaS — Government Procurement / Construction & Contracting
**Geography:** Saudi Arabia (primary: Riyadh + Jeddah); noted Gulf expansion interest (Kuwait, UAE)
**Market size:** ~140,000 registered construction companies per Ministry of Commerce; ~112,000 SMEs (80%). At SAR 500–1,500/month per company, TAM is SAR 672M–2B/year. Methodology is top-down; willingness-to-pay at stated price points not validated.
**Timing signal:** Saudi government procurement digitization under Vision 2030 is expanding portal mandates (Etimad, Monafasat). SME contractor compliance burden is increasing without corresponding tools designed for their scale.
**Key regional dynamic:** KSA government procurement is portal-specific — Etimad and Monafasat are not transferable to other MENA markets. This creates a moat for a Saudi-first product that deeply integrates with local portal requirements, and a barrier for any cross-market competitor entering from the outside.

---

## The Founder

**Name:** Faisal Al-Harbi
**Credibility signal:** Charged consulting fees to solve this problem manually for clients at Riyad Consulting — he has lived it from both the practitioner and seller side, with 10 years of domain depth.
**Risk:** Zero software product experience. No company founded, no team built, no code shipped. Dreamy carries 100% of the technical execution.
**Coachability:** High — raised the API integration risk himself before being asked; presented 20% equity as a starting point and not a fixed position; explicitly named the tech gap.

---

## Product & Technical Fit

**What's being built:** Web SaaS platform where contractors enter company data once (profile, certifications, commercial registration, project history) and auto-generate qualification files for specific tenders. Phase 1 is structured data storage and document generation; Phase 2 adds AI-based tender recommendations.
**Build complexity:** Medium — Core v1 (profile storage + document generation + deadline tracking) is standard web SaaS. Arabic RTL, bilingual UI, and Saudi-specific compliance fields add surface area. Phase 2 AI/ML matching is higher complexity but explicitly deferred.
**Dreamy's technical angle:** Web SaaS (React/Next.js frontend, Node or Python backend), PostgreSQL data model, Arabic RTL UI from day one. Government portal API integration is a Phase 2 investigation item, not a v1 dependency.
**MVP scope estimate:** 4–6 months for a 2-person team (1 frontend + 1 backend/full-stack). Figma prototype is complete; no code exists.

---

## Business Model

**Revenue model:** SaaS subscription — SAR 500/month (small contractors, 5–20 employees) and SAR 1,500/month (medium contractors, 20–50 employees). Founder previously considered SAR 200/month but revised upward; higher tier indicates confidence in value.
**Equity model fit:** Faisal offered 20% to Dreamy over 18–24 months. This is below Dreamy's standard co-founding range for a 100%-technical-lift engagement. Negotiation required. Company is not incorporated — equity is legally unexecutable until a Saudi entity (LLC or equivalent) is established.
**Path to unit economics:** First-customer CAC is near zero — Faisal's 40+ warm contractor relationships are direct-reachable without paid acquisition. LTV at SAR 500–1,500/month is strong if churn is low; the stickiness hypothesis is that contractor qualification data becomes a system of record that's painful to move. Unit economics have not been modeled by the founder.

---

## Traction Summary

**Current state:** Pre-revenue. No MVP. No paying customers.
**Strongest signal:** 9 of 12 contractors interviewed expressed willingness to pay — these are not cold contacts; they are contractors who have worked with Faisal professionally. The conversion intent rate (75%) from warm relationships in a B2B product category is meaningful.
**What's missing:** A signed LOI or paid pilot commitment from at least one of the 9 interested contractors. Without hard commitment, traction is verbal. A single pilot at SAR 500/month would materially change the risk profile.

---

## Risk Register

| Risk | Severity | Mitigation |
|------|----------|------------|
| Company not incorporated — equity legally unenforceable | High | Require Saudi LLC registration before Dreamy signs any agreement. Non-negotiable gate. |
| Government portal API access unconfirmed (Etimad, Monafasat) | High | Run API feasibility check before finalizing product scope. If unavailable, redesign v1 as manual-input with structured data storage — product still has value, differentiation narrows. |
| Solo founder, no team — Dreamy carries 100% technical execution | High | Dreamy must ensure equity reflects this. Escalation path for founder friction must be defined in the SOW. |
| Pricing not validated at stated price points | Medium | Require founder to have one explicit pricing conversation with a pilot customer (SAR 500 or SAR 1,500/month) before build begins. |
| Gulf expansion (Kuwait, UAE) requires separate compliance work | Low | Defer entirely to post-product-market-fit in KSA. Not a v1 concern. |

---

## Dreamy's Strategic Upside

**Equity stake rationale:** A GovTech SaaS targeting 112,000 potential SME customers in KSA is a credible equity bet if the founder can convert warm relationships to first revenue. First-mover in a regionally specific vertical with a founder who has genuine domain access. Seed valuation should reflect pre-revenue stage; Dreamy's equity at 25–35% range is defensible given full technical co-founding responsibility.
**Portfolio synergy:** Arabic RTL web SaaS is core Dreamy capability. This deepens Dreamy's GovTech/RegTech positioning in KSA — a vertical with growing procurement digitization spend under Vision 2030.
**Reputational angle:** Saudi government procurement SaaS is a niche with enterprise expansion potential (large contractors, government departments as direct buyers). First successful product here builds Dreamy's MENA GovTech reference customer.

---

## Evaluation Score

**Score:** 68/100 | **Recommendation:** Conditional

Faisal is the right type of founder for this market — he lived the problem and has the network to acquire first customers. The market is real, the product insight is sound, and the technical build is achievable for Dreamy. What's missing are the structural prerequisites: no company, equity below range, no hard customer commitments, and a key technical dependency unresolved. Proceed if the four conditions above are met within 30 days. If incorporation and equity are not resolved by then, close the file.

---

## Recommended Next Steps

- **Require company incorporation** within 30 days as a non-negotiable gate before Dreamy commits resources. Saudi LLC (ذات مسؤولية محدودة) or equivalent. No entity = no deal.
- **Schedule API feasibility call** with Faisal to investigate Etimad and Monafasat developer access. If no official API exists, assess scraping viability and redesign v1 scope accordingly before writing the PRD.
- **Ask Faisal to secure one LOI** from his 9 interested contractors — even a signed letter of intent to pilot at SAR 500–1,500/month. Converts verbal to commercial.
- **Open equity negotiation** with Samir before the next founder conversation. Confirm Dreamy's floor for a 100%-technical co-founding engagement, then counter Faisal's 20% offer.
