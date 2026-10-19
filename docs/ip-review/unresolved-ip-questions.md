# InReal: Unresolved Intellectual Property, Ownership, and Legal Questions

**Document Reference:** `INREAL-IP-AUD-008`  
**Classification:** Internal Confidential / Attorney-Client Privileged Work Product  
**Target Legal Jurisdiction:** India (The Patents Act, 1970; Copyright Act, 1957; IT Act, 2000; Trade Marks Act, 1999)  
**Date of Assessment:** October 19, 2026  

---

## 1. Executive Summary

This document enumerates unresolved legal, regulatory, ownership, and inventorship questions identified during the repository audit of **InReal**. These issues require formal consultation with a registered Indian Patent Agent and IP Attorney prior to commercial launch, investor disclosure, or filing patent applications with the Indian Patent Office (IPO).

---

## 2. Inventorship, Authorship, and Assignment of Rights

### 2.1 The Multi-Contributor Inventorship Profile
The git commit history and development records establish contributions across distinct modules by four individuals:
1. **Akshat Gupta (`akshatguptamtr@gmail.com`):** Authored the core multi-stage moderation architecture, pattern vector indexer, link resolver, and n8n decision logic.
2. **Anirudh Kumar (`245243892+anirudhx07@users.noreply.github.com`):** Authored the automated test framework, Node.js execution bridge, offline test fixtures, and web templates.
3. **Abhishek Goswami (`abhibarsana1@gmail.com`):** Authored scraper modernization routines and frontend static assets.
4. **Parv (`90614865+Parv3@users.noreply.github.com`):** Authored multi-disciplinary career ontologies, deterministic scam and employer anomaly checks, orchestrator pipelines, and concurrency-safe persistence.

### 2.2 Legal Questions Requiring Counsel Review
1. **Section 6 Standing to Apply (Patents Act, 1970):** Under Indian patent law, an application can only be filed by the "true and first inventor(s)" or their legal assignee. If a corporate entity (startup or company) files the patent, an explicit, written **Assignment Deed (Form 1, Proof of Right)** must be executed by all four contributors.
2. **True and First Inventor Declarations:** For each candidate patent claim (e.g. Evidence Verification vs. Vector Calibration vs. Contamination Propagation), counsel must determine which specific contributors conceived and reduced to practice the claimed technical features, ensuring accurate inventorship declarations under Section 28.
3. **Student / Institutional IP Ownership:** If any development occurred using university computing infrastructure or during academic coursework, institutional IP policies must be reviewed to confirm that the educational institution asserts no competing ownership claims.

---

## 3. Public Disclosure and Section 31 Grace Period Vulnerability

### 3.1 Known Public Disclosure Events
1. **Public Remote Repository:** The codebase was pushed to a public GitHub repository (`https://github.com/akshat-gupta-111/InReal_v1`) on October 13, 2026.
2. **Hackathon Demonstrations:** The project was demonstrated at CodeX 3.0 (PIET Panipat, October 2026).
3. **Public Cloudflare Tunnels:** Dynamic endpoints were temporarily exposed via `run_public.bat` (`https://*.trycloudflare.com`).

### 3.2 Statutory Grace Period Analysis under Indian Patent Law
* **Absolute Novelty Rule (Section 2(1)(j), Section 13):** India strictly follows the absolute novelty standard. Any public disclosure, publication, or demonstration prior to the priority date can be cited as anticipatory prior art by a patent examiner.
* **Limitations of Section 31:** Section 31 of the Patents Act, 1970 provides a limited 12-month grace period for public disclosures made:
  - At an industrial or other exhibition certified by the Central Government, or
  - In a paper read by the inventor before a learned society.
  *Critical Finding:* College hackathons, non-certified tech competitions, and public GitHub repositories are **not** automatically recognized exhibitions under Section 31.
