# Limitações conhecidas e validação

Esta lista registra observações estáticas do conteúdo do módulo. Não representa uma auditoria completa nem substitui testes em um ambiente isolado.

## Bloqueio de execução a resolver primeiro

- [`core/agents.py`](../../core/agents.py) termina com um trecho de código órfão (`=True ... )`) depois da definição do último agente. Esse trecho é sintaxe Python inválida e impede a importação do módulo. Como vários laboratórios importam `core.agents`, isso bloqueia sua execução até que o código seja corrigido.
- O acesso ao LLM exige `GROQ_API_KEY` válida e conectividade com o provedor; o arquivo de exemplo do Kubernetes não fornece uma credencial válida.
- Não foram encontrados testes automatizados na árvore inventariada. As saídas dos laboratórios não devem ser interpretadas como validação automática.

## Simulação versus integração

- `tools/obs_tools.py`, `tools/k8s_diag.py`, `tools/aiops_tools.py` e `tools/governance_tools.py` devolvem respostas fixas ou baseadas em texto; não são conectores completos para Prometheus, Jaeger, ML, Grafana ou sistemas de custo.
- A rotina de canary em `tools/k8s_ops.py` procura palavras em uma string, não avalia séries de métricas nem executa rollback.
- `tools/chatops_tools.py` não executa Terraform. A senha fixa `GESTOR-APROVA` é um exemplo didático, aparece na documentação e não é um mecanismo de autenticação.
- O módulo 11 exibe um pedido de aprovação e imprime um resultado simulado. Não invoca `kubectl`, não verifica o resultado do dry-run e não grava um log de auditoria.
- O módulo 12 cria agentes sem ferramentas neste fluxo, portanto não coleta evidências reais do cluster, do scanner ou da conta cloud.
- O módulo 7 lê uma fixture JSON do Trivy; não inicia um scan e não remedia a vulnerabilidade reportada.
- O nome `policy_rag.py` não corresponde a um sistema RAG completo: a ferramenta retorna uma única resposta fixa, sem recuperação de documentos.

## Comandos e ações com efeitos externos

- `tools/k8s_ops.apply_k8s_manifest` executa `kubectl apply -f` se `kubectl` estiver instalado. Com um contexto configurado, isso pode escrever em um cluster real.
- Quando `kubectl apply` termina com código diferente de zero, a ferramenta apresenta uma mensagem de simulação dizendo que o YAML é válido, sem distinguir erro de sintaxe, autorização, conectividade ou falha do cluster. Não use essa resposta como validação.
- `tools/file_writer.write_file` grava o nome fornecido pelo agente no diretório atual, sem restringir o caminho. Execute os exemplos em diretório controlado e revise arquivos antes de usá-los.
- O sandbox de governança da UI usa verificações de substring, não OPA/Rego. Passar por essa verificação não é aprovação de segurança ou compliance.
- O dashboard permite criar buckets no endpoint configurado. Confirme que o endpoint é LocalStack antes de testar; não aponte o sandbox a uma conta real sem revisão.

## Inconsistências e qualidade dos exemplos

- O README descreve o modelo Llama 3.3/3.1, enquanto `core/llm_config.py` configura `groq/llama-3.1-8b-instant`.
- O relatório [`data/trivy.json`](../../data/trivy.json) identifica a imagem `python:3.11-slim`, enquanto o Dockerfile do projeto usa `python:3.12-slim`; o relatório não representa necessariamente a imagem que o Dockerfile constrói.
- Os slides contêm módulos 13.1–13.5, mas o launcher e a pasta `labs/` cobrem somente 1–12.
- O runbook PostgreSQL termina após uma consulta de diagnóstico; a instrução do módulo 10 para recuperar o comando SQL exato de limpeza não está apoiada pelo conteúdo atual.
- `k8s/ollama.yaml` repete a chave YAML `limits` no mesmo mapa de recursos; o segundo valor pode sobrescrever o primeiro dependendo do parser.
- O `.dockerignore` omite `data/*.json`, mas os módulos 7 e 9 esperam ler fixtures JSON.
- `boto3` é importado por `ui/app.py` e `k8s/connect-test.yaml`, mas não é dependência explícita em `requirements.txt`.
- O workflow chamado “rápido” usa `actions/cache@v3` e `npm install`; é um exemplo didático e não prova, por si só, compatibilidade ou ganho de performance.
- O nome do bucket de [`main.tf`](../../main.tf) é fixo e precisa ser único globalmente na AWS.

## Caminho recomendado para validação futura

1. Corrigir o bloqueio de sintaxe em `core/agents.py` e instalar as dependências em ambiente virtual isolado.
2. Rodar verificações de sintaxe e testes automatizados, adicionando testes para regras, ferramentas e fluxos de aprovação.
3. Validar YAML com parser/schema Kubernetes e Terraform com `terraform fmt`, `terraform validate` e plano revisado.
4. Testar integrações somente em ambiente local/isolado (por exemplo, LocalStack e cluster descartável), sem credenciais de produção.
5. Substituir credenciais e aprovações fixas por mecanismos seguros antes de qualquer integração operacional.

Esses passos são recomendações; não foram executados como parte da criação desta documentação.
