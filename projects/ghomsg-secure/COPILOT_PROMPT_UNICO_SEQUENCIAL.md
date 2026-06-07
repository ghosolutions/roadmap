# GhoMsg Secure — Prompt Único Sequencial para VS Code + Copilot

## Como usar

Cole o prompt abaixo no **GitHub Copilot Chat** dentro do VS Code com o projeto aberto.

Recomendação operacional:

```text
1. Cole o prompt inteiro.
2. Aguarde o Copilot analisar.
3. Peça para executar apenas a Etapa 01.
4. Revise os arquivos alterados.
5. Só depois avance para a etapa seguinte.
```

---

# Prompt Único Sequencial

```text
Você é o copiloto técnico sênior do projeto GhoMsg Secure v1.1.2 — Text-only MVP.

Atue como arquiteto full-stack, especialista em NestJS, Prisma, PostgreSQL, Redis, Flutter Android, segurança de aplicações, RBAC, autenticação JWT e governança de produto.

CONTEXTO DO PRODUTO

O GhoMsg Secure é uma aplicação self-hosted de comunicação operacional segura, com backend NestJS + Prisma + PostgreSQL + Redis e app Android-first em Flutter.

A versão atual é:
GhoMsg Secure v1.1.2 — Text-only MVP.

O MVP atual permite somente:
- mensagem de texto criptografada;
- mensagem de sistema;
- convite único com expiração padrão de 10 minutos;
- 1 convite = 1 dispositivo;
- device binding;
- Identity Vault;
- Operational Identity;
- Blind Audit;
- Metadata Reduction;
- Sealed Channels;
- retenção/expiração de mensagens;
- protocolo Ômega;
- modo de coação;
- auditoria operacional.

FORA DO ESCOPO DO MVP

Não implementar, sugerir ou reativar:
- envio de áudio;
- envio de imagem;
- envio de vídeo;
- envio de PDF;
- envio de arquivos;
- anexos;
- galeria;
- câmera;
- upload/download de mídia;
- viewer seguro de mídia;
- MinIO obrigatório;
- GhostTrace aplicado a arquivos;
- biometria;
- cadastro público;
- login por e-mail público;
- lista pública de usuários;
- lista de membros visível para usuário comum.

REGRAS CRÍTICAS DE SEGURANÇA

1. Nenhuma resposta pública da API pode retornar:
- telefone;
- nome real;
- e-mail;
- avatar;
- documento;
- endereço;
- password_hash;
- compliance_password_hash;
- coercion_password_hash;
- quick_pin_hash;
- coercion_pin_hash;
- encryptedPayload;
- contact_number_encrypted;
- real_identity_notes_encrypted.

2. A operação comum deve usar apenas identidades operacionais:
- OP-ID para usuário comum;
- ADM2-ID para Admin N2;
- ADM1-ID para Admin N1;
- MST-ID para Master;
- CH-ID para canal;
- DEV-ID para dispositivo;
- SES-ID para sessão.

3. Identity Vault só pode ser aberto por MASTER, com:
- JWT válido;
- DeviceGuard válido;
- role MASTER;
- senha de conformidade;
- justificativa obrigatória;
- registro em identity_access_logs;
- registro em audit_logs com metadataSensitivity = SEALED.

4. Admin N2 não pode ver:
- telefone;
- nome real;
- e-mail;
- Identity Vault;
- estrutura global;
- usuários fora do escopo.

5. Admin N1 não pode ver Identity Vault por padrão.

6. Usuário comum não pode:
- gerar convite;
- criar canal livremente;
- ver membros;
- buscar usuários;
- encaminhar;
- copiar;
- exportar;
- anexar arquivos.

7. Se senha ou PIN de coação for usado:
- simular acesso normal;
- marcar coercion = true no JWT;
- registrar Blind Audit sensível;
- não revelar ao usuário que o modo de coação foi detectado.

8. Não usar biometria em nenhuma etapa.

OBJETIVO DA EXECUÇÃO

Analise o projeto aberto no VS Code e implemente a próxima menor sequência segura para transformar o scaffold em uma base funcional do MVP text-only.

Você deve trabalhar em etapas pequenas, com validação após cada etapa. Não implemente tudo de uma vez. Para cada etapa:

1. Explique brevemente o objetivo.
2. Liste os arquivos que serão alterados.
3. Faça as alterações.
4. Informe os comandos de validação.
5. Pare e aguarde confirmação antes de avançar para a próxima etapa.

ORDEM OBRIGATÓRIA DE IMPLEMENTAÇÃO

ETAPA 01 — Auditoria de sanidade do projeto

Objetivo:
Revisar a estrutura atual do projeto antes de alterar código.

Verifique:
- se backend/package.json está coerente;
- se backend/prisma/schema.prisma existe;
- se backend/src/app.module.ts registra os módulos necessários;
- se imports estão quebrados;
- se o MessagesModule existe;
- se o enum MessageType ainda contém AUDIO, IMAGE, VIDEO ou FILE;
- se MinIO está obrigatório no docker-compose;
- se há referências ativas a upload, mídia, anexos ou viewer.

Não implemente feature nesta etapa. Apenas gere diagnóstico e correções mínimas de build, se necessário.

Comandos de validação:
cd backend
npm install
npx prisma generate
npm run build
npm run test

ETAPA 02 — Prisma Schema text-only

Objetivo:
Garantir que o banco aceite apenas mensagens TEXT e SYSTEM no MVP.

Ajustar:
backend/prisma/schema.prisma

Regra:
O enum MessageType deve ser:

enum MessageType {
  TEXT
  SYSTEM
}

A model File pode permanecer como backlog v1.2, mas não deve ser usada operacionalmente.

Adicionar comentário acima da model File:
/// Backlog v1.2: mídia, PDF, vídeo, áudio e arquivos.
/// Não usar no MVP text-only.

Não remover:
- users;
- devices;
- sessions;
- invitations;
- operational_identities;
- identity_vault;
- identity_access_logs;
- channels;
- messages;
- audit_logs;
- metadata_policies;
- sealed_channel_policies;
- security_policies.

Validação:
npx prisma generate
npm run build

ETAPA 03 — MessagesModule text-only

Objetivo:
Implementar ou revisar o módulo de mensagens somente texto.

Arquivos esperados:
backend/src/modules/messages/messages.module.ts
backend/src/modules/messages/messages.controller.ts
backend/src/modules/messages/messages.service.ts
backend/src/modules/messages/dto/create-text-message.dto.ts
backend/src/app.module.ts

Regras:
- aceitar apenas criação de mensagem TEXT;
- rejeitar texto vazio;
- limitar texto a 4000 caracteres;
- criptografar payload usando CryptoService;
- salvar encryptedPayload no banco;
- nunca retornar encryptedPayload em resposta pública;
- registrar Blind Audit com action TEXT_MESSAGE_CREATED;
- usar senderOperationalCode;
- aplicar retentionSeconds do canal para expiresAt;
- não aceitar AUDIO, IMAGE, VIDEO, FILE, PDF ou anexos.

DTO esperado:
- channelId: UUID;
- senderUserId: UUID;
- text: string, 1 a 4000 caracteres.

Rotas mínimas:
POST /messages/text
GET /messages/channel/:channelId

Validação:
npm run build
npm run test

ETAPA 04 — JWT Strategy, JwtAuthGuard e CurrentUser

Objetivo:
Implementar autenticação real baseada em JWT.

Arquivos esperados:
backend/src/modules/auth/strategies/jwt.strategy.ts
backend/src/common/guards/jwt-auth.guard.ts
backend/src/common/decorators/current-user.decorator.ts
backend/src/common/types/current-user.type.ts

Payload JWT obrigatório:
{
  sub: string;
  role: 'MASTER' | 'ADMIN_N1' | 'ADMIN_N2' | 'USER';
  operationalCode: string;
  organizationId: string;
  deviceId: string;
  coercion: boolean;
}

Regras:
- request.user deve conter apenas dados operacionais;
- não incluir telefone, nome real, e-mail, avatar ou dados do Identity Vault;
- token inválido deve retornar erro genérico;
- token expirado deve bloquear acesso.

Validação:
npm run build
npm run test

ETAPA 05 — RBAC com RolesGuard

Objetivo:
Implementar controle de acesso por perfil.

Arquivos esperados:
backend/src/common/decorators/roles.decorator.ts
backend/src/common/guards/roles.guard.ts

Regras:
- @Roles('MASTER') protege rotas Master;
- @Roles('ADMIN_N1') protege rotas Admin N1;
- @Roles('ADMIN_N2') protege rotas Admin N2;
- @Roles('USER') protege rotas de usuário comum;
- MASTER pode acessar rotas globais;
- ADMIN_N1 não pode abrir Identity Vault;
- ADMIN_N2 não pode ver telefone, nome real ou Identity Vault;
- USER acessa apenas rotas operacionais autorizadas.

Validação:
npm run build
npm run test

ETAPA 06 — DeviceGuard

Objetivo:
Validar que toda rota operacional vem de um dispositivo autorizado.

Arquivo esperado:
backend/src/common/guards/device.guard.ts

Regras:
- ler userId e deviceId de request.user;
- consultar Device no banco;
- permitir apenas status ACTIVE;
- bloquear PENDING, SUSPENDED, QUARANTINE e REVOKED;
- garantir que o device pertence ao userId do JWT;
- erro deve ser genérico e não revelar detalhes internos.

Validação:
npm run build
npm run test

ETAPA 07 — IdentityVaultController real

Objetivo:
Remover actor mockado e proteger abertura de identidade real.

Arquivos esperados:
backend/src/modules/identity-vault/identity-vault.controller.ts
backend/src/modules/identity-vault/identity-vault.service.ts

Regras:
- remover qualquer actor hardcoded;
- usar @CurrentUser();
- proteger rota reveal com JwtAuthGuard, RolesGuard MASTER e DeviceGuard;
- exigir senha de conformidade;
- exigir justificativa com no mínimo 10 caracteres;
- registrar identity_access_logs;
- registrar audit_logs com metadataSensitivity SEALED;
- retornar contato apenas na resposta específica de reveal;
- não retornar dados reais em outras rotas.

Validação:
npm run build
npm run test

ETAPA 08 — Coercion Mode no login

Objetivo:
Implementar detecção de senha ou PIN de coação sem alertar o usuário.

Arquivo provável:
backend/src/modules/auth/auth.service.ts

Regras:
- se password bater com coercionPasswordHash, login deve simular sucesso;
- JWT deve conter coercion = true;
- resposta não deve dizer “modo coação ativado” para o usuário comum;
- registrar Blind Audit com categoria COERCION e action COERCION_LOGIN_DETECTED;
- não bloquear o login automaticamente;
- não enviar notificações reais nesta etapa, apenas estruturar evento.

Validação:
npm run build
npm run test

ETAPA 09 — Endpoints Admin N2

Objetivo:
Criar endpoints mínimos para gestão local.

Arquivos sugeridos:
backend/src/modules/admin-n2/admin-n2.module.ts
backend/src/modules/admin-n2/admin-n2.controller.ts
backend/src/modules/admin-n2/admin-n2.service.ts

Rotas mínimas:
GET /admin-n2/requests
POST /admin-n2/devices/:deviceId/approve
POST /admin-n2/invitations/user
GET /admin-n2/channels

Regras:
- proteger com JwtAuthGuard, DeviceGuard e RolesGuard ADMIN_N2;
- não retornar telefone;
- não retornar nome real;
- não retornar e-mail;
- não retornar Identity Vault;
- retornar apenas OP-ID, DEV-ID, status, CH-ID e dados operacionais mínimos;
- convite para usuário deve expirar em 10 minutos;
- convite deve ser 1 dispositivo por link.

Validação:
npm run build
npm run test

ETAPA 10 — Endpoints Admin N1

Objetivo:
Criar endpoints mínimos para gestão organizacional.

Arquivos sugeridos:
backend/src/modules/admin-n1/admin-n1.module.ts
backend/src/modules/admin-n1/admin-n1.controller.ts
backend/src/modules/admin-n1/admin-n1.service.ts

Rotas mínimas:
GET /admin-n1/admins-n2
POST /admin-n1/invitations/admin-n2
GET /admin-n1/blind-audit
GET /admin-n1/quarantines

Regras:
- proteger com JwtAuthGuard, DeviceGuard e RolesGuard ADMIN_N1;
- visão por organização/célula;
- auditoria pseudonimizada;
- sem Identity Vault;
- sem dados civis.

Validação:
npm run build
npm run test

ETAPA 11 — Endpoints Master

Objetivo:
Criar endpoints mínimos para governança global.

Arquivos sugeridos:
backend/src/modules/master/master.module.ts
backend/src/modules/master/master.controller.ts
backend/src/modules/master/master.service.ts

Rotas mínimas:
GET /master/organizations
GET /master/audit
GET /master/identity-vault/:userId/status
POST /master/identity-vault/reveal
GET /master/quarantines

Regras:
- proteger com JwtAuthGuard, DeviceGuard e RolesGuard MASTER;
- Identity Vault apenas para MASTER;
- reveal exige senha de conformidade e justificativa;
- auditoria SEALED obrigatória.

Validação:
npm run build
npm run test

ETAPA 12 — Flutter Android text-only UI

Objetivo:
Criar estrutura inicial do app Flutter sem mídia/anexos.

Estrutura sugerida:
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

Telas mínimas:
- Splash / Verificação;
- Convite / Primeiro acesso;
- Criação de credenciais;
- Aguardando aprovação;
- Login;
- PIN rápido;
- Lista de canais seguros;
- Chat text-only;
- Detalhe da mensagem;
- Protocolo Ômega;
- Quarentena;
- Admin N2 Dashboard.

Regras da UI:
- não mostrar botão de áudio;
- não mostrar botão de imagem;
- não mostrar botão de anexo;
- não mostrar câmera;
- não mostrar galeria;
- não mostrar upload PDF;
- não mostrar viewer de mídia;
- campo de mensagem deve aceitar apenas texto;
- chat deve mostrar OP-ID e CH-ID, não nome real;
- tela de canais não mostra membros;
- app deve usar tema escuro operacional.

Validação:
cd mobile
flutter pub get
flutter analyze
flutter test

ETAPA 13 — Testes mínimos

Objetivo:
Criar cobertura mínima para pontos críticos.

Testes obrigatórios:
- CryptoService criptografa e descriptografa;
- PasswordService gera hash e valida;
- OperationalCode generator gera prefixos corretos;
- MessagesService rejeita texto vazio;
- MessagesService cria TEXT;
- MessagesService não retorna encryptedPayload;
- AuthService marca coercion true quando senha de coação é usada;
- DeviceGuard bloqueia dispositivo não ACTIVE;
- RolesGuard bloqueia role incorreta;
- IdentityVault reveal exige MASTER.

Validação:
cd backend
npm run test
npm run build

ETAPA 14 — Docker Compose MVP

Objetivo:
Garantir que o MVP suba sem MinIO obrigatório.

Regras:
- docker compose padrão deve subir postgres, redis e api;
- MinIO deve ficar opcional em profile future-media;
- API não deve depender de MinIO no MVP text-only.

Comando esperado:
docker compose up -d postgres redis api

MinIO futuro:
docker compose --profile future-media up -d

ETAPA 15 — Documentação final da sprint

Objetivo:
Atualizar documentação técnica.

Arquivos sugeridos:
README.md
CHANGELOG.md
docs/06_SCOPE_TEXT_ONLY_MVP.md
docs/SPRINT_02_ACCESS_SECURITY_RBAC.md
docs/COPILOT_EXECUTION_LOG.md

A documentação deve registrar:
- escopo text-only;
- mídia removida do MVP;
- comandos de execução;
- rotas implementadas;
- regras de segurança;
- lacunas pendentes;
- próxima sprint.

FORMATO DE RESPOSTA OBRIGATÓRIO DO COPILOT APÓS CADA ETAPA

Ao terminar cada etapa, responda assim:

## Etapa concluída
Nome da etapa.

## Arquivos alterados
- arquivo 1
- arquivo 2

## O que foi feito
- item 1
- item 2

## Comandos para validar
```bash
comandos aqui
```

## Riscos ou pendências
- risco 1
- pendência 1

## Próxima etapa recomendada
Nome da próxima etapa.

IMPORTANTE

Não avance automaticamente para a próxima etapa sem confirmação do operador.
Não implemente funcionalidades fora do MVP text-only.
Não exponha dados reais.
Não reative mídia.
Não use biometria.
Não trate este projeto como app de chat comum. Ele é uma plataforma operacional com governança, identidade operacional e redução de metadados por padrão.
```
