# InReal: Development Evidence and Public Disclosure Audit

**Document Reference:** `INREAL-IP-AUD-005`  
**Classification:** Internal Confidential / IP Review  
**Target Legal Jurisdiction:** India (The Patents Act, 1970 — Section 29, 30, 31)  
**Date of Audit:** October 15, 2026  

---

## 1. Executive Summary

This document consolidates the evidentiary records establishing the chronological development of InReal, author attributions, test execution artifacts, and public disclosure events. 

From an Indian patent perspective:
* Establishing the exact timeline of conception and reduction to practice is necessary to determine priority dates.
* Analyzing public disclosure history is vital under Section 31 of the Indian Patents Act, 1970 (which provides a limited 12-month grace period for public disclosures made under specific statutory conditions, such as exhibitions recognized by the Central Government or disclosures made without the consent of the inventor).

---

## 2. Chronological Git Audit Trail

The repository history comprises 32 commits recorded on October 13, 2026, capturing the rapid evolution from base scraper components to an enterprise moderation platform.

### 2.1 Git Commit Log (Chronological Order)

| Commit Hash | Timestamp (IST) | Author | Commit Message / Milestone | Key Files Added / Modified |
|---|---|---|---|---|
| `b2cbc0e` | 2026-10-13 04:41 | akshat-gupta-111 | docs : adding the README file | `README.md` |
| `84273ee` | 2026-10-13 07:34 | akshat-gupta-111 | feat: index builder | `moderation/build_index.py` |
| `23b21be` | 2026-10-13 07:35 | akshat-gupta-111 | test : azure connections | `moderation/check_azure.py` |
| `ea0e2d6` | 2026-10-13 07:36 | akshat-gupta-111 | test : environment variables | `moderation/env.py` |
| `fcd7864` | 2026-10-13 07:36 | akshat-gupta-111 | test : link parser & validator | `moderation/link_check.py` |
| `15b0777` | 2026-10-13 07:37 | akshat-gupta-111 | feat : frontend verbose | `moderation/notify.py` |
| `0152782` | 2026-10-13 07:40 | akshat-gupta-111 | feat : the filtered listing utility | `moderation/recheck.py` |
| `0ab2f82` | 2026-10-13 07:41 | akshat-gupta-111 | feat : data store for vector searchings | `moderation/scam_patterns.json` |
| `7e9e7a4` | 2026-10-13 07:43 | akshat-gupta-111 | feat : n8n workflow builder | `n8n/build_workflow.py` |
| `9044603` | 2026-10-13 07:44 | akshat-gupta-111 | feat : structure defined via json schema | `n8n/inreal_moderation.json` |
| `0421bae` | 2026-10-13 07:47 | akshat-gupta-111 | feat: verdict logic | `n8n/code/*` (all 8 Node.js modules) |
| `77c6081` | 2026-10-13 07:48 | akshat-gupta-111 | feat : expired verdict logic | `n8n/code/expired_verdict.js` |
| `c4dbebf` | 2026-10-13 07:49 | akshat-gupta-111 | feat : fusion layer added | `n8n/code/fuse.js` |
| `ecb03f3` | 2026-10-13 07:50 | akshat-gupta-111 | feat : judge prompt defined | `n8n/code/judge_prompt.js` |
| `6663389` | 2026-10-13 07:50 | akshat-gupta-111 | feat : fetching vector embeddings | `n8n/code/pack_embed.js` |
| `7da169b` | 2026-10-13 07:52 | akshat-gupta-111 | feat : telemetry signals added | `n8n/code/signals.js` |
| `24d12eb` | 2026-10-13 07:52 | akshat-gupta-111 | feat : stage1 verdict logic | `n8n/code/stage1_verdict.js` |
| `cca1c1c` | 2026-10-13 07:54 | akshat-gupta-111 | feat : performing vector searchings | `n8n/code/vector_search.js` |
| `ab72905` | 2026-10-13 08:01 | Anirudh Kumar | feat : added website templates | `templates/index.html` |
| `e919add` | 2026-10-13 08:06 | Abhishek Goswami | Added sites | `sites/*.py` (15 scraper modules) |
| `56d190d` | 2026-10-13 08:08 | Abhishek Goswami | feat : adding templates | Merge commit |
| `f69395a` | 2026-10-13 08:11 | Anirudh Kumar | feat : adding test/fixtures | `tests/fixtures/*` (15 offline fixtures) |
| `74e7d17` | 2026-10-13 08:11 | Anirudh Kumar | Merge branch 'main' of https://github.com/akshat-gupta-111/InReal_v1 | Merge commit |
| `edaafe9` | 2026-10-13 08:12 | Anirudh Kumar | test : performed platform scrappers testings | `tests/*` (all 14 test modules + `js_runner.js`) |
| `4891dd7` | 2026-10-13 08:16 | Abhishek Goswami | feat : Added static | `static/*` (`app_radar.js`, images, svgs) |
| `a21ed3c` | 2026-10-13 08:16 | Abhishek Goswami | Merge branch 'main' of https://github.com/akshat-gupta-111/InReal_v1 | Merge commit |
| `e517b6a` | 2026-10-13 08:25 | parv | chore : defined environment variables | `.env.example` |
| `d83bf36` | 2026-10-13 08:27 | parv | feat : added filters and fields for app | `fields.py`, `filters.py` |
| `ff20ee6` | 2026-10-13 08:27 | parv | feat : add logic for scraping genuine sites | `genuine.py` |
| `bb744d7` | 2026-10-13 08:28 | parv | util : addding a test util file for sites | `test_sites.py` |
| `75deab5` | 2026-10-13 08:29 | parv | chore : freezing the requirements | `requirements.txt` |
| `43bbfe0` | 2026-10-13 08:29 | parv | feat : orchestrating the pipeline | `app.py` |
| `c1aa81b` | 2026-10-13 08:30 | parv | feat : scheduling & future features | `scraper.py`, `output_handler.py`, `scheduler.py`, `scraper_utils.py`, `SELECTOR_TRACKER.md`, `LICENSE`, `.gitignore`, `run_public.bat`, `run_scheduler.bat` |

