# Almoxarifado Online

Repositório de desenvolvimento do sistema de controle de almoxarifado.

## Situação atual

- Base: Almoxarifado V6 — Dashboard Premium Escuro.
- Armazenamento atual do HTML: localStorage do navegador; **ainda não há sincronização online**.
- Supabase: integração e autenticação pendentes.
- Publicação: não habilitada. **Não usar para operação compartilhada até concluir a integração e os testes.**

## Próximas etapas

1. Validar os módulos e a base de dados existente.
2. Criar esquema SQL, autenticação, permissões e funções de movimentação seguras no Supabase.
3. Migrar registros locais com backup e conferência.
4. Integrar o frontend ao backend e testar múltiplos usuários.
5. Habilitar hospedagem HTTPS somente após validação das regras de segurança.
