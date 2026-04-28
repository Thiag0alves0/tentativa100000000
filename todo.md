# Recover Energy - TODO

## Banco de Dados e Estrutura
- [x] Configurar schema de leads com campos: nome, telefone, email, energyBillAmount, status, createdAt
- [x] Criar helpers de banco de dados para CRUD de leads
- [x] Implementar integração com Google Sheets via script fornecido

## Formulário e Captura de Leads
- [x] Criar utilitário para enviar dados ao Google Sheets (prioridade máxima)
- [x] Implementar mutation de criação de leads com fallback para banco de dados
- [x] Validar campos do formulário (nome, telefone, email, valor da conta)
- [x] Testar envio ao Google Sheets

## Landing Page - Seções
- [x] Seção Hero (título, subtítulo, CTA)
- [x] Seção Sobre (descrição da empresa/serviço)
- [x] Seção Como Funciona (passos do processo)
- [x] Seção Benefícios (vantagens da solução)
- [x] Seção Simulação de Economia (calculadora/formulário)
- [x] Rodapé com informações de contato

## Página de Agradecimento
- [x] Criar página de thank you após envio do formulário
- [x] Exibir mensagem de sucesso e próximos passos

## Dashboard CRM
- [x] Criar página de dashboard para o dono
- [x] Listar todos os leads capturados
- [x] Exibir colunas: nome, telefone, email, valor da conta, status
- [x] Implementar filtros por status
- [x] Permitir atualização de status do lead
- [x] Implementar proteção de acesso (apenas dono)

## Widget de Chat
- [x] Criar componente de chat assistente
- [x] Implementar respostas rápidas pré-definidas
- [x] Adicionar link direto para WhatsApp
- [x] Integrar chat na landing page

## Branding e Estilo
- [x] Definir paleta de cores para Recover Energy
- [x] Configurar tipografia e espaçamento
- [x] Aplicar branding em todas as páginas
- [x] Garantir consistência visual

## Testes e Deploy
- [x] Testar formulário de leads
- [x] Testar envio ao Google Sheets
- [x] Testar dashboard CRM
- [x] Testar responsividade
- [x] Criar checkpoint final
- [x] Deploy do site permanente

## Design Premium - UI/UX Melhorado
- [x] Atualizar paleta de cores com gradientes suaves
- [x] Melhorar tipografia com melhor hierarquia
- [x] Adicionar glassmorphism e soft shadows
- [x] Implementar sistema de espaçamento consistente
- [x] Redesenhar Hero com social proof e valor prop
- [x] Adicionar seção de testemunhos
- [x] Adicionar seção de FAQ
- [x] Melhorar formulário com urgency e trust elements
- [x] Redesenhar Dashboard com gráficos e filtros
- [x] Adicionar animações e efeitos hover
- [x] Otimizar responsividade mobile-first
- [x] Revisar a implementação atual do dashboard para explicar sua estrutura, dados exibidos e regras de acesso
- [x] Ajustar o projeto para facilitar versionamento no GitHub e deploy na Vercel
- [x] Documentar como o dashboard funcionará em produção e quais dependências precisarão ser configuradas na Vercel
