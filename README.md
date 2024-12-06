# Curso Docker e Stack de Microserviços com Elastic Stack

## Descrição

Este repositório contém uma configuração de ambiente utilizando Docker para orquestrar e executar uma aplicação de microserviços. A arquitetura utiliza Docker Compose para definir múltiplos containers, incluindo uma API personalizada, um banco de dados Oracle, Elasticsearch, Kibana, APM Server, Metricbeat e Heartbeat, todos integrados para oferecer uma solução robusta para monitoramento e análise de dados.

O curso abordou os conceitos de containerização, integração de diversos serviços e como utilizar o Elastic Stack para monitorar e analisar a aplicação.

## Estrutura do Ambiente

A estrutura do ambiente é definida por um arquivo `docker-compose.yml`, onde estão configurados os seguintes serviços:

### 1. minha-api
- **Função**: API personalizada que se comunica com o banco de dados Oracle e Elasticsearch.
- **Portas**: 3000
- **Dependências**: Oracle DB, Elasticsearch
- **Variáveis de ambiente**:
  - `DB_USER`: Nome do usuário para o banco de dados.
  - `DB_PASSWORD`: Senha do banco de dados.
  - `DB_CONNECT_STRING`: String de conexão do banco de dados Oracle.
  - `ELASTICSEARCH_NODE`: URL do Elasticsearch.
  - `ELASTIC_APM_SERVER_URL`: URL do APM Server.
  - `ELASTIC_APM_SERVICE_NAME`: Nome do serviço no APM.
  - `ELASTIC_USER` e `ELASTIC_PASSWORD`: Credenciais de acesso ao Elasticsearch.

### 2. oracle-db
- **Função**: Banco de dados Oracle XE (versão 21) usado pela API.
- **Portas**: 1521 (conexão de banco), 5500 (Oracle SQL Developer)
- **Variáveis de ambiente**:
  - `ORACLE_PASSWORD`: Senha do usuário SYS no Oracle.
- **Saúde**: Possui um healthcheck que verifica a conexão ao banco de dados executando uma query simples.

### 3. elasticsearch
- **Função**: Servidor de pesquisa e análise de dados.
- **Versão**: 8.10.2
- **Portas**: 9200 (HTTP), 9300 (Transport)
- **Variáveis de ambiente**:
  - `node.name`: Nome do nó do Elasticsearch.
  - `cluster.name`: Nome do cluster.
  - `xpack.security.enabled`: Habilita a segurança no Elasticsearch.
  - `ES_JAVA_OPTS`: Parâmetros de memória para a JVM.
- **Saúde**: Utiliza `curl` para verificar a saúde do Elasticsearch.

### 4. kibana
- **Função**: Interface para visualização de dados armazenados no Elasticsearch.
- **Portas**: 5601
- **Dependência**: Elasticsearch
- **Configuração**: O arquivo `kibana.yml` é montado diretamente no container.

### 5. apm-server
- **Função**: Coleta de métricas e dados de performance da API e outros serviços.
- **Portas**: 8200
- **Configuração**: O arquivo `apm-server.yaml` é montado diretamente no container.

### 6. metricbeat
- **Função**: Coleta de métricas de infraestrutura e containers Docker.
- **Portas**: Não expõe portas externas.
- **Configuração**: O arquivo `metricbeat.yml` é montado diretamente no container e o Metricbeat coleta dados do Docker através do socket.

### 7. heartbeat
- **Função**: Ferramenta para monitoramento de disponibilidade e latência de serviços.
- **Configuração**: O arquivo `heartbeat.yml` é montado diretamente no container.

## Instruções para Execução

### 1. Clonar o Repositório
Clone este repositório para o seu ambiente local:

```bash
git clone https://github.com/menegasso/curso_elasticsearch-kibana-apm-docker
cd curso_elasticsearch-kibana-apm-docker

```

### 2. Construir e Subir os Containers: Utilize o `docker-compose` para construir e iniciar os containers:

```bash
docker-compose up --build
```
Isso irá iniciar todos os serviços configurados no arquivo `docker-compose.yml`.

### 3. Acessar a Aplicação:

Após o Docker Compose subir os containers, você pode acessar os seguintes serviços:

API: Acesse a API na URL `http://localhost:3000`.

Kibana: Acesse o painel de visualização de dados do Elasticsearch na URL `http://localhost:5601`.

APM Server: Para visualizar os dados de performance da API, acesse o APM Server na URL `http://localhost:8200`.



## Recursos Aprendidos

Durante o curso, foram abordados os seguintes tópicos:

Docker e Docker Compose: Como criar, gerenciar e orquestrar containers para diferentes serviços.
Integração de Banco de Dados Oracle com Docker: Configuração de containers com bancos de dados como o Oracle XE.
Elasticsearch e Kibana: Como configurar e usar o Elasticsearch para busca e análise de dados, e Kibana para visualização.
Monitoramento de Aplicações com Elastic APM: Como integrar o APM Server para coletar dados de performance e monitorar a API.
Coleta de Métricas com Metricbeat e Heartbeat: Como configurar o Metricbeat e Heartbeat para monitorar o estado e a saúde da infraestrutura e serviços.



## Conclusão
Este curso forneceu uma compreensão prática de como configurar e orquestrar múltiplos serviços usando Docker e Docker Compose, bem como integrar esses serviços ao Elastic Stack para monitoramento e análise de dados. A utilização do APM, Metricbeat e Heartbeat adiciona uma camada poderosa de observabilidade à arquitetura de microserviços.
