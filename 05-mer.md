# Clinexus — Modelo Entidade-Relacionamento (MER)

```mermaid
erDiagram
    USUARIO ||--o{ PROTOCOLO : "é responsável por"
    USUARIO ||--o{ EVENTO_ANIMAL : registra
    USUARIO ||--o{ SESSAO_TESTE : realiza
    USUARIO ||--o{ LOG_AUDITORIA : gera
    LINHAGEM ||--o{ ANIMAL : classifica
    ANIMAL ||--o{ ANIMAL_GRUPO : participa
    GRUPO_EXPERIMENTAL ||--o{ ANIMAL_GRUPO : agrupa
    PROTOCOLO ||--o{ GRUPO_EXPERIMENTAL : possui
    ANIMAL ||--o{ EVENTO_ANIMAL : tem
    SUBSTANCIA o|--o{ EVENTO_ANIMAL : "administrada em"
    ANIMAL ||--o{ SESSAO_TESTE : "é avaliado em"
    PROTOCOLO ||--o{ SESSAO_TESTE : contém
    TIPO_TESTE ||--o{ SESSAO_TESTE : define
    SESSAO_TESTE ||--o{ MEDIDA_SESSAO : registra
    SESSAO_TESTE ||--o{ ANEXO_SESSAO : possui

    USUARIO {
        int id_usuario PK
        string nome
        string email UK
        string senha_hash
        string perfil
        boolean ativo
        datetime criado_em
    }
    LINHAGEM {
        int id_linhagem PK
        string nome UK
        string especie
        string descricao
    }
    ANIMAL {
        int id_animal PK
        string identificacao UK
        int id_linhagem FK
        char sexo
        date data_nascimento
        string origem
        string status
        string observacoes
        datetime criado_em
    }
    PROTOCOLO {
        int id_protocolo PK
        string titulo
        string descricao
        string especie
        int qtd_animais_prevista
        string numero_ceua
        int id_responsavel FK
        date data_inicio
        date data_fim
        string status
    }
    GRUPO_EXPERIMENTAL {
        int id_grupo PK
        int id_protocolo FK
        string nome
        string tipo
        string descricao
    }
    ANIMAL_GRUPO {
        int id_animal_grupo PK
        int id_animal FK
        int id_grupo FK
        date data_entrada
        date data_saida
    }
    SUBSTANCIA {
        int id_substancia PK
        string nome UK
        string tipo
        string descricao
    }
    EVENTO_ANIMAL {
        int id_evento PK
        int id_animal FK
        int id_usuario FK
        string tipo_evento
        datetime data_hora
        string descricao
        int id_substancia FK
        decimal dose
        string unidade_dose
        string via_administracao
    }
    TIPO_TESTE {
        int id_tipo_teste PK
        string nome UK
        string descricao
    }
    SESSAO_TESTE {
        int id_sessao PK
        int id_animal FK
        int id_protocolo FK
        int id_tipo_teste FK
        int id_usuario FK
        datetime data_hora
        string observacoes
    }
    MEDIDA_SESSAO {
        int id_medida PK
        int id_sessao FK
        string parametro
        decimal valor
        string unidade
    }
    ANEXO_SESSAO {
        int id_anexo PK
        int id_sessao FK
        string nome_arquivo
        string caminho_arquivo
        string tipo_mime
        bigint tamanho_bytes
        datetime enviado_em
    }
    LOG_AUDITORIA {
        bigint id_log PK
        int id_usuario FK
        string entidade
        int id_registro
        string acao
        string dados_anteriores
        string dados_novos
        datetime data_hora
    }
```

## Decisões de modelagem
- **ANIMAL_GRUPO** resolve a relação N:N entre animais e grupos, guardando data de entrada e saída (histórico).
- **SESSAO_TESTE** liga animal, protocolo e tipo de teste; a regra de que o animal precisa estar em um grupo do protocolo é validada pela aplicação.
- **MEDIDA_SESSAO** guarda cada medida em linha própria, o que permite tipos de teste com parâmetros diferentes sem alterar o esquema.
- **EVENTO_ANIMAL** concentra procedimentos, tratamentos e observações de saúde; `id_substancia`, `dose`, `unidade_dose` e `via_administracao` só se aplicam ao tipo *TRATAMENTO*, no qual são obrigatórios.
- **LOG_AUDITORIA** é genérico (entidade + id do registro), dispensando uma tabela de log por entidade.
- Exclusões físicas são evitadas; usa-se `status`/`ativo` para preservar o histórico da pesquisa.
