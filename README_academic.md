# The Affinari Method: A Transparent Framework for Contextual Alignment and Matching Across Domains
### Academic Reference Edition — Affinari Project (v1.0)

**Author:** Andrew Fielden  
**Affiliation:** The Affinari Project  
**Date:** October 2025  
**License:** CC-BY-4.0 International  
**DOI:** To be assigned (via Zenodo/GitHub release)

---

## Abstract
The **Affinari Method** defines a universal, transparent process for aligning entities—users, items, or conditions—by explicitly declaring their traits and computing similarity through interpretable, domain-agnostic metrics.  
Unlike inference-based AI, which learns latent weights from large corpora, Affinari operates through *declared alignment*, ensuring transparency, auditability, and adaptability across domains.

---

## Keywords
Alignment AI · Explainable Systems · Human-Centric Computing · Schema Design · Contextual Matching · Doppelganger Logic

---

## Summary of Contribution
Affinari introduces a domain-independent alignment grammar expressed as JSON schemas of scalars, tags, and categoricals.  
Matching is performed via deterministic functions (weighted Manhattan and cosine) yielding normalized scores with full trait-level explanations.  
This approach creates an **interpretable alternative to probabilistic inference models**, suitable for medicine, hospitality, finance, and education.

---

## Method Overview
1. Define schema (`scalars`, `tags`, `categoricals`)  
2. Declare entity traits  
3. Apply contextual weights  
4. Filter by inclusion/exclusion  
5. Compute match score  
6. Return ranked, explainable results

---

## Doppelganger Logic
Affinari extends alignment by comparing instance feedback patterns via cosine similarity × positivity weight, enabling group derived, explainable recommendations.

---

## Implementation References
- `Affinari_Core` — reference Streamlit application  
- `Affinari_Lite` — offline HTML/JS engine  
- `Affinari_RFC_0001.md` — formal technical standard  

---

## License
This work is released under **Creative Commons Attribution 4.0 International (CC-BY-4.0)**.  
Attribution required:  
> “Implements or extends the Affinari Method (Fielden, 2025).”

---

## Citation
Fielden, Andrew (2025). *The Affinari Method: A Transparent Framework for Contextual Alignment and Matching Across Domains.* Affinari Project, v1.0. DOI: pending.

---

## Relationship to Developer README
This academic edition is **formally written for citation and DOI purposes**, whereas [`README.md`](README.md) provides a **developer-friendly overview** for open-source users and implementers.  
Both describe the same method; their tone and purpose differ.
