# Especificação: Nexus AI-Ops — visão geral

## Propósito

Descrever o escopo atual do módulo educacional Nexus AI-Ops, que demonstra padrões de agentes CrewAI e operações de infraestrutura por meio de doze laboratórios Python, interfaces Streamlit e fixtures.

## Atores e componentes

- **Operador/aluno:** inicia laboratórios pelo menu CLI, executa scripts ou usa as interfaces Streamlit.
- **Agentes especialistas:** personas CrewAI configuradas para arquitetura cloud, SRE, AIOps, DevSecOps, CI/CD, FinOps, ChatOps e coordenação.
- **Ferramentas:** funções CrewAI que leem fixtures, retornam dados simulados, gravam arquivos ou chamam alguns comandos locais.
- **Serviços externos opcionais:** Groq, Checkov, kubectl/Kubernetes e LocalStack, dependendo do caminho executado.

## Capacidades no escopo

1. Demonstrar consultas simples a políticas de infraestrutura e geração/auditoria didática de Terraform.
2. Gerar manifestos Kubernetes e demonstrar uma tentativa de reconciliação.
3. Demonstrar diagnóstico de incidentes, análise conceitual de observabilidade e criação de um manifesto corretivo.
4. Apresentar exemplos de NL-to-PromQL, alerta preditivo, triagem DevSecOps, otimização CI/CD e FinOps.
5. Consultar um runbook local e produzir uma proposta de remediação/post-mortem.
6. Demonstrar aprovação humana em terminal ou ChatOps e coordenação hierárquica de agentes.
7. Expor um dashboard para navegação de agentes, interação com endpoint S3 configurado e avaliação de regras locais.

## Limites fora do escopo atual

- Não presumir que o projeto opera produção ou que integrações simuladas consultam serviços reais.
- Não executar remediações reais em cluster, Terraform ou cloud como resultado automático das respostas de agentes.
- Não tratar o exemplo de política Python como OPA/Rego, as regras por palavra-chave como modelo preditivo, ou a leitura de um runbook como busca vetorial.
- Não considerar os slides 13.1–13.5 funcionalidades implementadas nos 12 scripts de laboratório.

## Requisitos transversais

- **SPEC-001 — Clareza de simulação:** documentação e respostas devem identificar simulações e dados fixture como tais.
- **SPEC-002 — Operações seguras:** qualquer ação com efeito externo exige inspeção do alvo e autorização explícita; usar dry-run quando disponível.
- **SPEC-003 — Segredos:** credenciais devem vir de mecanismo local/protegido e não ser commitadas. Valores placeholder não devem ser tratados como credenciais utilizáveis.
- **SPEC-004 — Evidência:** não afirmar que testes, scans ou deploys passaram sem executar e observar o resultado.
- **SPEC-005 — Rastreabilidade:** mudanças em fluxos documentados devem atualizar a base de conhecimento e os critérios relevantes.

## Critérios de aceitação deste documento

- A especificação diferencia claramente objetivo pedagógico e comportamento atual.
- As capacidades são rastreáveis aos arquivos descritos em [`docs/knowledge/labs.md`](../../docs/knowledge/labs.md) e [`docs/knowledge/architecture.md`](../../docs/knowledge/architecture.md).
- Riscos e problemas conhecidos permanecem registrados em [`docs/knowledge/limitations-and-validation.md`](../../docs/knowledge/limitations-and-validation.md).
