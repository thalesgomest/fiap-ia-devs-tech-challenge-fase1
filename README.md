<div align="center">

# 🩺 Classificação de Risco de Câncer Cervical

**Pipeline completo de Machine Learning aplicado à saúde da mulher** — da análise exploratória à discussão crítica sobre uso clínico real.

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![scikit--learn](https://img.shields.io/badge/scikit--learn-Pipeline-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![SHAP](https://img.shields.io/badge/Explicabilidade-SHAP-8A2BE2)](https://shap.readthedocs.io/)
[![Dataset](https://img.shields.io/badge/Dataset-UCI%20Machine%20Learning%20Repository-0f2a3f)](https://archive.ics.uci.edu/dataset/383/cervical+cancer+risk+factors)

**Tech Challenge — Fase 1 · Pós-graduação em IA (FIAP)**

📄 [Relatório Técnico Completo](reports/relatorio_tecnico.pdf) · 🔗 [Repositório](https://github.com/thalesgomest/fiap-ia-devs-tech-challenge-fase1)

</div>

---

## 👥 Equipe

| Nome | RM |
|---|---|
| Daniel Henrique Soter de Souza | rm376376 |
| Otavio Braga Goulart | rm376980 |
| Thales Gomes Targino | rm376365 |

---

## 📌 Sobre o Projeto

Este projeto constrói e avalia, de forma crítica, um pipeline de classificação de
risco de biópsia positiva (`Biopsy`) para câncer cervical, com base no dataset
[Cervical Cancer (Risk Factors)](https://archive.ics.uci.edu/dataset/383/cervical+cancer+risk+factors)
do UCI Machine Learning Repository (858 pacientes, 6,4% de casos positivos).

O trabalho cobre o ciclo completo de um projeto de ML aplicado — **análise
exploratória → pré-processamento → modelagem → calibração de limiar de decisão →
interpretabilidade** — encerrando com uma discussão honesta sobre os limites do
modelo e sua real viabilidade de uso em um cenário clínico.

> **⚠️ Conclusão central do projeto:** o modelo final **não está pronto para uso
> clínico real**. No estado atual, ele poderia no máximo apoiar a priorização de
> casos para exame, sempre sob supervisão médica — nunca substituir o diagnóstico.
> Essa conclusão, e as justificativas por trás dela, estão detalhadas no
> [relatório técnico](reports/relatorio_tecnico.pdf) e no notebook
> `05_interpretability.ipynb`.

### Resultados em resumo

| Etapa | Resultado |
|---|---|
| Modelo final | Random Forest (`n_estimators=400`, selecionado por AUC-PR em validação cruzada) |
| Desempenho no teste (limiar padrão 0,5) | Recall de apenas 9% (1 de 11 casos positivos) |
| Após calibração de limiar (0,10) | Recall de 82% (9 de 11 casos), com aumento de falsos positivos |
| Variáveis mais explicativas | Nº de gestações, idade na primeira relação sexual, nº de parceiros, uso de contraceptivo hormonal, idade |
| Concordância Feature Importance × SHAP | Correlação de Spearman de 0,985 |

Detalhamento completo de cada etapa, métricas e limitações: **[relatório técnico em PDF](reports/relatorio_tecnico.pdf)**.

---

## 🗂️ Estrutura de Pastas

```
tech-challenge-1/
├── data/
│   ├── raw/                  # Dataset original (CSV baixado do UCI)
│   └── processed/            # Dados de treino/teste já tratados (gerados pelo notebook 02)
├── notebooks/                 # As 5 etapas do projeto, em ordem de execução
│   ├── 01_eda.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_modeling.ipynb
│   ├── 04_threshold_calibration.ipynb
│   └── 05_interpretability.ipynb
├── models/                     # Modelo final, transformadores e configuração de limiar
│                                # (gerados pelos notebooks 02, 03 e 04)
├── reports/
│   ├── figures/                # Gráficos selecionados para o relatório técnico
│   └── relatorio_tecnico.pdf   # Relatório técnico final (Fase 1)
├── docs/                       # Documentação complementar do projeto
├── requirements.txt
└── README.md
```

---

## 🚀 Como Executar

### Ambiente

Os notebooks foram desenvolvidos para rodar no **Google Colab**, mas também rodam
localmente com Jupyter/VS Code, usando Python 3.9+.

### Dependências

Instale as dependências listadas em `requirements.txt`:

```bash
pip install -r requirements.txt
```

`shap` e `imbalanced-learn` são as únicas bibliotecas que não vêm pré-instaladas no
Google Colab — os notebooks que precisam delas (`03_modeling.ipynb` e
`05_interpretability.ipynb`) já incluem uma célula que instala essas dependências
automaticamente (`%pip install -q ...`) caso não estejam disponíveis.

### ⚠️ Convenção de Caminhos (importante)

Os notebooks estão em `/notebooks/`, **um nível abaixo da raiz do projeto**, e
assumem que são executados a partir dessa pasta. Por isso, toda referência a arquivos
de outras pastas usa o prefixo `../` (ex.: `../data/raw/...`, `../data/processed/...`,
`../models/...`). Isso vale tanto no Google Colab (após clonar o repositório e abrir
o notebook a partir de `/notebooks/`) quanto localmente — se você abrir o Jupyter a
partir da raiz do projeto, é preciso navegar até `/notebooks/` antes de rodar as
células, ou ajustar o diretório de trabalho do kernel para essa pasta.

### Ordem de execução

Os notebooks devem ser executados **em sequência**, pois cada um consome os
artefatos gerados pelo anterior (dados processados, modelo treinado, limiar
calibrado):

| # | Notebook | O que faz | Gera |
|---|---|---|---|
| 1 | `01_eda.ipynb` | Análise exploratória do dataset bruto | — (não gera arquivos) |
| 2 | `02_preprocessing.ipynb` | Limpeza, tratamento de nulos, remoção de leakage, padronização, split treino/teste | `data/processed/*.csv`, `models/imputer_*.joblib`, `models/scaler_numerico.joblib`, `models/preprocessing_metadata.json` |
| 3 | `03_modeling.ipynb` | Treino e comparação de classificadores, seleção e ajuste do modelo final | `models/modelo_final_random_forest.joblib`, `models/modeling_metadata.json` |
| 4 | `04_threshold_calibration.ipynb` | Calibração do limiar de decisão (sem retreinar) | `models/threshold_config.json` |
| 5 | `05_interpretability.ipynb` | Feature importance, SHAP e discussão crítica (sem retreinar) | — (análise final) |

### Dataset original

O arquivo já está incluído em `data/raw/kag_risk_factors_cervical_cancer.csv`. Caso
precise baixá-lo novamente, ele está disponível em:

- UCI Machine Learning Repository: <https://archive.ics.uci.edu/dataset/383/cervical+cancer+risk+factors>
- Também disponível no Kaggle, sob o nome "Cervical Cancer Risk Classification"

---

## 📄 Relatório Técnico

O relatório técnico completo desta fase — introdução, síntese de cada etapa,
métricas consolidadas, discussão crítica sobre uso prático e limitações — está em
[`reports/relatorio_tecnico.pdf`](reports/relatorio_tecnico.pdf).

## 🔗 Repositório

<https://github.com/thalesgomest/fiap-ia-devs-tech-challenge-fase1>
