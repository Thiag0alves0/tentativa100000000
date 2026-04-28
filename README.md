# Recover Energia

Este projeto é um site institucional com captura de leads e um **dashboard CRM** privado para gestão comercial. A aplicação usa **React + Vite** no frontend, **tRPC + Express** no backend e **MySQL com Drizzle** para persistência dos leads.

## Como o dashboard funciona

O painel fica na rota `/dashboard`. Ele foi pensado para uso interno do dono da operação e hoje funciona em quatro camadas.

| Camada | Função | Arquivos principais |
| --- | --- | --- |
| Interface | Exibe métricas, gráficos, tabela de leads, busca, filtros e edição | `client/src/pages/Dashboard.tsx` |
| Autenticação | Identifica o usuário autenticado e bloqueia acesso indevido | `client/src/_core/hooks/useAuth.ts`, `server/_core/oauth.ts` |
| API | Lista leads e atualiza status/notas | `server/routers.ts` |
| Persistência | Busca e grava dados no MySQL | `server/db.ts`, `drizzle/schema.ts` |

Na prática, o dashboard mostra total de leads, distribuição por status, taxa de conversão, tabela com dados capturados e ações para visualizar ou editar cada lead. O acesso é restrito ao usuário que atender ao fluxo de autenticação atual e também corresponder ao `OWNER_OPEN_ID` configurado no ambiente.

## Importante sobre deploy na Vercel

O projeto foi preparado para ficar **mais próximo de um deploy utilizável na Vercel**, mas o dashboard depende de serviços externos que precisam existir em produção.

| Dependência | Obrigatória para | Observação |
| --- | --- | --- |
| `DATABASE_URL` | Listar e atualizar leads no dashboard | Sem banco, o site público pode continuar, mas o dashboard ficará sem dados persistidos |
| `JWT_SECRET` | Sessão/autenticação | Necessário para cookies de sessão |
| `OWNER_OPEN_ID` | Restringir acesso ao dono | Define quem pode ver o dashboard |
| `OAUTH_SERVER_URL` + `VITE_APP_ID` | Fluxo atual de login | O projeto ainda usa o fluxo de autenticação atual do template |
| `GOOGLE_SHEETS_WEBHOOK_URL` | Envio de leads ao Google Sheets | Agora configurável por ambiente |

## Ajustes feitos para facilitar Vercel

| Ajuste | Resultado |
| --- | --- |
| Criação de `api/index.ts` | Permite expor backend como função serverless |
| Criação de `server/_core/app.ts` | Reaproveita a configuração do Express entre local e Vercel |
| Criação de `vercel.json` | Define build do frontend, rewrites da API e fallback SPA |
| Criação de `.env.example` | Lista as variáveis esperadas em produção |
| Ajuste em `googleSheets.ts` | Permite configurar webhook sem hardcode |

## Fluxo recomendado para publicar

Primeiro, suba este projeto no GitHub. Depois, importe o repositório na Vercel. Em seguida, configure as variáveis de ambiente listadas em `.env.example`. Por fim, valide três pontos: o envio do formulário, o carregamento do dashboard e a autenticação do usuário proprietário.

## Observação importante sobre autenticação

Se você quiser que o dashboard funcione na Vercel **sem depender do fluxo atual de autenticação**, o próximo passo ideal é trocar o login atual por uma autenticação própria, por exemplo com login por email e senha, magic link ou provedor externo como Clerk/Auth.js/Supabase Auth. Sem essa troca, o deploy pode subir, mas o acesso privado continuará dependente das variáveis e do fluxo de autenticação já existente no projeto.
