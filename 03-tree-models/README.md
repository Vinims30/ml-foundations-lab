# Spaceship Titanic: Classificação com XGBoost

Este diretório contém a resolução do desafio "Spaceship Titanic" do Kaggle, focada na implementação de um modelo de Extreme Gradient Boosting (XGBoost). O objetivo da competição é prever se um passageiro foi transportado para uma dimensão alternativa durante a colisão da nave espacial com uma anomalia no espaço-tempo.

## Visão Geral do Projeto
O principal foco deste notebook foi aplicar técnicas avançadas de pré-processamento de dados e engenharia de características fundamentadas nas regras de negócio (lore) do próprio universo do desafio, seguidas pela otimização de hiperparâmetros de um modelo baseado em árvores de decisão.

## Regras de Negócio e Pré-processamento
Para tratar os dados nulos sem causar vazamento de dados (Data Leakage) entre os conjuntos de treino e teste, as seguintes regras lógicas foram aplicadas:

* **Sono Criogênico (CryoSleep):** Passageiros com a soma total de despesas maior que zero tiveram os valores nulos preenchidos com `False`, enquanto aqueles com zero gastos totais foram classificados como `True`.
* **Planeta Natal (HomePlanet) e Destino (Destination):** Valores nulos foram preenchidos mapeando o identificador do grupo familiar (extraído do `PassengerId`). Para o `HomePlanet`, passageiros restantes em decks específicos (A, B, C, T) foram alocados em 'Europa', e deck 'G' em 'Earth'.
* **Despesas Financeiras:** Passageiros em sono criogênico ou com idade inferior a 13 anos não podem realizar gastos; logo, valores nulos nessas condições foram preenchidos com 0. Os restantes foram preenchidos com a mediana calculada exclusivamente no treino.
* **Idade (Age):** Valores nulos foram substituídos pela mediana de idade do conjunto de treino (27.0 anos).
* **Status VIP:** Passageiros originários da Terra ('Earth') não possuem status VIP; portanto, valores nulos para esses passageiros foram definidos como `False`.

As variáveis categóricas resultantes foram processadas utilizando a técnica de One-Hot Encoding (`pd.get_dummies`) para adaptação ao modelo matemático.

## Modelagem e Otimização
O algoritmo escolhido foi o `XGBClassifier`. Para garantir a melhor capacidade de generalização e evitar overfitting, foi utilizada a validação cruzada (Cross-Validation) com 5 divisões (folds) através do `GridSearchCV`.

A busca percorreu 144 candidatos de hiperparâmetros. Os melhores parâmetros encontrados pelo GridSearch foram:
* `colsample_bytree`: 1.0
* `learning_rate`: 0.1
* `max_depth`: 5
* `n_estimators`: 100
* `subsample`: 1.0

## Resultados
O modelo demonstrou uma excelente capacidade de generalização, com os resultados de teste oficial ficando extremamente próximos da validação local:
* **Acurácia em Validação Cruzada (Local):** 0.8016
* **Score Oficial Kaggle (Submissão):** 0.79167

## Ideias Futuras (Feature Engineering)
O modelo atual serve como uma baseline sólida com foco no entendimento do funcionamento interno do XGBoost. Para iterações futuras visando ultrapassar a marca de 80% de acurácia, as seguintes estratégias de Engenharia de Características (Feature Engineering) estão mapeadas para implementação:

* **Inclusão do Deck da Cabine:** Reincorporar a coluna referente à cabine do passageiro, preenchendo os valores nulos com uma categoria de "Desconhecido" (ex: 'U') e aplicando o One-Hot Encoding. A localização estrutural na nave (níveis superiores vs. inferiores) tem forte correlação com o alvo.
* **Criação da Variável Gasto_Total:** Agregar as colunas financeiras (`RoomService`, `FoodCourt`, `ShoppingMall`, `Spa` e `VRDeck`) em uma única métrica contínua. Isso permite que a árvore de decisão crie regras de corte rigorosas (splits) isolando imediatamente passageiros de baixa renda/gastos nulos daqueles da alta classe.
* **Tamanho do Grupo Familiar:** Utilizar as ocorrências do mesmo prefixo no `PassengerId` para criar a variável contínua `Tamanho_Grupo`, testando a hipótese de que famílias maiores ou passageiros solitários possuem diferentes probabilidades estatísticas de serem transportados.