# Pipeline de Dados em Streaming com Azure + Databricks + Delta Lake

## 1. Visão Geral  
Este projeto implementa um pipeline de dados em **streaming** utilizando **Azure Databricks**, **Auto Loader** e **Delta Lake**, estruturado na arquitetura **Medalhão (Bronze → Silver → Gold)**.  

O objetivo é demonstrar um fluxo profissional e completo para ingestão, tratamento e disponibilização de dados, adequado para portfólio e alinhado às boas práticas oficiais de engenharia de dados.

---

## 2. Problema Resolvido  
APIs externas normalmente sofrem com:

- Falta de histórico persistente  
- Mudanças de schema  
- Dados duplicados ou incompletos  
- Fluxos manuais e frágeis  
- Ausência de governança e versionamento  

Este projeto resolve esses desafios ao fornecer:

- Ingestão contínua e incremental com **Auto Loader**  
- Controle ACID e Time Travel com **Delta Lake**  
- Transformações limpas e separadas por camadas  
- Orquestração confiável com **Databricks Jobs**  
- Governança via **Unity Catalog**  
- Versionamento profissional com **GitHub + Databricks Repos**

---

## 3. Arquitetura da Solução  

### 3.1 Diagrama de Arquitetura  
**INSERIR AQUI O DIAGRAMA COMPLETO DA ARQUITETURA**

Exemplo de elementos que devem aparecer no diagrama:
- API externa  
- Notebook de ingestão (streaming)  
- Auto Loader  
- ADLS containers (`raw`, `curated`)  
- Bronze → Silver → Gold (Delta Tables)  
- Unity Catalog  
- Databricks Jobs  
- GitHub  

```
[INSERIR IMAGEM: diagrama_arquitetura_principal.png]
![](/Workspace/Repos/dataeng.victor@outlook.com/databricks-weather-realtime/docs/diagrama_arquitetura.png)

```

---

### 3.2 Componentes Principais  

#### Azure
- Resource Group  
- Azure Databricks Workspace  
- Azure Data Lake Storage Gen2 (HNS ativado)  
- Containers: `raw` e `curated`  
- Access Connector for Databricks  
- RBAC para acesso ao ADLS  

#### Databricks
- Unity Catalog ativado  
- Schemas: Bronze, Silver, Gold  
- Notebooks versionados com Databricks Repos  
- Auto Loader para ingestão incremental  
- Tabelas Delta com otimização  
- Databricks Jobs para orquestração  

---

## 4. Fluxo (DAG) do Pipeline

```
API → Notebook de Ingestão (Streaming)
                ↓
           Bronze Layer
                ↓
           Silver Layer
                ↓
            Gold Layer
```

```
[INSERIR IMAGEM: pipeline_dag.png]
```

---

## 5. Detalhamento das Camadas  

### 5.1 Bronze — Ingestão  
Responsável por:

- Ler continuamente a API  
- Detectar novos arquivos automaticamente  
- Persistir dados brutos em **Delta Lake**  
- Utilizar **Auto Loader** para schema inference + evolution  
- Garantir idempotência via checkpoint  

### 5.2 Silver — Tratamento  
Responsável por:

- Limpar e padronizar o schema  
- Remover duplicidades  
- Tratar valores nulos  
- Normalizar tipos  
- Criar colunas derivadas  

### 5.3 Gold — Análises  
Responsável por:

- Agregações (ex.: hora, dia, tendência)  
- Criação de KPIs  
- Construção de tabelas otimizadas  
- Aplicação de **OPTIMIZE** e **ZORDER**  

---

## 6. Orquestração com Databricks Jobs  

As tarefas foram configuradas em um job com execução sequencial:

1. Bronze  
2. Silver  
3. Gold  

Agendamento recomendado: **a cada 5 minutos**

```
[INSERIR IMAGEM: databricks_jobs_config.png]
```

---

## 7. Infraestrutura no Azure  

### 7.1 Storage Account (ADLS)  
Configurações necessárias:

- Hierarchical Namespace = Enabled  
- Containers:
  - `raw`
  - `curated`
- Permissões do Access Connector (Storage Blob Data Contributor)

```
[INSERIR IMAGEM: storage_account_config.png]
```

### 7.2 Access Connector + RBAC  
- Criado em Azure  
- Atribuição de roles no Storage Account  
- Permissões herdadas pelo Databricks  

```
[INSERIR IMAGEM: access_connector_permissions.png]
```

---

## 8. GitHub + Databricks Repos (Versionamento)  

Estrutura do repositório:

```
/notebooks/bronze
/notebooks/silver
/notebooks/gold
/infra/azure
/docs/diagramas
README.md
```

Integração feita por **Personal Access Token**.

```
[INSERIR IMAGEM: databricks_repos_integration.png]
```

---

## 9. Tecnologias Principais  

### Databricks  
- Apache Spark  
- Delta Lake  
- Auto Loader  
- Unity Catalog  
- Databricks Jobs  
- Databricks Repos  

### Azure  
- Azure Databricks  
- Azure Data Lake Storage Gen2  
- Access Connector  
- IAM/RBAC  

---

## 10. Justificativas Técnicas  

- **Auto Loader** reduz custo e elimina necessidade de Event Hub/Functions  
- **Delta Lake** garante transações ACID e histórico completo  
- **Arquitetura Medalhão** separa responsabilidades  
- **Unity Catalog** centraliza governança  
- **Jobs Databricks** simplificam orquestração  
- **GitHub** profissionaliza o fluxo de versionamento  

---

## 11. Como Executar o Projeto  

1. Criar Databricks Workspace com Unity Catalog  
2. Criar Storage Account com HNS ativado  
3. Criar containers `raw` e `curated`  
4. Configurar Access Connector  
5. Conectar repositório ao Databricks Repos  
6. Executar notebooks Bronze → Silver → Gold  
7. Criar job para automação  

---

## 12. Melhorias Futuras  

- Migrar para **Delta Live Tables**  
- Implementar monitoração com expectations  
- Adicionar CI/CD com GitHub Actions  
- Criar API ou dashboard conectado à camada Gold  

---

## 13. Estrutura Final do Repositório

```
├── notebooks
│   ├── bronze
│   │   └── ingestao_bronze.py
│   ├── silver
│   │   └── transform_silver.py
│   └── gold
│       └── analytics_gold.py
│
├── infra
│   └── azure
│       └── configuracoes_terraform_opcional/
│
├── docs
│   ├── diagrama_arquitetura_principal.png
│   ├── pipeline_dag.png
│   ├── storage_account_config.png
│   ├── access_connector_permissions.png
│   └── databricks_jobs_config.png
│
└── README.md
```

---

## 14. Autor  
Projeto desenvolvido para fins educacionais e portfólio de Engenharia de Dados, seguindo práticas baseadas em Databricks, Azure e Apache Spark.

![diagrama_arquitetura.png](./diagrama_arquitetura.png "diagrama_arquitetura.png")