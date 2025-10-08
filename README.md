# The Affinari Method v1.0
### Transparent Alignment Framework for Human-Aligned AI

**Author:** Andrew Fielden  
**Date:** October 2025  
**License:** CC-BY-4.0 International  

---

## 🔍 What Is the Affinari Method?

The **Affinari Method** is a lightweight, transparent framework for matching any two sets of things — people, options, or conditions — based on explicitly declared traits.  
It replaces opaque AI inference with **declared alignment**: schemas that show exactly *why* something fits.

Affinari powers multiple domains — hospitality, medicine, finance, education — all using the same clear logic.

---

## 🧩 Core Idea
> Intelligence isn’t guessing — it’s alignment.

Each entity (user, item, context) declares traits under a simple JSON schema:
- **Scalars** → numeric values (0–1)  
- **Tags** → binary includes/excludes  
- **Categoricals** → multi-select descriptors  

Affinari compares them using **weighted Manhattan** or **cosine** similarity, producing deterministic and explainable “fit” scores.

---

## 🧠 Key Features
- 🔍 Transparent scoring — no hidden weights  
- 🔒 Local-first and privacy-safe  
- 🧾 Human-readable JSON schemas  
- 🤝 Cross-domain universality  
- 🧭 Explainable outcomes (“why this fits you”)

---

## 📄 Contents of This Repository

| File | Description |
|------|--------------|
| `Affinari_Method_v1.0.md` | Public reference document |
| `Affinari_RFC_0001.md` | Formal technical specification (RFC) |
| `README_academic.md` | Academic version for citation |
| `LICENSE.txt` | CC-BY-4.0 license |
| `metadata.json` | Version and citation metadata |

---

## 🧰 Reference Implementations
- [Affinari Core](https://github.com/affinari/affinari_core) – full Streamlit engine  
- [Affinari Lite](https://github.com/affinari/affinari_lite) – static HTML/JS offline engine  

---

## 🧾 Citation
If you use or extend this work, please cite:

> Fielden, A. (2025). *The Affinari Method: A Transparent Framework for Contextual Alignment and Matching Across Domains.*  
> Affinari Project, v1.0. DOI: (to be added)

---

## 🧮 License
This project is released under the [Creative Commons Attribution 4.0 International License](LICENSE.txt).

---

## 🎓 Academic Version
For a formal, citable research version with abstract, keywords, and DOI metadata, see [`README_academic.md`](README_academic.md).
