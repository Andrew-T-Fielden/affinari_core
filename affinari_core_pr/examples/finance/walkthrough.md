Trait alignments:
- avg_monthly_expenditure: diff = 200 / 10000 = 0.02 → score = 0.98  
- credit_score: diff = 20 / (850 − 300 = 550) = 0.0364 → score ≈ 0.9636  
- payment_preference: match → 1.0

Base_score = (0.98 + 0.9636 + 1.0) / 3 = 0.9812

Sponsor lift: 0.05, α = 0.1 → multiplier = 1 + 0.005 = 1.005  
Final_score ≈ 0.9861

**Explainability**
```json
{
  "base_score": 0.9812,
  "trait_contributions": {
    "avg_monthly_expenditure": 0.3271,
    "credit_score": 0.3212,
    "payment_preference": 0.3333
  },
  "sponsorship_lift": 0.0049,
  "final_score": 0.9861,
  "notes": ["small sponsor lift applied"]
}
```
