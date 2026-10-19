# InReal: Patent Candidate Features and Patentability Analysis

**Document Reference:** `INREAL-IP-AUD-007`  
**Classification:** Internal Confidential / Attorney-Client Privileged Work Product  
**Target Legal Jurisdiction:** India (The Patents Act, 1970 — Sections 2(1)(j), 3(k); CRI Guidelines 2017)  
**Date of Assessment:** October 15, 2026  

---

## 1. Executive Summary & Legal Framework (India)

This document evaluates the proprietary technical features of **InReal** for patent eligibility under the legal framework of the Indian Patents Act, 1970, and the official *Guidelines for Examination of Computer Related Inventions (CRI)* issued by the Office of the Controller General of Patents, Designs and Trade Marks (CGPDTM, 2017).

### 1.1 The Section 3(k) Statutory Threshold
Under Section 3(k) of the Indian Patents Act, 1970, the following are non-patentable subject matter:
> *"a mathematical or business method or a computer programme per se or algorithms"*

Following the landmark Delhi High Court judgments in *Ferid Allani v. Union of India (2019)*, *Microsoft Technology Licensing v. Assistant Controller (2023)*, and *Raytheon Company v. Controller General (2023)*:
* Computer-implemented inventions are **patentable** if they solve a specific **technical problem**, demonstrate a **technical contribution**, and produce a **technical effect** that goes beyond the normal physical interactions between the program and the hardware.
* Pure business methods (e.g. general job matching, hiring logic, ranking candidates) are strictly barred.
* Generic web scraping, simple string matching, and abstract mathematical equations per se are non-patentable.

**Assessment Strategy:**  
We focus patent claims strictly on **computer-implemented technical mechanisms**: automated multi-modal evidence verification, anti-hallucination confidence discounting, contrastive vector space calibration with boundary margin enforcement, and graph/entity contamination propagation.

---

## 2. Evaluation of Patent Candidate Inventions

---

### Candidate Invention 1: Evidence-Grounded Multi-Modal Verification Engine with Hallucination Discounting and Divergence Routing

