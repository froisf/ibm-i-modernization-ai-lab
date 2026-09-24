# 🔍 Caderno de Estudos: Reversa (Engenharia Reversa com IA)

Anotações sobre o uso do Reversa para modernização de sistemas legados IBM i.

## 📚 Tópicos
- [x] O que é o Reversa e como instalar
- [x] Fluxo completo de análise (5 fases)
- [x] Como responder as perguntas da IA
- [ ] Uso do `/reversa-migrate` para plano de migração
- [ ] Uso do `/reversa-docs` para gerar mini-site HTML
- [ ] Boas práticas de segurança (`.gitignore`, dados sensíveis)

## 📝 Anotações

### O que é o Reversa
Framework open source de engenharia reversa que usa IA para extrair especificações executáveis de sistemas legados. Ele não substitui a análise humana, mas **acelera drasticamente** o processo.

### Fluxo de Análise (5 Fases)

1. **Scout (Reconhecimento):** Mapeia estrutura, dependências e pontos de entrada.
2. **Arqueólogo (Escavação):** Analisa módulos em profundidade.
3. **Detetive + Arquiteto (Interpretação):** Extrai regras de negócio, ERD, diagramas C4.
4. **Redator (Geração):** Cria SDD, OpenAPI, User Stories, Tasks.
5. **Revisor (Revisão):** Validação cruzada e relatório de confiança.

### Lições Aprendidas

- **Sempre isolar o escopo:** Analisar um módulo por vez é mais eficiente que o sistema todo.
- **Usar código fictício:** Nunca expor propriedade intelectual de empresas.
- **README é contexto:** A IA usa o `README.md` como guia para entender o objetivo.
- **Paciência é fundamental:** A análise de um sistema real pode levar horas.
- **Sempre revisar os resultados:** A IA pode gerar especificações incorretas; o humano valida.

### Boas Práticas de Segurança

- ❌ Nunca subir código real da empresa para o GitHub
- ❌ Nunca expor chaves de API ou tokens
- ✅ Usar `.gitignore` para excluir arquivos sensíveis
- ✅ Criar versões fictícias para portfólio
- ✅ Anonimizar nomes de programas, tabelas e empresas

### Ferramentas Complementares

| Ferramenta | Uso |
|---|---|
| **Reversa** | Engenharia reversa com IA |
| **OpenCode + Claude** | Agente que executa o Reversa |
| **VS Code** | Ambiente de desenvolvimento |
| **NotebookLM** | Estudo de documentação técnica |

## 🔗 Projetos Relacionados
- [PoC: Modernização de Cadastro de Transportadora](../../projetos/poc-modernizacao-rpg/README.md)
