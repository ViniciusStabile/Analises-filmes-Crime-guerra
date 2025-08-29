#  Projeto de Engenharia de Dados - Filmes & Séries  

##  Visão Geral  
Este projeto implementa um **pipeline de dados em nuvem** utilizando serviços da **AWS** para ingestão, transformação e análise de informações sobre **filmes e séries**.  

A solução integra fontes **on-premise** (arquivos CSV) e **APIs externas** (TMDB), processa os dados em múltiplas camadas no **S3** (Raw, Trusted e Refined), aplica transformações com **AWS Glue (PySpark)** e disponibiliza insights por meio do **AWS QuickSight**.  

![Arquitetura](./imagens/Desafio-FilmesSeries-Completo.png) 

---

##  Objetivos do Projeto  
- Criar um **pipeline completo** de dados usando boas práticas de Data Lake.  
- Integrar **dados on-premise (CSV)** com **APIs externas (TMDB)**.  
- Organizar os dados em **camadas (Raw, Trusted, Refined)**.  
- Processar e transformar os dados usando **AWS Glue + PySpark**.  
- Consultar e analisar os dados com **Athena**.  
- Disponibilizar insights visuais via **QuickSight**.  


---

##  Estrutura do Data Lake  

- **Raw Layer** → Armazena dados brutos, sem transformação.  
- **Trusted Layer** → Dados limpos e padronizados.  
- **Refined Layer** → Dados modelados e otimizados para análise.  

---

##  Tecnologias Utilizadas  

- **Linguagens & Ferramentas**: Python, PySpark, Docker  
- **Cloud AWS**:  
  - S3 (Raw, Trusted, Refined)  
  - Lambda (ingestão de dados + integração TMDB API)  
  - Glue (ETL e modelagem dimensional)  
  - Athena (consultas SQL sobre dados no S3)  
  - QuickSight (dashboards interativos)  
  - CloudWatch (monitoramento e logs)


---

##  Pipeline de Dados  

###  **Step I - On-premise Sources**  
- Base de **Filmes (164 MB)** e **Séries (74 MB)** em CSV.  
- Pré-processamento com **Python + Docker**.  
- Envio dos dados para o **S3 Raw**.  

![Arquitetura 1](./imagens/Desafio-FilmesSeries-Parte1.png) 


### **Step II - External Sources**  
- Integração com a **TMDB API** para enriquecimento dos dados.  
- Coleta de atributos adicionais: popularidade, orçamento, receita, ROI, etc.

![Arquitetura 2](./imagens/Desafio-FilmesSeries-Parte2.png) 


###  **Step III - AWS Platform (ETL)**  
- **AWS Lambda**: orquestra o envio e chamadas para APIs.  
- **AWS Glue (PySpark)**:  
  - Criação da **Trusted Layer** com dados limpos. 

![Arquitetura 3](./imagens/Desafio-FilmesSéries-Parte3.png)


###  **Step IV - AWS Platform (ETL)** 
  - Criação da **Refined Layer** com modelo dimensional.

![Arquitetura 4](./imagens/Desafio-FilmesSéries-Parte4.png)

###  **Step V - Analytics & Dashboards**  
- **AWS Glue Data Catalog**: catálogo de tabelas.  
- **Athena**: consultas SQL sobre o Data Lake.  
- **QuickSight**: dashboards para análise de ROI, notas, popularidade e tendências.

![Arquitetura 5](./imagens/Desafio-FilmesSéries-Parte5.png)

---

##  Modelagem Dimensional  

Na camada **Refined**, os dados foram organizados em modelo dimensional:  

- **Fato**: `fato_desempenho_filmes`  
- **Dimensões**:  
  - `dim_filme`  
  - `dim_artista`  
  - `dim_data` 

![Modelagem](./imagens/modelo_dimensional.png) 

---

##  Análises Realizadas

##  Resultado Final
![Dashboard Completo](./imagens/Dashboard.jpg)

###  Paleta de Cores
![Paleta de cores](./imagens/paleta.png)

###  Cabeçalho
![Cabeçalho - O Que Define o Sucesso no Cinema?](./imagens/cabeçalho.png)

#### **O Que Define o Sucesso no Cinema?**  
**Uma Análise de ROI, Receita e Popularidade**

---

###  KPIs Principais
![KPIs do Dashboard](./imagens/KPIs.png)

- **Total de filmes:** `829`  
- **Total de artistas:** `1.758`  
- **Orçamento médio:** `US$ 35,65M`  
- **Receita média:** `US$ 91,14M`  
- **ROI médio:** `2,03`  
- **Popularidade média:** `5,08`  
- **Nota IMDb média:** `6,65`  

---

### 1️ ROI dos Filmes
![Top 10 Filmes ROI - Gráfico](./imagens/top_10_1.png)  
![Top 10 Filmes ROI - Tabela](./imagens/top_10_2.png)

---

### 2️ Relação entre Nota, Popularidade e ROI
![Nota x Popularidade x ROI](./imagens/popularidade_nota_roi.png)

---

### 3️ Orçamento vs Receita
![Orçamento vs Receita](./imagens/orcamento_receita.png)

---

### 4️ Evolução ao Longo dos Anos
![Evolução Orçamento e Receita](./imagens/investimento_anos.png)

---

### 5️ ROI por Década
![ROI por Década](./imagens/decada.png)

---

### 6️ Sazonalidade dos Lançamentos
![Sazonalidade - ROI por Mês](./imagens/mes.png)

---

### 7️ Artistas de Alto Desempenho
![Artistas - Gráfico](./imagens/top_artistas_2.png)  
![Artistas - Tabela](./imagens/top_artistas_1.png)

---

##  Conclusão

- A indústria cinematográfica se tornou **mais cara** e **menos eficiente** financeiramente ao longo do tempo.  
- Histórias bem construídas e **investimentos enxutos** ainda se provam extremamente lucrativos.  
- O sucesso é multifatorial: **nota crítica, popularidade, marketing, timing e apelo cultural**.  
- **Artistas estratégicos** e **datas de lançamento certas** continuam sendo peças-chave para o ROI.  

---