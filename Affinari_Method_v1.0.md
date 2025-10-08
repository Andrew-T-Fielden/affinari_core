# Affinari Method v1.0 — Public Reference
**Author:** Andrew Fielden  
**Project:** The Affinari Project  
**Date:** October 2025  
**License:** CC-BY-4.0

---

## 1. Purpose
This document defines the **calculation procedure** used by Affinari to produce an alignment (match) score between a *context* (often a user + goal) and a *candidate* (option, intervention, venue, etc.). The method is intentionally simple, transparent, and domain-agnostic.

---

## 2. Notation
- Let the domain schema expose **n scalar traits** indexed by *i = 1..n* on [0,1].  
- Context (preference) vector: **p** ∈ [0,1]^n  
- Candidate (instance) vector: **x** ∈ [0,1]^n  
- Non‑negative weights: **w** ∈ [0,∞)^n (user/context importance per trait)  
- Active mask **A**: A_i = 1 if the trait i is **usable** (trait exists in schema, weight w_i>0, and both p_i and/or x_i are defined); else 0.

> Missing values are **ignored** (not imputed). Normalisation always uses the **sum of used weights**.

---

## 3. Filters (pre-score gating)
1. **Tags (required/excluded).** The candidate must contain all required tags and none of the excluded tags.  
2. **Categoricals (multiselect).** Candidate must intersect at least one selected category in each filtered categorical set (unless schema specifies different logic).  

Candidates failing filters are **excluded before scoring**.

---

## 4. Scalar Alignment Score (default)
### 4.1 Weighted Manhattan Distance
\[
D(p,x) = \frac{\sum_{i=1}^{n} A_i \, w_i \, \lvert p_i - x_i \rvert}{\sum_{i=1}^{n} A_i \, w_i + \epsilon}
\]
- \(\epsilon\) is a tiny constant (e.g., 1e-12) to avoid division by zero when no scalars are active.  
- If no scalars are active after filtering, the schema MUST define a fallback (e.g., treat score as 0.5 or drop the item).

### 4.2 Alignment Score
\[
S_{\text{align}}(p,x) = 1 - D(p,x) \in [0,1].
\]

### 4.3 Missing Values
If \(x_i\) is undefined for a trait with \(w_i>0\), that trait is **omitted** from both numerator and denominator. (Optionally, schemas may define **critical traits** that, if missing, impose a fixed penalty \(\delta\) or exclude the candidate.)

### 4.4 Optional Trait Throttling (advanced)
If a schema enables **preference throttling**, compare on the **difference** scaled by weight; there is **no multiplication of a slider and a weight** into a hidden factor. All scaling is explicit in the formula above.

---

## 5. Tie-Breaks (deterministic ordering)
When two candidates have identical \(S_{\text{align}}\) (to 3 decimal places by default), order by:
1. Schema-defined secondary scalar(s) (e.g., `value_for_money` descending),  
2. Deterministic tiebreak key (e.g., `name` ascending).

Tiebreak policy must be declared in the schema `scoring.ties` field.

---

## 6. Doppelganger Logic (peer similarity)
### 6.1 Peer Similarity (Cosine)
Given two users \(u, v\) with **feedback vectors** over a shared set of entities \(E_{uv}\):  
Let \(r_u, r_v \in [0,1]^{|E_{uv|}}\) be their normalised feedback (e.g., overall sentiment). Then
\[
\text{sim}(u,v) = \frac{r_u \cdot r_v}{\|r_u\|\;\|r_v\|} \in [0,1].
\]

### 6.2 Positivity Modulation
Let \(P(u)\) be user \(u\)'s mean positivity in \([0,1]\). Define a **peer weight**:
\[
W(u,v) = \text{sim}(u,v)\; \times \; \frac{P(u)+P(v)}{2}.
\]

### 6.3 Recommendation via Doppelgangers
To estimate user \(u\)'s affinity for a candidate entity \(e\), aggregate peers \(v \in \mathcal{D}(u)\) who rated \(e\):
\[
\hat{r}_u(e) = \frac{\sum_{v \in \mathcal{D}(u)} W(u,v)\, r_v(e)}{\sum_{v \in \mathcal{D}(u)} W(u,v) + \epsilon}.
\]

A **hybrid score** can combine explicit alignment and peer signal:
\[
S_{\text{hybrid}}(p,x;u) = \alpha \, S_{\text{align}}(p,x) + (1-\alpha)\, \hat{r}_u(e),
\]
with \(\alpha \in [0,1]\) declared by the schema or UI.

---

## 7. Worked Example
**Schema scalars:** `quiet`, `local_flavour`, `value_for_money` (all in [0,1])  
**Context (p):** (0.9, 0.8, 0.4)  
**Weights (w):** (0.5, 0.3, 0.2)  
**Candidate A (x):** (0.7, 0.9, 0.6)  

Step-by-step:
- Absolute diffs: |p-x| = (0.2, 0.1, 0.2)  
- Weight × diff: (0.5*0.2, 0.3*0.1, 0.2*0.2) = (0.10, 0.03, 0.04)  
- Numerator sum = 0.17  
- Denominator sum of used weights = 1.0  
- Distance D = 0.17/1.0 = 0.17  
- Alignment score S_align = 1 - 0.17 = 0.83

**Interpretation:** A aligns strongly (0.83), driven by high `quiet` and `local_flavour`, with a small penalty for `value_for_money` mismatch.

---

## 8. Compliance Requirements
An implementation claiming "Affinari-compatible" MUST:
1. Apply tag/categorical filters before scoring.  
2. Compute S_align exactly as defined (or document any variant).  
3. Normalise by the **sum of used weights** only.  
4. Disclose tiebreak rules and any penalties for missing critical traits.  
5. Provide trait-level contribution breakdown for every score.  

---

## 9. Attribution
Please cite:  
**Fielden, A. (2025). _The Affinari Method v1.0._ The Affinari Project.**

Licensed under CC-BY-4.0.
