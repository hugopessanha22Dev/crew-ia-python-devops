# Contexto local dos agentes

Este diretório contém contexto compartilhado do projeto para agentes compatíveis com o formato Agent Skills.

## Conteúdo

- [`skills/`](skills/): skills reutilizáveis de CrewAI, Kubernetes e Terraform instaladas para este workspace.
- [`knowledge/`](knowledge/): índice para a documentação técnica detalhada em [`docs/knowledge/`](../docs/knowledge/README.md).
- [`rules/`](rules/): convenções e limites de segurança para mudanças neste módulo.
- [`specs/`](specs/): especificação de alto nível do comportamento e dos limites atuais do sistema.
- [`../AGENTS.md`](../AGENTS.md): instruções breves carregadas para tarefas neste projeto.

As skills locais evitam depender apenas da instalação pessoal em `~/.agents/skills` e podem ser versionadas junto do projeto. Confira a origem e o conteúdo de skills antes de atualizá-las.
