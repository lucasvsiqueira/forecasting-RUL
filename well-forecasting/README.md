# Well Parameter Forecasting — Pressure & Temperature

Forecasting models for pressure and temperature time series from offshore well sensors (Christmas tree), developed as part of the **Remaining Useful Life (RUL)** research project at **LCCV/UFAL** in partnership with **Petrobras**.

The models were benchmarked on daily-resampled data from 6 offshore wells: LL-11, LL-22, SPH-2, SPH-6, SPH-8, SPS-77.

> 📄 Related paper: **SPE-231653-MS** — presented at LACPEC 2026 (Rio de Janeiro, Brazil)

---

## 📁 Repository Structure

```
well-forecasting/
│
├── notebooks/
│   ├── 01_pressure_linear_wave_lstm.ipynb       # Linear Regression, Wavelet & LSTM
│   ├── 02_pressure_prophet_hybrid_catboost.ipynb # Prophet, Hybrid Prophet+LightGBM & CatBoost
│   ├── 03_pressure_model_comparison.ipynb        # Full comparison across all pressure models
│   └── 04_temperature_all_models.ipynb           # All 5 models applied to temperature (TPT-T)
│
└── README.md
```

---

## 🧠 Models Implemented

| Model | Description |
|---|---|
| **Linear Regression** | Baseline model using time index as feature |
| **Wavelet (Wave)** | Savitzky-Golay smoothing + linear extrapolation |
| **LSTM** | Recurrent neural network for sequence prediction |
| **Prophet** | Facebook's additive time series model with seasonality |
| **Hybrid Prophet+LightGBM** | Prophet trend/seasonality + LightGBM on residuals |
| **CatBoost** | Gradient boosting with time and lag features |

---

## 📊 Target Variables

| Variable | Description | Notebooks |
|---|---|---|
| `TPT-P` | Downhole pressure (psi) | `01`, `02`, `03` |
| `TPT-T` | Downhole temperature (°C) | `04` |

---

## ⚙️ Setup

All notebooks are designed to run on **Google Colab**. No local installation needed.

**Required input:** CSV files with at least two columns:
- `datetime` — timestamp
- `TPT-P` or `TPT-T` — target parameter

Upload your CSVs to `/content/` in Colab before running.

**Key dependencies** (installed automatically in each notebook):
```
prophet==1.1.5
lightgbm
catboost
PyWavelets
tensorflow
scikit-learn
pandas / numpy / matplotlib
```

---

## 🔑 Key Configuration Parameters

Each notebook has a `CONFIGS` section at the top where you can adjust:

```python
DATA_PATH      = "/content"   # folder with CSV files
PARAMETRO_ALVO = "TPT-P"      # target column
TRAIN_SPLIT    = 0.9          # 90% train / 10% test
TEST_MIN_STEPS = 200          # minimum test points
RESAMPLE       = True
FREQUENCIA     = "D"          # daily resampling
```

---

## 📈 Results (Pressure — SPE-231653-MS)

Average metrics across 6 wells:

| Model | MAE | MAPE (%) |
|---|---|---|
| Linear | — | — |
| Wave | — | — |
| Prophet | — | — |
| CatBoost | — | — |
| **Hybrid Prophet+LightGBM** | **25.62** | **9.56** |

> ✅ Best performer: Hybrid Prophet+LightGBM

---

## 🔭 Next Steps

- Integrate forecasts with corrosion models (e.g. NORSOK M-506) to compute actual **RUL** via time-dependent safety factors
- Extend to **multivariate modeling** (pressure + temperature + flow rate jointly)
- Explore **uncertainty quantification** and deep learning architectures (Informer, Transformers)

---

## 👥 Authors

Developed at **LCCV/UFAL** (Laboratory of Scientific Computing and Visualization, Federal University of Alagoas) in collaboration with **Petrobras**.
