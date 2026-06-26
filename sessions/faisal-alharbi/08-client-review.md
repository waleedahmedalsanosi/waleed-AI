# Client Review Package

**Company:** Munaqis (مناقص)
**Prepared by:** Waleed Al-Sanosi, PM — Dreamy
**Date:** 2026-06-26
**Review type:** Pre-development scope confirmation

---

## Overview

This document summarizes what Dreamy proposes to build for Munaqis as part of our technical co-founding partnership. Please review each section carefully and confirm or raise questions before we begin development.

---

## What We're Building

### Product Summary

Munaqis is a web platform that lets Saudi contractors store all their company information, documents, and project history in one place — and then, whenever they want to bid on a new tender, generate a professional, complete qualification file in minutes rather than spending hours collecting and reformatting the same documents again. Think of it as a permanent qualification file for your company that you update once and use for every tender.

### Who It's For

**Primary user:** A bid coordinator or owner at a Saudi contracting company with 5 to 50 employees.
**How they use it:** They set up their company profile once, upload their certificates and project records, and then generate a ready-to-submit tender qualification packet whenever a new opportunity comes up.

---

## Scope: What's Included in v1

The following features will be built in the v1 MVP:

1. **Company Profile** — A permanent record for your company: commercial registration, VAT, GOSI, Zakat, your MoC classification tier, and your specializations. Enter it once; the system uses it everywhere.
2. **Document Vault** — Upload and store all your qualification documents (CR certificate, Zakat certificate, GOSI, professional licenses, ISO certs). The system tracks expiry dates and warns you 30 days before anything expires.
3. **Project History** — A searchable log of all your past projects: client name, contract value, project type, dates, and reference contact. Export as a formatted table for any tender that asks for similar project experience.
4. **Qualification File Generator** — Enter what a specific tender requires, and the system assembles the right documents and data from your vault and history into a single PDF, ready to submit.
5. **Tender Deadline Tracker** — A calendar view of all your active tender deadlines and document expiry dates. Automated reminders by email or SMS at 7 days and 1 day before each deadline.
6. **Subscription & Billing** — Two plan options: SAR 500/month for small companies and SAR 1,500/month for medium companies. Paid via a Saudi payment gateway. A 14-day free trial with no credit card required. Full VAT-compliant tax invoices after every payment.

**Expected timeline:**
- Design complete and approved by you: 4 weeks from start
- Core profile, vault, and project history features: 12 weeks from start
- All features complete: 22 weeks from start
- Beta testing with your pilot contractors: weeks 23–26
- Public launch: approximately 7 months from start

---

## Scope: What's NOT Included in v1

To ship on time and focus on what matters most for your first customers, the following have been deferred to future versions:

1. **Direct connection to Etimad or Monafasat** — We'll investigate whether these portals allow third-party connections, but v1 does not depend on this. The platform works without it. If the connection becomes available, we'll add it in v2.
2. **AI-powered tender recommendations** — A feature that suggests relevant tenders to you based on your company profile. Planned for v2 once the core platform is live and you have active users.
3. **Mobile app (iPhone or Android)** — v1 is a website that works on desktop and tablet. A mobile app comes after the web version has users and feedback.
4. **Expansion to Kuwait or UAE** — Each country has different portal requirements. We focus on KSA first and expand once the product has proven itself here.
5. **Multiple user logins per company** — In v1, each company has one login. The ability to add team members with different roles is a v2 feature.
6. **Automatic tender alerts** — Monitoring government portals and notifying you when new tenders match your profile. Deferred pending portal access policy.

*These can all be built after launch. We'll reprioritize based on what your users need most.*

---

## What Dreamy Will Deliver

| Deliverable | Description | When |
|-------------|-------------|------|
| Technical Discovery Report | We'll investigate whether Etimad/Monafasat allow third-party integration and finalize the tech plan | Week 1 |
| Full Product Design | Every screen of the app designed in Arabic RTL, reviewed and approved by you in Figma before coding begins | Week 4 |
| Company Profile feature | Working, live feature: full data entry and persistent company record with expiry warnings | Week 8 |
| Document Vault feature | Working, live feature: secure document upload, tagging, expiry tracking, and in-browser preview | Week 8 |
| Project History feature | Working, live feature: project log with search, filter, and PDF export | Week 12 |
| Qualification File Generator | Working, live feature: tender checklist entry, readiness view, and PDF generation in Arabic | Week 16 |
| Tender Deadline Tracker | Working, live feature: calendar, deadline management, automated email/SMS reminders | Week 20 |
| Subscription & Billing | Working, live feature: plan selection, Saudi payment gateway, tax invoice | Week 22 |
| MVP Complete | All 6 features integrated, tested, and running in production | Week 22 |
| Beta Testing Support | 3 rounds of sessions with your pilot contractors; all feedback-driven fixes included | Weeks 23–26 |
| v1 Public Launch | Live product with your first paying customers onboarded | Week 28 |

---

## What We Need from You

Before development begins, we need you to confirm or provide:

