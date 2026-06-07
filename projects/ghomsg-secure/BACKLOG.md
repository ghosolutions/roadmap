# GhoMsg Secure — Backlog MVP Text-only

## Status

```text
Projeto: GhoMsg Secure
Versão: v1.1.2
Escopo: Text-only MVP
Repositório de código recomendado: ghosolutions/ghomsg-secure
Status atual: aguardando criação de repositório privado dedicado
```

---

# M0 — Repository Setup

## Tarefa 1 — Criar repositório privado

Criar:

```text
ghosolutions/ghomsg-secure
```

Configuração:

```text
Visibility: Private
Initialize with README: No
Add .gitignore: No
License: No
```

Critérios:

- [ ] Repositório privado criado.
- [ ] Acesso confirmado pela conexão GitHub.
- [ ] Branch `main` disponível.
- [ ] Primeiro commit preparado.

---

# M1 — Backend Auth/RBAC

## Tarefa 2 — Implementar JWT Auth Guard

Entregas:

- [ ] JwtStrategy.
- [ ] JwtAuthGuard.
- [ ] Payload com `sub`, `role`, `operationalCode`, `organizationId`, `deviceId`, `coercion`.
- [ ] Decorator `@CurrentUser()`.

Critério crítico:

- [ ] Nenhuma resposta deve expor identidade real.

## Tarefa 3 — Implementar RBAC

Entregas:

- [ ] Decorator `@Roles()`.
- [ ] RolesGuard.
- [ ] Perfis MASTER, ADMIN_N1, ADMIN_N2, USER.
- [ ] Proteção de endpoints críticos.

---

# M2 — Text-only Messaging

## Tarefa 4 — Consolidar MVP somente texto

Entregas:

- [ ] Aplicar migration `003_scope_text_only_mvp.sql`.
- [ ] Restringir `MessageType` a `TEXT` e `SYSTEM`.
- [ ] Implementar `MessagesModule` text-only.
- [ ] Remover fluxo de mídia do MVP.

Critérios:

- [ ] AUDIO/IMAGE/VIDEO/FILE não aceitos.
- [ ] `encryptedPayload` não retorna em resposta pública.
- [ ] Blind Audit registra envio de texto.

---

# M3 — Sealed Channels

## Tarefa 5 — Implementar canais lacrados

Entregas:

- [ ] `SealedChannelsService` funcional.
- [ ] `hide_member_list = true`.
- [ ] `hide_typing_indicator = true`.
- [ ] `hide_online_status = true`.
- [ ] `disable_forward = true`.
- [ ] `disable_copy = true`.
- [ ] `disable_export = true`.

---

# M4 — Android MVP

## Tarefa 6 — Implementar telas Android text-only

Telas:

- [ ] Splash.
- [ ] Login.
- [ ] PIN.
- [ ] Convite.
- [ ] Aguardando aprovação.
- [ ] Canais seguros.
- [ ] Chat lacrado.
- [ ] Ômega.

Remover:

- [ ] Áudio.
- [ ] Imagem.
- [ ] Vídeo.
- [ ] PDF.
- [ ] Anexos.

---

# M5 — Deploy Local H61

## Tarefa 7 — Preparar deploy local

Entregas:

- [ ] Docker Compose somente com `postgres`, `redis`, `api`.
- [ ] MinIO opcional em profile `future-media`.
- [ ] `.env.example` atualizado.
- [ ] Script de health check.
- [ ] Documentação de execução local.

---

# Backlog v1.2 — Mídia e Arquivos

Itens removidos temporariamente do MVP:

- [ ] PDF somente visualização.
- [ ] Imagem somente visualização.
- [ ] Áudio.
- [ ] Vídeo.
- [ ] Arquivos genéricos.
- [ ] Viewer seguro.
- [ ] MinIO obrigatório.
- [ ] GhostTrace em mídia.
