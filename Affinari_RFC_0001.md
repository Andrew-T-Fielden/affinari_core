# Affinari RFC-0001 — Scalar Alignment and Doppelganger Similarity (v1.0)
**Status:** Draft Standard  
**Category:** Foundational Specification  
**Author:** Andrew Fielden <andrew@affinari.ai>  
**Issued:** October 2025  
**License:** CC-BY-4.0

---

## 1. Scope
This RFC specifies the **canonical calculations** for Affinari alignment scoring, filtering, tiebreaking, and doppelganger-based recommendation. It is normative for implementations claiming Affinari compatibility.

---

## 2. Data Model
A domain **schema** defines:
- Scalars \(S = \{s_i\}_{i=1}^n\), each \(s_i \in [0,1]\)  
- Tags \(T\) (binary set) with required/excluded subsets  
- Categoricals \(C = \{C_k\}\), each a multiselect set

A **context** supplies \(p \in [0,1]^n\) and weights \(w \in [0,\infty)^n\).  
A **candidate** supplies \(x \in [0,1]^n\) and metadata (tags, categoricals).

Missing scalar values are permitted; missing values are **ignored** in scoring (see §4.3).

---

## 3. Filtering
### 3.1 Tag Filter
A candidate passes iff: `required ⊆ tags(candidate)` and `excluded ∩ tags(candidate) = ∅`.

### 3.2 Categorical Filter
For each filtered categorical field \(C_k\), require:  
`selection(C_k) ∩ categories_k(candidate) ≠ ∅`.  
Alternate logic MUST be declared if used.

Candidates failing any filter are **removed** before scoring.

---

## 4. Alignment Calculation
### 4.1 Active Set
Define \(A_i = 1\) if \(w_i>0\) and the schema exposes \(s_i\); 0 otherwise.  
An implementation MAY also require \(x_i\) to be defined to set \(A_i=1\).

### 4.2 Weighted Manhattan Distance
\[
D(p,x) = \frac{\sum_i A_i\, w_i\, |p_i - x_i|}{\sum_i A_i\, w_i + \epsilon}, \quad \epsilon \approx 10^{-12}.
\]

### 4.3 Alignment Score
\[
S_{\text{align}} = 1 - D \in [0,1].
\]

### 4.4 Missing Scalars
If \(x_i\) is missing, exclude index \(i\) from **both** numerator and denominator.  
Schemas MAY define a set of **critical scalars** whose absence imposes penalty \(\delta \in [0,1]\) or exclusion.

### 4.5 Normalisation
Only the **sum of used weights** may appear in the denominator; global or hidden normalisers are **non-compliant**.

---

## 5. Tiebreaking
Implementations MUST provide deterministic ordering when \(S_{\text{align}}\) matches to tolerance \(\tau\) (default \(\tau=10^{-3}\)):  
1. Sort by configured secondary scalar(s) (desc/asc as declared).  
2. Then by stable key (e.g., `name` asc).

The tiebreak configuration MUST be exposed under `schema.engine.scoring.ties`.

---

## 6. Doppelganger Recommendation
### 6.1 Cosine Similarity
Given users \(u, v\) with feedback vectors \(r_u, r_v\) over shared items:
\[
\text{sim}(u,v) = \frac{r_u \cdot r_v}{\|r_u\|\;\|r_v\|}.
\]

### 6.2 Positivity Weight
Let \(P(u)\) be the mean of \(r_u\). Define:
\[
W(u,v) = \text{sim}(u,v) \times \frac{P(u)+P(v)}{2}.
\]

### 6.3 Item Score via Peers
For user \(u\) and item \(e\):
\[
\hat{r}_u(e) = \frac{\sum_{v \in \mathcal{D}(u)} W(u,v)\, r_v(e)}{\sum_{v \in \mathcal{D}(u)} W(u,v) + \epsilon}.
\]

### 6.4 Hybrid Score (Optional)
\[
S_{\text{hybrid}}(p,x;u) = \alpha S_{\text{align}}(p,x) + (1-\alpha)\hat{r}_u(e), \quad \alpha \in [0,1].
\]
Implementations MUST disclose \(\alpha\).

---

## 7. Compliance
A system is **Affinari-compatible** iff it:
1. Applies filtering (§3) before scoring.  
2. Computes \(S_{\text{align}}\) per (§4).  
3. Handles missing values per (§4.4).  
4. Exposes tiebreaks per (§5).  
5. Provides a trait-level breakdown for any score it displays.

---

## 8. Security & Privacy
Local-first computation is RECOMMENDED. No external model inference is required by this RFC.

---

## 9. Versioning
Future RFCs:  
- **RFC‑0002** Temporal Alignment (“Affinari Time”)  
- **RFC‑0003** Trust/Provenance Vectors  
- **RFC‑0004** Schema Interoperability

---

## 10. IPR and License
This RFC is © 2025 The Affinari Project, licensed CC‑BY‑4.0. “Affinari” is a trademark of The Affinari Project.
