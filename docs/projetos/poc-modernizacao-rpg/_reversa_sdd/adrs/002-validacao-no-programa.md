# ADR 002: Validação no próprio programa de manutenção

- Status: Aceito
- Data: 2026-09-23

## Contexto

O programa `TRN001` realiza validações antes de gravar os dados em `TRNPF`. A lógica inclui obrigatoriedade de campos, size checks e verificação do e-mail (`@`).

## Decisão

Centralizar a validação de entrada e domínio em `ValidaDados`, chamada antes da escrita em arquivo e antes da atualização.

## Justificativa

O código usa um único fluxo de tela de manutenção e uma estrutura de dados compartilhada. O ponto mais natural para garantir consistência é a rotina de validação antes de persistir o registro.

## Consequências

- Vantagens:
  - regra de negócio centralizada
  - consistência entre inclusão e alteração
  - feedback imediato para o usuário na tela

- Desvantagens:
  - acoplamento da regra de negócio à tela
  - ausência de camada de domínio separada
  - difícil reutilização fora do programa

## Alternativas consideradas

- Validar no nível do arquivo DDS
- Adicionar camada de regras em módulos independentes
- Validar somente no banco ou em trigger

A solução adotada foi a mais simples e compatível com a arquitetura procedural do projeto.
