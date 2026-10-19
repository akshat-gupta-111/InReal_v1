# InReal: Algorithmic Specifications and Mathematical Formulations

**Document Reference:** `INREAL-IP-AUD-004`  
**Classification:** Internal Confidential / IP Review  
**Target Legal Jurisdiction:** India (The Patents Act, 1970 — CRI Guidelines 2017)  
**Date of Specification:** October 15, 2026  

---

## 1. Overview

This document provides formal algorithmic descriptions, mathematical models, and pseudo-code implementations for the proprietary algorithms implemented in **InReal**. 

In compliance with the Indian Patent Office Guidelines for Examination of Computer Related Inventions (CRI, 2017), each algorithmic process is defined in terms of:
1. Technical input data structures.
2. Computational transformation steps.
3. Mathematical formulas and boundary constraints.
4. Technical output and tangible system state modifications.

---

## 2. Algorithm 1: Contrastive Vector Space Calibration & Semantic Scam Matching

### 2.1 Technical Purpose
Automated textual matching of fraudulent opportunities using embedding vectors without triggering false positives on negated phrases (e.g. "no registration fee required").

### 2.2 Mathematical Model
Let \( \mathcal{S} = \{s_1, s_2, \dots, s_m\} \) be a calibrated seed bank of deceptive patterns (scam phrases) and \( \mathcal{H} = \{h_1, h_2, \dots, h_k\} \) be a calibrated seed bank of honest/benign anchor phrases.

Each phrase is mapped to an \(L_2\)-normalized embedding vector in \(\mathbb{R}^d\) (where \(d = 768\)):
\[
\mathbf{v} = \frac{\mathbf{u}}{\|\mathbf{u}\|_2} = \frac{\mathbf{u}}{\sqrt{\sum_{i=1}^d u_i^2}}
\]
The cosine similarity between any two normalized vectors \(\mathbf{a}, \mathbf{b} \in \mathbb{R}^d\) reduces to their inner product:
\[
\text{sim}(\mathbf{a}, \mathbf{b}) = \mathbf{a} \cdot \mathbf{b} = \sum_{i=1}^d a_i b_i
\]

#### 2.2.1 Offline Geometry Calibration
The calibration parameters are derived empirically from the pairwise similarity distributions within the seed bank:
1. **Intra-Category Cohesion:**
   \[
   \mathcal{D}_{\text{same}} = \left\{ \text{sim}(\mathbf{s}_i, \mathbf{s}_j) \mid s_i, s_j \in \mathcal{S}, \, \text{category}(s_i) = \text{category}(s_j), \, i \ne j \right\}
   \]
   \[
   \theta_{\text{floor}} = \text{median}(\mathcal{D}_{\text{same}})
   \]
2. **Inter-Class Separation:**
   \[
   \mathcal{D}_{\text{cross}} = \left\{ \text{sim}(\mathbf{s}_i, \mathbf{h}_j) \mid s_i \in \mathcal{S}, \, h_j \in \mathcal{H} \right\}
   \]
   \[
   \mu_{\text{cross}} = \text{median}(\mathcal{D}_{\text{cross}})
   \]
3. **Safety Margin Determination:**
   \[
   \Delta_{\text{margin}} = \max\left(0.02, \, \frac{\theta_{\text{floor}} - \mu_{\text{cross}}}{4}\right)
   \]
4. **Strong Floor Determination:**
   \[
   \theta_{\text{strong}} = Q_3(\mathcal{D}_{\text{same}}) \quad (\text{75th percentile of intra-category similarities})
   \]

#### 2.2.2 Online Chunk Matching Rule
A candidate document chunk \(c\) with normalized embedding vector \(\mathbf{c}\) is evaluated against all \(\mathbf{s} \in \mathcal{S}\) and \(\mathbf{h} \in \mathcal{H}\):
\[
\hat{s} = \arg\max_{s_i \in \mathcal{S}} (\mathbf{c} \cdot \mathbf{s}_i), \quad S_{\max} = \mathbf{c} \cdot \hat{\mathbf{s}}
\]
\[
\hat{h} = \arg\max_{h_j \in \mathcal{H}} (\mathbf{c} \cdot \mathbf{h}_j), \quad H_{\max} = \mathbf{c} \cdot \hat{\mathbf{h}}
\]
\[
\text{Margin}(c) = S_{\max} - H_{\max}
\]

