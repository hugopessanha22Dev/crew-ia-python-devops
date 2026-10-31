# Arquitetura e componentes

## Visão geral

O Nexus é um projeto educacional de automação de operações assistida por LLM. O fluxo pretendido combina:

1. um modelo Groq configurado em `core/llm_config.py`;
2. agentes com papéis especializados criados em `core/agents.py`;
3. tarefas e equipes CrewAI declaradas nos scripts de `labs/`;
4. ferramentas em `tools/` que consultam fixtures, geram arquivos ou chamam alguns binários locais;
5. interfaces CLI e Streamlit para disparar exercícios;
6. arquivos Terraform, Kubernetes, JSON e YAML usados como exemplos e entradas de laboratório.

Este é um **laboratório local**, não um operador de produção. Prometheus, Jaeger, OPA, Grafana, Slack e várias ações de cloud são apresentados como conceitos ou respostas simuladas, não como integrações completas.

## Componentes

### Configuração e agentes

- [`core/llm_config.py`](../../core/llm_config.py) carrega variáveis do `.env` com `python-dotenv` e configura `groq/llama-3.1-8b-instant`, temperatura `0.2`, usando `GROQ_API_KEY`.
- [`core/agents.py`](../../core/agents.py) define fábricas para Arquiteto, Auditor, SRE, AIOps, ChatOps, DevSecOps, CI/CD, FinOps, SRE de runbooks e Nexus Manager.
- Os laboratórios criam `Task` e `Crew`; usam processos sequenciais ou, no projeto final, hierárquico.
- A configuração é compartilhada: os agentes consomem o mesmo objeto `nexus_llm`.

### Ferramentas

| Arquivo | Responsabilidade | Natureza observada |
|---|---|---|
| [`policy_rag.py`](../../tools/policy_rag.py) | Retorna regras Nexus de nomenclatura, região e bucket privado. | Resposta fixa; não há índice ou recuperação documental. |
| [`file_writer.py`](../../tools/file_writer.py) | Persiste texto em arquivo. | Escrita real no diretório de trabalho atual. |
| [`security_scan.py`](../../tools/security_scan.py) | Executa Checkov e valida regras adicionais. | Checkov usa subprocesso; a validação chamada OPA é lógica Python por busca de texto. |
| [`k8s_ops.py`](../../tools/k8s_ops.py) | Gera manifestos, aplica com kubectl e avalia métricas de canary. | Geração e chamada kubectl são reais; a análise de canary é heurística textual. |
| [`k8s_diag.py`](../../tools/k8s_diag.py) | Diagnostica pods e sugere correções. | Respostas fixas por nome/tipo de incidente. |
| [`obs_tools.py`](../../tools/obs_tools.py) | Consulta de métricas e traces. | Respostas fixas; não faz chamadas a Prometheus ou Jaeger. |
| [`aiops_tools.py`](../../tools/aiops_tools.py) | Traduz linguagem natural, prevê saturação e cria dashboard. | Regras por palavras-chave e previsão fixa; escreve JSON no diretório atual. |
| [`chatops_tools.py`](../../tools/chatops_tools.py) | Representa execução Terraform com aprovação. | Simulação de resultado; não inicia Terraform nem autentica um gestor. |
| [`governance_tools.py`](../../tools/governance_tools.py) | Respostas de triagem DevSecOps, CI/CD e FinOps. | Respostas fixas; não analisa os parâmetros recebidos. |

### Interfaces e execução

- [`nexus_iac_copilot.py`](../../nexus_iac_copilot.py) oferece menu para disparar os módulos 1–12 e o dashboard principal.
- [`ui/app.py`](../../ui/app.py) implementa o dashboard Streamlit principal. O explorador lista e cria buckets no endpoint S3 configurado; o sandbox chama as regras de validação Python.
- [`labs/modulo6_chatops.py`](../../labs/modulo6_chatops.py) é outra interface Streamlit, focada em conversa com o agente ChatOps.
- [`Dockerfile`](../../Dockerfile) constrói a imagem e inicia o módulo 12 por padrão.

## Fluxos didáticos principais

### IaC com validação

`labs/modulo1_foundation.py` consulta as regras fixas de compliance. `labs/modulo2_iac_copilot.py` combina geração por agente, escrita de `main.tf`, chamada do Checkov e validação simplificada de política.

### Kubernetes e incidentes

`labs/modulo3_k8s_ops.py` encadeia geração de manifesto, `kubectl apply` e avaliação textual de métricas. `labs/modulo4_troubleshooting.py` combina diagnósticos fixos de pod, respostas estáticas de observabilidade e geração de um manifesto corretivo.

### AIOps, ChatOps e operação

Os módulos 5–11 demonstram análise preditiva, chat, triagem de vulnerabilidades, análise de workflow, FinOps, consulta de runbook e aprovação humana. A maior parte das ferramentas devolve valores de demonstração; confira [as limitações](limitations-and-validation.md) antes de tratar uma saída como evidência operacional.

### Orquestração final

`labs/modulo12_projeto_final.py` configura um `Crew` hierárquico com um manager e especialistas SRE, segurança e FinOps. A tarefa formula um cenário integrado, mas os especialistas são criados sem ferramentas concretas nesse script, portanto o exemplo não consulta sistemas reais.

## Dependências entre pastas

- Os scripts de laboratório ajustam `sys.path` para importar `core` e `tools` quando executados como arquivos.
- As ferramentas e agentes dependem de CrewAI; os agentes também dependem da configuração Groq.
- A UI principal importa Boto3 para conversar com LocalStack.
- A aplicação containerizada depende de `PYTHONPATH=/app`.
- Manifestos e scripts assumem que os binários e contextos externos — especialmente `kubectl`, Checkov, Docker/Minikube e LocalStack — estão instalados e configurados quando aplicável.
