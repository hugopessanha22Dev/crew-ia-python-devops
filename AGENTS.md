# Orientações para agentes

- Antes de alterar o projeto, consulte [a base de conhecimento](docs/knowledge/README.md), especialmente as limitações conhecidas.
- Siga as regras específicas em [.agents/rules/project-rules.md](.agents/rules/project-rules.md).
- Consulte as skills em [.agents/skills/](.agents/skills/) para tarefas de CrewAI, Terraform e Kubernetes.
- Trate módulos, dados e ferramentas como material didático: identifique explicitamente o que é simulado e não apresente respostas fixas como integração real.
- Não execute comandos que alterem cloud, arquivos de infraestrutura ou clusters sem verificar o alvo e obter autorização explícita. Prefira validações locais/dry-run.
- Preserve as fixtures e os exemplos existentes, salvo quando a tarefa pedir sua alteração; atualize a documentação relacionada quando o comportamento mudar.
- Execute apenas verificações adequadas ao escopo e reporte claramente o que foi e não foi validado.
