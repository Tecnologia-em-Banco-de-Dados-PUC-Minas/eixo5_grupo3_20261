# ETAPA 4 - Aprendizagem de Máquina

O objetivo desta etapa foi desenvolver um modelo capaz de prever a categoria de permanência hospitalar do paciente (Baixa, Média ou Alta). Essa classificação é crucial para a gestão de leitos e planejamento de custos assistenciais, permitindo que o hospital identifique precocemente casos de longa permanência.

Definição das Classes:

Baixa (Curta): Até 3 dias (Foco em giro rápido de leito).

Média: De 4 a 7 dias (Padrão assistencial comum).

Alta (Longa): Acima de 7 dias (Casos complexos e maior custo).

Modelos Utilizados:
Para explicar as razões das classificações, utilizamos a Árvore de Decisão, pois ela permite a visualização das regras (ex: se o valor total for > X e a complexidade for Y, então a permanência é Alta). Comparamos seu desempenho com um modelo de Regressão Logística (para fins de classificação).

Métricas:
Diferente da regressão simples, aqui utilizamos a Acurácia e a Matriz de Confusão, que mostram onde o modelo está acertando ou confundindo os perfis de internação.


```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, export_text
from sklearn.metrics import classification_report, accuracy_score

# 1. Carregamento e Preparação
df = pd.read_parquet("data/processed/base_integrada.parquet")
df['VAL_TOT'] = pd.to_numeric(df['VAL_TOT'], errors='coerce')
df['DIAS_PERM'] = pd.to_numeric(df['DIAS_PERM'], errors='coerce')
df = df.dropna()

# 2. CRIAÇÃO DAS CLASSES (O que o professor pediu)
def categorizar_permanencia(dias):
    if dias <= 3: return 'Baixo'
    elif dias <= 7: return 'Medio'
    else: return 'Alto'

df['PERM_CLASSE'] = df['DIAS_PERM'].apply(categorizar_permanencia)

# 3. Preparação para o Modelo
X = df[['VAL_TOT', 'COMPLEX']]
y = df['PERM_CLASSE']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 4. Treinando Árvore de Decisão (Excelente para explicar o "porquê")
modelo_dt = DecisionTreeClassifier(max_depth=3, random_state=42)
modelo_dt.fit(X_train, y_train)

# 5. Avaliação
pred_dt = modelo_dt.predict(X_test)
print(f"Acurácia do Modelo: {accuracy_score(y_test, pred_dt):.2%}")
print("\nRelatório de Classificação:\n", classification_report(y_test, pred_dt))

# 6. MOSTRANDO A LÓGICA (O "porquê" de ser alto, médio ou baixo)
print("\n--- REGRAS DE DECISÃO DO MODELO ---")
tree_rules = export_text(modelo_dt, feature_names=['VAL_TOT', 'COMPLEX'])
print(tree_rules)

# 7. Importância das Variáveis
importancias = pd.Series(modelo_dt.feature_importances_, index=X.columns)
print("\nImportância das colunas na classificação:")
print(importancias)
```