**Hit Condition:**
A chunk triggers a pattern hit if and only if:
\[
\text{IsHit}(c) \iff \left( S_{\max} \ge \theta_{\text{floor}} \right) \land \left( \text{Margin}(c) \ge \Delta_{\text{margin}} \right)
\]

**Strong Hit Condition:**
\[
\text{IsStrong}(c) \iff \left( S_{\max} \ge \theta_{\text{strong}} \right) \land \left( \text{Margin}(c) \ge 2 \cdot \Delta_{\text{margin}} \right)
\]

### 2.3 Category and Overall Vector Risk Aggregation
For each hit, the category strength is modulated by its predefined severity weight \(w_{\text{sev}} \in \{0.9, 0.6, 0.3\}\):
\[
\text{Strength}(\text{cat}) = w_{\text{sev}} \times \begin{cases} 1.0 & \text{if IsStrong}(c) \\ 0.6 & \text{otherwise} \end{cases}
\]
The cumulative vector risk \(V_{\text{vector}}\) combines all distinct category hits using independent probabilistic aggregation:
\[
V_{\text{vector}} = 1 - \prod_{\text{cat} \in \text{Hits}} \left( 1 - \text{Strength}(\text{cat}) \right)
\]

---

## 3. Algorithm 2: Evidence-Grounded Multi-Modal Fusion and Disagreement Resolution

### 3.1 Technical Purpose
Verifies the factual integrity of Large Language Model (LLM) assessments by eliminating hallucinations, discounting unverified claims, and fusing multi-modal evidence with automatic human escalation upon signal divergence.

### 3.2 Formal Algorithm Pseudo-Code

```python
def fuse_evidence(listing, stage0_signals, stage1_vector, llm_response, config):
    """
    Inputs:
        listing: dict containing raw text fields (role_title, company_name, description, stipend)
        stage0_signals: list of extracted contact/link signals with weights
        stage1_vector: dict containing vector_risk and category hits
        llm_response: parsed JSON containing scam_probability and list of evidence objects
        config: threshold configuration
    Outputs:
        verdict: dict containing status ('approved', 'review', 'blocked', 'retry'),
                 scam_risk (float [0.0, 1.0]), and human-readable audit reasons.
    """
    # Step 1: Normalize source text for verbatim string matching
    source_corpus = normalize_text(
        f"{listing.role_title} {listing.company_name} {listing.stipend} {listing.description}"
    )

    # Step 2: Verbatim Evidence Verification (Hallucination Suppression)
    verified_quotes = []
    rejected_quotes = []
    contradicting_quotes = []

    NEGATED_FEE_REGEX = r"\b(no|not|never|zero|without|free of|nil|dont|doesnt)\b[^.]{0,40}\b(fee|fees|charge|deposit|money)\b"

    for ev in llm_response.get("evidence", []):
        quote_norm = normalize_text(ev.get("quote", ""))
        
        # Must be substantive (>= 8 characters) and physically present in source text
        if len(quote_norm) >= 8 and quote_norm in source_corpus:
            if regex_search(NEGATED_FEE_REGEX, quote_norm):
                contradicting_quotes.append(ev)  # Argument against scam
            else:
                verified_quotes.append(ev)
        else:
            rejected_quotes.append(ev)  # Hallucinated quote

    # Step 3: Confidence Dampening for Ungrounded Claims
    p_raw = float(llm_response.get("scam_probability", 0.5))
    if p_raw > 0.5 and len(verified_quotes) == 0:
        # Penalize high scam assertion that lacks textual evidence
        p_adj = 0.5 + (p_raw - 0.5) * 0.25
        reasons.append("LLM asserted high risk without verifiable quotes; score discounted.")
    else:
        p_adj = p_raw

    # Step 4: Multi-Modal Score Fusion
    v_risk = float(stage1_vector.get("vector_risk", 0.0))
    s_risk = combine_independent_signals(stage0_signals)

    WEIGHT_LLM = 0.60
    WEIGHT_VECTOR = 0.25
    WEIGHT_SIGNALS = 0.15

    composite_risk = (WEIGHT_LLM * p_adj) + (WEIGHT_VECTOR * v_risk) + (WEIGHT_SIGNALS * s_risk)
    composite_risk = min(0.99, max(0.0, composite_risk))

    # Step 5: Divergence Detection (Disagreement Guard)
    signal_divergence = abs(p_adj - v_risk)
    if signal_divergence >= 0.60:
        status = "review"
        reasons.append(f"AI divergence detected (|LLM {p_adj:.2f} - Vector {v_risk:.2f}| >= 0.60); routed to review.")
    else:
        if composite_risk >= config.BLOCK_THRESHOLD (0.75):
            status = "blocked"
        elif composite_risk >= config.REVIEW_THRESHOLD (0.35):
            status = "review"
        else:
            status = "approved"

    return {
        "status": status,
        "scam_risk": round(composite_risk, 2),
        "reasons": reasons,
        "metrics": {
            "p_raw": p_raw,
            "p_adj": round(p_adj, 3),
            "v_risk": v_risk,
            "s_risk": s_risk,
            "verified_quotes": len(verified_quotes),
            "rejected_quotes": len(rejected_quotes)
        }
    }
```

