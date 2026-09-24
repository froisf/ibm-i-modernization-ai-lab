# PoC: Modernização de Cadastro de Transportadora (RPG ILE → Java/IA)

## ⚠️ Aviso de Propriedade Intelectual

Este projeto é uma **Prova de Conceito (PoC) sintética**, criada exclusivamente para fins de estudo e demonstração de metodologia.

O código aqui presente é **100% fictício** e **não contém** qualquer propriedade intelectual, regra de negócio ou informação confidencial de empresas onde o autor trabalhou. O objetivo é demonstrar a metodologia de engenharia reversa com IA aplicada a um cenário típico de sistemas legados IBM i (AS/400).

---

## 🎯 Objetivo

Demonstrar um processo completo de **modernização de um sistema legado IBM i (AS/400) em RPG ILE**, utilizando ferramentas open source e IA, sem depender de soluções proprietárias.

O fluxo é:

1. **Engenharia Reversa com IA (Reversa):** Extrair regras de negócio, modelo de dados e especificações de um programa RPG ILE.
2. **Geração de Especificações:** Produzir documentação executável (SDD, OpenAPI, User Stories) pronta para uso por agentes de IA.
3. **Modernização (Java/Spring Boot):** Converter a lógica legada em uma API REST moderna.
4. **Integração (JTOpen/DB2):** Conectar a nova API ao sistema IBM i original.

---

## 🛠️ Ferramentas Utilizadas

| Ferramenta | Propósito |
|---|---|
| **Reversa** | Engenharia reversa com IA (extração de especificações) |
| **OpenCode + Claude** | Agente de IA que executa o Reversa |
| **VS Code** | Ambiente de desenvolvimento |
| **Git/GitHub** | Versionamento e portfólio |

---

## 📂 Estrutura do Projeto

```
poc-modernizacao-rpg/
├── QRPGLESRC/
│   └── TRN001.rpgle          # Programa principal (CRUD de transportadoras)
├── QDDSRC/
│   ├── TRNPF.dds             # Arquivo físico (tabela de transportadoras)
│   └── TRNDSP.dds            # Arquivo de display (telas de manutenção)
├── _reversa_sdd/             # Especificações geradas pelo Reversa (auto)
│   └── cadastro-transportadora/
│       ├── requirements.md
│       ├── design.md
│       ├── tasks.md
│       ├── architecture.md
│       ├── c4-*.md
│       ├── erd-complete.md
│       ├── data-dictionary.md
│       ├── code-analysis.md
│       ├── spec-impact-matrix.md
│       ├── confidence-report.md
│       ├── questions.md
│       └── gaps.md
└── README.md                 # Este arquivo
```
---

## 📊 Regras de Negócio Extraídas

O programa `TRN001` implementa um **cadastro de transportadoras** com as seguintes regras:

### Operações CRUD
- **Incluir:** Cadastrar nova transportadora com código sequencial automático.
- **Alterar:** Modificar dados de transportadora existente.
- **Excluir:** Remover transportadora do cadastro.
- **Listar:** Exibir todas as transportadoras em tela paginada (subfile).

### Validações Obrigatórias
1. **Razão Social:** Campo obrigatório.
2. **CNPJ:** Obrigatório e deve conter 14 dígitos.
3. **UF:** Deve ter exatamente 2 caracteres.
4. **Status:** Deve ser `A` (Ativo) ou `I` (Inativo).
5. **E-mail:** Deve conter o caractere `@`.

### Regras Automáticas
- **Código sequencial:** Gerado automaticamente pelo sistema.
- **Data de Cadastro:** Preenchida na inclusão.
- **Data de Alteração:** Atualizada em cada modificação.

---

## 📈 Resultados da Análise com IA

A análise com o Reversa foi concluída em **menos de 2 horas**, gerando **15 artefatos de especificação**:

| Artefato | Descrição |
|---|---|
| `requirements.md` | Requisitos funcionais e não-funcionais |
| `design.md` | Design da solução |
| `tasks.md` | Tarefas para implementação |
| `architecture.md` | Arquitetura do sistema |
| `c4-context.md` | Diagrama C4 — Contexto |
| `c4-containers.md` | Diagrama C4 — Containers |
| `c4-components.md` | Diagrama C4 — Componentes |
| `erd-complete.md` | Modelo de Dados completo (14 campos) |
| `data-dictionary.md` | Dicionário de dados |
| `code-analysis.md` | Análise do código RPG |
| `spec-impact-matrix.md` | Matriz de rastreabilidade |
| `confidence-report.md` | Relatório de confiança |
| `questions.md` | Perguntas pendentes |
| `gaps.md` | Lacunas identificadas |
| `transportadora.md` | Especificação do módulo |

### Lacunas Identificadas pela IA
1. Semântica exata dos valores `A` e `I` em `TRNSTS`
2. Controle de autorização por usuário/perfil
3. Política de exclusão definitiva vs. histórico (soft delete)

---

## 🚀 Próximos Passos

- [x] Criar programa RPG ILE fictício (PoC)
- [x] Executar análise do Reversa
- [x] Gerar especificações (SDD, OpenAPI, User Stories)
- [ ] Criar API REST em Java (Spring Boot)
- [ ] Implementar integração com IBM i (JTOpen)
- [ ] Documentar processo completo no LinkedIn

---

## 👤 Autor

**Flavio Frois**
- LinkedIn: [www.linkedin.com/in/flaviofrois](https://www.linkedin.com/in/flaviofrois)
- GitHub: [github.com/froisf](https://github.com/froisf)

*"Unindo a robustez do legado à inovação do ecossistema Cloud."*

