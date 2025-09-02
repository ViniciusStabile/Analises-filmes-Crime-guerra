# Step II - External Sources  

##  Objetivo  
O objetivo desta etapa é **enriquecer os dados locais** já ingeridos na camada **Raw** utilizando uma **fonte externa**: a **API do The Movie Database (TMDB)**.  
Essa integração possibilita buscar informações adicionais sobre os filmes (orçamento, receita, popularidade, avaliação etc.), aumentando a qualidade e o valor analítico dos dados armazenados no Data Lake.  

---

##  Arquitetura da Solução  
1. **Entrada**:  
   - Arquivo `movies.csv` previamente armazenado no **S3** em:  
     ```
     Raw/Local/CSV/Movies/{ano}/{mes}/{dia}/movies.csv
     ```

2. **Filtragem**:  
   - Leitura do CSV diretamente do **S3**.  
   - Filtros aplicados:  
     - Ano de lançamento > 1960  
     - Número de votos > 10.000  
     - Gênero contendo “crime” ou “war”  

3. **Enriquecimento**:  
   - Extração dos IDs filtrados.  
   - Consulta assíncrona à **API do TMDB** usando **asyncio**, com limite de concorrência para respeitar o rate limit.  
   - Extração dos seguintes atributos:  
     - `id`, `imdb_id`, `title`, `runtime`, `release_date`,  
       `budget`, `revenue`, `popularity`, `vote_average`, `vote_count`.  

4. **Persistência**:  
   - Resultados armazenados em lotes (`lote_1.json`, `lote_2.json`...) no **S3** no padrão:  

     ```
     Raw/TMDB/JSON/Movies/{ano}/{mes}/{dia}/lote_N.json
     ```

   - IDs que retornaram erro são armazenados separadamente em:  

     ```
     Raw/TMDB/JSON/Movies/{ano}/{mes}/{dia}/erros_geral.json
     ```

5. **Execução**:  
   - A orquestração é realizada via **AWS Lambda**, que:  
     - Lê os IDs do CSV.  
     - Processa os lotes.  
     - Salva resultados e erros no S3.  

---

##  Funcionalidades Principais  

- **Leitura de CSV diretamente do S3**.  
- **Filtragem de dados** por critérios de qualidade e relevância.  
- **Consulta assíncrona à API TMDB** com tratamento de:  
  - IDs inexistentes (404).  
  - Rate limit (429).  
  - Outros erros de rede ou API.  
- **Processamento em lotes de 100 filmes** para otimizar uso de recursos.  
- **Armazenamento em JSON** no S3 em formato **line-delimited** (um JSON por linha).  
- **Separação dos IDs com erro** para posterior reprocessamento ou análise.

##  Tecnologias Utilizadas  

- **Python 3.10**  
- **Pandas** (manipulação de dados)  
- **Boto3** (interação com AWS S3)  
- **Requests** (requisições HTTP)  
- **Asyncio** (execução assíncrona com controle de concorrência)  
- **AWS Lambda** (orquestração da execução serverless)  
- **Amazon S3** (armazenamento dos resultados)  
- **TMDB API** (fonte externa de dados) 

##  Conclusão  
A **Step II - External Sources** foi responsável por enriquecer os dados da camada Raw, adicionando informações de filmes obtidas via API externa (TMDB).  
Essa etapa permitiu:  
- Garantir maior profundidade analítica nos dados.  
- Criar um fluxo resiliente com tratamento de erros e rate limit.  
- Estruturar a base para as próximas fases de transformação (**Trusted**) e análise (**Refined**).  