---

## 4. Algorithm 3: Recruiter Telemetry Anomaly Detection

### 4.1 Technical Purpose
Identifies predatory employment postings and unselective mass-intake schemes using platform behavioral telemetry.

### 4.2 Formulation and Threshold Bounds
Let \( H \) be the total lifetime hires reported by the employer on the portal, \( P \) be the total job postings, and \( A_{\text{days}} \) be the account age in days:

1. **Mass-Hire Anomaly Trap:**
   \[
   \text{IsMassScheme}(H, P) \iff (H \ge 10,000) \lor \left( P > 0 \land \frac{H}{P} \ge 20 \right)
   \]
   *Rationale:* Selective internships do not hire tens of thousands of applicants per listing. When triggered, the listing is dropped immediately.

2. **Unverified New Employer Skew:**
   \[
   \text{IsSuspiciousNewAccount}(A_{\text{days}}, H) \iff (A_{\text{days}} < 90) \land (H = 0)
   \]

3. **High-Post Low-Conversion Spam:**
   \[
   \text{IsPostingSpam}(P, H) \iff (P \ge 200) \land \left( \frac{H}{P} < 0.05 \right)
   \]

4. **Multi-Town Dispersal Flood:**
   Let \( T = \{t_1, t_2, \dots, t_k\} \) be the set of distinct municipal locations listed on a single opportunity posting:
   \[
   \text{IsLocationSpam}(T) \iff |T| \ge 6
   \]

---

## 5. Algorithm 4: Entity Clustering, Copy Collapsing, and Contamination Propagation

### 5.1 Technical Purpose
Suppresses duplicate job floods across regional portals and automatically propagates distrust markers to all associated postings by fraudulent entities.

### 5.2 Mathematical Formulation

#### 5.2.1 Canonical Key Generation
For each opportunity listing \(i\), define normalization functions:
\[
\text{Key}_{\text{comp}}(i) = \text{StripLegalSuffixes}(\text{LowerCase}(\text{company}_i))
\]
\[
\text{Key}_{\text{title}}(i) = \text{FirstTwoTokens}(\text{RemoveStopwords}(\text{LowerCase}(\text{title}_i)))
\]
\[
\text{GroupKey}(i) = \left( \text{Key}_{\text{comp}}(i), \, \text{Key}_{\text{title}}(i) \right)
\]

