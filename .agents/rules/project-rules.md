# Regras de trabalho do projeto

## Fidelidade ao comportamento

- Leia a implementação relacionada antes de editar; nomes como “RAG”, “OPA”, “AIOps” ou “ChatOps” podem designar apenas uma simulação.
- Diferencie em documentação, respostas e testes o comportamento implementado do objetivo didático descrito nos slides.
- Preserve a interface das ferramentas CrewAI e os fluxos dos 12 laboratórios, a menos que a tarefa peça explicitamente uma mudança.
- Evite adicionar integrações externas, dependências ou chamadas de escrita a cloud/cluster sem requisito explícito.

## Segurança operacional

- Trate `kubectl apply`, Terraform, APIs de cloud e criação de buckets como operações com efeitos reais; confirme contexto, endpoint e autorização antes de executá-las.
- Prefira fixtures locais, dry-run e validadores sem escrita; explique quando uma ferramenta não oferece um dry-run confiável.
- Nunca coloque chaves, tokens, senhas reais ou dados privados em arquivos versionados. O Secret de demonstração e a senha fixa do ChatOps não são padrões de produção.
- Não confie em saídas de sucesso simuladas como prova de que a operação aconteceu ou foi validada.

## Alterações e validação

- Faça mudanças pequenas e relacionadas ao pedido; mantenha estilo Python e configuração existentes.
- Atualize `docs/knowledge/` quando componentes, fluxos, setup ou limitações forem alterados.
- Use verificações locais e focalizadas. Não acesse contas cloud nem clusters para validar mudanças sem autorização específica.
- Ao concluir, informe arquivos alterados, verificações executadas e limitações pendentes.
