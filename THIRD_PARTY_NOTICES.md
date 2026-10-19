# Third-Party Software Notices and Attribution

This document contains licensing and copyright notices for third-party software, open-source projects, and dependencies incorporated into or utilized by **InReal**.

---

## 1. Upstream Base Repository

* **Project Name:** Internship_Scraper (originally distributed as AI/ML Internship Radar)
* **Upstream Repository Owner:** Srikant Choubey (GitHub: Srikant-03)
* **Upstream Repository URL:** [https://github.com/Srikant-03/Internship_Scraper](https://github.com/Srikant-03/Internship_Scraper)
* **License:** MIT License
* **Copyright Notice:**
  ```text
  Copyright (c) Srikant Choubey (Srikant-03) and Internship_Scraper contributors
  ```

### Reused Components
* Initial scaffolding for web scrapers:
  * Baseline Playwright and Requests routines for portals (Internshala, Unstop, Naukri, LinkedIn, Shine, Foundit, Apna, Cutshort, Remotive, WeWorkRemotely, BigTech, DuckDuckGo dorks).
* Initial keyword matching concepts for technical internship roles.
* Early utility functions for delays, user-agent rotation, and basic retry logic (`scraper_utils.py`).
* Early scheduling wrapper using Python `schedule` (`scheduler.py`).

### Material Modifications and New InReal Architecture
The codebase was heavily refactored, hardened, and expanded from a basic crawler script into an enterprise opportunity discovery and fraud-moderation engine:
1. **Selector and Protocol Modernization (`sites/`, `SELECTOR_TRACKER.md`):** Complete rewrite of scrapers to handle dynamic client-side rendering, JSON endpoints (Unstop, Foundit, LinkedIn guest API, AICTE Government portal), Akamai bot-defense workarounds (Naukri, Foundit), and schema.org JSON-LD extraction.
2. **Deterministic Fraud & Scam Detection Engine (`genuine.py`):** Original multi-factor heuristics detecting registration/processing fees, caution deposits, task-for-money / prepaid task scams (like/review schemes), commission-only schemes, mass training-seller operations, and platform employer metrics anomaly detection.
3. **Multi-Disciplinary Ontology Expansion (`fields.py`):** Expansion beyond generic AI/ML to 12 distinct career domains (Software, Data Analytics, Healthcare, Pharma/Biotech, Core Engineering, Finance, Marketing, Design, Content, HR, Law) with thread-safe runtime configuration.
4. **Copy Collapsing and Cross-Post Flagging (`scraper.py`):** Algorithmic clustering of duplicate/flooded posts across locations and contamination propagation for repeat offenders.
5. **Multi-Stage Moderation and Evidence-Verified Fusion Pipeline (`moderation/`, `n8n/`):**
   - Stage 0: Deterministic link resolution, redirect chain analysis, closed-listing parsing, and dangerous contact detection (UPI IDs, free webmail, direct WhatsApp/Telegram links, shortener uncloaking).
   - Stage 1: Vector semantic similarity search against calibrated contrastive embeddings with honest-wording margin enforcement.
   - Stage 2: Hallucination-suppressed LLM judge requiring verbatim quote matching, combined via multi-modal evidence fusion with human review fallback on AI disagreement.
6. **Concurrency, Security, and Production Serving (`output_handler.py`, `app.py`):**
   - Windows-safe thread-locked CSV/JSON synchronization with retry backoff.
   - Fair round-robin batch claim and recovery for moderation workers.
   - Perimeter proxy detection enforcing read-only access for public tunnel visitors and token-gated admin writes.
7. **Production Web Interface (`templates/index.html`, `static/app_radar.js`):**
   - Professional dashboard featuring 3D interactive WebGL visualization, real-time status polling, CAPTCHA alerts, and instant multi-dimensional client-side filtering.

---

## 2. Python Dependencies and Libraries

The following open-source Python packages are required by InReal as defined in `requirements.txt`:

| Package | Upstream License | Purpose in InReal |
|---|---|---|
| **Flask** | BSD 3-Clause | Application web framework and REST API server |
| **Waitress** | Zope Public License (ZPL 2.1) | Production WSGI HTTP server for Windows deployment |
| **Gunicorn** | MIT License | Production WSGI HTTP server for POSIX/Linux deployment |
| **Playwright** | Apache License 2.0 | Headless and visible browser automation engine |
| **playwright-stealth** | MIT License | Anti-bot evasion techniques for Playwright |
| **Requests** | Apache License 2.0 | Synchronous HTTP client library |
| **BeautifulSoup4** | MIT License | HTML/XML parsing and DOM tree traversal |
| **lxml** | BSD 3-Clause | High-performance XML/HTML parser backend |
| **feedparser** | BSD 2-Clause | RSS and Atom syndicate feed ingestion |
| **duckduckgo-search / ddgs** | MIT License | DuckDuckGo search integration for dynamic lab discovery |
| **pandas** | BSD 3-Clause | Tabular data manipulation and analysis |
| **dateparser** | BSD 3-Clause | Natural language date parsing and validation |
| **fake-useragent** | Apache License 2.0 | Randomized realistic browser header generation |
| **tenacity** | Apache License 2.0 | Declarative exponential backoff and retry handling |
| **loguru** | MIT License | Structured file and terminal logging |
| **schedule** | MIT License | In-process periodic task scheduling |
| **pytest** | MIT License | Test execution framework |

---

## 3. Frontend Libraries and Assets

The web dashboard incorporates the following client-side libraries:

| Library / Asset | Upstream License | Purpose in InReal |
|---|---|---|
| **Tailwind CSS (CDN)** | MIT License | Utility-first CSS layout and styling framework |
| **Phosphor Icons** | MIT License | Modern icon system |
| **Three.js (r128)** | MIT License | WebGL 3D background animation canvas |
| **Google Fonts** (Plus Jakarta Sans, JetBrains Mono) | SIL Open Font License 1.1 | Typography styling |

---

## 4. Upstream MIT License Text

The text of the MIT License covering the original upstream repository components is preserved below:

```text
MIT License

Copyright (c) Srikant Choubey (Srikant-03) and Internship_Scraper contributors (https://github.com/Srikant-03/Internship_Scraper)
Copyright (c) 2026 InReal Contributors / Team Chiranjeevi (changes and additions in this repository)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 5. Intellectual Property & Licensing Review Flags

* **License Compatibility:** All incorporated third-party libraries (MIT, BSD-2-Clause, BSD-3-Clause, Apache-2.0, ZPL-2.1) are permissive open-source licenses compatible with commercial distribution and dual-licensing of independent InReal proprietary modules.
* **No Copyleft Contamination:** There are no GPL, AGPL, or LGPL dependencies in the runtime stack that would impose copyleft obligations on newly developed InReal proprietary components.
* **Upstream Attribution Compliance:** All requirements of the upstream MIT License have been satisfied through retention of copyright notices and explicit documentation in this file.
