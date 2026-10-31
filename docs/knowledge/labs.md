# Catálogo dos laboratórios e slides

O README apresenta 12 laboratórios. Os slides também incluem material de implantação rotulado como módulos 13.1–13.5, sem scripts Python correspondentes na pasta `labs/`.

## Laboratórios 1–12

| Módulo | Arquivo principal | Objetivo e comportamento do código |
|---|---|---|
| 1 — Foundation | [`modulo1_foundation.py`](../../labs/modulo1_foundation.py) | Um agente Arquiteto consulta uma regra fixa de compliance e propõe um desenho para bucket S3. Não cria infraestrutura. |
| 2 — IaC Copilot | [`modulo2_iac_copilot.py`](../../labs/modulo2_iac_copilot.py) | Pipeline sequencial para gerar `main.tf`, gravá-lo, chamar Checkov e verificar regras simplificadas. |
| 3 — K8s AI-Ops/GitOps | [`modulo3_k8s_ops.py`](../../labs/modulo3_k8s_ops.py) | Gera manifesto, tenta aplicar com `kubectl` e avalia métricas via ferramenta baseada em texto. `kubectl apply` pode alterar o cluster ativo. |
| 4 — Troubleshooting/Self-Healing | [`modulo4_troubleshooting.py`](../../labs/modulo4_troubleshooting.py) | Combina diagnóstico simulado e respostas fixas de métricas/traces, depois pede ao Arquiteto um arquivo YAML corretivo. O script não aplica esse arquivo. |
| 5 — AIOps preditivo | [`modulo5_aiops.py`](../../labs/modulo5_aiops.py) | Demonstra tradução heurística para PromQL, alerta fixo de saturação e geração de `incident_dashboard.json`; não consulta Prometheus nem executa ML/Grafana. |
| 6 — ChatOps | [`modulo6_chatops.py`](../../labs/modulo6_chatops.py) | Interface Streamlit de chat que chama `execute_terraform`; essa ferramenta apenas simula o resultado. |
| 7 — DevSecOps | [`modulo7_devsecops.py`](../../labs/modulo7_devsecops.py) | Lê `data/trivy.json` e pede ao agente uma triagem. O JSON é uma fixture; o laboratório não executa Trivy nem corrige vulnerabilidades. |
| 8 — CI/CD | [`modulo8_cicd.py`](../../labs/modulo8_cicd.py) | Lê o workflow lento e solicita recomendações ao agente. Não atualiza o arquivo de workflow. |
| 9 — FinOps | [`modulo9_finops.py`](../../labs/modulo9_finops.py) | Lê inventário JSON para análise do agente; a leitura é real, mas não consulta AWS nem calcula custos por API. |
| 10 — Runbooks/RAG | [`modulo10_remediation.py`](../../labs/modulo10_remediation.py) | Lê `data/runbook_db.md` e pede ao agente um plano/post-mortem. A ferramenta é leitura de arquivo, sem pipeline de embeddings ou busca vetorial. |
| 11 — Guardrails/HITL | [`modulo11_guardrails.py`](../../labs/modulo11_guardrails.py) | Solicita uma proposta de comando com dry-run, pede aprovação via `input()` e imprime um sucesso simulado. Não chama `kubectl`; a opção `--dry-run` dos slides não é processada pelo script. |
| 12 — Projeto final | [`modulo12_projeto_final.py`](../../labs/modulo12_projeto_final.py) | Demonstra uma Crew hierárquica para incidente multidomínio. Os especialistas são criados sem ferramentas neste script, e a missão é descritiva. |

### Arquivos usados pelos laboratórios

- IaC: [`main.tf`](../../main.tf), [`tools/file_writer.py`](../../tools/file_writer.py), [`tools/security_scan.py`](../../tools/security_scan.py).
- Kubernetes: [`tools/k8s_ops.py`](../../tools/k8s_ops.py), [`tools/k8s_diag.py`](../../tools/k8s_diag.py), [`tools/obs_tools.py`](../../tools/obs_tools.py), [`checkout-broken.yaml`](../../checkout-broken.yaml) e [`checkout-k8s-fix.yaml`](../../checkout-k8s-fix.yaml).
- AIOps: [`incident_dashboard.json`](../../incident_dashboard.json).
- DevSecOps: [`data/trivy.json`](../../data/trivy.json).
- CI/CD: [`data/workflow_lento.yaml`](../../data/workflow_lento.yaml) e [`data/workflow_rapido.yaml`](../../data/workflow_rapido.yaml).
- FinOps: [`data/inventario_cloud.json`](../../data/inventario_cloud.json).
- Remediação: [`data/runbook_db.md`](../../data/runbook_db.md).

## Conteúdo dos slides

| Slides | Tema |
|---|---|
| [`slides1.md`](../../slides/slides1.md) | Fundamentos de IA para DevOps, agentes, RAG e prompting. |
| [`slides2.md`](../../slides/slides2.md) | Geração IaC, Checkov, OPA e governança. |
| [`slides3.md`](../../slides/slides3.md) | Manifestos Kubernetes, canary e GitOps. |
| [`slides4.md`](../../slides/slides4.md) | ReAct, diagnóstico de pods, observabilidade e self-healing. |
| [`slides5.md`](../../slides/slides5.md) | NL-to-PromQL, previsão de anomalias e dashboard. |
| [`slides6.md`](../../slides/slides6.md) | ChatOps, RBAC e Human-in-the-Loop. |
| [`slides7.md`](../../slides/slides7.md) | Triagem DevSecOps e caso CVE-2024-3094. |
| [`slides8.md`](../../slides/slides8.md) | Otimização de workflow CI/CD e cache. |
| [`slides9.md`](../../slides/slides9.md) | FinOps, recursos zumbis e rightsizing. |
| [`slides10.md`](../../slides/slides10.md) | Runbooks, RAG e post-mortem. |
| [`slides11.md`](../../slides/slides11.md) | Guardrails, dry-run e aprovação humana. |
| [`slides12.md`](../../slides/slides12.md) | Orquestração hierárquica multidomínio. |
| [`slides131.md`](../../slides/slides131.md) | Dockerização. |
| [`slides132.md`](../../slides/slides132.md) | Kubernetes local com Minikube. |
| [`slides133.md`](../../slides/slides133.md) | LocalStack dentro do Kubernetes. |
| [`slides134.md`](../../slides/slides134.md) | Interface Streamlit no Kubernetes. |
| [`slides135.md`](../../slides/slides135.md) | Ollama e modelo local no Kubernetes. |

## Execução

Os comandos gerais estão no [README do módulo](../../README.md). Em termos gerais:

```powershell
python .\nexus_iac_copilot.py
python .\labs\modulo11_guardrails.py
python -m streamlit run .\ui\app.py
```

Execute a partir da raiz deste módulo para que caminhos relativos gravados pelas ferramentas apontem para o local esperado. Alguns exercícios podem chamar ferramentas locais ou atuar no cluster Kubernetes configurado; veja [infraestrutura e dados](infrastructure-and-data.md) e [limitações](limitations-and-validation.md) antes de rodar.
