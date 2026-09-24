# Dicionário de Dados — módulo `transportadora`

> Gerado pelo Reversa em 2026-09-23
> Fonte: `QDDSRC/TRNPF.dds`

## 1. Entidade principal

- Nome: `TRNREC`
- Arquivo físico: `TRNPF`
- Chave primária: `TRNCOD`
- Uso: cadastro de transportadoras

## 2. Tabela de campos

| Campo | Tipo DDS | Tamanho | Obrigatório | Significado | Observação |
|---|---|---:|---|---|---|
| `TRNCOD` | A | 10 | Sim | Código da transportadora | Chave primária, sequência gerada pelo sistema |
| `TRNNOM` | A | 50 | Sim | Razão social | Nome jurídico da empresa |
| `TRNFAN` | A | 30 | Não | Nome fantasia | Nome comercial |
| `TRNCNPJ` | A | 18 | Sim | CNPJ | Valido com mínimo de 14 caracteres |
| `TRNIEP` | A | 15 | Não | Inscrição estadual | Campo livre, geralmente numérico |
| `TRNEND` | A | 60 | Não | Endereço | Rua/avenida + número |
| `TRNCID` | A | 40 | Não | Cidade | Cidade de cadastro |
| `TRNUF` | A | 2 | Não | UF | Ex.: SP, RJ, PR |
| `TRNCEP` | A | 9 | Não | CEP | Formato textual, sem máscara |
| `TRNFON` | A | 15 | Não | Telefone | Campo textual, sem máscara |
| `TRNEML` | A | 60 | Não | E-mail | Validação exige presença de `@` |
| `TRNSTS` | A | 1 | Sim | Status | Valores permitidos: `A` (Ativo), `I` (Inativo) |
| `TRNDTC` | S | 8,0 | Sim | Data de cadastro | Armazenado em formato numérico YYYYMMDD |
| `TRNDTA` | S | 8,0 | Sim | Data de alteração | Armazenado em formato numérico YYYYMMDD |

## 3. Regras e convenções

- `TRNCOD` é a chave primária e identifica cada registro de transportadora.
- `TRNSTS` funciona como flag de estado do cadastro.
- `TRNDTC` registra a data do primeiro cadastro e nunca é sobrescrita em alterações.
- `TRNDTA` atualiza sempre que o registro é alterado.
- O programa gera automaticamente o próximo código por sequência, com base no maior valor existente.

## 4. Valores esperados por domínio

### Status

| Valor | Descrição |
|---|---|
| `A` | Ativo |
| `I` | Inativo |

## 5. Observações de integridade

O código valida:

- obrigatoriedade de razão social e CNPJ
- limite mínimo de 14 dígitos em CNPJ
- tamanho de UF igual a 2
- status restrito aos valores `A` e `I`
- presença de `@` no e-mail

Essas regras aparecem na rotina `ValidaDados` em `TRN001.rpgle`.

## 6. Relacionamentos

Não há chaves estrangeiras nem relacionamento com outras entidades no código atual. A estrutura é um arquivo único e auto-contido.

## 7. Resumo

A entidade `TRNREC` representa o cadastro principal de transportadoras, com dados cadastrais básicos, status e auditoria temporal de criação/alteração.
