# Fluxograma do módulo `transportadora`

> Gerado pelo Reversa em 2026-09-23
> Fonte principal: `QRPGLESRC/TRN001.rpgle`

```mermaid
flowchart TD
    A[Inicia TRN001] --> B[Abrir TRNPF e TRNDSP]
    B --> C[Definir modo = Lista]
    C --> D{wModo}

    D -->|L| E[CarregaLista]
    E --> E1[Limpar subfile]
    E1 --> E2[SetLL / Read registros]
    E2 --> E3[Escrever TRNSFL]
    E3 --> E4[ExFmt TRNCTL]
    E4 --> E5{Usuário escolheu registro?}
    E5 -->|Sim| F[wModo = Alterar]
    E5 -->|Não| G{Exit?}
    G -->|Sim| H[Encerrar]
    G -->|Não| E

    D -->|I| I[IncluirRegistro]
    I --> I1[Limpar TrnDsOut]
    I1 --> I2[ExFmt TRNDTL]
    I2 --> I3{Cancel?}
    I3 -->|Sim| C
    I3 -->|Não| I4[ValidaDados]
    I4 --> I5{Dados válidos?}
    I5 -->|Não| I2
    I5 -->|Sim| I6[GeraCodigo]
    I6 --> I7[Definir datas]
    I7 --> I8[Write TRNPF]
    I8 --> C

    D -->|A| A1[AlterarRegistro]
    A1 --> A2[Chain por TRNCOD]
    A2 --> A3[ExFmt TRNDTL]
    A3 --> A4{Cancel?}
    A4 -->|Sim| C
    A4 -->|Não| A5[ValidaDados]
    A5 --> A6{Dados válidos?}
    A6 -->|Não| A3
    A6 -->|Sim| A7[Atualizar data de alteração]
    A7 --> A8[Update TRNPF]
    A8 --> C

    D -->|E| E1R[ExcluirRegistro]
    E1R --> E2R[Chain por TRNCOD]
    E2R --> E3R[ExFmt TRNDTL]
    E3R --> E4R{Cancel?}
    E4R -->|Sim| C
    E4R -->|Não| E5R[Delete TRNPF]
    E5R --> C

    D -->|Erro| W[MostrarErro]
    W --> W1[ExFmt TRNDTL]
    W1 --> C
