# Yield (Programmatic) Optimization Playbook & PRD

This repository contains the operational architecture and Product Requirement Document (PRD) for migrating a multi-property digital publisher from a legacy ad server waterfall setup to a high-yield, low-latency hybrid monetization stack. 

## 📄 Executive Portfolio Delivery
For the fully formatted, presentation-ready document including complete operational workflows and strategic business cases, please view the PDF directly:
👉 **[View the Complete Hybrid Header Bidding PRD (PDF)](./PRD.pdf)**

---

## 1. Project Overview & Business Case
Sequential waterfall configurations in Google Ad Manager (GAM) inherently introduce heavy latency, data discrepancy, and severe revenue leakage due to the lack of simultaneous auction pressure. This initiative outlines the technical and operational requirements to implement Client-Side Header Bidding (Prebid.js) running concurrently with Server-Side Marketplace Exchange (Google Open Bidding).

### Target Performance Key Performance Indicators (KPIs):
* Yield Optimization: Achieve an immediate +18-22% baseline eCPM uplift by forcing simultaneous bids.
* Latency Mitigation: Reduce global Time-to-First-Ad (TTFA) by a targeted 350ms.
* Discrepancy Control: Reclaim lost revenue by shrinking the reporting delta between SSPs and the Ad Server down under 5%.

---

## 2. Core Architectural Principles
To balance core web vitals with maximum revenue density, demand partners are programmatically split between client-side and server-side execution frameworks based on their identity match rates and script performance weight.

### Prebid.js Configuration Parameters
To ensure standard engineering execution, the wrapper configuration enforces a strict latency budget. The bidderTimeout is locked at 800ms as a maximum threshold before an auto-drop occurs. The priceGranularity is set to 'dense' to optimize incremental tracking in 1-cent steps, and userSync is enabled globally to maintain critical DSP cookie match rates.

### Demand Matrix Deployment Overview
The optimization stack divides major Supply-Side Platforms (SSPs) to balance programmatic fill versus page overhead:
* Client-Side Wrappers (Prebid.js): Allocated to partners requiring aggressive cookie-syncing capabilities to capture high-value DSP budgets (e.g., Index Exchange, Magnite).
* Server-Side Integration (Open Bidding): Reserved for scale-focused global partners with high script overhead to preserve user browser performance (e.g., PubMatic, OpenX).

---

## 3. Ad Server Ad Matching & Key-Value Logic
To ensure flawless monetization delivery without breaking line item limits within Google Ad Manager, this playbook establishes a Dense Price Granularity strategy. By passing precise bids utilizing custom keys (hb_pb and hb_bidder), we dynamically trigger corresponding price priority line items in real-time.

---

## 4. Operational QA & Canary Rollout Framework
To mitigate the risk of structural revenue collapse during deployment, the rollout follows a strict mitigation playbook:
1. Canary Deployment: Route exactly 5% of production traffic to the hybrid wrapper.
2. Console Verification: Audit execution via browser developer tools to verify that all client-side partners are returning clean bid payloads without syntax errors.
3. Automated Rollback: If the billing discrepancy delta between SSP triggers and GAM impressions scales beyond 8% over a rolling 24-hour baseline, traffic automatically rolls back to the legacy setup.

---
*For a granular breakdown of the full integration matrix, configuration checklists, and revenue safety parameters, open the primary [PRD.pdf](./PRD.pdf) document hosted above.*