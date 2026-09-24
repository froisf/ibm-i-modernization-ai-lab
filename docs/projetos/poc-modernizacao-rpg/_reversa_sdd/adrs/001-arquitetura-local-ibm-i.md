# ADR 001: Arquitetura local de cadastro em IBM i

- Status: Aceito
- Data: 2026-09-23

## Contexto

O sistema de cadastro de transportadoras foi implementado como um programa RPG ILE standalone em IBM i, com arquivo físico DDS (`TRNPF`) e display file (`TRNDSP`). Não há componentes web, camadas de serviço, frameworks ou infraestrutura de banco externo.

## Decisão

Manter o cadastro em uma arquitetura monolítica local, com:

- programa principal `TRN001`
- arquivo físico `TRNPF` para persistência
- display file `TRNDSP` para UI
- regras de validação e sequenciamento no próprio programa

## Justificativa

A implementação atual consolida a lógica, a UI e a persistência no mesmo fluxo procedural, o que é compatível com o padrão clássico de sistemas IBM i antigos e reduz complexidade operacional no projeto PoC.

## Consequências

- Vantagens:
  - baixa dependência de componentes externos
  - execução simples em ambiente IBM i
  - manutenção direta do código e das telas

- Desvantagens:
  - acoplamento alto entre UI, regras e persistência
  - baixa testabilidade automatizada
  - difícil evolução para arquitetura distribuída ou API

## Alternativas consideradas

- Separar regras em módulos ou procedures
- Migrar para camada de serviço e banco relacional
- Expor API REST para cadastro

Todas essas opções foram descartadas pela simplicidade do PoC e pela estrutura nativa do IBM i.