---

## 3. Team Authorship & Role Mapping

The git records indicate contributions by four primary contributors:
1. **Akshat Gupta (`akshatguptamtr@gmail.com`):** Authored the core moderation pipeline (`moderation/build_index.py`, `moderation/link_check.py`, `scam_patterns.json`, `n8n/build_workflow.py`, and `n8n/code/*` decision logic).
2. **Anirudh Kumar (`245243892+anirudhx07@users.noreply.github.com`):** Authored the automated test framework (`tests/*`), JavaScript execution bridge (`js_runner.js`), offline test fixtures (`tests/fixtures/*`), and initial template integrations.
3. **Abhishek Goswami (`abhibarsana1@gmail.com`):** Authored portal scraping modernizations (`sites/*.py`) and frontend static assets (`static/*`).
4. **Parv (`90614865+Parv3@users.noreply.github.com`):** Authored multi-disciplinary career ontologies (`fields.py`), deterministic scam/anomaly traps (`genuine.py`), filtered listing utilities (`filters.py`), orchestrator (`app.py`), and concurrency-safe persistence (`output_handler.py`, `scraper.py`).

**Legal Consideration:** Before executing any assignment of patent rights or filing a joint application under Indian patent law, an explicit **Inventorship and Ownership Agreement** must be executed between all four contributors and the corporate entity/assignee.

---

## 4. Empirical Benchmark Records (`SELECTOR_TRACKER.md`)

The file [`SELECTOR_TRACKER.md`](file:///D:/InReal_v1/SELECTOR_TRACKER.md) provides date-stamped technical execution evidence generated during development on October 12, 2026:
* **Round 2 Test (12 Oct 2026, 06:06 – 10:09 IST):**
  * *Internshala:* 393 listings scraped; 81 kept; 4 flagged for multi-town dispersal (6–11 towns); 29 with softer warnings.
  * *Unstop:* 589 read; 15 kept; 3 flagged "stipend not stated", 1 commission pay, 1 training wording.
  * *Naukri:* Identified Akamai CAPTCHA barriers on page 2; resolved via 20 multi-faceted search configurations; 121 read, 21 kept. Discovered and flagged course sellers (e.g. "Fireblaze AI School").
  * *LinkedIn:* 859 read; 305 kept (51 in India; AMD, ByteDance, Qualcomm, IBM, Amazon); 32 duplicate copies folded.
  * *AICTE Government Portal:* 165 read; 17 kept (including 5 Indian Army research fellowships across Southern, Central, and Western Commands with stipends ₹30,000–₹75,000); 90 unpaid rejected; course sellers flagged.

---

## 5. Offline Test Fixtures and Reproducibility

To guarantee regression-free test reproducibility without continuous live network calls, InReal features 15 offline fixtures in `tests/fixtures/`:
* `aicte_search.json` (Real government API payload)
* `apna_search.html`, `apna_detail.html`
* `foundit_search.json`, `foundit_detail.json`
* `internshala_list.html`, `internshala_detail.html`
* `linkedin_search.html`, `linkedin_detail.html`
* `naukri_list.html`, `naukri_detail.html`
* `shine_search.html`, `shine_detail.html`
* `unstop_api.json`, `unstop_list.html`

The entire suite of 85 pytest tests executes deterministically in ~5–7 seconds using these fixtures.

---

## 6. Public Disclosure Analysis and Grace Period Considerations

### 6.1 Known Disclosure Context
The project was presented or developed in connection with:
* **Event:** CodeX 3.0 Hackathon (PIET Panipat, October 2026).
* **Remote Demos:** The file `run_public.bat` provided temporary public demonstration links via Cloudflare Tunnels (`https://*.trycloudflare.com`).
* **Public GitHub Remote:** `https://github.com/akshat-gupta-111/InReal_v1` (pushed October 13, 2026).

### 6.2 Patent Law Impact (India)
1. **Absolute Novelty Principle:** Under Section 2(1)(j) and Section 13 of the Indian Patents Act, 1970, an invention must not be publicly known or published anywhere in the world prior to the date of filing the patent application.
2. **Section 31 Grace Period Limitations:** Section 31 provides that prior public working or publication does not anticipate an invention if:
   * It occurred at an exhibition recognized by the Central Government, or
   * It was communicated to a learned society by the inventor,
   * **Provided** the patent application is filed within **12 months** of such exhibition or communication.
3. **Status of Public Hackathons and GitHub Repositories:** Standard university hackathons and public GitHub repositories are generally **not** automatically gazetted exhibitions under Section 31. Therefore, making the GitHub repository public constitutes an anticipatory prior public disclosure of the exact code and features disclosed therein.
4. **Strategic Recommendation for Patent Counsel:**
   * **File Immediately:** If a patent application is to be pursued in India, a Provisional Patent Application should be filed as expeditiously as possible to minimize the risk of third-party prior art citations.
   * **Emphasize Undisclosed Features:** Focus claims on internal algorithmic interactions, vector calibration bounds, and mathematical score fusion that were not fully exposed in high-level hackathon presentations or UI demos.
   * **Repository Visibility:** If not already private, set the development repository to Private pending patent filing.
