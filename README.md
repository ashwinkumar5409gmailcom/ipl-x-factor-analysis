# 🏆 IPL Championship X-Factor Analysis

## 📌 Overview

This project analyzes historical IPL ball-by-ball data to identify the strongest statistical predictors of winning the IPL trophy.

While common narratives emphasize explosive powerplay batting, this study evaluates whether phase-wise performance metrics (Powerplay, Middle Overs, Death Overs) significantly influence championship probability.

The analysis is conducted using feature engineering and logistic regression modeling.

---

## 🎯 Research Question

What phase of the game most strongly predicts IPL championship success?

---

## 📊 Dataset

- Ball-by-ball dataset (deliveries)
- Match-level dataset (season winners)

Each team-season is labeled:
- `1` → Champion
- `0` → Non-Champion

---

## ⚙️ Feature Engineering

Phase-wise metrics were computed at the team-season level:

- **Powerplay Run Rate** (Overs 1–6)
- **Middle Overs Run Rate** (Overs 7–15)
- **Death Overs Economy** (Overs 16–20)

Example snippet:

```python
deliveries['total_runs'] = deliveries['batsman_runs'] + deliveries['extras']

# Powerplay (overs 0–5 since indexing starts from 0)
pp = deliveries[(deliveries['over'] >= 0) & (deliveries['over'] <= 5)]

pp_stats = (
    pp.groupby(['season','batting_team'])
      .agg(runs=('total_runs','sum'),
           balls=('ball','count'))
      .reset_index()
)

pp_stats['pp_rr'] = (pp_stats['runs'] / pp_stats['balls']) * 6
