# Domínio de Negócio — `transportadora`

> Gerado pelo Reversa em 2026-09-23
> Escala de confiança: 🟢 CONFIRMADO / 🟡 INFERIDO / 🔴 LACUNA

## 1. Glossário do domínio

| Termo | Significado | Evidência |
|---|---|---|
| `Transportadora` | Entidade principal do cadastro | `TRNPF`, `TRN001`, `TRNREC` |
| `Código` | Identificador único | `TRNCOD` |
| `Razão Social` | Nome Jurídico da transportadora | `TRNNOM` |
| `Nome Fantasia` | Nome comercial / apelido | `TRNFAN` |
| `CNPJ` | Identificador fiscal | `TRNCNPJ` |
| `Inscrição Estadual` | Registro estadual | `TRNIEP` |
| `Endereço` | Endereço principal | `TRNEND` |
| `Cidade / UF / CEP` | Localização geográfica | `TRNCID`, `TRNUF`, `TRNCEP` |
| `Status` | Estado do cadastro | `TRNSTS` (`A` / `I`) |
| `Data de Cadastro` | Data de criação do registro | `TRNDTC` |
| `Data de Alteração` | Última modificação aplicada | `TRNDTA` |

## 2. Regras de negócio confirmadas

### 2.1 Registro único por código

A chave primária é `TRNCOD`, e o programa usa `Chain` por essa chave em consulta, alteração e exclusão.

- 🟢 CONFIRMADO

### 2.2 Status de entidade

A entidade aceita apenas dois valores para `TRNSTS`:

- `A` = Ativo
- `I` = Inativo

Validação explicitamente implementada em `ValidaDados`.

- 🟢 CONFIRMADO

### 2.3 Dados mínimos obrigatórios

Antes de gravar, o sistema exige:

- `TRNNOM` preenchido
- `TRNCNPJ` preenchido
- `TRNCNPJ` com no mínimo 14 caracteres
- `TRNUF` com exatamente 2 caracteres
- `TRNSTS` em `A` ou `I`
- `TRNEML` contendo `@`

- 🟢 CONFIRMADO

### 2.4 Geração automática de código

O programa busca o maior código existente no arquivo, incrementa 1 e usa esse valor como novo `TRNCOD`.

- 🟢 CONFIRMADO

## 3. Regras de negócio inferidas

### 3.1 O sistema foi desenhado para um cadastro operacional, não para processamento comercial

A estrutura de dados indica alto foco em identificação cadastral e auditoria, mas sem campos financeiros, tributários detalhados ou relacionamento com outros módulos.

- 🟡 INFERIDO

### 3.2 O status ativo/inativo parece funcionar como habilitação lógica e não como exclusão física

A exclusão do registro é suportada, mas a presença do status sugere que o sistema usa `A`/`I` para controlar visibilidade ou uso sem remover o registro.

- 🟡 INFERIDO

### 3.3 O programa assume uma interface 5250 totalmente presencista

Não há camada web, batch de integração ou API; a UI e o banco estão acoplados ao mesmo programa.

- 🟡 INFERIDO

## 4. Escopo de domínio e limitações

### 4.1 O que está explícito no código

- Cadastro completo de transportadoras
- Listagem por subfile
- Inclusão, alteração e exclusão
- Validação de dados essenciais
- Controle de datas de criação e alteração

### 4.2 O que não está evidenciado

- Regras de negócio por segmento de transportadora
- Hierarquia de clientes ou fornecedores
- Fluxos de aprovação
- Controle de permissões por usuário
- Integração externa

- 🔴 LACUNA

## 5. Resumo executivo

O domínio principal é o cadastro de transportadoras em ambiente IBM i, com entidade única e operações clássicas de manutenção. O sistema prioriza integridade mínima dos dados cadastrais e a auditoria temporal de criação/alteração, sem apresentar complexidade operacional além do registro em si.
