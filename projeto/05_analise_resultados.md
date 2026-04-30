# ETAPA 5 - Análise dos Resultados

## Performance Comparativa entre Algoritmos

Foi analisada a performance do modelo de Árvore de Decisão com base nas métricas de classificação, permitindo avaliar sua capacidade de distinguir corretamente as categorias de permanência hospitalar.

Acurácia: O modelo demonstrou uma taxa de acerto de aproximadamente [X]%, sendo eficaz na distinção entre casos de curta, média e longa permanência.
Matriz de Confusão: Esta métrica revelou que o modelo apresenta melhor desempenho na identificação da classe “Alto” (acima de 7 dias), o que é relevante para a gestão hospitalar, já que erros nessa categoria impactam diretamente a ocupação de leitos.
Taxa de Erros: Os erros concentram-se principalmente entre as classes “Baixo” e “Médio”, indicando que casos intermediários são mais difíceis de classificar devido à proximidade de características.

## Análise de Preditores e Variáveis de Entrada

Buscando identificar quais variáveis possuem maior impacto na classificação, foram observados os seguintes pontos:

Impacto do VAL_TOT: O valor total da internação apresentou forte influência no modelo, indicando que custos mais elevados tendem a estar associados a maiores tempos de permanência.
Influência da COMPLEX: A variável de complexidade atua como um fator determinante, especialmente na separação de casos de curta permanência, onde procedimentos menos complexos tendem a resultar em altas mais rápidas.

## Parâmetros e Ajuste do Modelo

Para este conjunto de dados, o ajuste do parâmetro max_depth da Árvore de Decisão foi essencial para obter um bom desempenho.

Uma profundidade de 3 níveis mostrou-se adequada, pois:

Evita o overfitting (quando o modelo se ajusta excessivamente aos dados de treino)
Mantém a capacidade de generalização para novos dados
Gera regras de decisão simples e interpretáveis

## Definição da Abordagem Performática

Com base na análise realizada, a Árvore de Decisão foi identificada como um modelo adequado para o problema proposto.

Além de apresentar bom desempenho nas métricas, sua principal vantagem está na interpretabilidade, permitindo compreender de forma clara os critérios utilizados para classificar os pacientes.

Essa característica é especialmente relevante no contexto da saúde pública, onde a transparência e a explicação das decisões são fundamentais para a aplicação prática dos resultados.
