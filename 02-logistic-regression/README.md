# Titanic - Machine Learning from Disaster: Regressão Logística

## Sobre a Competição
A competição "Titanic - Machine Learning from Disaster" é um dos desafios mais clássicos e introdutórios da plataforma Kaggle. O objetivo é utilizar dados reais dos passageiros (como idade, sexo, classe do bilhete e porto de embarque) para construir um modelo preditivo capaz de determinar quem sobreviveu e quem não sobreviveu ao naufrágio do RMS Titanic.

## O Problema
O naufrágio do Titanic resultou na perda de diversas vidas, mas a sobrevivência não foi um evento puramente aleatório; alguns grupos de pessoas (como mulheres, crianças e passageiros da primeira classe) tiveram maiores chances de escapar. O nosso desafio é extrair esses padrões matemáticos da base de treino e aplicá-los para prever a sobrevivência de 418 passageiros na base de teste cega fornecida pelo Kaggle.

## Metodologia e Workflow (Jupyter Notebook)

A solução foi desenvolvida de forma iterativa e documentada no notebook `desafio_titanic.ipynb`, seguindo as melhores práticas de análise e modelagem de dados:

*   **Carregamento e Conversão de Dados**: Importamos as bases e transformamos dados categóricos em numéricos, mapeando a variável `Sex` para binário (0 para homens, 1 para mulheres) e aplicando a técnica de variáveis Dummy para os portões de `Embarked`.
*   **Tratamento de Dados Ausentes (Imputação)**: Diagnosticamos 177 valores nulos em `Age` e 687 na coluna `Cabin`. Para evitar distorções estatísticas (como criar "falsos bebês" ao preencher idades nulas com zero), imputamos os valores vazios de idade utilizando a mediana calculada exclusivamente a partir da base de treino.
*   **Limpeza e Seleção de Features**: Removemos colunas textuais de alta cardinalidade (`Name`, `Ticket`) e com excesso de nulos (`Cabin`) para não prejudicar a matemática do modelo linear. Também isolamos a coluna `PassengerId` para compor o arquivo de submissão no final.
*   **Análise Exploratória (EDA)**: Investigamos visualmente o impacto das relações familiares (`SibSp` e `Parch`) e a distribuição de tarifas (`Fare`) por classe. Constatamos que a variável de tarifas possuía outliers extremos, o que exigiria tratamento de escala.
*   **Divisão e Escalonamento**: Para validar o modelo localmente, separamos os dados de treino em 80% para aprendizado e 20% para validação (holdout) com `random_state=42`. Em seguida, aplicamos o `StandardScaler` para padronizar as métricas e evitar que variáveis de maior grandeza dominassem os cálculos matemáticos.
*   **Treinamento e Interpretabilidade**: Instanciamos um modelo de `LogisticRegression`. A extração dos coeficientes revelou que a variável `Sex` possuía o maior impacto positivo (1.27) nas chances de sobrevivência, enquanto a classe do bilhete `Pclass` exercia o maior peso negativo (-0.78).
*   **Engenharia de Atributos e Validação**: O nosso modelo baseline atingiu uma precisão local de **81.01%**. Registramos e descartamos uma tentativa de Feature Engineering ("Family Size"), pois a unificação reduziu a eficácia do algoritmo para 78% na base de validação, provando que a granularidade dos dados originais extraía mais valor.
*   **Retreinamento e Submissão Final**: Com as escolhas analíticas validadas, retreinamos o modelo utilizando 100% dos dados originais para maximizar a generalização. Tratamos os NaNs da base de teste do Kaggle com a mediana de treino, aplicamos o escalonamento e exportamos o arquivo `submissao_titanic.csv` sem índice.

## Resultados Alcançados

*   **Acurácia de Validação Local (Holdout 20%)**: 81.01%
*   **Score Oficial no Kaggle (Test Set)**: **0.76555**.

A pequena diferença entre a validação local e o score do Kaggle é um comportamento esperado em competições de Machine Learning, refletindo o ajuste natural do modelo aos dados não vistos da plataforma.
