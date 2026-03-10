# ETAPA 2 - Coleta de Dados

O processo de coleta de dados deste projeto baseia-se na extração automatizada de microdados públicos do DATASUS, com foco nas bases do Sistema de Informações Hospitalares (SIH), Sistema de Informações Ambulatoriais (SIA) e Cadastro Nacional de Estabelecimentos de Saúde (CNES). A automação é essencial para garantir a reprodutibilidade da pesquisa e a atualização constante das informações analisadas, minimizando intervenções manuais que estão frequentemente propensas a falhas operacionais.

Para superar o desafio técnico do formato proprietário e compactado (.dbc) utilizado pelo Ministério da Saúde, a arquitetura de extração utiliza a biblioteca Python PySUS. Esta ferramenta atua como um conector direto com os servidores FTP do governo, encarregando-se do download, da descompactação estrutural e da conversão imediata dos registros para DataFrames da biblioteca Pandas. Esta abordagem otimiza o fluxo de trabalho da equipe de engenharia de dados e elimina a necessidade de conversores externos.

A estratégia de armazenamento adota o formato Parquet para os dados brutos. Esta escolha arquitetônica garante uma compressão superior em relação aos arquivos CSV tradicionais e preserva a tipagem original das colunas. Este cuidado com o esquema de dados desde a origem acelera drasticamente as consultas nas fases subsequentes de pré-processamento e análise exploratória, mantendo a eficiência computacional do projeto.

O script unificado abaixo consolida a extração das três fontes para o estado de Minas Gerais. O código inclui pontos de controle que registram o volume de linhas e colunas de cada base imediatamente após o download, assegurando a integridade inicial da linhagem de dados antes da gravação no diretório de destino.

```python
import os
import pandas as pd
from pysus.online_data import SIH, SIA, CNES

estado_alvo = 'MG'
ano_referencia = 2023
mes_referencia = 1
diretorio_saida = "../data/raw"

os.makedirs(diretorio_saida, exist_ok=True)

print("Iniciando a extração da base hospitalar (SIH)...")
df_sih = SIH.download(estado_alvo, ano_referencia, mes_referencia)
print("SIH extraído. Dimensões:", df_sih.shape)
df_sih.to_parquet(f"{diretorio_saida}/sih_bruto_{estado_alvo}_{ano_referencia}_{mes_referencia:02d}.parquet", index=False)

print("Iniciando a extração da base ambulatorial (SIA)...")
df_sia = SIA.download(estado_alvo, ano_referencia, mes_referencia)
print("SIA extraído. Dimensões:", df_sia.shape)
df_sia.to_parquet(f"{diretorio_saida}/sia_bruto_{estado_alvo}_{ano_referencia}_{mes_referencia:02d}.parquet", index=False)

print("Iniciando a extração do cadastro de estabelecimentos (CNES)...")
df_cnes = CNES.download('ST', estado_alvo, ano_referencia, mes_referencia)
print("CNES extraído. Dimensões:", df_cnes.shape)
df_cnes.to_parquet(f"{diretorio_saida}/cnes_bruto_{estado_alvo}_{ano_referencia}_{mes_referencia:02d}.parquet", index=False)

print("Processo de coleta concluído com sucesso e arquivos isolados no diretório raw.")

## Adendo: Automação, Resiliência e Tratamento de Erros

A execução prática da extração de dados exige uma arquitetura resiliente a falhas de rede e indisponibilidades temporárias dos servidores FTP do governo. A esteira de coleta foi projetada para operar com rotinas de tentativas múltiplas e registro detalhado de eventos. Caso a conexão com o repositório do Ministério da Saúde sofra interrupção durante o descarregamento dos arquivos compactados, o script aciona um mecanismo de espera e nova tentativa, evitando a interrupção silenciosa do pipeline de dados.


Além da resiliência de rede, a automação prevê a execução periódica e o isolamento dos dados. Como as atualizações do DATASUS ocorrem em ciclos mensais, o ambiente de produção simulará um agendamento temporal que aciona o script de forma autônoma. Todo o volume extraído é validado através de funções de contagem de bytes e verificação de integridade estrutural antes da conversão definitiva para o formato Parquet. Este rigor prático assegura que a etapa seguinte de pré-processamento consuma exclusivamente lotes de dados validados e completos. O script abaixo ilustra a aplicação prática de logs e tratamento de exceções na extração.

```python
import os
import logging
import pandas as pd
from pysus.online_data import SIH
from time import sleep

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')
diretorio_saida = "../data/raw"
os.makedirs(diretorio_saida, exist_ok=True)

estado_alvo = 'MG'
ano_referencia = 2023
mes_referencia = 1
tentativas_maximas = 3

logging.info("Iniciando rotina de extração com tratamento de exceções.")

for tentativa in range(1, tentativas_maximas + 1):
    try:
        logging.info(f"Tentativa {tentativa} de descarregamento da base hospitalar (SIH)...")
        df_sih = SIH.download(estado_alvo, ano_referencia, mes_referencia)
        caminho_arquivo = f"{diretorio_saida}/sih_bruto_{estado_alvo}_{ano_referencia}_{mes_referencia:02d}.parquet"
        df_sih.to_parquet(caminho_arquivo, index=False)
        logging.info(f"Extração bem-sucedida. Arquivo salvo em: {caminho_arquivo}")
        break
    except Exception as erro:
        logging.error(f"Falha na tentativa {tentativa} devido a erro de conexão ou processamento: {erro}")
        if tentativa < tentativas_maximas:
            logging.info("Aguardando 10 segundos antes da próxima tentativa...")
            sleep(10)
        else:
            logging.critical("Número máximo de tentativas atingido. O pipeline foi abortado para intervenção manual.")
