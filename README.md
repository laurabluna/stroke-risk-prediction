# Predição de Risco de AVC (Stroke Risk Prediction)

Projeto prático interdisciplinar de Ciência de Dados, Engenharia de Dados e Análise de Dados desenvolvido para a disciplina de **Métodos Quantitativos (TADS 2026.1)**. O sistema cobre o ciclo de vida completo de um modelo de Machine Learning: desde a exploração inicial dos dados brutos e tratamento de imperfeições, passando pelo pipeline ETL automatizado e treinamento com balanceamento, até a disponibilização final em um painel interativo **Streamlit**.

---

## Componentes do Grupo

- **[Insira o Nome do Aluno 1]** — *(Matrícula / Papel no Projeto)*
- **[Insira o Nome do Aluno 2]** — *(Matrícula / Papel no Projeto)*
- **[Insira o Nome do Aluno 3]** — *(Matrícula / Papel no Projeto)*
- **[Insira o Nome do Aluno 4]** — *(Matrícula / Papel no Projeto)*

*(Substitua os campos acima pelos nomes e dados reais dos integrantes do grupo).*

---

## Stack Tecnológica Utilizada

O projeto foi construído sobre um ecossistema moderno e modular em **Python**:

- **Core & Manipulação de Dados (Fase 1 e 2):**
  - [`Python 3`](https://www.python.org/)
  - [`Pandas`](https://pandas.pydata.org/) & [`NumPy`](https://numpy.org/) — manipulação vetorial, criação de faixas (*binning*) e limpeza de dados.
- **Visualização & BI (Fase 1 e 4):**
  - [`Plotly Express` & `Plotly Graph Objects`](https://plotly.com/python/) — criação de gráficos interativos, medidores dinâmicos de probabilidade (*Gauge Chart*), curvas ROC e matrizes de confusão.
  - [`Seaborn`](https://seaborn.pydata.org/) & [`Matplotlib`](https://matplotlib.org/) — visualizações exploratórias no notebook de EDA.
- **Engenharia de Features & Machine Learning (Fase 2 e 3):**
  - [`Scikit-Learn`](https://scikit-learn.org/) — `Pipeline`, `ColumnTransformer`, `SimpleImputer`, `StandardScaler`, `OneHotEncoder` e `LogisticRegression(class_weight="balanced")`.
  - [`Imbalanced-Learn (imblearn)`](https://imbalanced-learn.org/) — aplicação da técnica de sobreamostragem **SMOTE** para balancear a classe minoritária de AVC.
  - [`Joblib`](https://joblib.readthedocs.io/) — serialização e exportação do pré-processador (`preprocessor.pkl`) e do modelo treinado (`stroke_risk_model.joblib`).
- **Aplicação Web Interativa (Fase 5):**
  - [`Streamlit`](https://streamlit.io/) — framework de interface web em Python, customizado com **CSS Adaptativo (Light/Dark Mode)** na paleta do **Claude Code** para experiência visual sóbria e profissional.

---

## Estrutura de Diretórios

```text
stroke-risk-prediction/
│
├── app/
│   ├── templates/          # Arquivos de suporte a templates
│   └── app.py              # Aplicação Web Streamlit (Fase 4 e 5)
│
├── data/
│   ├── raw/                # Dataset bruto (healthcare-dataset-stroke-data.csv)
│   └── processed/          # CSVs processados, codificados e balanceados com SMOTE (`preprocessor.pkl`)
│
├── models/
│   └── stroke_risk_model.joblib # Artefato contendo o modelo de Regressão Logística e limiar calibrado
│
├── notebooks/
│   └── 01_eda_e_insights.ipynb  # Notebook com Análise Exploratória (Fase 1)
│
├── reports/
│   ├── metrics_report.md   # Relatório formatado em Markdown com métricas finais
│   ├── metrics.json        # Dicionário de métricas estruturadas
│   ├── metrics_summary.csv # Resumo tabular das métricas nas partições
│   └── roc_curve_test.csv  # Pontos da curva ROC no conjunto de teste
│
├── src/
│   ├── pipeline.py         # Script de Engenharia de Dados (ETL, Binning e SMOTE)
│   └── train.py            # Script de Treinamento, Otimização de Limiar e Avaliação
│
├── requirements.txt        # Dependências do projeto
└── README.md               # Documentação principal
```

---

## Instruções de Execução passo a passo

Siga os passos abaixo para configurar o ambiente, executar todo o pipeline do zero e abrir a aplicação web em seu computador.

### 1. Configurar o Ambiente Virtual

Abra o terminal na pasta raiz do projeto e crie/ative o ambiente virtual (`.venv`):

```powershell
# Criar o ambiente virtual
python -m venv .venv

# Ativar o ambiente virtual no PowerShell (Windows)
.\.venv\Scripts\Activate.ps1
# (Ou se preferir executar diretamente o Python do venv: .\.venv\Scripts\python.exe)
```

### 2. Instalar as Dependências

Com o ambiente ativado, instale todas as bibliotecas necessárias:

```powershell
pip install --upgrade pip
pip install -r requirements.txt
```

---

### 3. Executar o Pipeline de Engenharia de Dados (ETL — Fase 2)

O script de ETL lê a base bruta (`data/raw/`), aplica imputação em nulos, cria faixas clínicas para idade (`age_group`) e glicose (`glucose_group`), aplica `StandardScaler` e `OneHotEncoder`, e gera a base de treino balanceada via **SMOTE**:

```powershell
python src/pipeline.py
```
> *Ao finalizar, os arquivos limpos e balanceados (`X_train.csv`, `X_test.csv`, `y_train.csv`, `y_test.csv` e `preprocessor.pkl`) estarão salvos na pasta `data/processed/`.*

---

### 4. Treinar o Modelo de Machine Learning (Fase 3)

O pre-processamento gera os conjuntos em `data/processed`. O treino usa diretamente `X_train.csv`, `y_train.csv`, `X_test.csv` e `y_test.csv` dessa pasta; os dados em `data/raw` são usados apenas pelo pré-processamento. A validação é separada de forma estratificada a partir do conjunto de treino (`train_test_split`).

O modelo final utiliza `LogisticRegression(class_weight="balanced")` para lidar com o desbalanceamento da classe alvo. O limiar de corte (*threshold*) é ajustado dinamicamente na partição de validação maximizando o `F1-Score`. A avaliação final no teste utiliza Precision, Recall, F1-Score, ROC AUC, Accuracy e Matriz de Confusão:

```powershell
python src/train.py
```
> *Ao finalizar, o modelo estará exportado em `models/stroke_risk_model.joblib` e os relatórios de métricas e curva ROC estarão atualizados na pasta `reports/`.*

---

### 5. Iniciar a Aplicação Web Streamlit (Fase 4 e 5)

Para abrir a interface interativa onde os usuários podem realizar predições ao vivo, visualizar o velocímetro de risco quantitativo, explorar os gráficos da Fase 4 e ler a metodologia do projeto:

```powershell
streamlit run app/app.py
```
> *O terminal exibirá uma URL local (geralmente `http://localhost:8501`). O navegador será aberto automaticamente com o sistema quantitativo de suporte à decisão.*

---

## Resumo das Fases do Projeto

1. **Fase 1 (EDA):** Identificação de desbalanceamento severo (`~4.8%` de AVC), nulos no IMC (`bmi`) e análise univariada/bivariada das condições clínicas.
2. **Fase 2 (ETL):** Construção de pipeline automatizado (`build_preprocessor`) com categorização médica em faixas (`pd.cut`) e sobreamostragem com `SMOTE`.
3. **Fase 3 (Treinamento):** Treinamento via Scikit-Learn em cima dos dados processados, com limiar ajustado dinamicamente para priorizar detecção de casos positivos (Recall **68.0%** / ROC AUC **0.8413**).
4. **Fase 4 (Dashboards):** Criação de gráficos interativos consolidados no `Plotly` para análise de curvas ROC, matrizes de confusão e distribuições demográficas.
5. **Fase 5 (Web App):** Encapsulamento do modelo em uma aplicação moderna com design adaptativo (Light/Dark Mode) focado em sobriedade técnica e terminologia de engenharia de software.
