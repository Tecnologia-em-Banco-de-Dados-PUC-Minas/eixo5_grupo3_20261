# ETAPA 2 - Coleta de Dados

O processo de coleta de dados deste projeto baseia-se na extração automatizada de microdados públicos do DATASUS, com foco nas bases do Sistema de Informações Hospitalares (SIH), Sistema de Informações Ambulatoriais (SIA) e Cadastro Nacional de Estabelecimentos de Saúde (CNES). A automação é essencial para garantir a reprodutibilidade da pesquisa e a atualização constante das informações analisadas, minimizando intervenções manuais que estão frequentemente propensas a falhas operacionais.

Para superar o desafio técnico do formato proprietário e compactado (.dbc) utilizado pelo Ministério da Saúde, a arquitetura de extração utiliza a biblioteca Python PySUS. Esta ferramenta atua como um conector direto com os servidores FTP do governo, encarregando-se do download, da descompactação estrutural e da conversão imediata dos registros para DataFrames da biblioteca Pandas. Esta abordagem otimiza o fluxo de trabalho da equipe de engenharia de dados e elimina a necessidade de conversores externos.

A estratégia de armazenamento adota o formato Parquet para os dados brutos. Esta escolha arquitetônica garante uma compressão superior em relação aos arquivos CSV tradicionais e preserva a tipagem original das colunas. Este cuidado com o esquema de dados desde a origem acelera drasticamente as consultas nas fases subsequentes de pré-processamento e análise exploratória, mantendo a eficiência computacional do projeto.

## Adendo: Execução em Ambiente 

Durante a execução do processo de coleta, foram identificadas limitações no ambiente local para realizar a extração completa dos dados. Como solução, foi utilizado o ambiente Google Colab, que oferece maior capacidade de processamento e melhor compatibilidade com as bibliotecas utilizadas.

A extração foi realizada separadamente para cada base (SIH, SIA e CNES), devido a limitações de processamento e estabilidade do ambiente.

Nesse contexto, o processo ocorreu da seguinte forma:

Execução do script de extração no Google Colab
Download dos dados via PySUS
Conversão dos dados para o formato Parquet
Transferência dos arquivos para o ambiente local (Visual Studio Code)

Essa abordagem garantiu a execução completa da coleta sem comprometer a integridade dos dados.

Como a extração foi realizada de forma separada para cada base, abaixo é apresentado um exemplo de script utilizado no processo, neste caso para a base CNES.

```python
import os
import pandas as pd
from pysus.online_data import CNES

estado = 'MG'
ano = 2023
mes = 1
diretorio = "dados_brutos"

def processar_download(objeto_pysus):
    if isinstance(objeto_pysus, list):
        return pd.concat([item.to_dataframe() for item in objeto_pysus])
    return objeto_pysus.to_dataframe()

print("--- Finalizando com o CNES ---")

try:
    print("Baixando CNES (Grupo ST)...")
    data_cnes = CNES.download(group='ST', states=estado, years=ano, months=mes)
    df_cnes = processar_download(data_cnes)
    df_cnes.to_parquet(f"{diretorio}/cnes_mg.parquet", index=False)
    print(f"CNES salvo! ({len(df_cnes)} registros)")
    print("Missão concluída.")

except Exception as e:
    print(f"Erro no CNES: {e}")
```
## Adendo: Validação dos Dados

Após a coleta e transferência dos arquivos para o ambiente local, foi realizada uma etapa de validação utilizando o Visual Studio Code.
Nessa fase, foi desenvolvido um script simples para exploração dos dados, permitindo:

Verificar a quantidade de registros
Visualizar as primeiras linhas das tabelas
Identificar as colunas disponíveis
Confirmar a integridade dos arquivos

Essa validação garantiu que os dados foram carregados corretamente e estão prontos para a etapa de pré-processamento.

```python
import pandas as pd
import os

pasta_raw = os.path.join("data", "raw")
pasta_processed = os.path.join("data", "processed") 

os.makedirs(pasta_processed, exist_ok=True)

def limpar_sih():
    print("--- Processando SIH (Internações) ---")
    df = pd.read_parquet(os.path.join(pasta_raw, "sih_mg.parquet"))
    
    colunas_foco = ['UF_ZI', 'ANO_CMPT', 'MES_CMPT', 'MUNIC_RES', 'NASC', 'SEXO', 
                    'UTI_INT_TO', 'VAL_TOT', 'DIAG_PRINC', 'DT_INTER', 'DT_SAIDA', 'CNES']
    df_limpo = df[colunas_foco].copy()
    
    df_limpo['DT_INTER'] = pd.to_datetime(df_limpo['DT_INTER'], format='%Y%m%d', errors='coerce')
    df_limpo['DT_SAIDA'] = pd.to_datetime(df_limpo['DT_SAIDA'], format='%Y%m%d', errors='coerce')
    
    df_limpo = df_limpo.dropna(subset=['DT_INTER', 'VAL_TOT'])
    
    print(f"SIH limpo! Registros restantes: {len(df_limpo)}")
    
    df_limpo.to_parquet(os.path.join(pasta_processed, "sih_mg_limpo.parquet"))

limpar_sih()
```
## Planejamento de Governança e Modelo de Dados Inicial

