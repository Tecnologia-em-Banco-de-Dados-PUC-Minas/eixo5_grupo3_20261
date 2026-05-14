# ETAPA 5 - Análise dos Resultados

## Performance Comparativa entre Algoritmos

Foi analisada a performance do modelo de Árvore de Decisão com base nas métricas de classificação, permitindo avaliar sua capacidade de distinguir corretamente as categorias de permanência hospitalar.

Acurácia: O modelo apresentou uma taxa de acerto de aproximadamente 63,72%, indicando desempenho moderado na classificação dos pacientes.

Matriz de Confusão: A análise revelou que o modelo apresenta bom desempenho na identificação da classe “Baixo” e desempenho razoável para a classe “Alto”. No entanto, a classe “Médio” não foi corretamente prevista pelo modelo, evidenciando uma limitação importante.

Taxa de Erros: Os erros concentram-se principalmente na incapacidade do modelo em distinguir a classe intermediária (“Médio”), que foi frequentemente classificada como “Baixo”.

## Análise de Preditores e Variáveis de Entrada

Buscando identificar quais variáveis possuem maior impacto na classificação, foram observados os seguintes pontos:

Impacto do VAL_TOT: O valor total da internação apresentou forte influência no modelo, com importância de aproximadamente 81%, indicando que custos mais elevados estão fortemente associados a internações mais longas.
Influência da COMPLEX: A variável de complexidade apresentou menor impacto (18%), mas ainda contribui para a diferenciação entre casos mais simples e mais complexos.

## Parâmetros e Ajuste do Modelo

Parâmetros e Ajuste do Modelo

Para este conjunto de dados, o parâmetro max_depth = 3 foi utilizado para limitar a complexidade da árvore.

Essa configuração:

Evita overfitting
Mantém a interpretabilidade do modelo
Gera regras de decisão claras

## Limitações Identificadas

Durante a análise, foi identificado um problema de desbalanceamento de classes, onde a categoria “Baixo” possui maior quantidade de registros em comparação às demais.

Esse fator influenciou diretamente o modelo, fazendo com que ele priorizasse a previsão das classes mais frequentes e ignorasse a classe “Médio”.

Essa limitação indica a necessidade de técnicas adicionais, como:

Balanceamento de dados (oversampling/undersampling)
Ajuste de pesos das classes
Teste com outros algoritmos

## Definição da Abordagem Performática

A Árvore de Decisão mostrou-se adequada por sua capacidade de interpretação e geração de regras claras.

Apesar das limitações, o modelo conseguiu identificar padrões relevantes, especialmente relacionados ao impacto do custo da internação no tempo de permanência.

Essa abordagem demonstra o potencial do uso de aprendizado de máquina para apoiar a gestão hospitalar, mesmo em cenários com desafios como desbalanceamento de dados.

# Confecção e Apresentação de Relatórios e Dashboards

Com o objetivo de facilitar a interpretação dos resultados obtidos, foram desenvolvidas visualizações e relatórios contendo as principais métricas geradas pelo modelo de aprendizado de máquina.

Os relatórios apresentam informações como:

Distribuição das classes de permanência hospitalar
Acurácia do modelo
Matriz de confusão
Importância das variáveis utilizadas
Regras de decisão geradas pela Árvore de Decisão

Além disso, foram desenvolvidas visualizações em HTML e CSS para representar de forma mais intuitiva os resultados do modelo, permitindo uma análise mais clara dos padrões identificados nos dados.

Esses dashboards contribuem para a interpretação dos resultados e demonstram o potencial da utilização de ferramentas visuais no apoio à tomada de decisão na gestão hospitalar.

# Comparação dos Algoritmos Utilizados

Inicialmente, o projeto utilizou uma abordagem de regressão, aplicando os algoritmos Regressão Linear e Árvore de Decisão Regressora para prever o número exato de dias de internação.

No entanto, observou-se que essa abordagem apresentava baixa interpretabilidade para o contexto de gestão hospitalar, dificultando a aplicação prática dos resultados.

Diante disso, o problema foi reformulado para uma abordagem de classificação, categorizando os pacientes em grupos de curta, média e longa permanência.

Após a reformulação, foi utilizado o algoritmo Árvore de Decisão Classificadora, que apresentou melhor capacidade de interpretação e maior aderência ao objetivo do projeto.

A principal vantagem observada foi a possibilidade de compreender as regras utilizadas pelo modelo, permitindo identificar quais fatores influenciam diretamente o tempo de permanência hospitalar.
