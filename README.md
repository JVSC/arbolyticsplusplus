# arbolyticsplusplus

# Arbolytics ++ - Guia de Execução

Este projeto utiliza Docker Compose para orquestrar uma stack completa de análise de dados epidemiológicos com Elasticsearch, Kibana, Redis e aplicações Flask/Celery.

## Pré-requisitos

- Docker (versão 20.10 ou superior)
- Docker Compose (versão 2.0 ou superior)
- Pelo menos 4GB de RAM disponível para os containers

## Configuração Inicial

### 1. Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:

```bash
# Versão do Elastic Stack
STACK_VERSION=8.11.0

# Senhas (ALTERE ESTAS SENHAS!)
ELASTIC_PASSWORD=your_elastic_password_here
KIBANA_PASSWORD=your_kibana_password_here
ARBOLYTICS_PASSWORD=your_arbolytics_password_here

# Configurações do Cluster
CLUSTER_NAME=arbolytics-cluster
LICENSE=basic

# Portas
ES_PORT=9200
KIBANA_PORT=5601

# Limite de Memória por Container
MEM_LIMIT=1073741824
```

### 2. Estrutura de Dados

Certifique-se de que os diretórios de dados existem:

```bash
mkdir -p data/cnes data/sinan
```

## Execução

### Iniciar todos os serviços

```bash
docker-compose up -d
```

### Verificar status dos containers

```bash
docker-compose ps
```

### Acompanhar logs

```bash
# Todos os serviços
docker-compose logs -f

# Serviço específico
docker-compose logs -f flask
```

### Parar os serviços

```bash
docker-compose down
```

### Parar e remover volumes (CUIDADO: apaga todos os dados)

```bash
docker-compose down -v
```

## Serviços Disponíveis

### API Flask
- **URL**: http://localhost:3000
- **Descrição**: API principal do Arbolytics para análise de dados epidemiológicos
- **Funcionalidades**:
  - Endpoints para análise de dados SINAN
  - Integração com Elasticsearch
  - Processamento de dados CNES

### Frontend
- **URL**: http://localhost:5173
- **Descrição**: Interface web do Arbolytics
- **Tecnologias**: Vue.js, Vite, Tailwind CSS

### Elasticsearch Cluster
- **URLs**: 
  - Nó 1: https://localhost:9200
  - Nós 2 e 3: Acesso interno apenas
- **Descrição**: Cluster de 3 nós para armazenamento e busca de dados
- **Credenciais**:
  - Usuário: `elastic`
  - Senha: Definida em `ELASTIC_PASSWORD`
  - Usuário app: `arbolytics` / Senha: `ARBOLYTICS_PASSWORD`

### Kibana
- **URL**: http://localhost:5601
- **Descrição**: Interface de visualização e análise dos dados do Elasticsearch
- **Credenciais**:
  - Usuário: `kibana_system`
  - Senha: Definida em `KIBANA_PASSWORD`

### Redis
- **Porta**: 6379 (acesso interno)
- **Descrição**: Cache e broker de mensagens para Celery
- **Persistência**: Dados salvos em volume `redisdata`

### Celery Worker
- **Descrição**: Processamento assíncrono de tarefas
- **Funcionalidades**:
  - Processamento de dados em background
  - Cálculos estatísticos
  - Agregações de dados

## Volumes Persistentes

- `esdata01`, `esdata02`, `esdata03`: Dados do Elasticsearch
- `kibanadata`: Configurações e dashboards do Kibana
- `redisdata`: Cache do Redis
- `certs`: Certificados SSL do Elasticsearch
- `cache`: Cache geral da aplicação

### Usuários Criados Automaticamente
1. **elastic**: Usuário superadmin do Elasticsearch
2. **kibana_system**: Usuário para conexão Kibana-Elasticsearch  
3. **arbolytics**: Usuário da aplicação com privilégios de superuser

## Índices Elasticsearch

O sistema cria automaticamente templates para os seguintes padrões:

- `sinan.*`: Dados do SINAN (Sistema de Informação de Agravos de Notificação)
- `cnes-*`: Dados do CNES (Cadastro Nacional de Estabelecimentos de Saúde)
- `id-cnes-*`: Dados de identificação CNES
- `dados_*`: Dados gerais processados

## Desenvolvimento



## Contribuição

Para contribuir com o projeto:

1. Fork o repositório
2. Crie uma branch para sua feature
3. Faça commit das mudanças
4. Abra um Pull Request

