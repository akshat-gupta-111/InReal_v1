# InReal: Third-Party Dependencies and License Risk Audit

**Document Reference:** `INREAL-IP-AUD-006`  
**Classification:** Internal Confidential / IP Review  
**Target Legal Jurisdiction:** India (The Patents Act, 1970; Indian Copyright Act, 1957)  
**Date of Audit:** October 15, 2026  

---

## 1. Executive Summary

This audit assesses the intellectual property, commercial viability, and legal exposure arising from third-party open-source software (OSS), cloud APIs, and web assets utilized by **InReal**.

**Key Findings:**
1. **Zero Copyleft Contamination:** There are no GPL, AGPL, or LGPL licenses in the runtime stack. All OSS libraries use permissive licenses (MIT, BSD-2-Clause, BSD-3-Clause, Apache-2.0, ZPL-2.1).
2. **Proprietary Commercialization Viability:** Permissive licensing allows InReal to maintain its newly developed verification, moderation, and architectural layers as proprietary trade secrets or patented assets.
3. **Upstream MIT Attribution Maintained:** Preserved in both `LICENSE` and `THIRD_PARTY_NOTICES.md`.

---

## 2. Python Runtime Dependencies Audit

The following table categorizes all direct dependencies specified in `requirements.txt`:

| Package Name | Declared License | Linking / Interaction Type | Copyleft Risk | Commercial Use Permitted |
|---|---|---|---|---|
| **flask** | BSD 3-Clause | Dynamic import (Web framework) | None | Yes |
| **waitress** | Zope Public License 2.1 (ZPL) | Subprocess / WSGI server | None | Yes |
| **gunicorn** | MIT License | WSGI runner (POSIX only) | None | Yes |
| **playwright** | Apache License 2.0 | Dynamic library automation | None | Yes |
| **playwright-stealth** | MIT License | Python module | None | Yes |
| **requests** | Apache License 2.0 | Dynamic HTTP client | None | Yes |
| **beautifulsoup4** | MIT License | In-memory HTML parser | None | Yes |
| **lxml** | BSD 3-Clause | C-extension parser binding | None | Yes |
| **feedparser** | BSD 2-Clause | Dynamic RSS parser | None | Yes |
| **duckduckgo-search / ddgs** | MIT License | REST/HTTP wrapper | None | Yes |
| **pandas** | BSD 3-Clause | In-memory data structures | None | Yes |
| **dateparser** | BSD 3-Clause | Date extraction utility | None | Yes |
| **fake-useragent** | Apache License 2.0 | In-memory string generator | None | Yes |
| **tenacity** | Apache License 2.0 | Function decorator utility | None | Yes |
| **loguru** | MIT License | File and console logging | None | Yes |
| **schedule** | MIT License | In-process timer | None | Yes |
| **pytest** | MIT License | Test framework (Dev only) | None | Yes |

### 2.1 License Category Breakdown
* **MIT License:** 8 packages
* **BSD 3-Clause / BSD 2-Clause:** 5 packages
* **Apache License 2.0:** 3 packages
* **ZPL 2.1:** 1 package
* **GPL / AGPL / LGPL (Copyleft):** **0 packages**

---

## 3. External API and Cloud Service Dependencies

InReal interfaces with external cloud APIs for semantic embeddings and LLM reasoning:

| Service / API | Provider | Usage in InReal | Failure Impact | Mitigation / Fallback Implemented |
|---|---|---|---|---|
| **Azure OpenAI Embeddings** | Microsoft Azure | Generating 768-dim embeddings (`text-embedding-3-small`) | Stage 1 vector search fails | Dual provider configuration: switches to Google Gemini via `EMBED_PROVIDER=gemini` |
| **Azure OpenAI Chat** | Microsoft Azure | LLM reasoning (`gpt-4o`) | Stage 2 judge fails | Fusion layer defaults to Stage 1 vector + signal rules; or returns `status = "retry"` |
| **Gemini Embeddings API** | Google | Alternative embedding provider (`gemini-embedding-001`) | Stage 1 vector search fails | Fallback to Azure or requeue |
| **Gemini GenerateContent API**| Google | Alternative generative judge | Stage 2 judge fails | Fallback to Azure or requeue |
| **DuckDuckGo Search Engine** | DuckDuckGo | Academic & research fellowship discovery | University scraping yields 0 results | Automated retry with randomized human delay (`human_delay()`) |

**IP / Data Privacy Consideration:**  
When calling external LLM APIs (Azure OpenAI / Google Gemini), job listing descriptions are transmitted over HTTPS to third-party endpoints. In enterprise production deployments, enterprise Azure OpenAI instances with "zero data retention" agreements should be mandated to preserve confidentiality.

---

## 4. Frontend Libraries and Media Assets

| Asset / Library | Origin / Provider | License | Terms Compliance |
|---|---|---|---|
| **Tailwind CSS (CDN)** | Tailwind Labs | MIT License | CDN link included in `<head>` |
| **Phosphor Icons** | Phosphor Icons | MIT License | Open icon font embedded via unpkg |
| **Three.js (r128)** | Mr.doob / Three.js | MIT License | Embedded via cdnjs for 3D canvas |
| **Google Fonts** (Plus Jakarta Sans, JetBrains Mono) | Google Fonts | SIL Open Font License 1.1 | Embedded via Google Fonts API |
| **Hero Image (`hero_team.png`)** | Custom artwork | InReal proprietary | Commissioned / team-authored flat illustration |
| **SVG Illustrations (`illustration_*.svg`)** | Custom vector graphics | InReal proprietary | Scalable vector illustrations |

---

## 5. Third-Party Intellectual Property Risk Assessment

### 5.1 Trademark Considerations
* **Platform Names:** InReal references third-party trademarks in scraper configurations and UI filters (e.g. "LinkedIn", "Naukri", "Internshala", "Unstop", "Shine", "Foundit", "Apna").
* **Trademark Fair Use:** Under Indian trademark law (Section 30 of the Trade Marks Act, 1999), referencing third-party marks purely for nominative descriptive purposes (to indicate the factual source from which public data was retrieved) constitutes nominative fair use, provided there is no representation of endorsement, sponsorship, or official affiliation.
* **Recommendation:** Ensure all user-facing documentation includes standard nominative fair use disclaimers:
  *"All product names, logos, and brands are property of their respective owners. All company, product, and service names used in InReal are for identification and descriptive purposes only."*

### 5.2 Terms of Service (ToS) & Scraping Jurisprudence
* **Legal Landscape:** Web scraping jurisprudence in India is guided by the Information Technology Act, 2000 (Section 43, Section 66) and common law principles of trespass to chattels. Scraping publicly available data without circumventing cryptographic access controls or overwhelming target servers is widely considered lawful, particularly when respecting rate limits.
* **Built-in InReal Mitigations:**
  - InReal incorporates randomized human delays (`human_delay()`, 1.5–4.5s) to prevent server strain.
  - Multi-query narrowing reduces total page queries.
  - Uses published guest APIs (LinkedIn, Unstop) and schema.org standard microdata where available.
  - Dry-run modes write to local CSV without persistent database pollution.

---

## 6. Audit Conclusion

The third-party dependency profile of InReal is **clean, compliant, and commercially unencumbered**. There are no viral open-source licenses that would compromise InReal's ability to protect its proprietary source code or seek patent protection for its original algorithmic architectures.
