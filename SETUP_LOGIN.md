# Setup do Login por Email/Senha

Este projeto foi configurado para usar autenticação por **email e senha** em vez de OAuth.

## Credenciais Padrão

- **Email**: `recoverenergiasite@gmail.com`
- **Senha**: `recover0287`

## Instalação e Configuração

### 1. Instalar dependências

```bash
pnpm install
```

### 2. Configurar banco de dados

Crie um arquivo `.env` com as seguintes variáveis:

```env
NODE_ENV=production
DATABASE_URL=mysql://user:password@host:3306/database_name
JWT_SECRET=seu-secret-super-seguro-aqui
GOOGLE_SHEETS_WEBHOOK_URL=sua-url-webhook
BUILT_IN_FORGE_API_URL=sua-url-api
BUILT_IN_FORGE_API_KEY=sua-chave-api
```

### 3. Executar migrações do banco de dados

```bash
pnpm db:push
```

### 4. Criar usuário admin

```bash
npx tsx scripts/init-admin.ts
```

Este script criará automaticamente o usuário admin com as credenciais padrão.

## Desenvolvimento Local

```bash
pnpm dev
```

A aplicação estará disponível em `http://localhost:5173`

- **Home**: `http://localhost:5173/`
- **Login**: `http://localhost:5173/login`
- **Dashboard**: `http://localhost:5173/dashboard`

## Build para Produção

```bash
pnpm build
```

## Deploy no Vercel

1. Conecte seu repositório ao Vercel
2. Configure as variáveis de ambiente no painel do Vercel
3. O build será executado automaticamente

### Variáveis de Ambiente no Vercel

- `DATABASE_URL`: URL do banco de dados MySQL
- `JWT_SECRET`: Secret para assinar tokens JWT
- `GOOGLE_SHEETS_WEBHOOK_URL`: URL do webhook do Google Sheets
- `BUILT_IN_FORGE_API_URL`: URL da API Forge
- `BUILT_IN_FORGE_API_KEY`: Chave da API Forge

## Fluxo de Autenticação

1. Usuário acessa `/login`
2. Insere email e senha
3. Servidor valida credenciais contra o banco de dados
4. Se válido, cria um token JWT e o armazena em um cookie
5. Usuário é redirecionado para `/dashboard`
6. Todas as requisições incluem automaticamente o cookie de sessão

## Mudanças Realizadas

- ✅ Removido OAuth e substituído por autenticação por email/senha
- ✅ Adicionado bcryptjs para hash de senhas
- ✅ Criada página de login (`/login`)
- ✅ Modificado sistema de autenticação do backend
- ✅ Atualizado schema do banco de dados com campo de senha
- ✅ Preparado para deploy no Vercel

## Estrutura de Autenticação

### Backend
- `/api/auth/login`: POST - Fazer login
- `/api/auth/register`: POST - Registrar novo admin (não exposto publicamente)
- `/api/trpc/auth.me`: Query - Obter usuário atual
- `/api/trpc/auth.logout`: Mutation - Fazer logout

### Frontend
- `useEmailAuth()`: Hook para gerenciar autenticação
- `/pages/Login.tsx`: Página de login
- `/pages/Dashboard.tsx`: Dashboard protegido por autenticação

## Segurança

- Senhas são hasheadas com bcryptjs (10 rounds)
- Tokens JWT são assinados com `JWT_SECRET`
- Cookies são `httpOnly` e `secure` em produção
- Proteção CSRF via `sameSite: 'none'` em desenvolvimento

