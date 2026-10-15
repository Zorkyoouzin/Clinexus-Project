# Clinexus — Dicionário de Dados

Tipos de dados do **PostgreSQL** (`DECIMAL` equivale a `NUMERIC`; chaves primárias podem usar `GENERATED ALWAYS AS IDENTITY`). Campos com valores fixos (`perfil`, `status`, `tipo`, `tipo_evento`, `acao`) devem receber restrição `CHECK`. **PK** = chave primária, **FK** = chave estrangeira, **UK** = única.


## USUARIO
Usuários do sistema.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_usuario` | INTEGER | — | Não | PK | Identificador (autoincremento). |
| `nome` | VARCHAR | 120 | Não |  | Nome completo. |
| `email` | VARCHAR | 150 | Não | UK | E-mail usado no login. |
| `senha_hash` | VARCHAR | 255 | Não |  | Hash da senha. |
| `perfil` | VARCHAR | 20 | Não |  | RESPONSAVEL ou COLABORADOR. |
| `ativo` | BOOLEAN | — | Não |  | Indica se o usuário pode acessar (padrão: verdadeiro). |
| `criado_em` | TIMESTAMP | — | Não |  | Data/hora de criação. |

## LINHAGEM
Linhagens/genótipos dos animais.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_linhagem` | INTEGER | — | Não | PK | Identificador. |
| `nome` | VARCHAR | 100 | Não | UK | Nome da linhagem. |
| `especie` | VARCHAR | 80 | Não |  | Espécie (ex.: rato). |
| `descricao` | TEXT | — | Sim |  | Detalhes da linhagem/genótipo. |

## ANIMAL
Animais de laboratório.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_animal` | INTEGER | — | Não | PK | Identificador. |
| `identificacao` | VARCHAR | 50 | Não | UK | Código do animal (ex.: número da gaiola/marcação). |
| `id_linhagem` | INTEGER | — | Não | FK → LINHAGEM | Linhagem do animal. |
| `sexo` | CHAR | 1 | Não |  | M ou F. |
| `data_nascimento` | DATE | — | Sim |  | Data de nascimento. |
| `origem` | VARCHAR | 150 | Sim |  | Procedência (biotério, fornecedor). |
| `status` | VARCHAR | 20 | Não |  | ATIVO, FINALIZADO ou BAIXA. |
| `observacoes` | TEXT | — | Sim |  | Observações gerais. |
| `criado_em` | TIMESTAMP | — | Não |  | Data/hora de cadastro. |

## PROTOCOLO
Protocolos experimentais.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_protocolo` | INTEGER | — | Não | PK | Identificador. |
| `titulo` | VARCHAR | 200 | Não |  | Título do protocolo. |
| `descricao` | TEXT | — | Sim |  | Descrição do experimento. |
| `especie` | VARCHAR | 80 | Não |  | Espécie envolvida. |
| `qtd_animais_prevista` | INTEGER | — | Sim |  | Nº de animais previsto. |
| `numero_ceua` | VARCHAR | 50 | Sim |  | Referência à aprovação ética (apenas informativo). |
| `id_responsavel` | INTEGER | — | Não | FK → USUARIO | Pesquisador responsável. |
| `data_inicio` | DATE | — | Sim |  | Início do protocolo. |
| `data_fim` | DATE | — | Sim |  | Fim previsto/real. |
| `status` | VARCHAR | 20 | Não |  | EM_ANDAMENTO, CONCLUIDO ou CANCELADO. |

## GRUPO_EXPERIMENTAL
Grupos de animais dentro de um protocolo.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_grupo` | INTEGER | — | Não | PK | Identificador. |
| `id_protocolo` | INTEGER | — | Não | FK → PROTOCOLO | Protocolo ao qual o grupo pertence. |
| `nome` | VARCHAR | 100 | Não | UK (com id_protocolo) | Nome do grupo. |
| `tipo` | VARCHAR | 20 | Não |  | CONTROLE, TRATADO ou OUTRO. |
| `descricao` | TEXT | — | Sim |  | Descrição do grupo. |

## ANIMAL_GRUPO
Associação entre animais e grupos, com histórico.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_animal_grupo` | INTEGER | — | Não | PK | Identificador. |
| `id_animal` | INTEGER | — | Não | FK → ANIMAL | Animal vinculado. |
| `id_grupo` | INTEGER | — | Não | FK → GRUPO_EXPERIMENTAL | Grupo do vínculo. |
| `data_entrada` | DATE | — | Não |  | Data de entrada no grupo. |
| `data_saida` | DATE | — | Sim |  | Data de saída (nula se ainda no grupo). |

