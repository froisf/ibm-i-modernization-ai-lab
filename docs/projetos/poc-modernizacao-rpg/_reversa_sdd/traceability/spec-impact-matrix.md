# Spec Impact Matrix — `transportadora`

| Componente | Função | Impacta | Tipo de impacto |
|---|---|---|---|
| `TRN001` | Programa principal | flow de cadastro, listagem e manutenção | alto |
| `CarregaLista` | Visualização de registros | operação de listagem | alto |
| `IncluirRegistro` | Inclusão | criação do cadastro | alto |
| `AlterarRegistro` | Alteração | atualização do cadastro | alto |
| `ExcluirRegistro` | Exclusão | remoção do cadastro | alto |
| `ValidaDados` | Validação | integridade dos dados | alto |
| `GeraCodigo` | Sequência | chave primária e unicidade | médio |
| `TRNPF` | Entidade persistente | modelo do domínio | alto |
| `TRNDSP` | Interface 5250 | UX e entrada de dados | alto |

## Resumo

A maior parte do impacto de qualquer mudança funcional recai sobre `TRN001`, `TRNPF` e `TRNDSP`, pois esses componentes concentram a lógica, a persistência e a interface do cadastro.
