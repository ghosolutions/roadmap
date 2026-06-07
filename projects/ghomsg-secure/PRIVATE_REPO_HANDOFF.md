# GhoMsg Secure — Handoff para Repositório Privado

## Objetivo

Criar o repositório privado dedicado ao código do produto:

```text
ghosolutions/ghomsg-secure
```

O repositório `ghosolutions/roadmap` deve permanecer apenas como governança pública e backlog não sensível.

---

# Configuração no GitHub

Criar novo repositório com:

```text
Owner: ghosolutions
Repository name: ghomsg-secure
Visibility: Private
Initialize with README: No
Add .gitignore: No
Choose a license: No
```

---

# Estrutura inicial a subir

Usar como base o pacote local:

```text
ghomsg-secure-v1.1-sprint01-backend-real
```

Aplicar o delta:

```text
ghomsg-secure-v1.1.2-text-only-delta
```

---

# Comandos Git no computador

## 1. Clonar o repositório vazio

```bash
git clone https://github.com/ghosolutions/ghomsg-secure.git
cd ghomsg-secure
```

## 2. Copiar os arquivos do pacote Sprint 01

Copiar todo conteúdo de:

```text
ghomsg-secure-v1.1-sprint01-backend-real/
```

para dentro de:

```text
ghomsg-secure/
```

## 3. Aplicar delta text-only

Copiar os arquivos de:

```text
ghomsg-secure-v1.1.2-text-only-delta/
```

para dentro do repositório, mantendo os caminhos.

## 4. Commit inicial

```bash
git add .
git commit -m "chore: initialize GhoMsg Secure v1.1.2 text-only MVP"
git push origin main
```

---

# Branches recomendadas

```bash
git checkout -b develop
git push -u origin develop

git checkout -b feature/access-security-rbac
git push -u origin feature/access-security-rbac
```

---

# Primeira validação local

```bash
cp .env.example .env
docker compose up -d postgres redis
cd backend
npm install
npx prisma generate
npm run build
npm run test
npm run start:dev
```

Health check:

```bash
curl http://localhost:3000/api/v1/health
```

---

# GitHub Actions

Ativar workflow:

```text
.github/workflows/backend-ci.yml
```

O CI deve executar:

```text
npm install
npx prisma generate
npm run build
npm run test
```

---

# Observação de segurança

Nunca commitar:

```text
.env
JWT_SECRET
JWT_REFRESH_SECRET
ENCRYPTION_KEY
GHOSTTRACE_SECRET
senhas
certificados
chaves privadas
```

Usar GitHub Secrets quando necessário.
