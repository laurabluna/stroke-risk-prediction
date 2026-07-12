# Relatorio de desempenho - Predicao de AVC

## Separacao dos dados

| Particao | Classe 0 | Classe 1 | Total |
| --- | ---: | ---: | ---: |
| treino | 2917 | 149 | 3066 |
| validacao | 972 | 50 | 1022 |
| teste | 972 | 50 | 1022 |

## Validacao

Modelo: `logistic_regression_balanced`

| Threshold | Precision | Recall | F1-Score | ROC AUC | Accuracy |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 0.5815 | 0.1577 | 0.7600 | 0.2612 | 0.8374 | 0.7896 |

## Teste final

| Threshold | Precision | Recall | F1-Score | ROC AUC | Accuracy |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 0.5815 | 0.1660 | 0.8000 | 0.2749 | 0.8436 | 0.7935 |

## Matriz de confusao no teste

|  | Previsto 0 | Previsto 1 |
| --- | ---: | ---: |
| Real 0 | 771 | 201 |
| Real 1 | 10 | 40 |

## Artefatos

- Modelo final: `models/stroke_risk_model.joblib`
- Curva ROC do teste: `reports/roc_curve_test.csv`
