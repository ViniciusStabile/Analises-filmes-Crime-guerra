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