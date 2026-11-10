# V7 — Conexão Supabase e autenticação (em homologação)

- `index.html` foi atualizado com login Supabase Auth e conexão ao projeto `quokynllfptfszwxhmvw`.
- A chave incluída no HTML é **publicável**; não há nenhuma chave `service_role`/secret no código.
- Novas contas entram como `leitor` e **inativas** até aprovação administrativa. O banco bloqueia consulta e movimentações de contas não aprovadas.
- Leitura paginada: patrimônios, notas, itens, histórico de movimentações e catálogo.
- Cadastro de entrada usa a RPC transacional `alm_registrar_entrada`.
- Baixa em lote usa a RPC transacional `alm_registrar_baixa_lote`.
- A edição direta, exclusão e importação de estoque estão temporariamente desativadas, para não corromper o histórico.
- O `localStorage` antigo não é excluído, mas o novo código **não exibe dados locais** e ainda não implementa a migração.
- Não há usuários aprovados nem produtos no banco. Não habilitar GitHub Pages antes de testar autorização, edição segura, migração e operações simultâneas.

## Primeira conta
1. Abrir o HTML V7 em um navegador conectado à Internet.
2. Criar conta e confirmar e-mail, se solicitado pelo Supabase Auth.
3. Administrador do Supabase deve identificar a conta verificada e aprová-la explicitamente no banco (definir perfil `admin`, `ativo=true`) **somente após identificar o titular**.
4. Testar login, cadastro e baixa com produtos fictícios.

## Verificações executadas
- Migrações aplicadas e verificação de permissões: `anon` não executa RPCs de entrada ou baixa.
- JavaScript passou em `node --check`.
- Teste isolado em Chromium com SDK simulado validou bloqueio da aplicação antes do login, sem erros de script.
- **Não** foi realizado teste de ponta a ponta com autenticação real e banco, nem verificação de fluxos reais multiusuário.

## Avisos de segurança
Supabase Advisor alerta sobre duas RPCs `SECURITY DEFINER` visíveis a `authenticated`. O acesso é intencional e validado internamente por perfil ativo, mas recomenda-se revisão adicional e testes de papéis antes da produção.
