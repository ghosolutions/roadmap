# GhoMsg Secure v1.1.2 — Text-only MVP

## Visão

O **GhoMsg Secure** é uma plataforma self-hosted de comunicação operacional segura, Android-first + Web Admin restrito, projetada para operação com sigilo, identidade operacional e redução de metadados.

## Escopo atual

Versão registrada neste roadmap:

```text
GhoMsg Secure v1.1.2 — Text-only MVP
```

## MVP atual

O MVP inicial permite somente:

- mensagem de texto criptografada;
- mensagem de sistema;
- convite único com expiração de 10 minutos;
- vínculo de 1 convite para 1 dispositivo;
- device binding;
- Identity Vault;
- Operational Identity;
- Blind Audit;
- Metadata Reduction;
- Sealed Channels;
- retenção/expiração;
- protocolo Ômega;
- modo de coação;
- auditoria operacional.

## Fora do MVP

As funções abaixo foram removidas temporariamente e movidas para backlog v1.2:

- envio de imagem;
- envio de áudio;
- envio de vídeo;
- envio de PDF;
- envio de arquivos;
- upload/download;
- viewer seguro de mídia;
- MinIO obrigatório;
- GhostTrace aplicado a arquivos.

## Arquitetura resumida

```text
Android App
    ↓
NestJS API
    ↓
Policy Engine
    ↓
PostgreSQL + Redis
```

MinIO fica reservado para a fase futura de mídia/arquivos.

## Regras críticas

- Nenhuma operação comum deve expor nome real.
- Nenhuma operação comum deve expor telefone, e-mail ou avatar.
- A operação deve usar OP-ID, ADM-ID, CH-ID, DEV-ID e SES-ID.
- Usuário comum não vê lista de membros.
- Usuário comum não acessa Web Admin.
- Entrada somente por convite.
- Android App é o canal operacional principal.

## Próxima sprint

```text
Sprint 02 — Access Security & RBAC
```

Entregas previstas:

- JwtStrategy;
- JwtAuthGuard;
- RolesGuard;
- DeviceGuard;
- CurrentUser decorator;
- Coercion event no login;
- endpoints Admin N2;
- endpoints Admin N1;
- endpoints Master;
- proteção real do Identity Vault via request.user.
