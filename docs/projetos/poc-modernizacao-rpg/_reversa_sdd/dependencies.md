# Dependências — poc-cadastro-transportadora

> Gerado pelo Scout em 2026-09-23

## Resumo

O projeto **não possui dependências externas**. É software nativo IBM i (OS/400), escrito em RPG ILE (free-format) e DDS, sem:

- Gerenciador de pacotes (sem `package.json`, `requirements.txt`, `pom.xml`, `go.mod`, etc.)
- Bibliotecas ou frameworks de terceiros
- Docker / imagens de contêiner
- Dependências de CI/CD

## Dependências de plataforma

| Dependência | Versão | Nota |
|---|---|---|
| IBM i (OS/400) | — | Runtime obrigatório |
| ILE RPG (RPGLE) | — | Compilador/linguagem |
| DDS (só *compilado via CRTDSPF / CRTPF*) | — | Definição de arquivos físicos e telas |

## Built-ins do RPG ILE utilizados

Procedimentos/built-ins nativos referenciados no fonte `TRN001.rpgle`:

- `%Eof()` — fim de arquivo
- `%Found()` — resultado de *Chain*
- `%Trim()` — remoção de espaços
- `%Len()` — comprimento da string
- `%Scan()` — busca de caractere
- `%Date()`, `%Time()`, `%Char()`, `%Dec()`
- `*HiVal` (*Hival) — chave de maior valor para `SetGT`/`ReadP`
- Comandos de manipulação de arquivo: `Open`, `Close`, `Chain`, `SetLL`, `Read`, `ReadP`, `Write`, `Update`, `Delete`
- Comandos de workstation: `ExFmt`, `Write` (subfile)

> Nenhuma dessas requer instalação — fazem parte do IBM i runtime.