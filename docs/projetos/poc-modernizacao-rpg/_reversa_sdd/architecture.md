# Arquitetura — `poc-cadastro-transportadora`

> Gerado pelo Reversa em 2026-09-23
> Escala de confiança: 🟢 CONFIRMADO / 🟡 INFERIDO / 🔴 LACUNA

## 1. Visão geral

A solução é uma aplicação monolítica nativa do IBM i, desenvolvida em RPG ILE com definição de tela em DDS e persistência em arquivo físico DDS. O programa principal `TRN001` concentra a lógica de negócio, a interface e o acesso ao banco em um único ponto de execução.

## 2. Contexto C4 (resumo)

O sistema opera como um cadastro local para transportadoras. O usuário interage diretamente com um terminal 5250 e o programa manipula o arquivo `TRNPF` sem depender de serviços externos.

- Usuário: operador de cadastro
- Sistema principal: `TRN001` (RPG ILE)
- Persistência: `TRNPF` (arquivo DDS)
- Apresentação: `TRNDSP` (display file DDS)
- Integrações externas: nenhuma detectada

## 3. Estrutura de containers

### Container principal

- `IBM i Application` — RPG ILE / DDS
  - Entrada: `QRPGLESRC/TRN001.rpgle`
  - UI: `QDDSRC/TRNDSP.dds`
  - Arquivo de dados: `QDDSRC/TRNPF.dds`

### Persistência

- `Arquivo físico DDS` — banco de dados legado do IBM i
  - Entidade: `TRNREC`
  - Chave: `TRNCOD`

## 4. Componentes internos

| Componente | Função | Observação |
|---|---|---|
| `TRN001` | Programa principal | Orquestra listagem, inclusão, alteração e exclusão |
| `CarregaLista` | Exibe subfile com registros | Monta a listagem do cadastro |
| `IncluirRegistro` | Cria novo registro | Valida dados e grava no arquivo |
| `AlterarRegistro` | Atualiza registro | Recarrega dados da tela e aplica atualização |
| `ExcluirRegistro` | Remove registro | Confirmação via tela |
| `ValidaDados` | Valida regra de negócio | Check central de consistência |
| `GeraCodigo` | Gera sequência | Baseado no valor máximo existente |
| `TRNPF` | Repositório de dados | Arquivo físico da entidade |
| `TRNDSP` | Interface de usuário | Subfile + detalhe |

## 5. ERD resumido

A entidade central é única e auto-contida:

- `TRNREC`
  - `TRNCOD` (PK)
  - `TRNNOM`
  - `TRNFAN`
  - `TRNCNPJ`
  - `TRNIEP`
  - `TRNEND`
  - `TRNCID`
  - `TRNUF`
  - `TRNCEP`
  - `TRNFON`
  - `TRNEML`
  - `TRNSTS`
  - `TRNDTC`
  - `TRNDTA`

## 6. Integrações externas

Nenhuma.

- APIs REST: nenhuma
- filas/mensageria: nenhuma
- webhooks: nenhum
- banco externo: nenhum

## 7. Dívidas técnicas

- Acoplamento forte de UI, regra e persistência
- Ausência de testes automatizados
- Ausência de camada de domínio ou serviço
- Falta de observabilidade e auditoria de operações fora do arquivo
- Dependência direta de DDS/5250, o que dificulta portabilidade

## 8. Conclusão arquitetural

A arquitetura é um monólito legado em IBM i, com forte acoplamento entre interface e dados, mas com baixa complexidade operacional e funcionamento adequado ao tipo de cadastro administrativo que o projeto representa.

- 🟢 CONFIRMADO: estrutura monolítica local
- 🟡 INFERIDO: uso operacional como sistema interno de cadastro
- 🔴 LACUNA: não há prova de integrações ou evolução futura em código atual
