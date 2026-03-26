# ETAPA 3 - Pré-Processamento e Limpeza de Dados

O pré-processamento (Data Wrangling) é a fase crítica onde os dados brutos extraídos do DATASUS são transformados em um formato estruturado, limpo e pronto para a modelagem analítica e algoritmos de aprendizado de máquina. Como as bases públicas frequentemente apresentam inconsistências de preenchimento, este estágio garante a confiabilidade dos insights que serão gerados para a gestão de saúde.

As principais atividades de engenharia de dados desenvolvidas nesta etapa incluem:

1. **Seleção de Variáveis (Feature Selection):** Filtragem apenas das colunas relevantes para o escopo de otimização de fluxos assistenciais (como código do estabelecimento, data de internação, complexidade do procedimento e tempo de permanência). O descarte de dados não essenciais otimiza o processamento e economiza recursos computacionais.
2. **Tratamento de Valores Nulos e Inconsistentes:** Identificação de registros incompletos. Valores vitais ausentes (como o identificador numérico do hospital) resultam na exclusão da linha para evitar viés, enquanto dados secundários nulos podem ser tratados com técnicas de imputação estatística.
3. **Integração Relacional (Joins):** O cruzamento entre as bases transacionais de atendimento (SIH e SIA) e a base cadastral de hospitais (CNES). A chave primária (Primary Key) utilizada para esta junção é o código `CNES` da unidade de saúde, permitindo enriquecer os dados de internação com informações sobre a capacidade instalada do local de atendimento.
4. **Padronização Tipológica:** Conversão rigorosa de strings para formatos de data (`datetime`) e numéricos (`float`/`int`), assegurando a consistência do banco de dados analítico final.

## Ferramentas Utilizadas

Para a realização do pré-processamento dos dados, foi utilizada a linguagem Python, devido à sua ampla adoção no contexto de análise de dados e aprendizado de máquina.

As principais bibliotecas utilizadas foram:

- Pandas: responsável pela manipulação, limpeza, transformação e integração dos dados em formato tabular.
- PyArrow: utilizada para leitura e escrita de arquivos no formato Parquet, garantindo maior eficiência de armazenamento e processamento.
- OS: utilizada para gerenciamento de diretórios e organização dos arquivos no sistema.

A escolha dessas ferramentas se deve à sua alta performance no processamento de grandes volumes de dados, facilidade de uso e forte integração com o ecossistema de ciência de dados.

O script Python abaixo ilustra o pipeline de transformação. Ele carrega os arquivos Parquet gerados na etapa de coleta, aplica as regras de limpeza dimensional, realiza o merge relacional das tabelas e salva o conjunto de dados final isolado na camada de dados processados.

```python
import pandas as pd
import os

diretorio_raw = "data/raw"
diretorio_processed = "data/processed"

os.makedirs(diretorio_processed, exist_ok=True)

print("Carregando bases brutas (Parquet)...")
df_sih = pd.read_parquet(f"{diretorio_raw}/sih_mg.parquet")
df_cnes = pd.read_parquet(f"{diretorio_raw}/cnes_mg.parquet")

print("Iniciando limpeza e seleção de variáveis do SIH...")
colunas_sih = ['CNES', 'DT_INTER', 'DT_SAIDA', 'DIAS_PERM', 'COMPLEX', 'VAL_TOT']
colunas_presentes_sih = [col for col in colunas_sih if col in df_sih.columns]
df_sih_clean = df_sih[colunas_presentes_sih].copy()

print(f"Colunas selecionadas SIH: {colunas_presentes_sih}")
print(f"Registros antes da limpeza: {len(df_sih)}")

df_sih_clean.dropna(subset=['CNES'], inplace=True)

print(f"Registros após remover nulos (CNES): {len(df_sih_clean)}")

print("Convertendo datas...")
df_sih_clean['DT_INTER'] = pd.to_datetime(df_sih_clean['DT_INTER'], format='%Y%m%d', errors='coerce')
df_sih_clean['DT_SAIDA'] = pd.to_datetime(df_sih_clean['DT_SAIDA'], format='%Y%m%d', errors='coerce')

print("Iniciando limpeza do CNES...")
colunas_cnes = ['CNES', 'COMPETEN', 'VINC_SUS']
colunas_presentes_cnes = [col for col in colunas_cnes if col in df_cnes.columns]
df_cnes_clean = df_cnes[colunas_presentes_cnes].copy()

print(f"Colunas selecionadas CNES: {colunas_presentes_cnes}")

print("Realizando integração (merge)...")
df_integrado = pd.merge(df_sih_clean, df_cnes_clean, on='CNES', how='inner')

print(f"Registros após merge: {len(df_integrado)}")

print("Salvando base processada...")
caminho_saida = f"{diretorio_processed}/base_integrada.parquet"
df_integrado.to_parquet(caminho_saida, index=False)

print("Pré-processamento concluído com sucesso!")
print(f"Arquivo salvo em: {caminho_saida}")
print("Salvando base processada e enriquecida...")
caminho_saida = f"{diretorio_processed}/base_integrada_MG_2023_01.parquet"
df_integrado.to_parquet(caminho_saida, index=False)

print(f"Pré-processamento concluído. Registros finais: {df_integrado.shape[0]}.")
print(f"Arquivo salvo em: {caminho_saida}")
