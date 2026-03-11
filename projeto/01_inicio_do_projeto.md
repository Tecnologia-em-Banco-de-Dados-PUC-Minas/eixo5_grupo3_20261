
# ETAPA 1 - Documentação de Contexto

Este documento detalha o escopo inicial do projeto, definindo o problema a ser resolvido, os dados utilizados e os objetivos esperados pela equipe.

## Introdução e Contexto Histórico
Idealizado por Hésio Cordeiro e Sérgio Arouca, o Sistema Único de Saúde (SUS) surgiu como resultado do movimento sanitário brasileiro, que defendia a saúde como um direito universal e dever do Estado. O sistema foi criado como resposta ao modelo de saúde excludente e à forte privatização existente durante o período da ditadura, buscando garantir atendimento público, gratuito, universal e descentralizado para toda a população.

Sua base legal está na Constituição Federal de 1988, especialmente nos artigos de 196 a 200, e foi regulamentado pelas leis orgânicas da saúde, como a Lei n° 8.080/1990, que organiza o funcionamento do sistema, e a Lei n° 8.142/1990, que garante a participação da população na gestão da saúde por meio de conselhos e conferências. O SUS representa uma importante conquista social, fruto da mobilização de profissionais de saúde, sanitaristas e movimentos sociais.

Nesse contexto, diante da grande demanda por serviços de saúde e da complexidade da rede pública de atendimento, torna-se fundamental desenvolver estratégias que garantam maior eficiência e organização no funcionamento do sistema. A solução de otimização de fluxos assistenciais no SUS surge como uma abordagem voltada para aprimorar os processos de atendimento.

A proposta consiste no mapeamento, organização e padronização da trajetória do paciente dentro do serviço de saúde, definindo um caminho lógico que o paciente percorre. Tendo como principal objetivo garantir eficiência e segurança aos pacientes e gerenciar a movimentação do mesmo entre os diferentes setores (recepção, triagem, atendimento e alta), evitando congestionamento e otimizando recursos.

## 1. Definição do Problema
O problema central a ser abordado é a **ineficiência e os gargalos no fluxo de agendamentos de procedimentos de alta complexidade no SUS**. 

Atualmente, a discrepância entre a oferta de leitos/exames e a demanda gera filas que, muitas vezes, sofrem com falhas de atualização cadastral e falta de rastreabilidade. Isso resulta em "vazios assistenciais", onde recursos (como equipamentos e horas médicas) podem ficar ociosos em determinadas unidades, enquanto pacientes aguardam em longas filas em outras.

## 2. Contexto e Conjunto de Dados
Para analisar e propor soluções para este cenário, o projeto utilizará microdados públicos disponibilizados pelo **DATASUS** (Departamento de Informática do SUS), acessados via sistema TABNET. 

Os conjuntos de dados selecionados são:
* **SIA (Sistema de Informações Ambulatoriais):** Para mapear o volume e o fluxo de procedimentos realizados de forma ambulatorial.
* **SIH (Sistema de Informações Hospitalares):** Para entender o tempo médio de internação e as taxas de ocupação de leitos.
* **CNES (Cadastro Nacional de Estabelecimentos de Saúde):** Para validar a capacidade instalada de cada unidade de saúde (quantidade de leitos, equipamentos e profissionais).

**Justificativa e Viabilidade:**
A escolha dessas bases se justifica por serem dados oficiais, de alta confiabilidade e com grande granularidade. Além disso, por serem dados anonimizados pelo Ministério da Saúde, o projeto é totalmente viável e mantém conformidade estrita com a LGPD (Lei Geral de Proteção de Dados), garantindo a segurança e a ética no manuseio das informações.

## 3. Objetivos e Resultados Esperados
O projeto foi desenhado a partir da perspectiva do usuário (pacientes e gestores de saúde) e tem como metas:

* **Objetivo Geral:** Desenvolver um modelo de análise de dados que identifique os principais gargalos operacionais no fluxo assistencial e proponha uma redistribuição inteligente da demanda.
* **Objetivos Específicos:**
    * Mapear a capacidade instalada versus a utilização real das unidades de saúde.
    * Identificar padrões de ineficiência ou ociosidade em unidades periféricas.
* **Resultados Esperados:** Espera-se entregar insights acionáveis (como relatórios ou dashboards) que sirvam de base para gestores públicos tomarem decisões pautadas em evidências, visando a redução do tempo de espera e o melhor aproveitamento dos recursos públicos.

## 4. Processo de Controle e Rastreabilidade
Para garantir a confiabilidade técnica, o projeto estabelecerá pontos de controle rígidos ao longo do ciclo de vida dos dados (Data Lineage):

1.  **Origem:** Os dados serão extraídos das bases do DATASUS preservando seu formato original.
2.  **Transformação (Controle):** Durante o pré-processamento (ETAPA 3), serão implementados logs de validação para rastrear qualquer limpeza de dados inconsistentes, nulos ou outliers.
3.  **Destino:** Os dados tratados serão armazenados em um repositório analítico, assegurando que o modelo final de otimização consuma apenas informações verificadas e íntegras.

## Adendo: Métricas de Sucesso e Regras de Negócio
Para mensurar a eficácia da proposta de otimização no fluxo assistencial, o projeto estabelece três Indicadores-Chave de Desempenho principais. O primeiro é o Tempo Médio de Espera, que quantifica os dias transcorridos entre a inserção do paciente no sistema de regulação e a efetiva realização do procedimento de alta complexidade. O segundo indicador foca na Taxa de Ocupação Hospitalar, cruzando o volume de internações do Sistema de Informações Hospitalares com a quantidade de leitos ativos registrados no Cadastro Nacional de Estabelecimentos de Saúde. O terceiro indicador aborda o Índice de Ociosidade Ambulatorial, rastreando equipamentos de alto custo que apresentam volume de utilização inferior à capacidade instalada declarada. 

A regra de negócio fundamental estabelece que qualquer proposta de redistribuição de pacientes deve priorizar o atendimento na macrorregião de saúde de origem, minimizando o deslocamento intermunicipal e respeitando as diretrizes de hierarquização do SUS aplicadas em Belo Horizonte e região metropolitana.
