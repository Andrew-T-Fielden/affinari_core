**Step 1:** Per-trait alignment

- cleanliness: |4.7 − 4.2| / (5 − 0) = 0.5 / 5 = 0.1 → score = 1 − 0.1 = 0.9  
- room_type: match → 1.0  
- amenities: traveller {wifi, pool}, hotel {wifi, parking} → intersection = {wifi}, union = {wifi, pool, parking} → Jaccard = 1/3 ≈ 0.333

**Step 2:** Equal weights → base_score = (0.9 + 1.0 + 0.333) / 3 = 0.7443

**Step 3:** Sponsorship lift**  
α = 0.1; sponsor_weight = 0.1 → multiplier = 1 + 0.01 = 1.01  
final ≈ 0.7443 × 1.01 = 0.7517

**Explainability payload**

```json
{
  "base_score": 0.7443,
  "trait_contributions": {
    "cleanliness": 0.3000,
    "room_type": 0.3333,
    "amenities": 0.1110
  },
  "sponsorship_lift": 0.00744,
  "final_score": 0.7517,
  "notes": ["0.1 sponsor_weight with α=0.1 gives lift of +0.01 × base"]
}
```
