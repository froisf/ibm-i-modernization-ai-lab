# Análise de Código — módulo `transportadora`

> Gerado pelo Reversa em 2026-09-23
> Nível de documentação: completo
> Escala de confiança: 🟢 confirmado / 🟡 inferido / 🔴 lacuna

## 1. Visão geral

O módulo `transportadora` é implementado por um único programa RPG ILE, `QRPGLESRC/TRN001.rpgle`, apoiado por:

- `QDDSRC/TRNPF.dds`: arquivo físico DDS da tabela `TRNPF`
- `QDDSRC/TRNDSP.dds`: display file com subfile de listagem e tela de manutenção

O programa oferece CRUD completo para cadastro de transportadoras, usando fluxo de tela 5250 e arquivo indexado pela chave `TRNCOD`.

## 2. Ponto de entrada

O entry point é o programa `TRN001`:

- `Ctl-Opt DftActGrp(*No) ActGrp(*Caller) Main(TRN001);`
- `Dcl-Proc TRN001; ... End-Proc;`

Ao iniciar, o programa abre `TRNPF` e `TRNDSP`, inicializa o modo de operação em `'L'` (lista) e entra em loop principal até que a flag `Exit` seja acionada.

## 3. Estruturas e dados principais

### Arquivo físico

O arquivo DDS `TRNPF` define o registro `TRNREC` com 13 campos e chave primária `TRNCOD`.

Campos principais:

- `TRNCOD` — código da transportadora, chave principal (`10A`)
- `TRNNOM` — razão social (`50A`)
- `TRNFAN` — nome fantasia (`30A`)
- `TRNCNPJ` — CNPJ (`18A`)
- `TRNIEP` — inscrição estadual (`15A`)
- `TRNEND` — endereço (`60A`)
- `TRNCID` — cidade (`40A`)
- `TRNUF` — UF (`2A`)
- `TRNCEP` — CEP (`9A`)
- `TRNFON` — telefone (`15A`)
- `TRNEML` — e-mail (`60A`)
- `TRNSTS` — status (`A`/`I`, ativo/inativo) (`1A`)
- `TRNDTC` — data de cadastro (`8S 0`)
- `TRNDTA` — data de alteração (`8S 0`)

## 4. Fluxo de controle

### 4.1 Modo lista (`wModo = 'L'`)

A rotina `CarregaLista`:

1. limpa o subfile e escreve o cabeçalho `TRNCTL`
2. usa `SetLL *Start TRNPF; Read TRNPF;`
3. percorre registros com `Dow (not %Eof(TRNPF));`
4. copia o registro para `TrnDsOut` e escreve em `TRNSFL`
5. contabiliza `wContador`
6. exibe o subfile via `ExFmt TRNCTL`
7. se houver registros, tenta localizar a linha selecionada por `Chain wRrn TRNPF` e alterna para `wModo = 'A'`

Ponto de atenção: há um uso de `wRrn` que parece manter o último número de linha acessado; a lógica não reinicializa `wRrn` antes da leitura do subfile, então a presença de registros antigos poderia afetar a seleção em algumas ocasiões. Isso é uma hipótese de comportamento, não uma confirmação operacional. 🟡 INFERIDO

### 4.2 Inclusão (`wModo = 'I'`)

A rotina `IncluirRegistro`:

1. limpa `TrnDsOut`
2. preenche `TRNSTS = 'A'`
3. exibe `TRNDTL` para entrada do usuário
4. se `Cancel`, volta para lista
5. chama `ValidaDados`
6. gera código com `GeraCodigo()`
7. aplica data de cadastro e alteração via `%Date()` / `%Dec()`
8. grava o registro com `Write TRNPF TrnDsOut`

### 4.3 Alteração (`wModo = 'A'`)

A rotina `AlterarRegistro`:

1. encontra o registro pelo código `TRNCOD`
2. carrega `TrnDsOut = TrnDs`
3. exibe `TRNDTL`
4. valida dados
5. atualiza data de alteração
6. executa `Update TRNPF TrnDsOut`

### 4.4 Exclusão (`wModo = 'E'`)

A rotina `ExcluirRegistro`:

1. recupera o registro pela chave
2. exibe tela de confirmação com `TRNDTL`
3. se o usuário confirmar, executa `Delete TRNPF`

## 5. Regras de validação

A rotina `ValidaDados` impõe validações rígidas:

- `TRNNOM` obrigatório
- `TRNCNPJ` obrigatório
- `TRNCNPJ` deve ter no mínimo 14 caracteres
- `TRNUF` deve conter 2 caracteres
- `TRNSTS` deve ser `A` ou `I`
- `TRNEML` deve conter o caractere `@`

Se qualquer regra falhar, `wErro = *On` e a interface retorna para a tela de detalhe com mensagem em `wMsgErro`.

## 6. Geração de código e controle de sequência

A rotina `GeraCodigo()`:

1. inicia com `wUltimo = '0000000000'`
2. busca o último registro com `SetGT *Hival TRNPF; ReadP TRNPF;`
3. se houver dados, usa `TrnDs.TRNCOD` como último código
4. converte para número e soma 1
5. em caso de erro de conversão, volta para `1`

Isto gera sequência numérica textual em 10 posições, por exemplo `0000000001`, `0000000002` etc.

## 7. Tratamento de erro e mensagens

A rotina `MostrarErro` exibe novamente a tela de manutenção após a ocorrência de erro. A variável `wMsgErro` é responsável por mensagens de validação e feedback do sistema.

## 8. Observações de negócio e regras implícitas

- O módulo trata transportadoras como registros únicos por `TRNCOD`
- O status é um discriminador de negócio: ativo/inativo
- O cadastro é 100% local, sem APIs externas, sem integrações e sem persistência em outros sistemas
- O programa usa as telas 5250 como interface principal, com subfile para listagem e detalhe para manutenção

## 9. Resumo executivo

O módulo `transportadora` é um CRUD clássico de IBM i, construído em RPG ILE e DDS:

- entrada única: `TRN001`
- persistência: arquivo físico `TRNPF`
- apresentação: display file `TRNDSP`
- operações: listar, incluir, alterar, excluir
- validação: regra de negócio aplicada no ponto de gravação

## 10. Conclusão

A implementação é consistente com um cadastro básico de transportadoras em ambiente legado IBM i. O código segue um padrão procedural clássico, com pouca abstração, fortes dependências de estruturas de tela e banco, e regra de negócio concentrada em `ValidaDados`.

---

### Arquivos analisados

- `QRPGLESRC/TRN001.rpgle`
- `QDDSRC/TRNPF.dds`
- `QDDSRC/TRNDSP.dds`

### Escala de confiança

- 🟢 CONFIRMADO: estrutura, campos, fluxo, validações e operações CRUD
- 🟡 INFERIDO: comportamento do `wRrn` em seleção e nuances de UX do subfile
- 🔴 LACUNA: comportamento completo e regras de negócio fora do código (ex.: política de status, integração com outros módulos)
