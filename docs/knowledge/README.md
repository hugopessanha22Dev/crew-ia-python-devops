# Base de conhecimento — Nexus AI-Ops

Esta documentação descreve o conteúdo encontrado no módulo `modulo06-aiops-engenharia-agentica`: uma trilha didática de AIOps e engenharia agêntica, com agentes CrewAI, exercícios de infraestrutura, dados de demonstração e slides.

> **Escopo e precisão:** esta base foi escrita a partir dos 67 arquivos do módulo. Ela diferencia o que o material ensina do que o código realmente executa. Muitos serviços externos são simulados; não considere os exemplos um sistema pronto para produção.

## Navegação

- [Arquitetura e componentes](architecture.md): aplicação, agentes, ferramentas e fluxo de dados.
- [Catálogo dos laboratórios e slides](labs.md): módulos 1–12, tópicos extras dos slides 13.1–13.5 e comandos.
- [Infraestrutura e dados](infrastructure-and-data.md): dependências, configuração, Terraform, Kubernetes, Docker e dados de exemplo.
- [Limitações e validação](limitations-and-validation.md): divergências e riscos a considerar antes de executar.

## Mapa do repositório

| Caminho | Conteúdo |
|---|---|
| `core/` | Configuração do LLM e fábricas de agentes CrewAI. |
| `tools/` | Ferramentas de demonstração para compliance, Kubernetes, observabilidade, custos, arquivos, ChatOps e segurança. |
| `labs/` | Doze scripts que representam a progressão didática, da IA consultiva à orquestração hierárquica. |
| `ui/` | Dashboard Streamlit com visão geral, explorador S3 do LocalStack e sandbox de regras de governança. |
| `data/` | Fixtures de inventário, relatório Trivy, workflow CI lento e runbook PostgreSQL. |
| `k8s/` | Manifestos para o Nexus, LocalStack, Ollama, Streamlit e jobs de demonstração. |
| `slides/` | Slides Marp dos módulos 1–12 e dos tópicos adicionais 13.1–13.5. |
| Raiz | README, launcher CLI, Dockerfile, Terraform e manifestos Kubernetes de exercícios. |

## Como começar a leitura

1. Leia este índice e [as limitações conhecidas](limitations-and-validation.md).
2. Siga os fluxos em [labs.md](labs.md) para entender a sequência didática.
3. Use [architecture.md](architecture.md) para localizar cada implementação.
4. Antes de executar, confira [infrastructure-and-data.md](infrastructure-and-data.md) e o estado do cluster Kubernetes selecionado.

O launcher interativo é [`nexus_iac_copilot.py`](../../nexus_iac_copilot.py). A documentação de instalação e inicialização existente está no [`README.md`](../../README.md).
