1. Definição do Problema
2. Contexto e Justificativa
3. Objetivos
4. Estrutura de Controle e Rastreabilidade
5. Habilidades Aplicadas (Equipe)

# ETAPA 1 - Documentação de Contexto

Este documento detalha o escopo inicial do projeto, definindo o problema a ser resolvido, os dados utilizados e os objetivos esperados pela equipe.

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
