# Perguntas de revisão

## 1) Semântica de status

Qual é a regra de negócio exata para os valores `A` e `I` em `TRNSTS`?

- A opção deve significar apenas `Ativo` e `Inativo`?
- Há algum processo adicional associado a `I` (inativo, bloqueado, suspendido, cancelado)?
- O programa deve impedir qualquer outra semântica ou apenas validar a enumeração?

## 2) Autorização / perfil

Existe algum controle de acesso ou segregação funcional por usuário, perfil ou papel no processo de cadastro?

- Se sim, o sistema deve reforçar isso além da validação atual do campo.
- Se não, a regra atual fica restrita à integridade dos dados e não à autorização.

## 3) Exclusão e recuperação

A exclusão do registro é definitiva ou existe alguma regra de histórico, soft delete ou recuperação posterior?

- O comportamento atual do código sugere exclusão física.
- A regra de negócio exige confirmação com uma camada extra de retenção?

## 4) E-mail e validação pós-gravação

A validação de e-mail atual apenas exige presença de `@`.

- A regra deve continuar assim em produção?
- Ou há necessidade de um formato mais estrito, como domínio, TLD ou validação por regex?
