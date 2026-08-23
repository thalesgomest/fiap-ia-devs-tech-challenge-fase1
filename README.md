# Classificação de Risco de Câncer Cervical

Projeto de Machine Learning para classificação de risco de câncer cervical a partir
de fatores de risco clínicos e comportamentais, desenvolvido como parte da Fase 1 do
Tech Challenge (Pós-graduação em IA).

## Objetivo

Construir e avaliar um pipeline de classificação capaz de sinalizar pacientes com
maior risco de biópsia positiva (`Biopsy`), com base no dataset
[Cervical Cancer (Risk Factors)](https://archive.ics.uci.edu/dataset/383/cervical+cancer+risk+factors)
do UCI Machine Learning Repository. O projeto cobre todo o ciclo de um projeto de ML
aplicado — análise exploratória, pré-processamento, modelagem, calibração de limiar
de decisão e interpretabilidade — com discussão crítica sobre os limites do modelo e
sua real viabilidade de uso prático (ver `reports/relatorio_tecnico.pdf` para a
discussão completa).

**Importante:** este é um projeto acadêmico/exploratório. O modelo final **não está
pronto para uso clínico real** — essa conclusão, e suas justificativas, estão
detalhadas no relatório técnico e no notebook `05_interpretability.ipynb`.

## Estrutura de Pastas

```
tech-challenge-1/
├── data/
│   ├── raw/              # Dataset original (CSV baixado do UCI)
│   └── processed/        # Dados de treino/teste já tratados (gerados pelo notebook 02)
├── notebooks/             # As 5 etapas do projeto, em ordem de execução
│   ├── 01_eda.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_modeling.ipynb
│   ├── 04_threshold_calibration.ipynb
│   └── 05_interpretability.ipynb
├── models/                 # Modelo final, transformadores e configuração de limiar
│                            # (gerados pelos notebooks 02, 03 e 04)
├── reports/
│   ├── figures/            # Gráficos selecionados para o relatório técnico
│   └── relatorio_tecnico.pdf  # Relatório técnico final (Fase 1)
├── docs/                   # Documentação complementar do projeto
├── requirements.txt
└── README.md
```

## Como Executar

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

### Convenção de Caminhos (importante)

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

1. **`01_eda.ipynb`** — carrega `../data/raw/kag_risk_factors_cervical_cancer.csv` e
   produz a análise exploratória. Não gera arquivos de saída.
2. **`02_preprocessing.ipynb`** — lê o CSV bruto, aplica as transformações definidas
   na EDA e salva `../data/processed/cervical_cancer_{train,test}_processed.csv`,
   além dos transformadores (`../models/imputer_*.joblib`,
   `../models/scaler_numerico.joblib`) e `../models/preprocessing_metadata.json`.
3. **`03_modeling.ipynb`** — lê os dados processados, treina e compara os
   classificadores, e salva o modelo final em
   `../models/modelo_final_random_forest.joblib` e `../models/modeling_metadata.json`.
4. **`04_threshold_calibration.ipynb`** — carrega o modelo final (sem retreinar),
   calibra o limiar de decisão e salva `../models/threshold_config.json`.
5. **`05_interpretability.ipynb`** — carrega o modelo final e o limiar calibrado
   (sem retreinar), e produz a análise de interpretabilidade (feature importance,
   SHAP) e a discussão crítica final.

### Dataset original

O arquivo já está incluído em `data/raw/kag_risk_factors_cervical_cancer.csv`. Caso
precise baixá-lo novamente, ele está disponível em:

- UCI Machine Learning Repository: <https://archive.ics.uci.edu/dataset/383/cervical+cancer+risk+factors>
- Também disponível no Kaggle, sob o nome "Cervical Cancer Risk Classification"

## Relatório Técnico

O relatório técnico completo desta fase — introdução, síntese de cada etapa,
métricas consolidadas, discussão crítica sobre uso prático e limitações — está em
[`reports/relatorio_tecnico.pdf`](reports/relatorio_tecnico.pdf).

## Repositório

<URL_DO_REPOSITORIO>
