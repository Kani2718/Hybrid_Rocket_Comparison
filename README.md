The Machine-learning used here in comparing the models and also for Hybrid Rocket Fuels firing data is of Supervised Learning. 
As it contains two types (Regression, Classification), The commonly used type for continuous Data values is Regression, in which we use it here.
The Classification is not used because it is only used when the data set is categorized into different data values or datasets.
The Python headers libraries are being listed in requirements files.

Now, We do the comparison on lab-scale Hybrid Rocket Fuels firing data for Paraffin and Beeswax-Based blends.  
Tree-based regressors (XGBoost, Random Forest, LightGBM) predict performance targets from DAQ and derived ballistic inputs.
MC Dropout, commomnly called as Monte-Carlo Dropout over a small neural net, is used only to estimate uncertainty on regression rate.

1) **Institution**: Global Academy of Technology (GAT), Department of Aeronautical Engineering  
2) **Project focus**: Compared experimental tests of biofuel-integrated hybrid fuels, with emphasis on regression rate and blend performance.

## Dataset
The previous dataset is found to be in LabVIEW `.lvm` exports.
Processed firing data are then stored as Excel:

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
| `Cf` | Target | Thrust Co-efficient (from force as applicable) |
| `cstar` | Target | Characteristic velocity |
| `specific density` | Target | Specific Density |


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
├── compare_models.ipynb
├── data/
    ├── LVM_data
        |── GAT_S1_04.11.2025.lvm
        |── GAT_S2_04.11.2025.lvm
        |── GAT_S3_12.11.2025.lvm
        └── GAT_S4_12.11.2025.lvm
    ├── LVM_to_XLSX.ipynb
    |── Pressure_vs_time.ipynb
    └── daq_raw_4blends.xlsx
├── models/
    ├── xgboost_model.ipynb
    ├── random_forest_model.ipynb
    └── lightgbm_model.ipynb
└── results/
    └── LightGBM
        |── Best_Fuel_Results_LGBM.xlsx
        |── Best_Fuel_Results_LGBM.txt
        |── Gox_vs_Predicted_Regression_Rate_LGBM.jpg
        └── Pressure_vs_Predicted_Regression_Rate_LGBM.jpg
    |── Model Comparison
        |── model_comparison.csv
        └── model_comparison.txt
    |── Random Forest
        |── Best_Fuel_Results_RF.xlsx
        |── Best_Fuel_Results_RF.txt
        |── Gox_vs_Predicted_Regression_Rate_RF.jpg
        └── Pressure_vs_Predicted_Regression_Rate_RF.jpg
    └── XGBoost
        |── Best_Fuel_Results.xlsx
        |── Best_Fuel_Results.txt
        |── Gox_vs_Predicted_Regression_Rate.jpg
        └── Pressure_vs_Predicted_Regression_Rate.jpg
```

## Related repository
For per-firing models with leave-one-out (LOO) validation, see:
https://github.com/Kani2718/hybrid-fuel-loo-models