- [ ] **Decision: Government portal API** — Can you use your Aramco/government network to request official developer access to Etimad or Monafasat? We need an answer or a contact by end of week 1.
- [ ] **Decision: Sample tender checklists** — Please share 3–5 real tender qualification checklists from past tenders you've worked on. We need these to design the Qualification File Generator correctly.
- [ ] **Decision: Payment gateway** — Do you have an existing merchant account with Moyasar or HyperPay, or does this need to be set up from scratch? Required by week 18 of the build.
- [ ] **Decision: Brand assets** — Do you have a Munaqis logo and brand identity ready, or does Dreamy design it from scratch? Required before design begins (week 1).
- [ ] **Legal: Company incorporation** — Munaqis must be registered as a Saudi legal entity before we sign the partnership agreement. Without a registered company, the equity grant cannot be executed. Please confirm this is underway.
- [ ] **Pilot contractors** — Identify at least 5 contractors from your network who agree to test the platform before public launch. We'll need them available starting week 23.
- [ ] **Weekly sync** — Confirm availability for a 30-minute weekly sync call with Dreamy throughout the build phases.

---

## Equity Terms Reminder

As agreed, Dreamy will receive **[___]%** equity in Munaqis in exchange for the services described in this document.

*Please confirm you've reviewed the full Scope of Work document and are aligned on these terms before we proceed.*

---

## Arabic Summary / ملخص للمراجعة

<div dir="rtl">

## ملخص حزمة المراجعة للعميل

**الشركة:** مناقص
**أعدّها:** وليد الأحمد السنوسي، Dreamy
**التاريخ:** 26/06/2026

---

### ما الذي سنبنيه؟

منصة **مناقص** هي تطبيق ويب يتيح للمقاول السعودي تخزين جميع بيانات شركته ومستنداته وسجل مشاريعه في مكان واحد، ثم توليد ملف تأهيل احترافي وكامل لأي مناقصة في دقائق، بدلاً من قضاء ساعات في جمع نفس الوثائق وإعادة تنسيقها في كل مرة.

### المميزات المشمولة في الإصدار الأول

١. **ملف الشركة الموحّد** — تسجيل البيانات الأساسية للشركة مرة واحدة: السجل التجاري، الزكاة، الغوسي، تصنيف وزارة التجارة، والتخصصات.
٢. **مستودع المستندات** — رفع وتخزين وثائق التأهيل مع تنبيهات تلقائية قبل انتهاء صلاحية أي مستند بـ30 يوماً.
٣. **سجل المشاريع** — توثيق المشاريع المنجزة مع إمكانية البحث والتصدير كجدول منسق.
٤. **مولّد ملفات التأهيل** — إدخال متطلبات أي مناقصة وتصدير ملف PDF احترافي جاهز للتقديم.
٥. **متتبّع مواعيد المناقصات** — تقويم للمواعيد مع تذكيرات تلقائية بالبريد الإلكتروني والرسائل القصيرة.
٦. **نظام الاشتراك والفواتير** — خطتان (500 ريال / 1,500 ريال شهرياً) مع بوابة دفع سعودية وفاتورة ضريبية.

### المميزات المرجأة

١. الربط المباشر ببوابتي اعتماد ومنافسات — مرجأ حتى تأكيد إمكانية الوصول.
٢. توصيات المناقصات بالذكاء الاصطناعي — مرجأ للإصدار الثاني.
٣. تطبيق الجوال (iPhone / Android) — مرجأ بعد إطلاق الموقع.
٤. التوسع خارج المملكة — مرجأ بعد تحقيق الملاءمة في السوق السعودي.
٥. حسابات متعددة المستخدمين — مرجأ للإصدار الثاني.

### ما نحتاجه منكم

- [ ] التحقق من إمكانية الوصول لـ API بوابتي اعتماد ومنافسات عبر شبكة علاقاتكم — الأسبوع الأول.
- [ ] مشاركة 3–5 قوائم متطلبات تأهيل من مناقصات حقيقية سبق التعامل معها.
- [ ] تأكيد وضع حساب التاجر (موسر أو HyperPay) — مطلوب قبل الأسبوع الثامن عشر.
- [ ] توفير الهوية البصرية لمناقص (شعار + ألوان) أو الموافقة على تصميمها من Dreamy.
- [ ] إتمام تسجيل الشركة كياناً قانونياً سعودياً — شرط أساسي قبل توقيع الاتفاقية.
- [ ] تحديد 5 مقاولين من شبكة العلاقات للمشاركة في الاختبار التجريبي.

### شروط الأسهم

ستحصل Dreamy على نسبة **[___]٪** من أسهم شركة مناقص مقابل الخدمات المتفق عليها في هذه الوثيقة.

---

*يُرجى مراجعة هذه الوثيقة والتواصل معنا بأي استفسارات قبل بدء التطوير.*

</div>

---

**To confirm:** Please reply with "Confirmed" or raise any questions.
*This document does not replace the formal Scope of Work agreement.*