* **Working Title:** *A System and Method for Computer-Implemented Document Authenticity Verification Using Grounded Evidence Extraction, Multi-Modal Score Fusion, and Divergence-Driven Escalation*
* **Primary Code Files:** [`n8n/code/fuse.js`](file:///D:/InReal_v1/n8n/code/fuse.js), [`n8n/code/judge_prompt.js`](file:///D:/InReal_v1/n8n/code/judge_prompt.js), [`moderation/link_check.py`](file:///D:/InReal_v1/moderation/link_check.py)
* **Status:** **Strong Patent Candidate (High Viability)**

#### A. Technical Problem Solved
Automated fraud detection using generative language models (LLMs) suffers from ungrounded hallucination, where models fabricate justifications or misinterpret negated clauses (e.g. asserting fraud on a genuine "zero registration fee" statement). Relying purely on generative models results in false positives, ungrounded blocking, and system unreliability.

#### B. Technical Solution & Novel Architectural Elements
1. **Verbatim Quote Verification Loop (`verifyEvidence`):** The system passes unstructured opportunity text to an LLM under a constrained prompt schema requiring extracted verbatim evidence quotes. A downstream deterministic verification processor intercepts the model payload and executes exact substring validation against the normalized source text. Unmatched quotes are computationally purged.
2. **Contradictory Evidence Filtering:** Verified quotes are cross-analyzed against negation regular expressions to identify counter-evidence (e.g. explicit disclaimers of fees) and discarded from the fraud evidence set.
3. **Evidence-Grounded Confidence Dampening:** When an LLM outputs an elevated fraud probability \( p_{\text{raw}} > 0.5 \) but zero supporting quotes survive deterministic verification, the confidence score is automatically attenuated toward neutral using a dampening operator:
   \[
   p_{\text{adj}} = 0.5 + (p_{\text{raw}} - 0.5) \times \gamma, \quad \text{where } \gamma = 0.25
   \]
4. **Multi-Modal Evidence Fusion:** Fuses adjusted LLM probability, vector semantic similarity, and deterministic network/contact signals using calibrated weights:
   \[
   \text{Risk} = w_{\text{llm}} p_{\text{adj}} + w_{\text{vec}} V_{\text{vector}} + w_{\text{sig}} S_{\text{signals}}
   \]
5. **Divergence-Driven Safety Interlock:** Calculates signal divergence \( |p_{\text{adj}} - V_{\text{vector}}| \). If the divergence exceeds a predetermined safety threshold (\( \Delta \ge 0.6 \)), the automated gatekeeper disables automatic approval/rejection and routes the payload to a human review queue.
6. **Dual-AI Failure Safe Mode:** If both embedding and LLM APIs fail, the record is placed into a `retry` state, guaranteeing that missing external services never trigger incorrect automated verdicts.

#### C. Section 3(k) Patentability Defense (Technical Effect)
* **Technical Effect:** The technical effect is the measurable reduction in computational hallucination, prevention of false-positive state transitions in automated classification systems, and deterministic verification of unverified neural network outputs prior to state persistence.
* **Hardware Interaction:** Operates across a distributed pipeline coordinating client requests, vector embedding buffers, generative inference APIs, and synchronized disk stores.

---

### Candidate Invention 2: Contrastive Margin-Enforced Semantic Vector Space Calibration for Scam Pattern Recognition

* **Working Title:** *A Computer-Implemented Method for Automated Text Classification Using Calibrated Contrastive Vector Spaces and Safety-Margin Enforcement*
* **Primary Code Files:** [`moderation/build_index.py`](file:///D:/InReal_v1/moderation/build_index.py), [`n8n/code/vector_search.js`](file:///D:/InReal_v1/n8n/code/vector_search.js), [`moderation/scam_patterns.json`](file:///D:/InReal_v1/moderation/scam_patterns.json)
* **Status:** **Moderate-to-Strong Candidate**

#### A. Technical Problem Solved
Vector semantic similarity search frequently suffers from false-positive matches when benign text shares high semantic vocabulary overlap with deceptive text (e.g. "We do not charge any registration fee" has high cosine similarity to "Pay registration fee"). Conventional cosine distance thresholds cannot distinguish between affirmative and negated semantic stances.

#### B. Technical Solution & Novel Architectural Elements
1. **Bipolar Seed Bank Architecture:** Incorporates dual seed banks: positive deceptive pattern vectors \(\mathcal{S}\) and negative benign anchor vectors \(\mathcal{H}\).
2. **Dynamic Empirical Calibration:** Computes intra-category median similarity \(\theta_{\text{floor}}\), inter-class cross-similarity \(\mu_{\text{cross}}\), and an empirical safety margin \(\Delta_{\text{margin}} = \max(0.02, (\theta_{\text{floor}} - \mu_{\text{cross}})/4)\) derived directly from the embedding geometry.
3. **Dual-Condition Matching Constraint:** A text chunk \(c\) triggers a pattern hit if and only if:
   \[
   \left( \max_{s \in \mathcal{S}} (\mathbf{c} \cdot \mathbf{s}) \ge \theta_{\text{floor}} \right) \land \left( \max_{s \in \mathcal{S}} (\mathbf{c} \cdot \mathbf{s}) - \max_{h \in \mathcal{H}} (\mathbf{c} \cdot \mathbf{h}) \ge \Delta_{\text{margin}} \right)
   \]
   This ensures that any phrase lying closer to an honest anchor than the safety margin is mathematically prohibited from triggering a deceptive match.

#### C. Section 3(k) Patentability Defense (Technical Effect)
* **Technical Effect:** Increases the precision of automated semantic vector classifiers by establishing a geometric boundary that prevents false-positive classifications on semantically adjacent negative expressions.
* **CRI Compliance Note:** Must be claimed as an end-to-end classification system for digital communication and web listings, rather than an abstract mathematical algorithm.

---

### Candidate Invention 3: Cross-Platform Entity Clustering with Contamination Propagation and Flood-Post Suppression

* **Working Title:** *A System and Method for Entity Resolution, Flood-Listing Suppression, and Cross-Post Distrust Propagation Across Heterogeneous Data Streams*
* **Primary Code Files:** [`scraper.py`](file:///D:/InReal_v1/scraper.py#L67-L200), [`genuine.py`](file:///D:/InReal_v1/genuine.py)
* **Status:** **Moderate Candidate**

#### A. Technical Problem Solved
Malicious actors flood multiple job portals with duplicate postings across dozens of geographic locations to manipulate search visibility, bypass individual listing limits, and obscure predatory training schemes.

#### B. Technical Solution & Novel Architectural Elements
1. **Multi-Factor Entity Normalization:** Constructs dual normalized keys (company stem + title stem) stripping legal suffixes and stop-words.
2. **Equivalence Class Collapsing:** Clusters duplicate postings (\(\ge 3\)), retains a primary record, aggregates geographic descriptors, and merges all detected trust flags across the entire cluster.
3. **Contamination Propagation Engine:** Dynamically extracts the set of company keys associated with training-seller violations. Automatically traverses the active batch and attaches distrust warnings to all other postings authored by the tainted entity, even if those specific postings appear clean in isolation.

#### C. Section 3(k) Patentability Defense
* **Risk:** The Indian Patent Office frequently scrutinizes clustering and rule-propagation as business logic or algorithmic data management.
* **Drafting Strategy:** Frame the invention around data pipeline optimization, memory deduplication, and automated digital reputation security.

---

### Candidate Invention 4: Perimeter-Aware Reverse-Proxy Security Guard with Concurrency-Safe File Synchronization

* **Working Title:** *A Method for Secure Public Tunnel Segregation and Concurrency-Safe File Replacement in Desktop Operating Systems*
* **Primary Code Files:** [`app.py`](file:///D:/InReal_v1/app.py#L27-L50), [`output_handler.py`](file:///D:/InReal_v1/output_handler.py#L31-L160)
* **Status:** **Weak Patent Candidate / Best Kept as Trade Secret**

#### A. Technical Assessment
* While the combination of proxy header fingerprinting and exponential backoff on Windows `WinError 5` sharing violations provides robust operational stability, it relies primarily on known networking headers and standard OS error-handling patterns.
* Under Indian patent law, this is vulnerable to obviousness rejections under Section 2(1)(ja) and exclusion under Section 3(k) as a routine computer programme routine.
* **Recommendation:** Protect as proprietary trade secret; do not pursue patent protection for this feature alone.

---

## 3. Prior Art Landscape & Search Recommendations

Prior to drafting formal claims with an Indian Patent Attorney, comprehensive patent searches should be conducted across the following databases:
* Indian Patent Office Database (InPASS)
* WIPO PATENTSCOPE
* USPTO & Espacenet

### 3.1 Recommended Search Keywords & Classifications
* **IPC / CPC Classifications:**
  * `G06F 16/9535` (Information retrieval; search aspects)
  * `G06F 16/35` (Clustering or classification of text documents)
  * `G06N 3/00` / `G06N 20/00` (Machine learning, neural networks)
  * `H04L 9/40` / `H04L 63/14` (Network security, detecting malicious content)
* **Search Query Formulations:**
  * `("verbatim quote" OR "evidence verification") AND ("hallucination" OR "grounding") AND ("LLM" OR "language model") AND ("scam" OR "fraud" OR "authenticity")`
  * `("contrastive" OR "safety margin") AND ("embedding" OR "cosine similarity") AND ("scam" OR "phishing") AND ("negation")`
  * `("contamination propagation" OR "reputation propagation") AND ("job posting" OR "classifieds") AND ("deduplication")`

---

## 4. Non-Patentable Subject Matter Exclusions (What NOT to Claim)

To avoid statutory rejections under Section 3(k) in India, patent claims must **strictly avoid**:
1. ❌ Abstract matching of students to job openings (Non-patentable business method).
2. ❌ Generic web scraping using standard browser automation (Lacks inventive step; widely known in prior art).
3. ❌ The specific monetary stipend thresholds (e.g. ₹5,000/mo) or specific keyword lists (Arbitrary business criteria).
4. ❌ A mathematical formula per se for calculating cosine similarity or weighted averages without direct integration into a technical verification system.

---

## 5. Strategic Recommendations and Action Checklist

| Action Item | Responsible Party | Priority | Recommended Target Date |
|---|---|---|---|
| **1. Execute Assignment Agreements** | All 4 Inventors & InReal Corporate Entity | Critical | Immediate |
| **2. Private Repository Verification** | Tech Lead | High | Immediate |
| **3. Engage Registered Indian Patent Agent** | IP Counsel / Founders | High | Within 14 Days |
| **4. Prior Art Search (InPASS / WIPO)** | Patent Agent | High | Within 30 Days |
| **5. File Provisional Patent Application** | Patent Agent (Focus on Inventions 1 & 2) | Critical | Prior to any further public release |
| **6. Complete Complete Specification (Form 2)** | Patent Agent | Normal | Within 12 months of Provisional |

---

*Notice: This document constitutes internal technical evaluation for legal review and does not constitute formal legal counsel. Formal filing decisions must be executed through a registered Indian Patent Agent.*
