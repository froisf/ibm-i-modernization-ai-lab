# Design: Cadastro de Transportadora

> Identificador: `DSN-001-cadastro-transportadora`
> Data: `2026-09-23`
> Pasta da extração reversa: `_reversa_sdd/`
> Confidência: 🟢 CONFIRMADO, 🟡 INFERIDO, 🔴 LACUNA

## 1. Objetivo da unidade

A unidade `cadastro-transportadora` tem como objetivo manter o registro cadastral de transportadoras com integridade mínima, permitindo operação de consulta, inclusão, alteração e exclusão a partir de uma interface 5250.

## 2. Mapeamento de legado para solução

| Componente legado | Papel na solução | Evidência |
|---|---|---|
| `QRPGLESRC/TRN001.rpgle` | Programa principal e orquestrador do fluxo | `TRN001` + `CarregaLista`, `IncluirRegistro`, `AlterarRegistro`, `ExcluirRegistro` |
| `QDDSRC/TRNPF.dds` | Entidade persistente | `TRNREC`, chave `TRNCOD` |
| `QDDSRC/TRNDSP.dds` | Interface de manutenção e listagem | `TRNSFL`, `TRNCTL`, `TRNDTL` |

## 3. Visão funcional

### Fluxo principal: listagem

1. O programa abre os arquivos `TRNPF` e `TRNDSP`.
2. Executa `CarregaLista`.
3. Limpa o subfile e exibe registros disponíveis.
4. Permite navegar e selecionar um registro para alteração.
5. Se a lista estiver vazia, exibe mensagem informativa.

### Fluxo principal: inclusão

1. O usuário aciona inclusão pela tela de listagem.
2. O programa exibe a tela de detalhe `TRNDTL`.
3. Os dados são validados antes de gravar.
4. Se válidos, o sistema gera próximo código e persiste o registro.
5. O sistema retorna à tela de listagem.

### Fluxo principal: alteração

1. O usuário seleciona um registro.
2. O programa carrega os dados do `TRNPF` na tela.
3. Realiza validação.
4. Atualiza o campo `TRNDTA` com a nova data.
5. Persiste o registro e retorna ao listagem.

### Fluxo principal: exclusão

1. O usuário entra em detalhe do registro.
2. O programa pede confirmação pela própria tela.
3. Se confirmado, executa `Delete TRNPF`.
4. O sistema retorna à listagem.

## 4. Regras de domínio e comportamento

### 4.1 Integridade de cadastro

- O identificador único é `TRNCOD`.
- Razão social e CNPJ são obrigatórios.
- UF deve ter 2 caracteres.
- Status aceita apenas `A` ou `I`.
- E-mail exige `@`.

### 4.2 Persistência

- Todos os registros são gravados em `TRNPF`.
- Dados de criação e alteração são armazenados em inteiros de 8 posições (`TRNDTC` e `TRNDTA`).
- O código é gerado sequencialmente pelo maior valor já cadastrado.

## 5. Componentização funcional

| Módulo / rotina | Responsabilidade | Dependências |
|---|---|---|
| `TRN001` | Controle do loop e orquestração | `TRNPF`, `TRNDSP` |
| `CarregaLista` | Exibição de registros | `SetLL`, `Read`, `ExFmt` |
| `IncluirRegistro` | Inclusão | `ValidaDados`, `GeraCodigo`, `Write` |
| `AlterarRegistro` | Alteração | `Chain`, `ValidaDados`, `Update` |
| `ExcluirRegistro` | Exclusão | `Chain`, `Delete` |
| `ValidaDados` | Regra de negócio | `TRnDsOut` |
| `GeraCodigo` | Sequenciamento | `SetGT`, `ReadP` |

## 6. Modelos de dados

### Entidade principal

```text
TRNREC
- TRNCOD: char(10), chave primária
- TRNNOM: char(50)
- TRNFAN: char(30)
- TRNCNPJ: char(18)
- TRNIEP: char(15)
- TRNEND: char(60)
- TRNCID: char(40)
- TRNUF: char(2)
- TRNCEP: char(9)
- TRNFON: char(15)
- TRNEML: char(60)
- TRNSTS: char(1)
- TRNDTC: packed(8:0)
- TRNDTA: packed(8:0)
```

## 7. Critérios de aceite do design

- O sistema mantém uma única entidade de cadastro.
- O fluxo tem separação funcional por rotina em código, mesmo sem arquitetura de camadas.
- O modelo persistente é compatível com a tela e a lógica de validação.
- O design respeita a estrutura do legado IBM i e é executável como especificação funcional.

## 8. Regras de implementação esperadas

| Regra | Estado |
|---|---|
| Persistência em arquivo DDS | 🟢 confirmada |
| Regras de validação no código | 🟢 confirmada |
| UI em display file 5250 | 🟢 confirmada |
| Sequência numérica para código | 🟢 confirmada |
| Autorização por perfil | 🔴 lacuna |

## 9. Histórico de alterações

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-09-23 | Versão inicial gerada por `/reversa-writer` | reversa |
