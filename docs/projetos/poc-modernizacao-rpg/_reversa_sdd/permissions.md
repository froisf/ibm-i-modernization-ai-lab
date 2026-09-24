# Permissões e Papéis — `transportadora`

> Gerado pelo Reversa em 2026-09-23
> Escala de confiança: 🟢 CONFIRMADO / 🔴 LACUNA

## 1. Diagnóstico

Não foi identificado nenhum mecanismo de autenticação, autorização, RBAC ou ACL no projeto.

Os artefatos analisados evidenciam:

- programa RPG ILE único em ambiente local IBM i
- acesso direto ao arquivo físico `TRNPF`
- telas 5250 para manutenção dos dados
- ausência de usuários, perfis, níveis de acesso ou políticas de segurança por funcionalidade

## 2. Matriz de permissões

| Papel | Listar | Incluir | Alterar | Excluir | Observação |
|---|---|---|---|---|---|
| Nenhum papel detectado | 🔴 | 🔴 | 🔴 | 🔴 | O sistema não implementa controle de autorização por perfil |

## 3. Conclusão

O módulo de transportadoras não possui RBAC/ACL implementado. O controle atual é operacional e local, sem distinção de papéis de usuário ou permissões granulares.

- 🟢 CONFIRMADO: ausência de estrutura de permissões
- 🔴 LACUNA: não há documentação formal de política de acesso fora do código
