# ETAPA 4 - Aprendizagem de Máquina

O objetivo desta etapa foi desenvolver um modelo capaz de classificar o tempo de permanência hospitalar dos pacientes em categorias, permitindo uma melhor análise e apoio à gestão dos fluxos assistenciais no SUS.

A classificação do tempo de internação é relevante para a gestão hospitalar, pois possibilita identificar antecipadamente casos de curta, média e longa permanência, contribuindo para o planejamento de leitos, alocação de recursos e redução de gargalos no sistema de saúde.

Definição do Problema

O problema foi modelado como uma tarefa de classificação, em que o objetivo é prever a categoria de permanência hospitalar com base em variáveis disponíveis na base de dados.

A variável alvo foi definida como:

PERM_CLASSE (categoria de permanência hospitalar)

As variáveis utilizadas como entrada foram:

VAL_TOT (valor total da internação)
COMPLEX (nível de complexidade do procedimento)
Definição das Classes

A variável alvo foi criada a partir da variável original DIAS_PERM, sendo categorizada da seguinte forma:

Baixo (Curta permanência): até 3 dias
Médio (Permanência padrão): de 4 a 7 dias
Alto (Longa permanência): acima de 7 dias

Essa categorização permite uma interpretação mais prática dos resultados, facilitando a tomada de decisão por gestores de saúde.

Preparação dos Dados

Antes do treinamento dos modelos, foram realizadas as seguintes etapas:

Conversão das variáveis para formato numérico
Remoção de valores nulos
Criação da variável categórica de permanência (PERM_CLASSE)

Essas etapas garantem a consistência e qualidade dos dados utilizados no treinamento.

Divisão dos Dados

Em seguida, a base de dados foi dividida em dois subconjuntos:

80% para treino
20% para teste

Essa divisão permite avaliar a capacidade do modelo de generalizar para novos dados, ou seja, sua capacidade de fazer previsões corretas em situações não vistas durante o treinamento.

Modelo Utilizado

Foi utilizado o algoritmo de Árvore de Decisão (Decision Tree Classifier), escolhido por sua capacidade de interpretação, permitindo visualizar as regras utilizadas para classificar os pacientes.

Esse modelo é especialmente útil em contextos de gestão, pois possibilita entender os fatores que influenciam cada tipo de permanência hospitalar.

Métricas de Avaliação

Para avaliar o desempenho do modelo, foram utilizadas as seguintes métricas:

Acurácia: percentual de previsões corretas realizadas pelo modelo
Matriz de Confusão: análise detalhada dos acertos e erros entre as classes
Relatório de Classificação: métricas de precisão, recall e F1-score para cada classe

Essas métricas permitem uma avaliação completa da qualidade do modelo.

Resultados e Interpretação

Os resultados obtidos demonstraram que o modelo é capaz de classificar os pacientes em categorias de permanência com bom desempenho, permitindo identificar padrões relevantes nos dados.

A utilização da árvore de decisão também possibilitou a extração de regras de decisão, evidenciando como variáveis como o valor total da internação e o nível de complexidade influenciam na duração da permanência hospitalar.

De forma geral, o modelo apresentou boa capacidade de generalização, sendo capaz de apoiar análises voltadas à gestão hospitalar.

Considerações

A reformulação do problema de regressão para classificação permitiu tornar os resultados mais interpretáveis e aplicáveis ao contexto real do SUS, facilitando a identificação de perfis de internação e contribuindo para a tomada de decisão baseada em dados.

Essa abordagem evidencia o potencial do uso de técnicas de aprendizado de máquina na análise de dados da saúde pública, auxiliando na otimização dos fluxos assistenciais e no uso mais eficiente dos recursos disponíveis.

```python
import pandas as pd
import numpy as np

from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, export_text
from sklearn.metrics import classification_report, accuracy_score, confusion_matrix

print("Carregando base processada...")
df = pd.read_parquet("data/processed/base_integrada.parquet")

print("Dados carregados com sucesso!")

print("Convertendo variáveis para formato numérico...")

df['VAL_TOT'] = pd.to_numeric(df['VAL_TOT'], errors='coerce')
df['DIAS_PERM'] = pd.to_numeric(df['DIAS_PERM'], errors='coerce')
df['COMPLEX'] = pd.to_numeric(df['COMPLEX'], errors='coerce')

# Remover nulos
df = df.dropna()

print(f"Total de registros após limpeza: {len(df)}")

print("Criando categorias de permanência...")

def categorizar_permanencia(dias):
    if dias <= 3:
        return 'Baixo'
    elif dias <= 7:
        return 'Medio'
    else:
        return 'Alto'

df['PERM_CLASSE'] = df['DIAS_PERM'].apply(categorizar_permanencia)

print("Distribuição das classes:")
print(df['PERM_CLASSE'].value_counts())

y = df['PERM_CLASSE']
X = df[['VAL_TOT', 'COMPLEX']]

print("\nSeparando dados em treino e teste...")

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print("\nTreinando Árvore de Decisão...")

modelo_dt = DecisionTreeClassifier(max_depth=3, random_state=42)
modelo_dt.fit(X_train, y_train)

print("\nAvaliando modelo...")

pred_dt = modelo_dt.predict(X_test)

# Acurácia
acuracia = accuracy_score(y_test, pred_dt)
print(f"\nAcurácia do Modelo: {acuracia:.2%}")

# Relatório completo
print("\nRelatório de Classificação:")
print(classification_report(y_test, pred_dt))

# Matriz de confusão
print("\nMatriz de Confusão:")
print(confusion_matrix(y_test, pred_dt))

print("\n--- REGRAS DE DECISÃO DO MODELO ---")
tree_rules = export_text(modelo_dt, feature_names=['VAL_TOT', 'COMPLEX'])
print(tree_rules)

print("\nImportância das variáveis:")

importancias = pd.Series(modelo_dt.feature_importances_, index=X.columns)
print(importancias)

print("\nProcesso de Machine Learning finalizado com sucesso!")
```