* **Counsel Action Required:**
  - **Immediate Filing of Provisional Application:** A Provisional Patent Application should be filed immediately with the Indian Patent Office (IPO) to secure the earliest possible priority date.
  - **Claim Focus:** Draft patent claims focusing on internal algorithmic transformations, mathematical vector margin calibrations, and evidence-verification interlocks that were not exposed in public UI demos or high-level slide presentations.
  - **Repository Privacy:** Maintain the development repository in a Private state.

---

## 4. Upstream MIT License Interaction and Dual-Licensing Boundaries

### 4.1 Upstream Attribution Status
* **Upstream Project:** `Internship_Scraper` (https://github.com/Srikant-03/Internship_Scraper)
* **Author:** Srikant Choubey (`Srikant-03`)
* **License:** MIT License

### 4.2 Legal Separation of Proprietary and Open-Source Modules
Under copyright law, derivative works of MIT-licensed software may be dual-licensed or maintained under proprietary terms, provided the original copyright and permission notice are preserved on the inherited files.

**Open Questions for Legal Review:**
1. **Module Clean-Room Boundaries:** Have we maintained sufficiently clear modular separation between inherited scraping utilities (`scraper_utils.py`, `sites/`) and proprietary InReal engines (`genuine.py`, `moderation/`, `n8n/`, `fields.py`)?
2. **Distribution Architecture:** When packaging InReal for commercial enterprise customers, counsel should confirm the cleanest packaging mechanism: distributing inherited scraper components under MIT alongside proprietary moderation modules under an InReal Commercial License.

---

## 5. Web Scraping, Data Ingestion, and Nominative Fair Use

### 5.1 Legality of Web Ingestion under the Information Technology Act, 2000
* **Statutory Provisions:** Section 43 (penalties for unauthorized access or copying of data) and Section 66 (computer-related offences) of the IT Act, 2000.
* **Current Safeguards in Code:**
  - InReal incorporates randomized delay jitter (`human_delay()`, 1.5–4.5s) to avoid denial-of-service impacts.
  - Consumes published, public guest APIs (LinkedIn, Unstop) and standard schema.org microdata.
  - Ingests only public factual employment listings; does not access user profiles, private databases, or password-protected content.
* **Counsel Action Required:** Draft a formal Terms of Service (ToS) and Acceptable Use Policy for InReal platform users disclaiming liability for third-party portal availability.

### 5.2 Trademark Fair Use (Trade Marks Act, 1999)
* InReal references platform names ("LinkedIn", "Naukri", "Internshala", "Unstop", "Shine", "Foundit", "Apna") across UI filter menus.
* Under Section 30 of the Indian Trade Marks Act, 1999, referencing third-party marks is permitted under **nominative fair use** to identify the factual source of data, provided it does not imply sponsorship or affiliation.
* **Counsel Action Required:** Verify that all client-facing UI panels, footers, and documentation carry clear nominative fair use disclaimers.

---

## 6. Prioritized Action Checklist for IP Counsel

| Priority | Action Item | Statutory Basis | Target Timeline |
|---|---|---|---|
| **P0 (Immediate)** | Execute Written Assignment Agreements (All 4 Contributors -> Corporate Entity) | Indian Patents Act § 6, Copyright Act § 19 | Within 7 days |
| **P0 (Immediate)** | File Indian Provisional Patent Application (Focus on Multi-Modal Evidence Verification & Vector Calibration) | Indian Patents Act § 9, § 10 | Within 14 days |
| **P1 (High)** | InPASS / WIPO Prior Art Search on Contrastive Semantic Margin Enforcement | Indian Patents Act § 13 | Within 30 days |
| **P1 (High)** | Trademark Registration Application for "InReal" in Class 9 (Software) & Class 42 (SaaS) | Trade Marks Act, 1999 | Within 30 days |
| **P2 (Medium)** | Draft Enterprise Commercial License Terms for InReal Proprietary Moderation Services | Indian Contract Act, 1872 | Prior to enterprise pilot |

---

*Notice: This internal analysis is prepared for the confidential guidance of the management and IP counsel of InReal and does not constitute formal legal representation.*
