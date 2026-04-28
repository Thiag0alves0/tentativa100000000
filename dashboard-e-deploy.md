# Dashboard e Deploy na Vercel

## Como ficará o dashboard

O dashboard deste site continuará sendo uma área interna acessada pela rota `/dashboard`. Ele já está implementado como um **CRM de leads** e foi pensado para uso do dono da operação.

| Bloco do dashboard | O que ele faz |
| --- | --- |
| Cards de métricas | Mostra total de leads, novos, contatados, qualificados e convertidos |
| Taxa de conversão | Calcula o percentual de leads convertidos sobre o total |
| Gráficos | Exibe distribuição por status e evolução visual de leads |
| Tabela principal | Lista nome, email, telefone, valor da conta, status e data |
| Busca e filtros | Permite localizar leads e segmentar por etapa comercial |
| Modal de visualização | Abre os detalhes completos do lead |
| Modal de edição | Permite atualizar status e observações |

Em termos práticos, o fluxo fica assim: o visitante preenche o formulário do site, o backend envia o lead para o Google Sheets, tenta salvar no banco MySQL e, depois disso, o dashboard consome esses dados para exibição e atualização.

## O que já deixei preparado

O projeto foi ajustado para ficar **mais fácil de subir na Vercel** sem perder a estrutura atual.

| Ajuste realizado | Finalidade |
| --- | --- |
| `api/index.ts` | Expõe o backend como função serverless |
| `server/_core/app.ts` | Centraliza a criação do app Express |
| `vercel.json` | Configura build, rotas de API e fallback SPA |
| `.env.example` | Lista as variáveis necessárias em produção |
| `server/googleSheets.ts` | Permite configurar o webhook por ambiente |
| `server/googleSheets.test.ts` | Garante por teste que a URL do webhook pode vir do ambiente |

## O que você precisa saber antes de publicar

O site público pode ser publicado com mais facilidade, mas o **dashboard depende de backend e autenticação**. Isso significa que, para funcionar corretamente em produção, você precisará configurar ambiente, banco e acesso do usuário dono.

| Requisito | Impacto no dashboard |
| --- | --- |
| `DATABASE_URL` | Sem banco, os leads não ficam persistidos para consulta e edição |
| `JWT_SECRET` | Sem ele, a sessão privada do painel não é mantida |
| `OWNER_OPEN_ID` | Define quem pode entrar no dashboard |
| `OAUTH_SERVER_URL` e `VITE_APP_ID` | Mantêm o fluxo atual de login privado |
| `GOOGLE_SHEETS_WEBHOOK_URL` | Mantém o envio automático dos leads para a planilha |

## Ponto mais importante

Hoje, a parte mais sensível para a Vercel não é o layout do dashboard, e sim a **autenticação privada**. O painel está pronto visualmente e funcionalmente, mas o acesso dele continua dependente do fluxo de autenticação atual do projeto. Se você quiser, no próximo passo eu posso adaptar isso para um modelo mais simples e típico de Vercel, como login por email e senha ou acesso administrativo próprio.
