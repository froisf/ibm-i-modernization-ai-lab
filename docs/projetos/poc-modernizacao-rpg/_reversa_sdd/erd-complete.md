# ERD Completo — `transportadora`

```mermaid
erDiagram
    TRNREC {
        string TRNCOD PK
        string TRNNOM
        string TRNFAN
        string TRNCNPJ
        string TRNIEP
        string TRNEND
        string TRNCID
        string TRNUF
        string TRNCEP
        string TRNFON
        string TRNEML
        string TRNSTS
        int TRNDTC
        int TRNDTA
    }
```

## Observações

- `TRNCOD` é a chave primária.
- `TRNSTS` representa o estado do registro (`A`/`I`).
- Não foram identificados relacionamentos com outras entidades.
