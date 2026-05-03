# Forecasting de Parâmetros de Poço — Pressão e Temperatura

Modelos de previsão de séries temporais de pressão e temperatura a partir de sensores de árvore de natal de poços offshore, desenvolvidos no âmbito do projeto **Vida Útil Remanescente (RUL)** do **LCCV/UFAL** em parceria com a **Petrobras**.

Os modelos foram avaliados em dados com reamostagem diária de 6 poços offshore: LL-11, LL-22, SPH-2, SPH-6, SPH-8, SPS-77.

> 📄 Artigo relacionado: **SPE-231653-MS** — apresentado no LACPEC 2026 (Rio de Janeiro, Brasil)

---

## Estrutura do repositório
well-forecasting/
│
├── notebooks/
│   ├── 01_pressure_linear_wave_lstm.ipynb        # Regressão Linear, Wavelet e LSTM
│   ├── 02_pressure_prophet_hybrid_catboost.ipynb  # Prophet, Híbrido Prophet+LightGBM e CatBoost
│   ├── 03_pressure_model_comparison.ipynb         # Comparação geral dos modelos de pressão
│   └── 04_temperature_all_models.ipynb            # Todos os modelos aplicados à temperatura (TPT-T)
│
└── README.md

---

## Modelos implementados

| Modelo | Descrição |
|---|---|
| **Regressão Linear** | Baseline usando índice de tempo como feature |
| **Wavelet (Wave)** | Suavização Savitzky-Golay + extrapolação linear |
| **LSTM** | Rede neural recorrente para previsão de sequências |
| **Prophet** | Modelo aditivo do Facebook com sazonalidade |
| **Híbrido Prophet+LightGBM** | Tendência/sazonalidade do Prophet + LightGBM nos resíduos |
| **CatBoost** | Gradient boosting com features de tempo e lags |

---

## Variáveis-alvo

| Variável | Descrição | Notebooks |
|---|---|---|
| `TPT-P` | Pressão de fundo (psi) | `01`, `02`, `03` |
| `TPT-T` | Temperatura de fundo (°C) | `04` |

---

## Como usar

Todos os notebooks rodam no **Google Colab**, sem instalação local.

**Entrada necessária:** arquivos CSV com pelo menos duas colunas:
- `datetime` — timestamp
- `TPT-P` ou `TPT-T` — variável-alvo

Faça o upload dos CSVs para `/content/` no Colab antes de executar.

As dependências são instaladas automaticamente no início de cada notebook (`prophet`, `lightgbm`, `catboost`, `PyWavelets`, `tensorflow`, `scikit-learn`).

---

## Parâmetros de configuração

No topo de cada notebook há uma seção de configurações:

```python
DATA_PATH      = "/content"   # pasta com os CSVs
PARAMETRO_ALVO = "TPT-P"      # coluna alvo
TRAIN_SPLIT    = 0.9          # 90% treino / 10% teste
TEST_MIN_STEPS = 200          # mínimo de pontos no teste
RESAMPLE       = True
FREQUENCIA     = "D"          # reamostagem diária
```

---

## Resultados (Pressão — SPE-231653-MS)

Médias das métricas nos 6 poços:

| Modelo | MAE | MAPE (%) |
|---|---|---|
| Linear | 36,2 | 16,48 |
| Wave | 36,85 | 16,87 |
| Prophet | 42,97 | 19,61 |
| CatBoost | 30,58 | 12,82 |
| **Híbrido Prophet+LightGBM** | **25,62** | **9,56** |

---

## Próximos passos (sugestões)

- Testar possíveis novos modelos de previsão visando a melhora dos parâmetros e métricas, e melhorar filtragem de dados dos poços
- Modelagem multivariada (pressão + temperatura em conjunto)
- Integrar as previsões com modelos de corrosão (ex: NORSOK M-506) para calcular o RUL via fator de segurança ao longo do tempo
- Quantificação de incerteza e arquiteturas de deep learning (Informer, Transformers)

---

## Autores

Lucas Veras de Siqueira com auxílio de Lucas Gouveia Omena Lopes

Desenvolvido no **LCCV/UFAL** (Laboratório de Computação Científica e Visualização, Universidade Federal de Alagoas) em colaboração com a **Petrobras**.