A governança de dados neste projeto atua como o alicerce metodológico para garantir que as informações extraídas do DATASUS sejam rastreáveis, seguras e perfeitamente alinhadas aos objetivos estratégicos de otimização dos fluxos assistenciais. Esta estrutura define o controle absoluto sobre todo o ciclo de vida da informação, desde o exato momento da ingestão nos servidores FTP até a disponibilização final para os modelos de aprendizado de máquina, assegurando que as decisões gerenciais do projeto sejam baseadas em dados íntegros e auditáveis.

A estrutura de gerenciamento distribui as responsabilidades operacionais em funções complementares dentro da equipe. A frente de Análise de Negócios assume a liderança no mapeamento das regras e na garantia de que as variáveis coletadas respondam diretamente aos problemas de gargalos do SUS, assegurando o cumprimento da Lei Geral de Proteção de Dados mesmo diante de bases públicas. A Engenharia de Dados concentra a responsabilidade pela construção e manutenção dos pipelines automatizados, garantindo a resiliência da arquitetura de extração. Por fim, a administração da governança centraliza a documentação contínua e a homologação dos dicionários de dados, garantindo que todo o grupo compartilhe o mesmo entendimento semântico sobre as tabelas manipuladas.

Os procedimentos adotados ao longo do ciclo de vida dos dados seguem uma arquitetura de particionamento em camadas rígidas. A etapa primária de ingestão aloca os arquivos convertidos na camada bruta, preservando a integridade original da fonte governamental sem qualquer modificação. Posteriormente, os procedimentos de transformação aplicam as regras de negócio focadas na limpeza, no tratamento de valores nulos e na padronização tipológica, movendo as informações homologadas para a camada de dados confiáveis. O ciclo se consolida na camada refinada, onde os dados são estruturados analiticamente para o consumo exclusivo dos algoritmos preditivos.

O modelo de dados inicial foi concebido sob uma perspectiva de modelagem dimensional, adotando uma estrutura em esquema estrela para maximizar o desempenho das consultas e cruzamentos. O núcleo deste modelo é composto por uma tabela de fatos central, responsável por registrar os eventos transacionais contendo as internações e os procedimentos ambulatoriais diários. Esta tabela de fatos relaciona-se intrinsecamente com dimensões de contexto, sendo a principal delas a dimensão de estabelecimentos de saúde. O relacionamento dimensional ocorre através do código identificador único de cada hospital, permitindo que a equipe cruze o histórico de atendimentos com a capacidade instalada das unidades, evidenciando de forma matemática quais polos operam com sobrecarga e quais apresentam ociosidade estrutural.

## Adendo: Automação, Resiliência e Tratamento de Erros

A execução da engenharia de dados em bases governamentais exige uma arquitetura estritamente resiliente contra instabilidades de rede. Os servidores FTP do DATASUS sofrem quedas frequentes de conexão, o que muitas vezes resulta no descarregamento de arquivos na extensão original corrompidos ou incompletos. Para mitigar esse risco operacional, a esteira de coleta foi desenhada com um mecanismo de validação de integridade pós-download.

Antes de converter o arquivo para o formato estruturado Parquet, o script verifica se o tamanho do arquivo em bytes corresponde ao padrão histórico esperado para o estado de Minas Gerais naquele mês específico. Caso uma divergência seja detectada ou a conexão caia, o sistema descarta o arquivo defeituoso e aciona um laço de repetição com atraso progressivo, evitando o bloqueio do endereço de rede pelo sistema de segurança do governo. Somente após a validação completa do lote mensal é que os dados são liberados para a camada de armazenamento bruto, garantindo que as análises futuras de fluxo hospitalar não sejam comprometidas por dados truncados.

## Adendo: Configuração do Ambiente Virtual de Desenvolvimento

A reprodutibilidade desta esteira de extração exige o isolamento das dependências do projeto através de um ambiente virtual Python. Esta prática impede que as bibliotecas utilizadas na coleta entrem em conflito com pacotes instalados globalmente nas máquinas dos desenvolvedores, garantindo que o script de conexão execute de forma idêntica para todos os integrantes da equipe.

Para inicializar o ambiente de desenvolvimento, deve-se abrir a interface de linha de comando na raiz do diretório do projeto e executar a criação do ambiente virtual com `python -m venv venv`. Em sistemas Windows, a ativação ocorre através da execução de `venv\Scripts\activate`, enquanto em ambientes Unix utiliza-se `source venv/bin/activate`. Com o ambiente isolado, a instalação das dependências homologadas é feita executando `pip install -r requirements.txt`, que fará o download das versões exatas do PySUS, Pandas e PyArrow. A partir deste momento, a máquina local possui a arquitetura idêntica à planejada para o ambiente de produção.
