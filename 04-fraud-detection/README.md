# 04 - Fraud Detection: Classificação vs. Anomaly Detection

## Objetivo

Comparar duas abordagens distintas para o mesmo problema de negócio — detecção de fraude em transações de cartão de crédito — discutindo em que cenários cada uma é mais adequada:

1. **Classificação supervisionada**, usando os rótulos de fraude confirmada
2. **Detecção de anomalias**, tratando fraude como um desvio do comportamento "normal" (sem depender dos rótulos no treino)

## Dataset

[Credit Card Fraud Detection (Kaggle)](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

- 284.807 transações, das quais apenas 492 são fraudes (~0.17% da base) — dataset extremamente desbalanceado
- Features numéricas anonimizadas via PCA (`V1` a `V28`), além de `Time` (tempo em segundos desde a primeira transação) e `Amount` (valor da transação)
- Coluna alvo: `Class` (0 = transação legítima, 1 = fraude)

## Perguntas que o projeto pretende responder

- Um classificador supervisionado (que já viu exemplos de fraude no treino) performa melhor do que um modelo de anomaly detection (que não usa rótulos)?
- As duas abordagens erram nos mesmos casos, ou cada uma captura um tipo diferente de fraude?
- Em qual cenário de negócio cada abordagem faria mais sentido na prática?

## Abordagens planejadas

### 1. Classificação Supervisionada
*(a detalhar: modelos testados, tratamento de desbalanceamento, métricas usadas)*

### 2. Detecção de Anomalias
*(a detalhar: modelos testados, critério de contaminação/threshold, métricas usadas)*

## Resultados

*(preencher após os experimentos)*

## Discussão e Conclusão

*(preencher após os experimentos)*

## Notas técnicas

*(anotar aqui, ao longo do desenvolvimento, decisões técnicas relevantes — ex: tratamento de data leakage, escolha de hiperparâmetros, dificuldades encontradas)*