#### 5.2.2 Copy Collapsing Procedure
Partition the active batch \(B\) into equivalence classes:
\[
\mathcal{G}_k = \{ i \in B \mid \text{GroupKey}(i) = k \}
\]
For each group \(\mathcal{G}_k\):
* If \( |\mathcal{G}_k| < 3 \): All listings remain distinct.
* If \( |\mathcal{G}_k| \ge 3 \):
  * Elect primary record \( i^* = \mathcal{G}_k[0] \).
  * Mark all subordinate records \( \{ i \in \mathcal{G}_k \mid i \ne i^* \} \) as `DROPPED` with reason:
    \[
    R_{\text{drop}} = \text{"Copy of another post by same company ("} + |\mathcal{G}_k| + \text{" copies)"}
    \]
  * Aggregate location descriptors:
    \[
    \text{location}(i^*) \leftarrow \text{location}(i^*) + \text{" (+"} + (|\mathcal{G}_k| - 1) + \text{" more)"}
    \]
  * Unify trust warnings across all copies:
    \[
    \text{Flags}(i^*) \leftarrow \bigcup_{j \in \mathcal{G}_k} \text{Flags}(j)
    \]

#### 5.2.3 Contamination Propagation
Let \(\mathcal{E}_{\text{tainted}}\) be the set of company keys associated with training-seller violations:
\[
\mathcal{E}_{\text{tainted}} = \left\{ \text{Key}_{\text{comp}}(i) \mid \text{TRAINING\_FLAG} \in \text{Flags}(i) \right\}
\]
For every clean listing \(m\) in the batch:
\[
\text{If } \text{Key}_{\text{comp}}(m) \in \mathcal{E}_{\text{tainted}} \land \text{TRAINING\_FLAG} \notin \text{Flags}(m):
\]
\[
\text{Flags}(m) \leftarrow \text{Flags}(m) \cup \{\text{"Another post by this employer sounds like a training programme"}\}
\]

---

## 6. Algorithm 5: Fair Round-Robin Queue and OS Lock Recovery

### 6.1 Technical Purpose
Ensures starvation-free batch claims across multiple ingestion sources and prevents operating-system-level file-lock crashes under concurrent Windows access.

### 6.2 Formal Specification

#### 6.2.1 Fair Round-Robin Selection
Let \( R \) be the set of unmoderated listings partitioned by source platform:
\[
\mathcal{Q}_s = [r_{s,1}, r_{s,2}, \dots, r_{s,n_s}] \quad \text{for } s \in \{\text{Internshala}, \text{Naukri}, \text{Unstop}, \dots\}
\]
To construct a batch of size \(L\):
```python
def round_robin_claim(queues, limit):
    batch = []
    platform_keys = list(queues.keys())
    i = 0
    while len(batch) < limit and any(queues.values()):
        k = platform_keys[i % len(platform_keys)]
        if queues[k]:
            batch.append(queues[k].pop(0))
        i += 1
    return batch
```
*Complexity:* \( O(L) \) time complexity, guaranteeing that no single portal monopolizes moderation resources.

#### 6.2.2 Stale Claim Expiration
Any listing claimed by a worker with status `"checking"` whose claim timestamp exceeds \( T_{\text{timeout}} = 4 \) minutes is automatically reset:
\[
\text{Status}(r) \leftarrow \text{"pending"} \quad \text{if } (t_{\text{current}} - t_{\text{claimed}}) > 240 \text{ seconds}
\]

#### 6.2.3 Windows OS Lock Backoff
To write `internships.csv` atomically across threads without OS sharing violations:
```python
def atomic_replace_with_backoff(temp_path, target_path, max_attempts=20):
    for attempt in range(max_attempts):
        try:
            os.replace(temp_path, target_path)
            return True
        except PermissionError:
            if attempt == max_attempts - 1:
                raise
            sleep_duration = 0.1 * (attempt + 1)
            time.sleep(sleep_duration)
    return False
```
*Guarantees:* Total wait capacity \( \sum_{i=1}^{20} 0.1 \times i = 21.0 \) seconds of non-blocking retry before failure, exceeding standard Windows desktop file handle release windows.
