# Lacunas identificadas

## 1. Autorização por usuário/perfil

- Status: 🔴
- Observação: não existe evidência de controles de acesso ou perfil de usuário no programa legado visível.
- Impacto: o cadastro pode ser operado sem distinção funcional em ambiente local.
- Recomendação: confirmar se esse controle existe fora do escopo do código atual ou se deve ser tratado como requisito novo.

## 2. Semântica do status `A`/`I`

- Status: 🔴
- Observação: a validação do campo existe, mas a regra funcional exata dos estados não está documentada no código.
- Impacto: há risco de interpretação errada na implementação futura.
- Recomendação: confirmar a regra com o usuário e registrar o significado exato em uma próxima revisão.

## 3. Política de exclusão

- Status: 🟡
- Observação: a exclusão aparece como física no código, mas a regra de negócio de retenção/soft delete não foi evidenciada.
- Impacto: possível diferença entre comportamento legado e política operacional atual.
- Recomendação: validar se a exclusão deve ser realmente definitiva.
