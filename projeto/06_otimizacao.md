# ETAPA 6 - Otimização

A etapa de otimização teve como objetivo confrontar o planejamento estratégico de governança de dados, definido na Etapa 2, com as ações efetivamente executadas ao longo do projeto, garantindo que todo o ciclo de vida da informação seguisse padrões de qualidade, segurança e rastreabilidade.

## Confronto com o Planejamento de Governança

O planejamento inicial previa uma estrutura organizada em camadas e a definição clara de responsabilidades. Na prática, a implementação foi consolidada nos seguintes pilares:

Prestação de Contas (Accountability):
Foi estabelecido um pipeline de dados bem definido, no qual cada etapa de transformação (da camada raw para processed) é documentada, permitindo rastrear a origem dos dados e identificar possíveis inconsistências.
Transparência e Administração de Dados:
A utilização do formato Parquet e a organização em diretórios padronizados garantiram uma estrutura de dados eficiente, legível e auditável por toda a equipe, facilitando a administração dos ativos de informação.
Conformidade com a LGPD:
Apesar de os dados do DATASUS serem públicos e anonimizados, foi aplicado o princípio da minimização de dados, utilizando apenas as variáveis essenciais (VAL_TOT, COMPLEX, DIAS_PERM), reduzindo riscos e promovendo o uso responsável das informações.
Padrões de Qualidade de Dados:
Foram implementadas rotinas de limpeza, tratamento de valores nulos e padronização de tipos, assegurando que os modelos de aprendizado de máquina operassem sobre dados consistentes e confiáveis.

## Histórico de Atualizações e Otimizações

Ao longo do desenvolvimento, o projeto passou por evoluções importantes que impactaram diretamente sua eficiência e qualidade analítica:

Eficiência de Armazenamento:
A migração do formato CSV para Parquet reduziu significativamente o uso de armazenamento e melhorou o desempenho de leitura e processamento dos dados.
Estruturação do Pipeline:
A separação clara entre coleta, pré-processamento e modelagem tornou o processo reproduzível e escalável.
Refinamento do Problema:
A principal otimização ocorreu na Etapa 4, com a mudança de abordagem de regressão para classificação, tornando os resultados mais interpretáveis e alinhados ao contexto de gestão hospitalar.
Identificação de Limitações:
Foi identificado o desbalanceamento das classes na variável de permanência, impactando a capacidade do modelo em prever a classe “Médio”, o que direciona melhorias futuras.

## Propostas Futuras e Oportunidades de Mercado

Para a continuidade e evolução do projeto, destacam-se as seguintes oportunidades:

Melhoria Preditiva:
Aplicação de técnicas de balanceamento de dados, como oversampling (ex: SMOTE), além da utilização de algoritmos mais robustos, como Random Forest e Gradient Boosting.
Expansão de Variáveis:
Incorporação de novas variáveis, como idade, diagnóstico (CID) e características sociodemográficas, permitindo análises mais completas e maior poder preditivo.
Visualização e Aplicação Prática:
Desenvolvimento de dashboards interativos para acompanhamento em tempo real dos indicadores e previsões.
Oportunidade de Mercado:
O modelo apresenta alto potencial de aplicação em Secretarias de Saúde, hospitais e operadoras de saúde suplementar. A solução pode ser evoluída para uma ferramenta de apoio à decisão, permitindo antecipar demandas por leitos, prever custos de internação e otimizar a alocação de recursos, contribuindo para maior eficiência operacional e melhor atendimento ao paciente.

## Considerações Finais

A etapa de otimização consolidou a maturidade do projeto ao alinhar prática e planejamento, evidenciando a importância da governança de dados, da qualidade da informação e da interpretação dos modelos de aprendizado de máquina.

Os resultados obtidos demonstram que, mesmo com limitações, é possível gerar valor real para a gestão da saúde pública por meio da análise de dados, reforçando o potencial da ciência de dados como ferramenta estratégica no contexto do SUS.
