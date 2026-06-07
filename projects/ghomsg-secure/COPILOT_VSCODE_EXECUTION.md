# GhoMsg Secure — Execução com GitHub Copilot no VS Code

## Versão

```text
GhoMsg Secure v1.1.2 — Text-only MVP
```

## Objetivo

Executar a próxima etapa do desenvolvimento usando **VS Code + GitHub Copilot**, com foco em:

- Flutter Android text-only;
- backend NestJS seguro;
- autenticação JWT;
- RBAC;
- DeviceGuard;
- Identity Vault protegido;
- remoção total de áudio, imagem, PDF e arquivos do MVP.

---

# 1. Pré-requisitos no VS Code

## Extensões obrigatórias

Instalar no VS Code:

```text
GitHub Copilot
GitHub Copilot Chat
Flutter
Dart
ESLint
Prettier
Prisma
Docker
GitLens
```

## Ferramentas locais

```text
Git
Node.js 20+
Docker Desktop ou Docker Engine
Flutter SDK
Android Studio
PostgreSQL client opcional
```

---

# 2. Abrir o projeto

Quando o repositório privado existir:

```bash
git clone https://github.com/ghosolutions/ghomsg-secure.git
cd ghomsg-secure
code .
```

Enquanto o repo privado não existir, usar a pasta local do pacote:

```text
ghomsg-secure-v1.1-sprint01-backend-real
+
ghomsg-secure-v1.1.2-text-only-delta
```

---

# 3. Ordem de execução no Copilot

## Fase 01 — Sanidade do projeto

### Prompt Copilot

```text
Revise todo o projeto GhoMsg Secure v1.1.2 Text-only MVP aberto no VS Code. Identifique erros de estrutura, imports quebrados, módulos NestJS não registrados, conflito no Prisma Schema e arquivos ausentes. Não implemente novas features ainda. Gere uma lista objetiva de correções em ordem de prioridade.
```

### Resultado esperado

- Lista de problemas.
- Sugestão de arquivos a corrigir.
- Nenhuma alteração automática sem revisão.

---

## Fase 02 — Prisma Schema text-only

### Prompt Copilot

```text
Ajuste o backend/prisma/schema.prisma para o MVP text-only. O enum MessageType deve aceitar apenas TEXT e SYSTEM. A model File deve permanecer apenas como backlog v1.2, sem uso operacional no MVP. Garanta que Prisma Client gere sem erro. Não remova entidades de auditoria, Identity Vault, Operational Identity, Devices, Sessions, Invitations, Channels, Messages ou SecurityPolicy.
```

### Comando depois da correção

```bash
cd backend
npx prisma generate
```

---

## Fase 03 — Registrar MessagesModule

### Prompt Copilot

```text
Implemente e registre o MessagesModule no AppModule do NestJS. O módulo deve permitir apenas criação e listagem de mensagens TEXT. Não deve aceitar áudio, imagem, vídeo, PDF ou arquivos. A resposta pública nunca deve retornar encryptedPayload. A criação deve registrar Blind Audit com action TEXT_MESSAGE_CREATED.
```

### Arquivos esperados

```text
backend/src/modules/messages/messages.module.ts
backend/src/modules/messages/messages.controller.ts
backend/src/modules/messages/messages.service.ts
backend/src/modules/messages/dto/create-text-message.dto.ts
backend/src/app.module.ts
```

---

## Fase 04 — JWT Strategy e Guard

### Prompt Copilot

```text
Implemente JwtStrategy e JwtAuthGuard no NestJS para o GhoMsg Secure. O payload deve conter sub, role, operationalCode, organizationId, deviceId e coercion. O request.user deve retornar apenas dados operacionais. Não inclua telefone, nome real, e-mail, avatar, documento ou qualquer dado do Identity Vault.
```

### Arquivos esperados

```text
backend/src/modules/auth/strategies/jwt.strategy.ts
backend/src/common/guards/jwt-auth.guard.ts
backend/src/common/decorators/current-user.decorator.ts
backend/src/common/types/current-user.type.ts
```

---

## Fase 05 — RBAC

### Prompt Copilot

```text
Crie @Roles() e RolesGuard para controlar acesso por MASTER, ADMIN_N1, ADMIN_N2 e USER. MASTER acessa rotas globais. ADMIN_N1 acessa gestão organizacional. ADMIN_N2 acessa gestão local. USER acessa somente canais autorizados. O guard deve usar request.user.role vindo do JWT.
```

