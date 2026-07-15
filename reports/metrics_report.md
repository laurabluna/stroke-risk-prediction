# Relatorio de desempenho - Predicao de AVC

## Separacao dos dados

| Particao | Classe 0 | Classe 1 | Total |
| --- | ---: | ---: | ---: |
| treino | 2916 | 2917 | 5833 |
| validacao | 973 | 972 | 1945 |
| teste | 972 | 50 | 1022 |

## Validacao

Modelo: `logistic_regression_balanced`

| Threshold | Precision | Recall | F1-Score | ROC AUC | Accuracy |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 0.3976 | 0.7342 | 0.9321 | 0.8214 | 0.8519 | 0.7974 |

## Teste final

| Threshold | Precision | Recall | F1-Score | ROC AUC | Accuracy |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 0.3976 | 0.1097 | 0.8400 | 0.1940 | 0.8407 | 0.6585 |

## Matriz de confusao no teste

|  | Previsto 0 | Previsto 1 |
| --- | ---: | ---: |
| Real 0 | 631 | 341 |
| Real 1 | 8 | 42 |

## Artefatos

- Modelo final: `models/stroke_risk_model.joblib`
- Curva ROC do teste: `reports/roc_curve_test.csv`
