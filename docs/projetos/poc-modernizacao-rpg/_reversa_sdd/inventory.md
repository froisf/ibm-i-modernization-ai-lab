# Inventário — poc-cadastro-transportadora

> Gerado pelo Scout em 2026-09-23
> Projeto IBM i (AS/400) nativo — PoC fictícia de Cadastro de Transportadoras

---

## 1. Estrutura de pastas

```
poc-cadastro-transportadora/
├── QDDSRC/                  # Fontes DDS (Data Description Specifications)
│   ├── TRNPF.dds            # Arquivo físico (tabela) — Cadastro de Transportadoras
│   └── TRNDSP.dds           # Arquivo de exibição (telas 5250)
├── QRPGLESRC/               # Fontes RPG (Programas RPG ILE)
│   └── TRN001.rpgle         # Programa principal — CRUD completo
├── AGENTS.md                # Documentação do framework Reversa
├── README.md                # Vazio
├── .agents/                 # Skills do Reversa
└── .reversa/                # Estado e configuração do Reversa
```

**Convenção IBM i:** `QDDSRC` e `QRPGLESRC` são os nomes clássicos de *source physical files* em bibliotecas IBM i. Seguem o padrão `<Q>` + `<TIPO>` + `<SRC>`:

- `QDDSRC` — fonte de arquivos DDS
- `QRPGLESRC` — fonte de programas RPGLE

## 2. Tecnologias e frameworks

| Tecnologia | Onde | Uso |
|---|---|---|
| RPG ILE (free-format) | `QRPGLESRC/TRN001.rpgle` | Programa principal (CRUD) |
| DDS (Data Description Specifications) | `QDDSRC/` | Definição de arquivos físicos e telas 5250 |
| IBM i (OS/400) | plataforma | Runtime nativo |

Não há frameworks externos, bibliotecas de terceiros nem gerenciador de pacotes: o projeto usa apenas APIs nativas do IBM i (`*SOLID`, `%Char`, `%Dec`, `%Scan`, etc.).

## 3. Pontos de entrada

| Caminho | Tipo |
|---|---|
| `QRPGLESRC/TRN001.rpgle` | Programa principal (entry point via `Main(TRN001)`) |

- Programa único, autocontido. Não há módulos, procedures exportadas nem serviço web.
- Configuração de runtime via `Ctl-Opt` no próprio fonte:
  - `DftActGrp(*No)` + `ActGrp(*Caller)` — ativo em grupo de ativação do chamador
  - `Main(TRN001)` — entry point nomeado

## 4. Schema de banco de dados (superficial)

| Caminho | Tipo | Conteúdo |
|---|---|---|
| `QDDSRC/TRNPF.dds` | Arquivo físico (DDS) | Registro `TRNREC`, 13 campos, chave primária `TRNCOD` |

Campos (definidos no DDS):

| Campo | Tipo DDS | Tamanho | Descrição |
|---|---|---|---|
| TRNCOD | A | 10 | Código da transportadora (chave) |
| TRNNOM | A | 50 | Razão Social |
| TRNFAN | A | 30 | Nome Fantasia |
| TRNCNPJ | A | 18 | CNPJ |
| TRNIEP | A | 15 | Inscrição Estadual |
| TRNEND | A | 60 | Endereço |
| TRNCID | A | 40 | Cidade |
| TRNUF | A | 2 | UF |
| TRNCEP | A | 9 | CEP |
| TRNFON | A | 15 | Telefone |
| TRNEML | A | 60 | E-mail |
| TRNSTS | A | 1 | Status (A/I) |
| TRNDTC | S | 8,0 | Data de cadastro |
| TRNDTA | S | 8,0 | Data de alteração |

> Análise detalhada fica a cargo do `reversa-data-master`. Apenas listagem a nível de superfície.

## 5. Telas (DDS display file — `QDDSRC/TRNDSP.dds`)

- **TRNSFL** — *subfile* de listagem de transportadoras (5 posições de exibição na tela, rolagem SFLPAG 10 / SFLSIZ 15)
- **TRNCTL** — record de controle do subfile (indicadores 30–33) + cabeçalhos e rodapé com atalhos de função
- **TRNDTL** — tela de manutenção (inclusão/alteração/exclusão)

Atalhos de função: `F3=Sair` (CA03), `F12=Cancelar` (CA12). Na tela de listagem `F6=Incluir`, `F10=Atualizar`; na tela de detalhe `F4=Excluir`.

## 6. Cobertura de testes

Não há testes automatizados (0 arquivos `*.test.*`, `*.spec.*`, unit tests ou BDD).

## 7. Integrações externas

Nenhuma. Sistema 100% offline, sem rede, sem APIs externas.

## 8. CI/CD, Docker e configurações

- **CI/CD:** ausente
- **Docker:** ausente
- **Arquivos `.env`, `config/`, `settings`:** ausentes

## 9. Resumo

| Item | Valor |
|---|---|
| Total de arquivos fonte | 3 |
| Linguagem principal | RPG ILE (free-format) |
| Framework principal | Nenhum (IBM i nativo) |
| Banco de dados | Arquivo físico DDS `TRNPF` |
| Módulos identificados | 1 domínio: `transportadora` |
| Features | 1: cadastro de transportadoras (CRUD completo) |