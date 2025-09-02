# Step I - On-premise Sources  

##  Objetivo  
O objetivo desta etapa é realizar a ingestão de dados provenientes de fontes **on-premise (locais)** para a camada **Raw** de um Data Lake na AWS.  
Essa fase garante que os arquivos **CSV** armazenados localmente sejam enviados para o bucket **Amazon S3**, seguindo um padrão de organização pré-definido para suportar futuras transformações e análises.  

---

##  Arquitetura da Solução  
1. **Fonte de dados local**: Arquivos `.csv` armazenados no diretório `./dados`.  
2. **Docker**: Container responsável por executar o script de upload de forma isolada e padronizada.  
3. **Script em Python (main.py)**:  
   - Autenticação com a AWS via **SSO** utilizando o `boto3`.  
   - Upload dos arquivos para o **S3** com tratamento de erros.  
   - Criação automática da **key** no padrão:  <camada>/<origem>/<formato>/<especificacao>/<ano>/<mes>/<dia>/<arquivo>
   Exemplo:  Raw/Local/CSV/Vendas/2025/09/02/filmes.csv

4. **AWS S3**: Repositório central de armazenamento dos dados.  

---

##  Funcionalidades do Script  

- **Autenticação na AWS** com tratamento para diferentes exceções (`ProfileNotFound`, `NoCredentialsError`, `EndpointConnectionError`, etc).  
- **Verificação de existência de objetos** no bucket antes do upload para evitar duplicação.  
- **Upload de arquivos CSV** locais para o bucket S3.  
- **Listagem dos objetos** presentes no bucket após a execução.  
- **Logs estruturados** para facilitar monitoramento e troubleshooting

##  Estrutura de Arquivos

step-1-on-premise-sources/
- main.py # Script principal de upload para o S3
- requirements.txt # Dependências Python (boto3, etc.)
- Dockerfile # Configuração do container para rodar o script
- docker-compose.yml # Orquestração e montagem de volumes
- dados/ # Diretório com arquivos .csv a serem enviados

##  Tecnologias Utilizadas  

- **Python 3.10**  
- **Boto3 (AWS SDK para Python)**  
- **AWS S3**  
- **Docker**  
- **Docker Compose**