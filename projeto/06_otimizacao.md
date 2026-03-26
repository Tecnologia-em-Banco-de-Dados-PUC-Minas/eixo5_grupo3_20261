# ETAPA 6 - Otimização

A etapa de otimização teve como objetivo avaliar as decisões adotadas ao longo do projeto, comparando o planejamento inicial de governança de dados com as implementações realizadas na prática.

Durante a Etapa 2, foi definido um modelo de governança baseado na organização em camadas (dados brutos, dados processados e dados analíticos), além da definição de responsabilidades e controle do ciclo de vida dos dados. Na execução do projeto, esse planejamento foi parcialmente implementado, especialmente na separação entre dados brutos (raw) e dados processados (processed), garantindo maior organização e rastreabilidade das informações.

Em relação à segurança e conformidade com a legislação, destaca-se que os dados utilizados são provenientes de bases públicas do DATASUS, não contendo informações sensíveis diretamente identificáveis. Ainda assim, foram respeitados os princípios da Lei Geral de Proteção de Dados (LGPD), especialmente no que se refere ao uso responsável das informações e à transparência no tratamento dos dados.

No aspecto de qualidade dos dados, foram aplicadas técnicas de limpeza, tratamento de valores nulos, padronização de formatos e integração entre diferentes bases, garantindo maior confiabilidade para a análise e modelagem. Essas ações contribuíram diretamente para a melhoria dos resultados obtidos nos modelos de aprendizado de máquina.

Como otimizações implementadas ao longo do projeto, destacam-se a redução do volume de dados por meio da seleção de variáveis relevantes, a conversão para formatos mais eficientes (Parquet) e a estruturação de um pipeline de processamento reproduzível.

Como propostas futuras, recomenda-se a ampliação do conjunto de variáveis utilizadas nos modelos, a inclusão de novas bases de dados do DATASUS, bem como a aplicação de algoritmos mais avançados, como Random Forest e Gradient Boosting, visando melhorar a performance preditiva.

Além disso, o projeto apresenta potencial de aplicação no mercado, especialmente em órgãos públicos e instituições de saúde, podendo auxiliar na análise de fluxo de pacientes, identificação de gargalos no sistema e otimização da alocação de recursos hospitalares.

Por fim, a apresentação final do projeto irá consolidar os resultados obtidos, demonstrando o processo completo desde a coleta até a modelagem e análise dos dados, evidenciando o valor da utilização de técnicas de ciência de dados no contexto da saúde pública.
