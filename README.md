Machine-learning comparison on lab-scale Hybrid Rocket Fuels firing data for Paraffin and Beeswax-Based blends.  
Tree-based regressors (XGBoost, Random Forest, LightGBM) predict performance targets from DAQ and derived ballistic inputs.  
MC Dropout, commomnly called as Monte-Carlo Dropout over a small neural net, is used only to estimate uncertainty on regression rate.

1) Institution: Global Academy of Technology (GAT), Department of Aeronautical Engineering  
2) Project focus: Compared experimental tests of biofuel-integrated hybrid fuels, with emphasis on regression rate and blend performance.

## Dataset

Processed firing data (from LabVIEW `.lvm` exports) are stored as Excel:

1) **File:** `data/daq_raw_4blends.xlsx`
2) **Sheets:** one sheet per fuel blend (4 blends)

| Column | Role | Meaning |
|--------|------|--------|
| `X_Value` | Input | Time (s) from DAQ |
| `Force` | Input | Load-cell force |
| `Pressure` / `Pressure_0` | Input / target* | Chamber-related pressure channels |
| `Gox` | Input | Oxidizer mass flux (kg/m²·s), burn-average |
| `rdot_mm_s` | Target | Average regression rate (mm/s) |
| `Isp` | Target | Specific impulse |
| `Thrust` | Target | Thrust (from force as applicable) |
| `cstar` | Target | Characteristic velocity |

---

## Models compared

| Model | Role |
|--------|------|
| **XGBoost** | Gradient-boosted trees (baseline) |
| **Random Forest** | Bagged decision trees |
| **LightGBM** | Gradient-boosted trees (leaf-wise) |

All models use the **same** train/test split, features, and metrics for a fair comparison.

**Uncertainty:** MC Dropout on a small MLP for regression-rate uncertainty only (not a fourth competing tree model).

---

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── data/
│   └── daq_raw_4blends.xlsx
├── models/
│   ├── xgboost_model.py
│   ├── random_forest_model.py
│   └── lightgbm_model.py
├── compare_models.py
└── results/
    └── model_comparison.csv
