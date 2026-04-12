# ETAPA 4 - Aprendizagem de Máquina

A etapa de aprendizagem de máquina teve como objetivo aplicar modelos preditivos sobre os dados previamente tratados, buscando identificar padrões e gerar insights relevantes para a análise dos fluxos assistenciais.

Foi definido como problema de estudo a previsão do tempo de permanência hospitalar (DIAS_PERM), caracterizando um problema de regressão, uma vez que a variável alvo é numérica.

Para a realização dos experimentos, foram utilizados dois algoritmos amplamente aplicados em problemas de regressão:

Regressão Linear
Árvore de Decisão (Decision Tree Regressor)

Inicialmente, os dados foram preparados por meio da conversão das variáveis para tipos numéricos e remoção de valores nulos, garantindo compatibilidade com os modelos de aprendizado de máquina.

Em seguida, a base de dados foi dividida em dois subconjuntos:

80% para treino
20% para teste

Essa divisão permite avaliar a capacidade de generalização dos modelos em dados não vistos durante o treinamento.

Como métrica de avaliação, foi utilizado o Erro Absoluto Médio (Mean Absolute Error - MAE), que mede a diferença média entre os valores reais e os valores previstos pelos modelos.

Os resultados obtidos foram:

Regressão Linear: MAE = 3.6510
Árvore de Decisão: MAE = 3.4439

Com base nos resultados, o modelo de Árvore de Decisão apresentou melhor desempenho, pois obteve menor erro médio na previsão do tempo de internação.

Essa comparação evidencia a importância de testar diferentes algoritmos, uma vez que modelos mais flexíveis, como árvores de decisão, podem capturar melhor padrões não lineares presentes nos dados da saúde pública.

A aplicação desses modelos demonstra o potencial do uso de técnicas de aprendizado de máquina na análise de dados do SUS, contribuindo para a identificação de padrões e suporte à tomada de decisão.


```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import mean_absolute_error

print("Carregando base processada...")
df = pd.read_parquet("data/processed/base_integrada.parquet")

print("Dados carregados com sucesso!")

print("Convertendo variáveis para formato numérico...")

df['VAL_TOT'] = pd.to_numeric(df['VAL_TOT'], errors='coerce')
df['DIAS_PERM'] = pd.to_numeric(df['DIAS_PERM'], errors='coerce')

df = df.dropna()

print(f"Total de registros após limpeza: {len(df)}")

y = df['DIAS_PERM']
X = df[['VAL_TOT', 'COMPLEX']]

print("Separando dados em treino e teste...")

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print("\nTreinando Regressão Linear...")

modelo_lr = LinearRegression()
modelo_lr.fit(X_train, y_train)

pred_lr = modelo_lr.predict(X_test)

erro_lr = mean_absolute_error(y_test, pred_lr)

print(f"Erro Regressão Linear (MAE): {erro_lr:.4f}")

print("\nTreinando Árvore de Decisão...")

modelo_dt = DecisionTreeRegressor(random_state=42)
modelo_dt.fit(X_train, y_train)

pred_dt = modelo_dt.predict(X_test)

erro_dt = mean_absolute_error(y_test, pred_dt)

print(f"Erro Árvore de Decisão (MAE): {erro_dt:.4f}")

print("\nComparando modelos...")

if erro_lr < erro_dt:
    print("Melhor modelo: Regressão Linear")
else:
    print("Melhor modelo: Árvore de Decisão")

print("\nProcesso de Machine Learning finalizado com sucesso!")
```
