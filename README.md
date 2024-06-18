## Projeto: Configuração e Operação de Aplicação com PostgreSQL, Ansible, Prometheus, Grafana, Jenkins e Kubernetes

Neste projeto, vou configurar e operar uma aplicação utilizando tecnologias modernas para banco de dados, automação, monitoramento e orquestração de containers.

## Passos do Projeto

### 1. Configuração de PostgreSQL em Docker

- **Objetivo:** Configurar um banco de dados PostgreSQL utilizando Docker.

- **Passos:**
  - Instalei o Docker no meu ambiente de desenvolvimento.
  - Criei um arquivo `docker-compose.yml` para configurar o PostgreSQL:

    ```yaml
    version: '3.8'

    services:
      postgresql:
        image: postgres:latest
        environment:
          POSTGRES_DB: mydatabase
          POSTGRES_USER: myuser
          POSTGRES_PASSWORD: mypassword
        ports:
          - "5432:5432"
    ```

  - Executei o PostgreSQL com o comando `docker-compose up -d`.
  - Verifiquei se o PostgreSQL está em execução com `docker ps`.

### 2. Backup e Recuperação com Ansible

- **Objetivo:** Automatizar o backup e a recuperação do banco de dados PostgreSQL usando Ansible.

- **Passos:**
  - Instalei o Ansible no meu ambiente.
  - Criei um playbook Ansible (`backup.yml`) para realizar o backup:

    ```yaml
    ---
    - name: Backup PostgreSQL Database
      hosts: localhost
      tasks:
        - name: Dump PostgreSQL database
          command: pg_dump -U myuser -h localhost mydatabase > /path/to/backup.sql
    ```

  - Criei um playbook para a recuperação (`restore.yml`):

    ```yaml
    ---
    - name: Restore PostgreSQL Database
      hosts: localhost
      tasks:
        - name: Restore PostgreSQL database from backup
          command: psql -U myuser -h localhost -d mydatabase < /path/to/backup.sql
    ```

  - Executei os playbooks com `ansible-playbook backup.yml` e `ansible-playbook restore.yml`.

### 3. Monitoramento com Prometheus e Grafana

- **Objetivo:** Configurar o monitoramento do PostgreSQL com Prometheus e visualização dos dados com Grafana.

- **Passos:**
  - Configurei o Prometheus para monitorar o PostgreSQL. Exemplo de configuração no arquivo `prometheus.yml`:

    ```yaml
    scrape_configs:
      - job_name: 'postgresql'
        static_configs:
          - targets: ['postgresql:5432']
    ```

  - Iniciei o Prometheus com Docker: `docker run -p 9090:9090 -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus`.
  - Configurei o Grafana para visualização dos dados do Prometheus.
  - Acessei Grafana em `http://localhost:3000`, configurei a fonte de dados Prometheus e criei dashboards para monitorar o PostgreSQL.

### 4. Automação de Deploy com Jenkins

- **Objetivo:** Automatizar o deploy da aplicação usando Jenkins.

- **Passos:**
  - Instalei o Jenkins no meu ambiente.
  - Configurei um job no Jenkins para puxar o código da aplicação do repositório (por exemplo, GitHub).
  - Configurei o Jenkins para construir e implantar a aplicação no Kubernetes.

### 5. Alta Disponibilidade com Kubernetes

- **Objetivo:** Implantar a aplicação em um cluster Kubernetes para alta disponibilidade.

- **Passos:**
  - Instalei o Kubernetes localmente (usando Minikube ou Docker Desktop com Kubernetes).
  - Criei manifests YAML para implantar a aplicação no Kubernetes:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-app
      labels:
        app: my-app
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
            - name: my-app
              image: my-app:latest
              ports:
                - containerPort: 8080
    ```

  - Apliquei os manifests com `kubectl apply -f deployment.yaml`.

## Conclusão

Este projeto abrange desde a configuração inicial do banco de dados PostgreSQL até a implantação em um ambiente Kubernetes, passando por automação de backup/recuperação com Ansible, monitoramento com Prometheus/Grafana e automação de deploy com Jenkins. Cada passo é crucial para garantir a operação eficiente e escalável de uma aplicação moderna. A complexidade aumenta gradualmente, mas cada componente é essencial para um ambiente de produção robusto e confiável.

Se tiver dúvidas ou precisar de mais detalhes sobre qualquer parte do projeto, estou à disposição para ajudar!
```
