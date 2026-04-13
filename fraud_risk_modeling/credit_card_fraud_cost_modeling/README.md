[en]
# Credit Card Fraud Detection: from naive classification to cost-oriented modeling

## Overview

This project investigates credit card fraud detection from a **decision-making** perspective, rather than focusing solely on statistical prediction.

Instead of relying only on traditional classification metrics, the analysis aims to reflect a more realistic financial setting: fraud is a rare event, data may change over time, and model evaluation should account for the economic consequences of errors, especially:

- **False Negatives (FN):** fraudulent transactions that go undetected
- **False Positives (FP):** legitimate transactions incorrectly blocked

The goal was to compare modeling strategies with different levels of maturity and show how a more robust approach can lead to better operational and financial outcomes.

---

## Motivation

In financial modeling problems, simply training models without careful data analysis can lead to misleading conclusions and costly decisions.

This project was developed to reinforce a central idea: **fraud modeling is not just about applying classifiers**. It also requires:

- understanding severe class imbalance;
- investigating temporal instability and drift;
- evaluating whether features remain stable over time;
- defining metrics aligned with economic impact;
- comparing models using business-relevant criteria.

---

## Dataset

- **Dataset:** Credit Card Fraud Detection (Kaggle)
- **Target variable:** `Class`  
  - `0` = legitimate transaction  
  - `1` = fraud
- **Main challenges:**
  - the fraud class is extremely rare;
  - variables are anonymized (`V1` to `V28`);
  - the problem must be evaluated in a way that is closer to real operational conditions.

---

## What was done

### 1. Exploratory and diagnostic analysis

Before modeling, an exploratory and diagnostic stage was carried out, including:

- class imbalance analysis;
- temporal behavior based on the `Time` variable;
- **target drift** investigation;
- **feature drift** investigation using the Kolmogorov-Smirnov test (KS test);
- analysis of the relationship between feature instability and model robustness.

This stage was essential to avoid a common mistake in fraud projects: treating the problem as if it were just a standard classification task.

---

### 2. Why accuracy alone is misleading

One of the main lessons of this project is that **very high accuracy can coexist with very poor business performance**.

Because fraud is rare, a model that classifies **all transactions as non-fraudulent** can still achieve extremely high accuracy. Even so, this same model produces the worst possible operational result: it lets every fraudulent transaction go through.

---

### 3. Cost-oriented evaluation

To go beyond traditional machine learning metrics, a cost metric was defined based on the financial impact of prediction errors:

- **False Negative (FN):** undetected fraud  
  → cost equals the transaction amount

- **False Positive (FP):** legitimate transaction incorrectly blocked  
  → operational/reputational cost modeled as a fraction of the transaction amount

This allowed model comparison to consider not only metrics such as ROC-AUC and PR-AUC, but also the **total financial loss associated with model decisions**.

---

## Scenarios analyzed

### Scenario 1 — Absurd baseline

In this scenario, a “model” was deliberately built to classify **all transactions as non-fraudulent**.

This case was included to highlight a classic contradiction in highly imbalanced datasets:

- very high accuracy;
- no fraud cases detected;
- high financial loss.

This scenario clearly shows why **accuracy cannot be the main metric** in problems like this.

---

### Scenario 2 — Naive modeling

In this scenario, traditional classifiers were tested without explicitly incorporating the insights obtained from the exploratory data analysis.

Models tested:

- Logistic Regression
- Random Forest
- Extra Trees
- HistGradientBoosting

These models were evaluated with the default threshold and without explicit cost-based threshold tuning.

---

### Scenario 3 — Robust modeling

In this scenario, the models were trained and evaluated with a more realistic approach, incorporating:

- temporal split between training, validation, and test sets;
- class imbalance treatment;
- threshold tuning based on observed validation cost;
- comparison guided by financial impact.

This approach produced better operational trade-offs and lower total cost for some of the tested models.

---

## Main findings

The analysis showed that:

- high accuracy is not enough to evaluate fraud models;
- naive evaluation can hide financially poor decisions;
- cost-based threshold tuning can make a model more useful in practice;
- a robust approach does not improve all models equally;
- **Random Forest with the robust setup** achieved the best financial result among the tested models;
- some models with very high ROC-AUC still performed worse in business terms due to inadequate thresholds or too many false positives.

---

## Example results

| Scenario | Model | Recall | PR-AUC | Total Cost |
|---|---:|---:|---:|---:|
| Absurd baseline | Always Zero | 0.000 | 0.001 | 6168.88 |
| Naive modeling | ExtraTrees | 0.673 | 0.764 | 2706.43 |
| Robust modeling | RandomForest | 0.750 | 0.775 | 2386.48 |

---

## Main conclusion

A fraud model should not be judged only by its classification ability.

It should be evaluated by its ability to **support decisions in an environment with severe class imbalance, temporal instability, and asymmetric error costs**.

---

[pt]
# Credit Card Fraud Detection: da classificação ingênua à modelagem orientada a custo

## Visão geral

Este projeto investiga detecção de fraude em cartões de crédito a partir de uma perspectiva de **tomada de decisão**, e não apenas de previsão estatística.

Em vez de focar somente em métricas tradicionais de classificação, a análise busca refletir um cenário mais realista do contexto financeiro: fraude é um evento raro, os dados podem mudar ao longo do tempo, e a avaliação de um modelo deve considerar as consequências econômicas dos erros, especialmente:

- **Falsos Negativos (FN):** fraudes que passam despercebidas
- **Falsos Positivos (FP):** transações legítimas bloqueadas incorretamente

O objetivo foi comparar estratégias de modelagem com diferentes níveis de maturidade e mostrar como uma abordagem mais robusta pode levar a melhores resultados operacionais e financeiros.

---

## Motivação

Em problemas de modelagem aplicados a finanças, simplesmente treinar modelos sem uma análise cuidadosa dos dados pode levar a conclusões enganosas e decisões custosas.

Este projeto foi desenvolvido para reforçar uma ideia central: **modelagem de fraude não é apenas aplicar classificadores**. Também exige:

- compreender o forte desbalanceamento entre classes;
- investigar instabilidade temporal e drift;
- avaliar se as variáveis permanecem estáveis ao longo do tempo;
- definir métricas alinhadas ao impacto econômico;
- comparar modelos com critérios relevantes para o negócio.

---

## Dataset

- **Base de dados:** Credit Card Fraud Detection (Kaggle)
- **Variável alvo:** `Class`  
  - `0` = transação legítima  
  - `1` = fraude
- **Principais desafios:**
  - classe fraudulenta extremamente rara;
  - variáveis anonimizadas (`V1` a `V28`);
  - necessidade de avaliar o problema de forma mais próxima da realidade operacional.

---

## O que foi feito

### 1. Análise exploratória e diagnóstica

Antes da modelagem, foi realizada uma etapa de exploração e diagnóstico dos dados, incluindo:

- análise do desbalanceamento de classes;
- comportamento temporal com base na variável `Time`;
- investigação de **target drift**;
- investigação de **feature drift** com o teste de Kolmogorov-Smirnov (KS test);
- análise da relação entre instabilidade das variáveis e robustez do modelo.

Essa etapa foi essencial para evitar um erro comum em projetos de fraude: tratar o problema como se fosse apenas uma tarefa de classificação padrão.

---

### 2. Por que acurácia sozinha é enganosa

Uma das principais lições deste projeto é que **acurácia muito alta pode coexistir com péssimo desempenho de negócio**.

Como a fraude é rara, um modelo que classifica **todas as transações como não fraude** ainda pode atingir acurácia extremamente alta. Ainda assim, esse mesmo modelo produz o pior resultado operacional possível: deixa todas as fraudes passarem.

---

### 3. Avaliação orientada a custo

Para ir além das métricas tradicionais de machine learning, foi definida uma métrica de custo baseada no impacto financeiro dos erros:

- **Falso Negativo (FN):** fraude não detectada  
  → custo igual ao valor da transação

- **Falso Positivo (FP):** transação legítima bloqueada  
  → custo operacional/reputacional modelado como uma fração do valor da transação

Com isso, a comparação entre modelos passou a considerar não apenas métricas como ROC-AUC e PR-AUC, mas também o **prejuízo financeiro total associado às decisões**.

---

## Cenários analisados

### Cenário 1 — Baseline absurdo

Neste cenário, foi construído propositalmente um “modelo” que classifica **todas as transações como não fraude**.

Esse caso foi incluído para evidenciar uma contradição clássica em bases altamente desbalanceadas:

- acurácia muito alta;
- nenhum caso de fraude detectado;
- prejuízo financeiro elevado.

Esse cenário mostra de forma didática por que **acurácia não pode ser a métrica principal** em problemas como esse.

---

### Cenário 2 — Modelagem ingênua

Neste cenário, foram testados classificadores tradicionais sem incorporar explicitamente os aprendizados obtidos na exploração dos dados.

Modelos testados:

- Regressão Logística
- Random Forest
- Extra Trees
- HistGradientBoosting

Esses modelos foram avaliados com threshold padrão e sem ajuste explícito orientado a custo.

---

### Cenário 3 — Modelagem robusta

Neste cenário, os modelos foram treinados e avaliados com uma abordagem mais realista, incorporando:

- divisão temporal entre treino, validação e teste;
- tratamento do desbalanceamento;
- ajuste de threshold com base no custo observado na validação;
- comparação orientada ao impacto financeiro.

Essa abordagem produziu melhores trade-offs operacionais e menor custo total em parte dos modelos testados.

---

## Principais achados

A análise mostrou que:

- acurácia alta não é suficiente para avaliar modelos de fraude;
- avaliações ingênuas podem esconder decisões financeiramente ruins;
- ajuste de threshold orientado a custo pode tornar o modelo mais útil na prática;
- uma abordagem robusta não melhora todos os modelos da mesma forma;
- o **Random Forest com configuração robusta** apresentou o melhor resultado financeiro entre os modelos testados;
- alguns modelos com ROC-AUC muito alto ainda assim tiveram desempenho inferior em termos de negócio, devido a thresholds inadequados ou excesso de falsos positivos.

---

## Exemplo de resultados

| Cenário | Modelo | Recall | PR-AUC | Custo Total |
|---|---:|---:|---:|---:|
| Baseline absurdo | Always Zero | 0.000 | 0.001 | 6168.88 |
| Modelagem ingênua | ExtraTrees | 0.673 | 0.764 | 2706.43 |
| Modelagem robusta | RandomForest | 0.750 | 0.775 | 2386.48 |

---

## Principal conclusão

Um modelo de fraude não deve ser julgado apenas por sua capacidade de classificação.

Ele deve ser avaliado pela sua capacidade de **apoiar decisões em um ambiente com desbalanceamento severo, instabilidade temporal e custos assimétricos entre erros**.
