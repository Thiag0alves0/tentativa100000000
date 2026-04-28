# Mudanças Realizadas - Autenticação por Email/Senha

## Resumo
O projeto foi modificado para usar autenticação por **email e senha** em vez de OAuth, permitindo que o dono da empresa acesse o dashboard CRM com credenciais simples.

## Arquivos Criados

### Backend
- **`server/_core/emailAuth.ts`**: Serviço de autenticação por email/senha
  - Hash de senhas com bcryptjs
  - Criação e verificação de tokens JWT
  - Endpoints `/api/auth/login` e `/api/auth/register`

- **`scripts/init-admin.ts`**: Script para criar usuário admin padrão
  - Cria usuário com email e senha fornecidos
  - Verifica se usuário já existe antes de criar

### Frontend
- **`client/src/pages/Login.tsx`**: Página de login
  - Interface limpa e responsiva
  - Validação de formulário
  - Toggle para mostrar/ocultar senha

- **`client/src/_core/hooks/useEmailAuth.ts`**: Hook de autenticação
  - Gerencia estado do usuário autenticado
  - Redirecionamento automático para login se não autenticado
  - Função de logout

## Arquivos Modificados

### Backend
- **`server/_core/context.ts`**: Adicionado suporte a autenticação por email
  - Tenta autenticação por email primeiro
  - Fallback para OAuth (compatibilidade)

- **`server/_core/app.ts`**: Registrado novo serviço de autenticação
  - Adicionado `registerEmailAuthRoutes(app)`

- **`server/routers.ts`**: Removida verificação de `ownerOpenId`
  - Dashboard agora acessível por qualquer usuário com `role === "admin"`

- **`server/db.ts`**: Adicionadas funções de banco de dados
  - `getUserByEmail()`: Buscar usuário por email
  - `createUser()`: Criar novo usuário
  - `updateUserLastSignedIn()`: Atualizar último login

### Frontend
- **`client/src/App.tsx`**: Adicionada rota `/login`
  - Importado componente `Login`
  - Adicionada rota no Router

- **`client/src/pages/Dashboard.tsx`**: Atualizado para usar novo hook
  - Alterado de `useAuth()` para `useEmailAuth()`
  - Adicionada proteção de rota com redirecionamento

- **`client/src/main.tsx`**: Modificado redirecionamento de erro
  - Redireciona para `/login` em vez de OAuth

### Banco de Dados
- **`drizzle/schema.ts`**: Atualizado schema de usuários
  - Adicionado campo `password` (TEXT)
  - Removido `notNull` de `openId` (compatibilidade com OAuth)
  - Adicionado `unique` em `email`

## Dependências Adicionadas

```json
{
  "bcryptjs": "^2.4.3"
}
```

## Configuração Necessária

### Variáveis de Ambiente

```env
DATABASE_URL=mysql://user:password@host:3306/database
JWT_SECRET=seu-secret-super-seguro-aqui
```

### Credenciais Padrão

- **Email**: `recoverenergiasite@gmail.com`
- **Senha**: `recover0287`

## Fluxo de Autenticação

```
1. Usuário acessa /login
   ↓
2. Insere email e senha
   ↓
3. POST /api/auth/login
   ↓
4. Servidor valida credenciais (bcryptjs)
   ↓
5. Cria JWT e armazena em cookie
   ↓
6. Redireciona para /dashboard
   ↓
7. Dashboard verifica autenticação via useEmailAuth()
   ↓
8. Se não autenticado, redireciona para /login
```

## Segurança

- ✅ Senhas hasheadas com bcryptjs (10 rounds)
- ✅ Tokens JWT com expiração de 1 ano
- ✅ Cookies `httpOnly` em produção
- ✅ Proteção `sameSite` contra CSRF
- ✅ Validação de email e senha no backend

## Compatibilidade

- ✅ Mantém compatibilidade com OAuth (fallback)
- ✅ Funciona com banco de dados existente
- ✅ Pronto para Vercel
- ✅ Suporta múltiplos usuários admin

## Próximos Passos

1. Executar migrações: `pnpm db:push`
2. Criar usuário admin: `npx tsx scripts/init-admin.ts`
3. Testar login localmente: `pnpm dev`
4. Deploy no Vercel com variáveis de ambiente configuradas

