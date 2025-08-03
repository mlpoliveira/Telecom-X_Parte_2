[RelatóriodePrevisãodeChurn-TelecomX.md](https://github.com/user-attachments/files/21566886/RelatoriodePrevisaodeChurn-TelecomX.md)
# Relatório de Previsão de Churn - Telecom X

## 1. Introdução

Este relatório detalha o processo de desenvolvimento de modelos preditivos para identificar clientes com alta probabilidade de evasão (churn) na Telecom X. A capacidade de prever o churn é crucial para que a empresa possa implementar estratégias proativas de retenção de clientes, minimizando perdas de receita e fortalecendo a base de clientes. O objetivo principal deste desafio foi construir um pipeline robusto de modelagem, desde a preparação dos dados até a interpretação dos resultados e a formulação de conclusões estratégicas.

### Estrutura do Projeto

├── Churn_Analysis_Improved.ipynb # Notebook principal com todo o pipeline

├── dados_transformados.csv # Base de dados tratada usada para modelagem

├── RelatóriodePrevisãodeChurn-TelecomX.md # Relatório detalhado do projeto

└── Imagens e gráficos gerados na EDA

## 2. Preparação dos Dados

A etapa de preparação dos dados é fundamental para garantir a qualidade e a adequação das informações para a modelagem de Machine Learning. O conjunto de dados inicial, `dados_transformados.csv`, foi submetido a um processo rigoroso de pré-processamento, que incluiu as seguintes fases:

### 2.1. Carregamento e Inspeção Inicial

O arquivo `dados_transformados.csv` foi carregado em um DataFrame pandas. Uma inspeção inicial revelou a estrutura dos dados, os tipos de variáveis (numéricas e categóricas) e a presença de valores ausentes. Identificou-se que as colunas `Churn`, `Charges.Total` e `AvgMonthlyCharge` possuíam valores ausentes.

### 2.2. Tratamento de Valores Ausentes

Para a variável alvo `Churn`, as linhas com valores ausentes foram removidas, garantindo que o modelo fosse treinado apenas com dados completos e consistentes para a variável de interesse. Para as variáveis numéricas `Charges.Total` e `AvgMonthlyCharge`, os valores ausentes foram imputados com a média da respectiva coluna. Esta abordagem foi escolhida para preservar o maior número possível de registros, ao mesmo tempo em que se preenche as lacunas de forma razoável.

### 2.3. Codificação de Variáveis Categóricas (Encoding)

As variáveis categóricas presentes no dataset foram transformadas em um formato numérico que pode ser compreendido pelos algoritmos de Machine Learning. Para isso, foi aplicado o One-Hot Encoding, que cria novas colunas binárias para cada categoria única em uma variável categórica. Esta técnica é preferível para variáveis nominais, pois evita a introdução de uma ordem artificial que não existe nos dados originais. A variável `Churn` foi convertida para 0 (No) e 1 (Yes).

### 2.4. Normalização de Variáveis Numéricas

As variáveis numéricas foram padronizadas utilizando o `StandardScaler`. Este processo transforma os dados para que tenham média zero e desvio padrão um. A padronização é crucial para algoritmos que são sensíveis à escala das features, como a Regressão Logística, garantindo que nenhuma feature domine o processo de treinamento devido à sua magnitude.

### 2.5. Divisão dos Dados

Finalmente, o conjunto de dados pré-processado foi dividido em conjuntos de treino e teste, com uma proporção de 80% para treino e 20% para teste. Esta divisão é essencial para avaliar o desempenho do modelo em dados não vistos, fornecendo uma estimativa mais realista de sua capacidade de generalização.

## 3. Análise de Correlação e Seleção de Variáveis

Após a preparação dos dados, foi realizada uma análise de correlação para entender a relação entre as features e a variável alvo (Churn). A matriz de correlação foi calculada e a correlação de cada feature com o Churn foi analisada. Esta etapa é importante para identificar as variáveis mais influentes e, potencialmente, para a seleção de features, embora neste desafio todas as features pré-processadas tenham sido mantidas para a modelagem.

As variáveis com maior correlação positiva com o Churn (indicando maior probabilidade de evasão) incluem:

*   `cat__Contract_Month-to-month`: Clientes com contrato mensal tendem a ter maior churn.
*   `cat__tenure_group_0-1 ano`: Clientes com menos de um ano de serviço têm maior churn.
*   `cat__PaymentMethod_Electronic check`: Clientes que usam cheque eletrônico como método de pagamento apresentam maior churn.
*   `cat__InternetService_Fiber optic`: Clientes com serviço de internet de fibra óptica têm maior churn.

Por outro lado, variáveis com maior correlação negativa (indicando menor probabilidade de evasão) incluem:

*   `num__tenure`: Clientes com maior tempo de permanência (tenure) têm menor churn.
*   `cat__Contract_Two year`: Clientes com contrato de dois anos têm menor churn.
*   `cat__tenure_group_+3 anos`: Clientes com mais de três anos de serviço têm menor churn.
*   `cat__InternetService_No`: Clientes sem serviço de internet têm menor churn.

Esta análise inicial fornece insights valiosos sobre os fatores que impulsionam o churn, que serão confirmados e aprofundados na interpretação dos modelos.

## 4. Modelagem Preditiva

Para a previsão de churn, foram treinados e avaliados dois modelos de classificação amplamente utilizados: Regressão Logística e Random Forest. Ambos os modelos foram treinados nos dados pré-processados e avaliados em um conjunto de teste separado para garantir uma avaliação imparcial de seu desempenho.

### 4.1. Modelos Utilizados

*   **Regressão Logística:** Um modelo linear que estima a probabilidade de uma ocorrência de um evento (neste caso, churn) ajustando os dados a uma função logística. É um modelo simples, mas eficaz, e seus coeficientes podem ser interpretados para entender a influência de cada variável.
*   **Random Forest:** Um algoritmo de ensemble que constrói múltiplas árvores de decisão durante o treinamento e produz a classe que é a moda das classes (classificação) ou a previsão média (regressão) das árvores individuais. É conhecido por sua alta precisão e capacidade de lidar com relações não lineares e interações entre variáveis.

### 4.2. Métricas de Avaliação

O desempenho dos modelos foi avaliado utilizando as seguintes métricas:

*   **Acurácia (Accuracy):** A proporção de previsões corretas em relação ao total de previsões.
*   **Precisão (Precision):** A proporção de verdadeiros positivos em relação a todos os resultados positivos previstos. É importante quando o custo de um falso positivo é alto.
*   **Recall:** A proporção de verdadeiros positivos em relação a todos os casos positivos reais. É importante quando o custo de um falso negativo é alto.
*   **F1-Score:** A média harmônica da precisão e do recall, oferecendo um equilíbrio entre as duas métricas.
*   **ROC-AUC (Receiver Operating Characteristic - Area Under the Curve):** Uma métrica que avalia a capacidade do modelo de distinguir entre as classes. Um valor mais próximo de 1 indica melhor desempenho.

### 4.3. Resultados da Avaliação

Os resultados da avaliação dos modelos no conjunto de teste foram os seguintes:

| Modelo              | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|---------------------|----------|-----------|--------|----------|---------|
| Regressão Logística | 0.8055   | 0.6689    | 0.5255 | 0.5886   | 0.8399  |
| Random Forest       | 0.8027   | 0.6848    | 0.4718 | 0.5587   | 0.8195  |

Ambos os modelos apresentaram um desempenho razoável, com acurácia em torno de 80%. A Regressão Logística obteve um ROC-AUC ligeiramente superior, indicando uma melhor capacidade de discriminação entre clientes que evadem e não evadem. O Random Forest, por sua vez, apresentou uma precisão um pouco maior, o que significa que, quando ele prevê churn, é mais provável que essa previsão esteja correta. No entanto, o recall do Random Forest foi menor, indicando que ele identificou uma proporção menor dos clientes que de fato evadiram.

## 5. Interpretação dos Resultados e Importância das Variáveis

A interpretação dos modelos é crucial para entender os fatores que mais influenciam o churn e para traduzir os resultados técnicos em insights de negócios acionáveis. Analisamos a importância das variáveis para ambos os modelos.

### 5.1. Importância das Variáveis (Regressão Logística - Coeficientes)

Para a Regressão Logística, a magnitude e o sinal dos coeficientes indicam a força e a direção da relação entre a variável e a probabilidade de churn. Coeficientes positivos altos sugerem que a variável aumenta a probabilidade de churn, enquanto coeficientes negativos altos sugerem que a variável diminui a probabilidade de churn.

As variáveis mais influentes para o churn, com base nos coeficientes da Regressão Logística, são:

*   `cat__Contract_Month-to-month`: Ter um contrato mensal é o fator mais forte para o churn.
*   `cat__InternetService_Fiber optic`: Clientes com fibra óptica têm maior probabilidade de churn.
*   `cat__PaymentMethod_Electronic check`: Pagamento via cheque eletrônico está associado a maior churn.
*   `num__Charges.Monthly`: Mensalidades mais altas tendem a aumentar o churn.

Por outro lado, as variáveis que mais reduzem a probabilidade de churn são:

*   `num__tenure`: Quanto maior o tempo de permanência, menor a probabilidade de churn.
*   `cat__Contract_Two year`: Contratos de dois anos são um forte indicador de retenção.
*   `cat__InternetService_No`: Clientes sem serviço de internet (apenas telefone) têm menor churn.

### 5.2. Importância das Variáveis (Random Forest - Feature Importances)

Para o Random Forest, a importância das features é calculada com base na redução média da impureza (Gini impurity) que cada feature proporciona nas árvores de decisão. Valores mais altos indicam maior importância.

As variáveis mais importantes, de acordo com o Random Forest, são:

*   `num__Charges.Total`: O valor total cobrado ao cliente.
*   `num__AvgMonthlyCharge`: A média do valor mensal cobrado.
*   `num__tenure`: O tempo de permanência do cliente.
*   `num__Charges.Monthly`: O valor mensal cobrado.

É interessante notar que, enquanto a Regressão Logística destaca a importância de variáveis categóricas específicas (tipo de contrato, serviço de internet, método de pagamento), o Random Forest atribui maior importância às variáveis numéricas relacionadas aos custos e tempo de serviço. Isso sugere que ambos os tipos de variáveis são cruciais para prever o churn, e a combinação de insights de ambos os modelos oferece uma visão mais completa.

## 6. Conclusão Estratégica

Com base na análise e modelagem realizadas, podemos extrair as seguintes conclusões estratégicas sobre os principais fatores que influenciam a evasão de clientes na Telecom X:

1.  **Contratos e Permanência:** Clientes com contratos mensais e menor tempo de permanência (especialmente no primeiro ano) são os mais propensos a churn. Contratos de dois anos são um forte indicador de lealdade. **Recomendação:** Focar em estratégias de fidelização para clientes novos e incentivar a migração para contratos de longo prazo.

2.  **Serviço de Internet e Custos:** O serviço de internet de fibra óptica, embora moderno, está associado a maior churn. Além disso, mensalidades e encargos totais mais altos são preditores significativos de churn. **Recomendação:** Investigar a satisfação de clientes com fibra óptica e avaliar a competitividade dos preços. Oferecer pacotes mais flexíveis ou descontos para clientes de alto valor pode ser uma estratégia.

3.  **Método de Pagamento:** Clientes que utilizam cheque eletrônico como método de pagamento têm maior probabilidade de churn. **Recomendação:** Analisar o perfil desses clientes e considerar oferecer alternativas de pagamento mais convenientes ou incentivos para a mudança de método.

4.  **Serviços Adicionais e Suporte:** Variáveis como segurança online e suporte técnico (ou a ausência deles) também desempenham um papel, embora com menor impacto que os fatores de contrato e custo. **Recomendação:** Garantir a qualidade do suporte técnico e a percepção de valor dos serviços adicionais, pois podem ser diferenciais na retenção.

Em resumo, a Telecom X deve priorizar a retenção de clientes com contratos mensais e de curta duração, monitorar de perto a satisfação dos clientes com fibra óptica e otimizar a estrutura de preços. A implementação de programas de fidelidade, ofertas personalizadas e um serviço de atendimento ao cliente proativo, focado nos fatores identificados, pode ser fundamental para reduzir a taxa de churn e garantir o crescimento sustentável da empresa.

Este relatório serve como um ponto de partida para a equipe de Machine Learning da Telecom X, fornecendo uma base sólida para futuras análises e o desenvolvimento de soluções de inteligência preditiva mais avançadas.


## 7. Instruções para Execução

Para executar o notebook `Churn_Analysis_Improved.ipynb`, siga os passos abaixo. Certifique-se de ter o Python 3.8+ instalado em sua máquina.

### Dependências

As principais bibliotecas utilizadas no projeto são:

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scikit-learn`

Você pode instalar todas de uma vez usando:

```bash
pip install -r requirements.txt
```
Ou, individualmente:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn
```
### Executando o Projeto

-  Clone ou baixe os arquivos do projeto.

- Instale as bibliotecas conforme descrito acima.

- Abra o notebook Churn_Analysis_Improved.ipynb com o Jupyter Notebook, JupyterLab ou VS Code.

- Certifique-se de que o arquivo dados_transformados.csv esteja no mesmo diretório do notebook.

- Execute as células em ordem para reproduzir a análise completa.
