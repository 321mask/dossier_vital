<div align="center">

# Dossier Vital

### Your medical year, finally clear — every French health reimbursement, tracked on-device.

[![Platform](https://img.shields.io/badge/platform-iOS%2026-1F3A5F)](https://www.apple.com/ios/)
[![SwiftUI](https://img.shields.io/badge/SwiftUI-only-2E8B9E)](https://developer.apple.com/xcode/swiftui/)
[![SwiftData](https://img.shields.io/badge/SwiftData-CloudKit-2E8B9E)](https://developer.apple.com/xcode/swiftdata/)
[![Privacy](https://img.shields.io/badge/privacy-100%25%20on--device-E26D5C)](#privacy-as-architecture)
[![Status](https://img.shields.io/badge/status-TestFlight-E8C547)](https://testflight.apple.com/join/2gDpYTZ7)

[**Try it on TestFlight**](https://testflight.apple.com/join/2gDpYTZ7) · Demo video *(coming soon)* · [Support &amp; privacy](#privacy-as-architecture)

</div>

---

## The problem

In France, every healthcare visit triggers two parallel reimbursement flows — the Sécurité Sociale, then your *mutuelle*. Between paper *feuilles de soins*, online statements, and dozens of billing codes, no one can actually answer three simple questions: **What did I pay? What was reimbursed? What am I still owed?**

The scale of the gap is documented. France's national auditor refused to certify the health-insurance accounts over **€3.3 billion** in billing errors in a single year. Patients with a long-term condition (ALD), who are supposedly covered at 100%, lose an average of **€790 a year** to care that should have been free. The state's own digital tool, Mon Espace Santé, sits at **33.6% activation** — and even then only stores documents; it never tells you what you're owed.

**Dossier Vital closes that gap — entirely on the user's iPhone.**

---

## What it does

1. **Scan or import** any health document — prescription, lab result, Sécu statement, mutuelle décompte, invoice.
2. **Extract** the structured data on-device with Apple's Foundation Models — amounts, dates, acts, lab values — nothing leaves the phone.
3. **Reconcile** what was actually reimbursed against what *should* have been, using bundled official rate tables and a deterministic rules engine.
4. **Detect** underpayments, missed ALD coverage, and reimbursements that never arrived — each flagged with a plain-language explanation of *why*.
5. **Dispute** in one tap: the app generates a formal, pre-filled contestation letter ready to send to the CPAM or mutuelle.

---

## What I built

I designed and built **the entire application solo** — product, architecture, and every line of the iOS app — from market research through a shipping TestFlight build.

The genuinely hard parts:

### On-device document intelligence
Scanning runs through VisionKit; structured extraction runs through the **Foundation Models framework** (iOS 26's on-device LLM), prompted with per-document-type schemas. Document classification, field extraction, and confidence scoring all happen locally. **No health data ever touches a server.**

### A deterministic reimbursement engine
The financial logic is **rules-based, not a black box** — a deliberate architecture decision. It computes expected reimbursement from bundled Sécu rate tables (CCAM / NGAP / NABM), applying ALD rules, sector and OPTAM logic, *parcours de soins* penalties, franchises, the participation forfaitaire, and the annual cap. **Every figure is traceable to the rule and legal source that produced it**, so the app can always explain itself.

### Reconciliation &amp; anomaly detection *(shipped)*
The app parses imported Sécu and mutuelle statements and **matches them to the right care episode** using fuzzy date / provider / amount matching, then flags discrepancies: Sécu underpayments, mutuelle shortfalls, missed ALD coverage at 100%, and paper claims that likely got lost. Each anomaly carries the exact triggered rule and an expected-vs-received breakdown.

### Privacy as architecture
No account. No password. No server. No tracking. All data persists locally via **SwiftData**, with optional **encrypted iCloud sync** through the user's own account via CloudKit. This isn't a feature bolted on — it's the foundation, and it's a genuine differentiator against every incumbent (Ameli, Mon Espace Santé, mutuelle apps) that structurally cannot offer it.

---

## Tech highlights

| Area | Stack &amp; decisions |
|---|---|
| **UI** | 100% **SwiftUI**, no UIKit. Targets **iOS 26** with the Liquid Glass design system. Universal (iPhone + iPad via `NavigationSplitView`). |
| **On-device AI** | **VisionKit** document scanning + **Foundation Models** for structured extraction and classification — fully local. |
| **Persistence** | **SwiftData** with optional **CloudKit** sync; encrypted at rest. No backend. |
| **Domain logic** | Deterministic reimbursement calculator over bundled CCAM/NGAP/NABM rate tables; explainable rule engine with source citations. |
| **Reconciliation** | Statement parsing + fuzzy episode matching + anomaly detection with traceable triggers. |
| **Platform integrations** | HealthKit (write lab values), WidgetKit, App Intents, share extension for PDF import. |
| **Concurrency** | Strict Swift concurrency — services as actors, UI on `@MainActor`. |

---

## Status

- **TestFlight:** live and testable → https://testflight.apple.com/join/2gDpYTZ7
- **App Store:** submission in progress
- **Source:** private repository — the app handles GDPR special-category health data, so the code is kept closed by design; the TestFlight build is available for full review.
- **Built by:** one person — solo, end to end

---

## Privacy as architecture

Dossier Vital collects **nothing**. It has no user accounts, runs no servers, and contains no analytics or tracking. Document scanning, AI extraction, and all reimbursement calculations happen on the device. Optional sync stays inside the user's own encrypted iCloud. For an app handling health data — a GDPR "special category" — this is the most protective architecture possible, and it's true by design, not by promise.

---

## Screens

> *(Add screenshots here — see captions below.)*

| Screen | |
|---|---|
| **Home dashboard** | A single view of the medical year: pending refunds, detected errors, ALD coverage. |
| **Anomaly detail** | Explainable error detection — expected vs received, and exactly why it was flagged. |
| **Dispute flow** | One tap generates a pre-filled contestation letter for the CPAM or mutuelle. |
| **Document scan** | On-device capture and AI extraction — nothing leaves the iPhone. |
| **Reimbursements** | Sécu and mutuelle reconciled against what should have been paid. |

---

<div align="center">

**Dossier Vital** · Built solo · SwiftUI · iOS 26 · On-device AI · Privacy-first

</div>
