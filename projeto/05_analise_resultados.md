# ETAPA 5 - Análise dos Resultados

A etapa de análise dos resultados teve como objetivo avaliar o desempenho dos modelos de aprendizado de máquina aplicados ao conjunto de dados processado, identificando qual abordagem apresentou melhor capacidade preditiva.

Foram utilizados dois algoritmos distintos: Regressão Linear e Árvore de Decisão, ambos aplicados com o objetivo de prever o tempo de permanência dos pacientes (DIAS_PERM) com base nas variáveis disponíveis.

A avaliação dos modelos foi realizada por meio da métrica Mean Absolute Error (MAE), que mede o erro médio absoluto entre os valores previstos e os valores reais. Essa métrica foi escolhida por ser adequada para problemas de regressão, permitindo uma interpretação direta do erro médio em unidades reais.

Os resultados obtidos foram:

Regressão Linear: MAE = 3.6510
Árvore de Decisão: MAE = 3.4439

A partir dos resultados, observa-se que o modelo de Árvore de Decisão apresentou melhor desempenho, com menor erro médio, indicando maior precisão nas previsões em comparação com a Regressão Linear.

A diferença de desempenho pode ser explicada pela capacidade da Árvore de Decisão em capturar relações não lineares entre as variáveis, enquanto a Regressão Linear assume um comportamento linear dos dados, o que nem sempre representa a realidade dos dados de saúde.

Além disso, a variável VAL_TOT mostrou influência na previsão do tempo de permanência, indicando que custos mais elevados podem estar associados a internações mais longas, embora essa relação não seja estritamente linear.

Dessa forma, conclui-se que o modelo de Árvore de Decisão é mais adequado para o problema proposto, apresentando melhor capacidade de generalização para o conjunto de dados analisado.
