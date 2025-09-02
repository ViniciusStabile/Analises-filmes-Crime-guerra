# Step III – AWS Platform (ETL)

##  Objetivo
Processar e padronizar os dados ingeridos nas Steps I e II utilizando **AWS Glue (Spark)**, gerando **Parquet** pronto para consulta (camada *Trusted*).  
Esta etapa:
- Corrige esquemas e nomes de colunas.
- Remove duplicidades e nulos.
- (JSON) adiciona **particionamento por ano/mês/dia** para otimizar consultas.
- Escreve a saída no **Amazon S3**.

---

##  Visão Geral da Solução

**Entradas**
- CSV: `Raw/Local/CSV/.../*.csv` (ex.: filmes base)
- JSON (line-delimited): `Raw/TMDB/JSON/Movies/{ano}/{mes}/{dia}/lote_*.json`

**Processamento (AWS Glue / Spark)**
- Job 1 (**processamento_csv.py**)
- Job 2 (**processamento_json.py**)

**Saídas sugeridas (camada Trusted em Parquet)**
- CSV → `Trusted/Local/Parquet/Movies/`  
- JSON → `Trusted/TMDB/Parquet/Movies/ano=YYYY/mes=MM/dia=DD/`

> Você pode ajustar os caminhos conforme seu padrão; os scripts recebem `S3_INPUT_PATH` e `S3_TARGET_PATH` via argumentos.

---

##  Jobs do Glue

### 1) `processamento_csv.py`
**Função**: Ler CSV (com schema explícito), corrigir coluna, **deduplicar**, **dropar nulos** e gravar em **Parquet**.

**Principais pontos**
- Schema definido via `StructType` (garante tipos consistentes).
- Renomeia `tituloPincipal` → `tituloPrincipal`.
- `dropDuplicates().dropna()`.
- Escrita em Parquet no `S3_TARGET_PATH` (modo `overwrite`).

**Parâmetros obrigatórios**
- `--JOB_NAME`
- `--S3_INPUT_PATH` (ex.: `s3://vinicius-desafio-pb/Raw/Local/CSV/Movies/2025/09/02/movies.csv`)
- `--S3_TARGET_PATH` (ex.: `s3://vinicius-desafio-pb/Trusted/Local/Parquet/Movies/`)

---

### 2) `processamento_json.py`
**Função**: Ler JSONs (TMDB), converter `release_date` para **date**, **deduplicar por `id`**, **dropar nulos**, **adicionar colunas de partição (ano/mes/dia)** com base no caminho de entrada e **gravar particionado** em **Parquet**.

**Principais pontos**
- `to_date(release_date, 'yyyy-MM-dd')`.
- `dropDuplicates(subset=["id"]).dropna()`.
- Extrai `{ano}/{mes}/{dia}` do próprio `S3_INPUT_PATH`.
- `write.partitionBy("ano","mes","dia").parquet(...)`.

**Parâmetros obrigatórios**
- `--JOB_NAME`
- `--S3_INPUT_PATH` (ex.: `s3://vinicius-desafio-pb/Raw/TMDB/JSON/Movies/2025/09/02/`)
- `--S3_TARGET_PATH` (ex.: `s3://vinicius-desafio-pb/Trusted/TMDB/Parquet/Movies/`)

---

##  Boas Práticas & Notas
- **Particionamento** (JSON): melhora performance e custo de consultas (Athena/Glue Data Catalog).
- **Tipos explícitos** (CSV): reduz erros de schema e facilita evolução.
- **Overwrite x Append**: CSV usa `overwrite`; ajuste conforme estratégia de reprocessamento.
- **Paths consistentes**: mantenha o padrão Raw → Trusted → (Refined).
- (Opcional) **Glue Data Catalog**: crie crawlers/tabelas para consultar via **Athena**.
- **Permissões IAM**: o role do Job deve permitir `s3:GetObject`, `s3:PutObject`, `s3:ListBucket` nos caminhos envolvidos.

---

##  Tecnologias Utilizadas
- **AWS Glue (Spark)**
- **Apache Spark (PySpark)**
- **Amazon S3**
- **AWS CloudWatch Logs**
- **AWS IAM**

---

##  Conclusão
A **Step III – AWS Platform (ETL)** consolida os dados da camada Raw em **Parquet** confiável e (quando aplicável) **particionado**, preparando a base para consultas eficientes (Athena/QuickSight) e para a etapa seguinte (**Refined**).