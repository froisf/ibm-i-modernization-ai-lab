# Requirements: Cadastro de Transportadora

> Identificador: `REQ-001-cadastro-transportadora`
> Data: `2026-09-23`
> Pasta da extração reversa: `_reversa_sdd/`
> Confidência: 🟢 CONFIRMADO, 🟡 INFERIDO, 🔴 LACUNA / DÚVIDA

## 1. Resumo executivo

O sistema deve permitir o cadastro, listagem, alteração e exclusão de transportadoras em ambiente legado IBM i, preservando as regras mínimas de consistência documental e funcional já observadas no programa `TRN001`.

## 2. Contexto a partir do legado

| Fonte | Trecho relevante | Confidência |
|-------|------------------|-------------|
| `_reversa_sdd/code-analysis.md#4-fluxo-de-controle` | O programa implementa listagem, inclusão, alteração e exclusão de transportadoras | 🟢 |
| `_reversa_sdd/domain.md#2.3-dados-mínimos-obrigatórios` | São exigidos razão social, CNPJ, UF e e-mail válido | 🟢 |
| `_reversa_sdd/architecture.md#5-erd-resumido` | Entidade central: `TRNREC` com os principais atributos cadastrais | 🟢 |

## 3. Personas e cenários de uso

| Persona | Objetivo | Cenário-chave |
|---------|----------|---------------|
| Operador de cadastro | Registrar e manter dados das transportadoras | Cadastrar nova empresa e manter informações cadastrais |
| Supervisor operacional | Consultar dados já cadastrados | Validar listagem e manter informações atualizadas |

## 4. Regras de negócio novas ou alteradas

1. **RN-01:** O sistema deve manter um identificador único para cada transportadora, gerado sequencialmente por código. 🟢
   - Origem no legado: `_reversa_sdd/domain.md#2.4-geração-automática-de-código`
   - Tipo: alterada
2. **RN-02:** A razão social e o CNPJ devem ser obrigatórios para gravar o registro. 🟢
   - Origem no legado: `_reversa_sdd/domain.md#2.3-dados-mínimos-obrigatórios`
   - Tipo: alterada
3. **RN-03:** O campo `TRNSTS` deve aceitar apenas `A` (Ativo) ou `I` (Inativo). 🟢
   - Origem no legado: `_reversa_sdd/domain.md#2.2-status-de-entidade`
   - Tipo: alterada
4. **RN-04:** O registro deve armazenar a data de criação e a data de última alteração. 🟢
   - Origem no legado: `_reversa_sdd/architecture.md#5-erd-resumido`
   - Tipo: alterada

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade | Critério de aceite | Confidência |
|----|-----------|------------|--------------------|-------------|
| RF-01 | O sistema deve listar transportadoras em tela de subfile, com código, razão social, CNPJ e status | Must | Ao abrir o cadastro, o usuário visualiza registros existentes em ordem e com dados básicos visíveis | 🟢 |
| RF-02 | O sistema deve permitir incluir nova transportadora | Must | Com dados válidos, o usuário consegue gravar um novo registro e receber confirmação | 🟢 |
| RF-03 | O sistema deve permitir alterar dados de uma transportadora existente | Must | O usuário pode editar um registro existente e salvar as mudanças | 🟢 |
| RF-04 | O sistema deve permitir excluir uma transportadora existente após confirmação | Must | O usuário confirma a exclusão e o registro deixa de existir no arquivo | 🟢 |
| RF-05 | O sistema deve validar campos obrigatórios antes de gravar | Must | Se razão social, CNPJ ou UF forem inválidos, a operação é bloqueada e a mensagem correspondente é exibida | 🟢 |
| RF-06 | O sistema deve impedir valores de status fora do domínio `A` e `I` | Must | Qualquer status diferente de `A` ou `I` é rejeitado | 🟢 |
| RF-07 | O sistema deve validar e-mail com presença de `@` | Should | E-mails sem `@` são rejeitados | 🟢 |
| RF-08 | O sistema deve gerar o próximo código sequencial do cadastro | Should | Cada inclusão nova recebe um código maior que o último existente | 🟢 |

## 6. Requisitos Não Funcionais

| Tipo | Requisito | Evidência ou justificativa | Confidência |
|------|-----------|----------------------------|-------------|
| Integridade | Os dados devem ser persistidos com campos mínimos e regras de consistência | Evidenciado em `ValidaDados` e no modelo `TRNREC` | 🟢 |
| Disponibilidade | O cadastro precisa operar em ambiente IBM i local sem dependência de serviços externos | Arquitetura monolítica local e ausência de integrações | 🟢 |
| Segurança | O sistema deve bloquear dados inválidos antes da gravação | Validação explícita em `ValidaDados` | 🟢 |
| Observabilidade | O usuário deve receber feedback textual em tela sobre erro ou sucesso da operação | `wMsgErro` e mensagens de sucesso em `TRN001` | 🟢 |

## 7. Critérios de Aceitação

```gherkin
Cenário: Listar transportadoras cadastradas
  Dado que existem registros em TRNPF
  Quando o usuário entra na tela de cadastro
  Então o sistema mostra o subfile com código, razão social, CNPJ e status

Cenário: Incluir transportadora com dados válidos
  Dado que o usuário preenche razão social, CNPJ e UF corretamente
  Quando confirma a inclusão
  Então o sistema grava o registro e exibe mensagem de sucesso

Cenário: Bloquear inclusão com CNPJ inválido
  Dado que o usuário preenche CNPJ com menos de 14 dígitos
  Quando tenta gravar
  Então o sistema rejeita a operação e informa a mensagem de validação

Cenário: Alterar status de uma transportadora
  Dado que o registro existe
  Quando o usuário altera status para `I`
  Então o sistema atualiza o registro e preserva a data de alteração

Cenário: Excluir transportadora após confirmação
  Dado que o registro existe
  Quando o usuário confirma a exclusão
  Então o registro é removido e a tela retorna ao estado de listagem
```

## 8. Prioridade MoSCoW

| Item | MoSCoW | Justificativa |
|------|--------|---------------|
| RF-01 | Must | Funcionalidade central de visualização |
| RF-02 | Must | Inclusão é base do cadastro |
| RF-03 | Must | Alteração é parte do ciclo CRUD |
| RF-04 | Must | Exclusão é parte do CRUD operacional |
| RF-05 | Must | Garante a integridade do cadastro |
| RF-06 | Must | Controla domínio de status |
| RF-07 | Should | Reduz erros de e-mail sem bloquear operação crítica |
| RF-08 | Should | Garante unicidade e ordenação da chave |

## 9. Esclarecimentos

> Nenhuma sessão de dúvidas registrada ainda. Rode `/reversa-clarify` quando houver `[DÚVIDA]` pendente.

## 10. Lacunas

- 🔴 [DÚVIDA] Não há evidência de regra de autorização por usuário, perfil ou papel
- 🔴 [DÚVIDA] Não há documentação sobre políticas de uso de status `A`/`I` fora do código vigente

## 11. Histórico de alterações

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-09-23 | Versão inicial gerada por `/reversa-writer` | reversa |
