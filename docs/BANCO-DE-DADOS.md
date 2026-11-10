# Banco de dados — Almoxarifado Online

Projeto Supabase exclusivo: `quokynllfptfszwxhmvw` (São Paulo, sa-east-1).

## Estrutura criada em 10/11/2026
Migração aplicada no Supabase: `almoxarifado_estrutura_segura_inicial`.

| Tabela | Finalidade |
|---|---|
| `alm_usuarios` | Usuários com perfis `admin`, `operador` e `leitor` e flag `ativo` |
| `alm_patrimonios` | Patrimônios MAQ / IMP / VEC / OUTROS |
| `alm_notas` | Notas por patrimônio, solicitante e data |
| `alm_itens` | Itens por nota, códigos, equivalência, quantidade original e saldo |
| `alm_movimentacoes` | Auditoria imutável pelo navegador de entrada / baixa / ajuste |
| `alm_catalogo` | Produtos de referência |
| `alm_equivalencias` | Vínculos explícitos de códigos confirmados |

### Segurança
- RLS ativado em todas as tabelas.
- `anon` não recebe acesso às tabelas nem ao RPC de baixas.
- Usuário recém-criado recebe perfil `leitor` e só ganha acesso se estiver ativo.
- `operador` e `admin` podem inserir notas e itens.
- A função `alm_registrar_baixa_lote(p_itens,p_responsavel,p_observacao)` faz bloqueio de linhas, verifica saldo e audita as retiradas de forma transacional.
- Não se permite `UPDATE`/`DELETE` direto via cliente nas tabelas de saldos e movimentações.
- A função de baixa é `SECURITY DEFINER`, exposta somente a `authenticated` com verificação interna de perfil. A análise de segurança do Supabase emite um aviso sobre ela; o acesso controlado é intencional e deve ser retestado antes de produção.

### Antes de liberar o sistema
1. Configurar Auth e criar o primeiro usuário; promover a `admin` com operação administrativa no banco (não pelo navegador).
2. Integrar frontend V6 ao Supabase, com cliente publishable e login; nunca colocar chave secret/service-role no HTML.
3. Implementar migração/importação segura e verificável de `localStorage['almoxarifado_operacional_v2']`, incluindo baixas anteriores.
4. Adicionar fluxos auditados de edição/correção/cancelamento de cadastros, sem edição direta de saldo.
5. Testar perfis e permissões, entradas, baixas concorrentes, equivalências, dashboards e consultas com dois usuários.
6. Só então habilitar hospedagem HTTPS. O `index.html` atual continua usando localStorage e **não está sincronizado com este banco**.

**Não usar o HTML atual em operação multiusuário até a conclusão da integração.**
