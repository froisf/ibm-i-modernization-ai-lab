# Tasks: Cadastro de Transportadora

> Identificador: `TASK-001-cadastro-transportadora`
> Data: `2026-09-23`
> Unidade: `_reversa_sdd/cadastro-transportadora`
> Confidência: 🟢 CONFIRMADO / 🟡 INFERIDO / 🔴 LACUNA

## 1. Objetivo

Executar e validar a funcionalidade de cadastro, listagem, alteração e exclusão de transportadoras, baseada no comportamento original em `TRN001` e no modelo `TRNPF`.

## 2. Tarefas de implementação

| ID | Tarefa | Base no legado | Critério de pronto | Confidência |
|----|--------|----------------|-------------------|-------------|
| T-01 | Abrir a tela de listagem e carregar registros existentes | `QRPGLESRC/TRN001.rpgle` → `CarregaLista` | A tela mostra registros e mensagem quando vazio | 🟢 |
| T-02 | Implementar inclusão de novo registro com validação de campos | `TRN001` → `IncluirRegistro` + `ValidaDados` | Nova transportadora é criada com código sequencial e feedback de sucesso | 🟢 |
| T-03 | Implementar alteração de dados cadastrais | `TRN001` → `AlterarRegistro` | Registro existente é atualizado e `TRNDTA` recebe nova data | 🟢 |
| T-04 | Implementar exclusão com confirmação | `TRN001` → `ExcluirRegistro` | Registro é removido após confirmação | 🟢 |
| T-05 | Validar regras mínimas de domínio | `TRN001` → `ValidaDados` | CNPJ, razão social, UF, status e e-mail seguem as regras do legado | 🟢 |
| T-06 | Gerar próximo código sequencial | `TRN001` → `GeraCodigo` | Novo código é maior que o último registro disponível | 🟢 |
| T-07 | Preservar mensagens de retorno ao usuário | `TRN001` → `wMsgErro` e `MostrarErro` | Tela informa sucesso ou erro de validação | 🟢 |
| T-08 | Definir status permitidos (`A`/`I`) | `TRNPF.dds` + `ValidaDados` | Somente esses valores são aceitos | 🟢 |

## 3. Dependências

- `TRNPF` deve existir como base de dados/entidade
- `TRNDSP` deve oferecer a interface de listagem e detalhe
- `TRN001` deve controlar o loop principal de estado

## 4. Riscos e lacunas

| Item | Detalhe | Confidência |
|------|---------|-------------|
| Permissões por usuário | Não há evidência de controle de acesso no código | 🔴 |
| Regras de negócio fora do cadastro | Não foi identificado fluxo adicional de aprovação ou status especial | 🟡 |

## 5. Critérios de conclusão

A tarefa está concluída quando:

- a listagem abre corretamente;
- inclusão, alteração e exclusão funcionam no fluxo principal;
- validações de domínio estão presentes;
- a geração de código segue a sequência da última chave; e
- o comportamento de sucesso/erro é consistente com a implementação legacy.

## 6. Histórico de alterações

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-09-23 | Versão inicial gerada por `/reversa-writer` | reversa |
