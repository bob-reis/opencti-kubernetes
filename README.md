# Deploy do OpenCTI no Kubernetes

Este repositório contém todos os arquivos necessários para configurar e implantar o OpenCTI em um cluster Kubernetes.

## Estrutura do Repositório

```plaintext
kube-opencti/
├── namespace.yaml
├── storageclass.yaml
├── kustomization.yaml
├── secrets/
│   ├── minio-secrets.yaml
│   ├── rabbitmq-secrets.yaml
│   └── opencti-secrets.yaml
├── pvc/
│   ├── redis-pvc.yaml
│   ├── elasticsearch-pvc.yaml
│   ├── minio-pvc.yaml
│   └── rabbitmq-pvc.yaml
├── deployments/
│   ├── redis-deployment.yaml
│   ├── elasticsearch-deployment.yaml
│   ├── minio-deployment.yaml
│   ├── rabbitmq-deployment.yaml
│   └── opencti-deployment.yaml
├── services/
|   ├── redis-service.yaml
|   ├── elasticsearch-service.yaml
|   ├── minio-service.yaml
|   ├── rabbitmq-service.yaml
|   └── opencti-service.yaml
└── connectors/
    ├── abuseipdb-connector-deployment.yaml
    ├── alienvault-connector-deployment.yaml
    ├── analysis-connector-deployment.yaml
    ├── cisa-connector-deployment.yaml
    ├── cuckoo-sandbox-connector-deployment.yaml
    ├── export-file-csv-connector.yaml
    ├── export-file-stix-connector.yaml
    ├── export-file-txt-connector.yaml
    ├── import-document-connector-deployment.yaml
    ├── import-file-stix-connector-deployment.yaml
    ├── malpedia-connector-deployment.yaml
    ├── malware-bazaar-connector-deployment.yaml
    ├── mitre-connector-deployment.yaml
    └── opencti-datasets-connector-deployment.yaml
```

## Pré-requisitos

- Cluster Kubernetes
- Kubectl instalado e configurado - [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- uuidgen instalado

## Passos para o Deploy

1. Clone o repositório:
```bash
git clone git@github.com:bob-reis/opencti-kubernetes.git
```

2. Configure os Secrets e fique atento para as trocas de senhas e tokens

`echo -n "seu email" | base64`

Adicione o resultado no arquivo secrets/opencti-secrets.yaml

`echo -n "sua senha" | base64`

Adicione o resultado no arquivo secrets/opencti-secrets.yaml

`uuidgen`

Anote o Token gerado

`echo -n "seu token" | base64`

Adicione o resultado no arquivo secrets/opencti-secrets.yaml

Da mesma forma, ajuste os usuarios e senhas dos secrets minio-secrets.yaml e rabbitmq-secrets.yaml

Tanto o conecter do Alienvault como o conetor do AbuseIPDB necessitam de uma chave de API, que deve ser adicionada nos arquivos de configuração do conector
secrets/alienvault-connector-secrets.yaml e secrets/abuseipdb-connector-secrets.yaml


3. Execute o kustomization:
    
```bash
kubectl apply -k .
```
4. Verifique a implementação:

```bash
kubectl get all -n kube-opencti
```

5. Acesse o OpenCTI no navegador:

```plaintext
http://<IP_DO_SERVICE_OPENCTI>:8080
```



