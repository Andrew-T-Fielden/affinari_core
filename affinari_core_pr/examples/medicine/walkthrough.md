Compute alignment:
- symptom_severity: diff = 0.5 / 10 → score = 0.95  
- preferred_treatment: match → 1.0  
- allergies: intersection ∅, union size 2 → Jaccard = 0

Base_score = (0.95 + 1.0 + 0) / 3 = 0.65

No sponsorship → final_score = 0.65

**Explainability**
```json
{
  "base_score": 0.65,
  "trait_contributions": {
    "symptom_severity": 0.3167,
    "preferred_treatment": 0.3333,
    "allergies": 0.0
  },
  "sponsorship_lift": 0.0,
  "final_score": 0.65,
  "notes": ["no sponsor_weight present"]
}
```
