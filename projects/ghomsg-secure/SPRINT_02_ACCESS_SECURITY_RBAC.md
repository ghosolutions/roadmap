# GhoMsg Secure — Sprint 02: Access Security & RBAC

## Objetivo

Implementar a camada real de autenticação, autorização, validação de dispositivo e separação de privilégios para o **GhoMsg Secure v1.1.2 — Text-only MVP**.

Esta sprint transforma o backend de scaffold funcional em base segura para operação controlada.

---

# Escopo da Sprint

## 1. JWT Strategy

Implementar estratégia JWT usando `passport-jwt`.

Payload obrigatório:

```json
{
  "sub": "user_uuid",
  "role": "MASTER | ADMIN_N1 | ADMIN_N2 | USER",
  "operationalCode": "OP-7F3A",
  "organizationId": "organization_uuid",
  "deviceId": "device_uuid",
  "coercion": false
}
```

Critérios:

- [ ] Validar assinatura JWT.
- [ ] Validar expiração.
- [ ] Anexar `request.user`.
- [ ] Não carregar identidade real no payload.

---

## 2. JwtAuthGuard

Criar guard global para rotas protegidas.

Critérios:

- [ ] Bloquear requisição sem token.
- [ ] Bloquear token inválido.
- [ ] Expor somente dados operacionais no `request.user`.

---

## 3. CurrentUser Decorator

Criar decorator:

```ts
@CurrentUser()
```

Retorno esperado:

```ts
{
  userId: string;
  role: 'MASTER' | 'ADMIN_N1' | 'ADMIN_N2' | 'USER';
  operationalCode: string;
  organizationId: string;
  deviceId: string;
  coercion: boolean;
}
```

---

## 4. RolesGuard

Criar decorator:

```ts
@Roles('MASTER', 'ADMIN_N1')
```

Critérios:

- [ ] MASTER acessa rotas globais.
- [ ] ADMIN_N1 acessa gestão organizacional/célula.
- [ ] ADMIN_N2 acessa gestão local.
- [ ] USER acessa apenas canais autorizados.

---

## 5. DeviceGuard

Validar dispositivo em todas as rotas operacionais.

Critérios:

- [ ] Dispositivo deve existir.
- [ ] Dispositivo deve estar `ACTIVE`.
- [ ] Dispositivo deve pertencer ao usuário do JWT.
- [ ] Dispositivo `QUARANTINE`, `SUSPENDED` ou `REVOKED` bloqueia acesso.

---

## 6. Coercion Event

Quando o login for feito com senha de coação:

- [ ] Resposta deve simular acesso normal.
- [ ] Payload JWT deve carregar `coercion: true`.
- [ ] Blind Audit deve registrar evento sensível.
- [ ] Futuramente notificar Admin superior.

Categoria de auditoria sugerida:

```text
COERCION
```

Ação sugerida:

```text
COERCION_LOGIN_DETECTED
```

---

## 7. IdentityVaultController real

Remover actor mockado.

Antes:

```ts
actor: {
  userId: '00000000-0000-0000-0000-000000000000',
  role: 'MASTER',
  operationalCode: 'MST-DEV'
}
```

Depois:

```ts
actor: request.user
```

Critérios:

- [ ] Rota protegida por `JwtAuthGuard`.
- [ ] Rota protegida por `RolesGuard` com MASTER.
- [ ] Exigir senha de conformidade.
- [ ] Exigir justificativa.
- [ ] Registrar `identity_access_logs`.
- [ ] Registrar `audit_logs` com `metadataSensitivity = SEALED`.

---

## 8. Admin N2 endpoints

Endpoints mínimos:

```text
GET  /admin-n2/requests
POST /admin-n2/devices/:deviceId/approve
POST /admin-n2/invitations/user
GET  /admin-n2/channels
```

Critérios:

- [ ] Não retornar telefone.
- [ ] Não retornar nome real.
- [ ] Não retornar Identity Vault.
- [ ] Retornar OP-ID, DEV-ID, status e canal.

---

## 9. Admin N1 endpoints

Endpoints mínimos:

```text
GET  /admin-n1/admins-n2
POST /admin-n1/invitations/admin-n2
GET  /admin-n1/blind-audit
GET  /admin-n1/quarantines
```

Critérios:

- [ ] Visão por organização/célula.
- [ ] Sem identidade real.
- [ ] Auditoria pseudonimizada.

---

## 10. Master endpoints

Endpoints mínimos:

```text
GET  /master/organizations
GET  /master/audit
GET  /master/identity-vault/:userId/status
POST /master/identity-vault/reveal
GET  /master/quarantines
```

Critérios:

- [ ] Identity Vault somente para MASTER.
- [ ] Abertura com justificativa.
- [ ] Auditoria SEALED.

---

# Fora do escopo da Sprint 02

- WebSocket de mensagens.
- App Android completo.
- MinIO.
- Envio de mídia/arquivos.
- Viewer seguro.
- GhostTrace em mídia.

---

# Definition of Done

- [ ] Backend compila.
- [ ] Prisma Client gera sem erro.
- [ ] Testes unitários críticos passam.
- [ ] Rotas críticas protegidas por JWT.
- [ ] RBAC funcional.
- [ ] DeviceGuard funcional.
- [ ] IdentityVaultController sem mock.
- [ ] Nenhuma resposta comum expõe identidade real.
