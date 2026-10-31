# Infraestrutura, configuração e dados

## Ambiente Python

As dependências estão em [`requirements.txt`](../../requirements.txt), sem versões fixadas:

- CrewAI e `crewai[tools]`;
- integração Groq via `langchain-groq` e `litellm`;
- `python-dotenv`;
- Streamlit;
- Checkov e cliente Kubernetes Python;
- Pandas e NumPy.

A UI principal importa `boto3`, mas ele não aparece como dependência explícita no arquivo. O README recomenda Python 3.10–3.13, e o Dockerfile usa Python 3.12.

### Variáveis e credenciais

- `GROQ_API_KEY`: carregada pelo `python-dotenv` e usada pelo LLM em `core/llm_config.py`.
- `AWS_ENDPOINT_URL`: opcional para apontar a UI ao LocalStack; a UI usa `http://localhost:4566` por padrão e substitui por `http://localstack:4566` quando `RUNNING_IN_DOCKER` está definido.
- A UI usa credenciais AWS de demonstração fixas para o endpoint S3. Não use essas configurações para uma conta AWS real.
- `k8s/secret.yml` contém um valor de exemplo codificado em Base64, não uma chave operacional. Base64 não protege segredos.

## Docker

O [`Dockerfile`](../../Dockerfile):

1. parte de `python:3.12-slim`;
2. instala ferramentas de compilação e `curl`;
3. instala `requirements.txt`;
4. copia o contexto para `/app`;
5. define `PYTHONPATH=/app`;
6. inicia `labs/modulo12_projeto_final.py`.

O [`.dockerignore`](../../.dockerignore) exclui `.env`, o ambiente virtual, caches e `data/*.json`. Essa exclusão impede que as fixtures JSON de inventário e Trivy sejam incluídas na imagem se os laboratórios 7 e 9 forem executados nela. A imagem padrão inicia o módulo 12, não o dashboard Streamlit.

## Terraform

[`main.tf`](../../main.tf) contém um exemplo AWS em `us-east-1` com bucket S3 privado, versionamento e criptografia SSE-S3. É um exemplo didático simples: não declara versão do provider, backend de estado ou estrutura de ambientes. O bucket usa nome fixo, que precisa ser globalmente único para um `terraform apply` funcionar.

`tools/security_scan.py` chama Checkov via subprocesso quando o comando está instalado. A função `validate_opa_policies` tem regras locais em Python; não carrega arquivos Rego nem executa OPA.

## Kubernetes e serviços locais

| Manifesto | Recurso/uso |
|---|---|
| [`k8s/deploy.yml`](../../k8s/deploy.yml) | Deployment do `nexus-bot`, com Secret de LLM, limites/requests e `imagePullPolicy: Never`. |
| [`k8s/job.yaml`](../../k8s/job.yaml) | Job para executar o Nexus-Bot; usa o Secret e endpoint LocalStack. |
| [`k8s/connect-test.yaml`](../../k8s/connect-test.yaml) | Job de teste de conexão S3 que importa Boto3. |
| [`k8s/localstack.yml`](../../k8s/localstack.yml) | Service e Deployment do LocalStack para S3/SQS/IAM. |
| [`k8s/ollama.yaml`](../../k8s/ollama.yaml) | Service e Deployment do Ollama. |
| [`k8s/streamlit.yaml`](../../k8s/streamlit.yaml) | Service e Deployment do dashboard Streamlit. |
| [`k8s/secret.yml`](../../k8s/secret.yml) | Secret de exemplo com placeholder. |

Na raiz também existem manifestos independentes para Nexus API: [`nexus-api-k8s.yaml`](../../nexus-api-k8s.yaml), [`nexus-api-error-k8s.yaml`](../../nexus-api-error-k8s.yaml) e [`nexus-api-unipds-k8s.yaml`](../../nexus-api-unipds-k8s.yaml). O cenário quebrado e sua correção para o módulo 4 estão em `checkout-broken.yaml` e `checkout-k8s-fix.yaml`.

Os slides mencionam nomes diferentes dos arquivos existentes em alguns comandos — por exemplo, `k8s/localstack.yaml` e `k8s/secrets.yaml`; os arquivos no projeto são `k8s/localstack.yml` e `k8s/secret.yml`. Os comandos dos slides de Minikube usam shell Unix e podem precisar de adaptação no PowerShell.

## Dados e fixtures

- [`data/inventario_cloud.json`](../../data/inventario_cloud.json): exemplo de volume EBS desanexado, instância EC2 ociosa e Elastic IP não associado, com valores estimados.
- [`data/trivy.json`](../../data/trivy.json): resultado de demonstração com CVE-2024-3094 crítica, além de alertas de outras severidades.
- [`data/workflow_lento.yaml`](../../data/workflow_lento.yaml): workflow GitHub Actions sem cache de dependências.
- [`data/workflow_rapido.yaml`](../../data/workflow_rapido.yaml): exemplo com cache de `~/.npm`; serve como comparação, não é gerado automaticamente pelo módulo 8.
- [`data/runbook_db.md`](../../data/runbook_db.md): runbook PostgreSQL para saturação de conexões; o arquivo atual termina no SQL de diagnóstico e não apresenta a etapa de remediação prometida pelo laboratório.
- [`incident_dashboard.json`](../../incident_dashboard.json): fixture de dashboard Grafana com painéis de disco e taxa de erro.

## Interfaces

- Dashboard principal: `python -m streamlit run ui/app.py`, com abas de agentes, armazenamento S3 local e sandbox de políticas.
- Simulador ChatOps: `python -m streamlit run labs/modulo6_chatops.py`.
- Launcher: `python nexus_iac_copilot.py`.

Os slides também descrevem Minikube, LocalStack, Streamlit e Ollama; são orientações de laboratório e não comprovam que esses serviços estejam provisionados ou integrados ao LLM da aplicação.
