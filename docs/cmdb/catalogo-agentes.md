# Catálogo de Agentes GhostSeg

Status: v0.1
Data-base: 2026-06-24

## 1. Agentes estratégicos

| ID | Agente | Tipo | Função | Status |
|---|---|---|---|---|
| AGT-0001 | SIA | IA/RAG | Assistente corporativo de conhecimento | Planejado |
| AGT-0002 | GHO-Agent | Automação móvel | Execução remota via Termux, SSH e ADB | Em execução |
| AGT-0003 | Registry Manager | Governança | Atualizar Master Registry e CMDB | Planejado |
| AGT-0004 | Auditor de Conectores | Governança | Validar Google Drive, GitHub, Figma, Dropbox e Dash | Em execução |
| AGT-0005 | PMO Assistant | Gestão | Checklist, sprint, status e decisões | Em execução |

## 2. Agentes técnicos

| ID | Agente | Domínio | Dependências | Status |
|---|---|---|---|---|
| AGT-0010 | GitHub Auditor | DevOps | GitHub, roadmap, GHOBIBLIOTECA | Planejado |
| AGT-0011 | Drive Librarian | Conhecimento | Google Drive, Docs, Sheets | Planejado |
| AGT-0012 | Figma Mapper | Visual | Figma/FigJam, Mermaid | Planejado |
| AGT-0013 | Dropbox Archivist | Backup | Dropbox, Dropbox Dash | Planejado |
| AGT-0014 | Prompt Curator | IA | Catálogo de prompts | Planejado |
| AGT-0015 | Artifact Controller | Governança | Inventário de artefatos | Planejado |

## 3. Agentes por produto

| ID | Produto | Agente previsto | Função |
|---|---|---|---|
| AGT-0101 | GhoCam | Video Intelligence Agent | Triagem de eventos, Frigate, alertas e dashboards |
| AGT-0102 | GhoTracker | Tracking Ops Agent | Rastreamento, Traccar, telemetria e alertas |
| AGT-0103 | GhoCidade | City Services Agent | Serviços locais, educação e atendimento |
| AGT-0104 | GhoRecovery | Recovery Dispatch Agent | Pronta resposta e recuperação veicular |
| AGT-0105 | Universidade Corporativa | Learning Curator Agent | Trilhas, cursos e certificações |

## 4. Regra de cadastro

Todo agente deve possuir: objetivo, escopo, entradas, saídas, permissões, ferramentas, logs, riscos e plano de rollback. Agente sem escopo vira estagiário digital solto no datacenter.