### Arquivos esperados

```text
backend/src/common/decorators/roles.decorator.ts
backend/src/common/guards/roles.guard.ts
```

---

## Fase 06 — DeviceGuard

### Prompt Copilot

```text
Implemente DeviceGuard. Toda rota operacional deve validar que o deviceId do JWT existe, está ACTIVE e pertence ao userId do JWT. Se o dispositivo estiver PENDING, SUSPENDED, QUARANTINE ou REVOKED, bloquear a requisição. Não retornar dados sensíveis no erro.
```

### Arquivos esperados

```text
backend/src/common/guards/device.guard.ts
```

---

## Fase 07 — IdentityVaultController real

### Prompt Copilot

```text
Remova o actor mockado do IdentityVaultController. Proteja a rota reveal com JwtAuthGuard, RolesGuard MASTER e DeviceGuard. Use @CurrentUser() para montar o actor real. Exija senha de conformidade e justificativa. Registre identity_access_logs e audit_logs com metadataSensitivity SEALED.
```

### Arquivos esperados

```text
backend/src/modules/identity-vault/identity-vault.controller.ts
backend/src/modules/identity-vault/identity-vault.service.ts
```

---

## Fase 08 — Flutter text-only UI

### Prompt Copilot

```text
Implemente a estrutura inicial Flutter do GhoMsg Secure v1.1.2 Text-only MVP. Crie rotas e telas conceituais funcionais para: Splash, Convite, Criação de Credenciais, Aguardando Aprovação, Login, PIN, Lista de Canais, Chat Text-only, Detalhe da Mensagem, Protocolo Ômega, Quarentena e Admin N2. Remova totalmente botões ou fluxos de áudio, imagem, vídeo, PDF e anexos.
```

### Estrutura sugerida

```text
mobile/lib/
├── main.dart
├── app.dart
├── core/theme/ghomsg_theme.dart
├── core/router/app_router.dart
├── features/auth/
├── features/invitations/
├── features/channels/
├── features/chat/
├── features/security/
└── shared/widgets/
```

---

## Fase 09 — Testes mínimos

### Prompt Copilot

```text
Crie testes unitários para garantir que MessagesService aceita somente mensagens TEXT, rejeita texto vazio, não retorna encryptedPayload e registra Blind Audit. Crie também testes para PasswordService, CryptoService e OperationalCode generator.
```

### Comandos

```bash
cd backend
npm run test
npm run build
```

---

# 4. Comandos de validação

## Backend

```bash
cd backend
npm install
npx prisma generate
npm run build
npm run test
npm run start:dev
```

## Infra

```bash
docker compose up -d postgres redis
```

## Health check

```bash
curl http://localhost:3000/api/v1/health
```

## Flutter

```bash
cd mobile
flutter pub get
flutter analyze
flutter test
flutter run
```

---

# 5. Regras que o Copilot não pode violar

```text
Não implementar envio de áudio.
Não implementar envio de imagem.
Não implementar envio de vídeo.
Não implementar envio de PDF.
Não implementar anexos.
Não retornar encryptedPayload em resposta pública.
Não expor telefone.
Não expor nome real.
Não expor e-mail.
Não expor avatar.
Não usar biometria.
Não permitir cadastro público.
Não permitir entrada sem convite.
Não mostrar lista de membros para usuário comum.
```

---

# 6. Prompt mestre para usar no Copilot Chat

```text
Você é o copiloto técnico do projeto GhoMsg Secure v1.1.2 Text-only MVP.

Contexto:
- App Android-first em Flutter.
- Backend NestJS + Prisma + PostgreSQL + Redis.
- MVP somente texto.
- Foram removidos áudio, imagem, vídeo, PDF, arquivos e anexos.
- Entrada apenas por convite temporário de 10 minutos.
- 1 convite = 1 dispositivo.
- Operação diária usa OP-ID, ADM-ID, CH-ID, DEV-ID e SES-ID.
- Identidade real fica isolada no Identity Vault.
- Usuário comum não vê lista de membros.
- Admin N2 não vê telefone, nome real ou Identity Vault.
- Master pode abrir Identity Vault somente com senha de conformidade, justificativa e auditoria SEALED.
- Sem biometria.

Tarefa atual:
Analise o código aberto no VS Code, implemente a próxima menor etapa segura e explique quais arquivos foram alterados. Não adicione funcionalidades fora do escopo text-only. Sempre preserve privacidade, metadata reduction, blind audit e separação de identidade real/operacional.
```