## SUBSTANCIA
Substâncias administradas (peptídeos, fármacos).

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_substancia` | INTEGER | — | Não | PK | Identificador. |
| `nome` | VARCHAR | 120 | Não | UK | Nome da substância. |
| `tipo` | VARCHAR | 50 | Sim |  | Categoria (ex.: peptídeo, fármaco, veículo). |
| `descricao` | TEXT | — | Sim |  | Detalhes. |

## EVENTO_ANIMAL
Histórico de procedimentos, tratamentos e saúde do animal.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_evento` | INTEGER | — | Não | PK | Identificador. |
| `id_animal` | INTEGER | — | Não | FK → ANIMAL | Animal do evento. |
| `id_usuario` | INTEGER | — | Não | FK → USUARIO | Quem registrou. |
| `tipo_evento` | VARCHAR | 20 | Não |  | PROCEDIMENTO, TRATAMENTO ou SAUDE. |
| `data_hora` | TIMESTAMP | — | Não |  | Quando o evento ocorreu. |
| `descricao` | TEXT | — | Não |  | Descrição do evento. |
| `id_substancia` | INTEGER | — | Sim | FK → SUBSTANCIA | Obrigatório se tipo_evento = TRATAMENTO. |
| `dose` | DECIMAL | 10,3 | Sim |  | Dose administrada (obrigatória se tipo_evento = TRATAMENTO). |
| `unidade_dose` | VARCHAR | 20 | Sim |  | Unidade da dose, ex.: mg/kg (obrigatória se tipo_evento = TRATAMENTO). |
| `via_administracao` | VARCHAR | 50 | Sim |  | Via de administração (obrigatória se tipo_evento = TRATAMENTO). |

## TIPO_TESTE
Tipos de teste comportamental.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_tipo_teste` | INTEGER | — | Não | PK | Identificador. |
| `nome` | VARCHAR | 100 | Não | UK | Nome do teste. |
| `descricao` | TEXT | — | Sim |  | Descrição do teste. |

## SESSAO_TESTE
Sessões de teste comportamental.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_sessao` | INTEGER | — | Não | PK | Identificador. |
| `id_animal` | INTEGER | — | Não | FK → ANIMAL | Animal avaliado. |
| `id_protocolo` | INTEGER | — | Não | FK → PROTOCOLO | Protocolo da sessão. |
| `id_tipo_teste` | INTEGER | — | Não | FK → TIPO_TESTE | Tipo de teste aplicado. |
| `id_usuario` | INTEGER | — | Não | FK → USUARIO | Quem realizou/registrou. |
| `data_hora` | TIMESTAMP | — | Não |  | Data/hora da sessão. |
| `observacoes` | TEXT | — | Sim |  | Observações livres. |

## MEDIDA_SESSAO
Medidas observadas em cada sessão.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_medida` | INTEGER | — | Não | PK | Identificador. |
| `id_sessao` | INTEGER | — | Não | FK → SESSAO_TESTE | Sessão da medida. |
| `parametro` | VARCHAR | 100 | Não |  | Parâmetro observado (ex.: tempo de congelamento). |
| `valor` | DECIMAL | 14,4 | Não |  | Valor medido. |
| `unidade` | VARCHAR | 30 | Sim |  | Unidade (ex.: s, cm). |

## ANEXO_SESSAO
Arquivos anexados a sessões.

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_anexo` | INTEGER | — | Não | PK | Identificador. |
| `id_sessao` | INTEGER | — | Não | FK → SESSAO_TESTE | Sessão do anexo. |
| `nome_arquivo` | VARCHAR | 200 | Não |  | Nome original do arquivo. |
| `caminho_arquivo` | VARCHAR | 500 | Não |  | Local de armazenamento. |
| `tipo_mime` | VARCHAR | 100 | Sim |  | Tipo do arquivo. |
| `tamanho_bytes` | BIGINT | — | Sim |  | Tamanho em bytes. |
| `enviado_em` | TIMESTAMP | — | Não |  | Data/hora do envio. |

## LOG_AUDITORIA
Registro de alterações (somente leitura).

| Campo | Tipo | Tamanho | Nulo? | Chave | Descrição |
|-------|------|---------|-------|-------|-----------|
| `id_log` | BIGINT | — | Não | PK | Identificador. |
| `id_usuario` | INTEGER | — | Não | FK → USUARIO | Quem realizou a ação. |
| `entidade` | VARCHAR | 60 | Não |  | Nome da tabela afetada. |
| `id_registro` | INTEGER | — | Não |  | Id do registro afetado. |
| `acao` | VARCHAR | 10 | Não |  | INSERT ou UPDATE (inativações e mudanças de status são UPDATE). |
| `dados_anteriores` | JSONB | — | Sim |  | Valores antes da alteração (JSON). |
| `dados_novos` | JSONB | — | Sim |  | Valores depois da alteração (JSON). |
| `data_hora` | TIMESTAMP | — | Não |  | Quando ocorreu. |
