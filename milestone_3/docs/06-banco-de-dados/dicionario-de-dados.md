# Dicionário de dados

Contexto: Fábrica de Software I · projeto FilaCidadã · modelagem relacional (PostgreSQL 17 + Prisma 6).

Fonte: `prisma/schema.prisma`, conferido coluna a coluna com a migração `prisma/migrations/20260805220051_init_uuid/migration.sql`. O DDL equivalente está em [schema.sql](schema.sql). Para a decisão de modelagem, veja o [README](README.md).

**Legenda**

- **Nulo**: se a coluna aceita `NULL`.
- **Default**: valor de `DEFAULT` no banco. `—` = sem default no banco. Em `id`, o valor vem de `@default(uuid())` e é gerado pelo Prisma no cliente. Em `updatedAt`, o Prisma (`@updatedAt`) preenche o valor a cada escrita.
- **Chave**: `PK` (chave primária), `UK` (único, com a lista de colunas quando composto), `FK → tabela · ação` (chave estrangeira com a regra de `ON DELETE`).
- **Origem**: classe e atributo do modelo de domínio de onde a coluna vem. Entre parênteses, `associação` indica coluna de chave estrangeira; `BetterAuth` indica a tabela exigida pelo BetterAuth.

## Tabelas de autenticação (BetterAuth)

### users

Tabela de `Usuario`. Contém os campos do contrato do BetterAuth e os campos do domínio FilaCidadã (`pais`, `ativo`, `fusoHorario`, `ultimoLoginEm`, `instituicaoId`).

| Coluna        | Tipo         | Nulo | Default           | Chave                        | Descrição                         | Origem                           |
| ------------- | ------------ | ---- | ----------------- | ---------------------------- | --------------------------------- | -------------------------------- |
| id            | UUID         | Não  | —                 | PK                           | Identificador do usuário          | Usuario.id                       |
| name          | TEXT         | Não  | —                 |                              | Nome do usuário                   | Usuario.name                     |
| email         | TEXT         | Não  | —                 | UK                           | E-mail de login, único            | Usuario.email                    |
| emailVerified | BOOLEAN      | Não  | false             |                              | Indica se o e-mail foi verificado | Usuario.emailVerified            |
| image         | TEXT         | Sim  | —                 |                              | Imagem de perfil                  | Usuario.image                    |
| createdAt     | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                              | Data de criação                   | Usuario.createdAt                |
| updatedAt     | TIMESTAMP(3) | Não  | —                 |                              | Data da última alteração (Prisma) | Usuario.updatedAt                |
| pais          | TEXT         | Não  | 'BR'              |                              | Código do país                    | Usuario.pais                     |
| ativo         | BOOLEAN      | Não  | true              |                              | Conta ativa; desativação lógica   | Usuario.ativo                    |
| fusoHorario   | VARCHAR(64)  | Não  | 'America/Manaus'  |                              | Fuso horário (identificador IANA) | Usuario.fusoHorario              |
| ultimoLoginEm | TIMESTAMP(3) | Sim  | —                 |                              | Momento do último login           | Usuario.ultimoLoginEm            |
| instituicaoId | UUID         | Sim  | —                 | FK → instituicoes · SET NULL | Instituição do usuário            | Usuario.instituicao (associação) |

Índices: `UK (email)`; `(instituicaoId)`; `(ativo)`.

### sessions

| Coluna    | Tipo         | Nulo | Default           | Chave                | Descrição                         | Origem                        |
| --------- | ------------ | ---- | ----------------- | -------------------- | --------------------------------- | ----------------------------- |
| id        | UUID         | Não  | —                 | PK                   | Identificador da sessão           | Sessao.id (BetterAuth)        |
| token     | TEXT         | Não  | —                 | UK                   | Token da sessão, único            | Sessao.token (BetterAuth)     |
| expiresAt | TIMESTAMP(3) | Não  | —                 |                      | Data de expiração da sessão       | Sessao.expiresAt (BetterAuth) |
| ipAddress | TEXT         | Sim  | —                 |                      | Endereço IP do cliente            | Sessao.ipAddress (BetterAuth) |
| userAgent | TEXT         | Sim  | —                 |                      | User-Agent do cliente             | Sessao.userAgent (BetterAuth) |
| createdAt | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                      | Data de criação                   | Sessao.createdAt (BetterAuth) |
| updatedAt | TIMESTAMP(3) | Não  | —                 |                      | Data da última alteração (Prisma) | Sessao.updatedAt (BetterAuth) |
| userId    | UUID         | Não  | —                 | FK → users · CASCADE | Usuário dono da sessão            | Sessao.user (associação)      |

Índices: `UK (token)`; `(userId)`; `(expiresAt)`.

### accounts

| Coluna                | Tipo         | Nulo | Default           | Chave                      | Descrição                                          | Origem                                   |
| --------------------- | ------------ | ---- | ----------------- | -------------------------- | -------------------------------------------------- | ---------------------------------------- |
| id                    | UUID         | Não  | —                 | PK                         | Identificador da conta                             | Conta.id (BetterAuth)                    |
| accountId             | TEXT         | Não  | —                 | UK (providerId, accountId) | Identificador da conta no provedor                 | Conta.accountId (BetterAuth)             |
| providerId            | TEXT         | Não  | —                 | UK (providerId, accountId) | Provedor de autenticação (ex.: credential, google) | Conta.providerId (BetterAuth)            |
| accessToken           | TEXT         | Sim  | —                 |                            | Token de acesso do provedor OAuth                  | Conta.accessToken (BetterAuth)           |
| refreshToken          | TEXT         | Sim  | —                 |                            | Token de renovação do provedor OAuth               | Conta.refreshToken (BetterAuth)          |
| accessTokenExpiresAt  | TIMESTAMP(3) | Sim  | —                 |                            | Expiração do token de acesso                       | Conta.accessTokenExpiresAt (BetterAuth)  |
| refreshTokenExpiresAt | TIMESTAMP(3) | Sim  | —                 |                            | Expiração do token de renovação                    | Conta.refreshTokenExpiresAt (BetterAuth) |
| scope                 | TEXT         | Sim  | —                 |                            | Escopos OAuth concedidos                           | Conta.scope (BetterAuth)                 |
| idToken               | TEXT         | Sim  | —                 |                            | Token de identidade do provedor                    | Conta.idToken (BetterAuth)               |
| password              | TEXT         | Sim  | —                 |                            | Hash da senha (usado no provedor credential)       | Conta.password (BetterAuth)              |
| createdAt             | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                            | Data de criação                                    | Conta.createdAt (BetterAuth)             |
| updatedAt             | TIMESTAMP(3) | Não  | —                 |                            | Data da última alteração (Prisma)                  | Conta.updatedAt (BetterAuth)             |
| userId                | UUID         | Não  | —                 | FK → users · CASCADE       | Usuário dono da conta                              | Conta.user (associação)                  |

Índices: `UK (providerId, accountId)`; `(userId)`.

### verifications

| Coluna     | Tipo         | Nulo | Default           | Chave | Descrição                             | Origem                              |
| ---------- | ------------ | ---- | ----------------- | ----- | ------------------------------------- | ----------------------------------- |
| id         | UUID         | Não  | —                 | PK    | Identificador do token                | Verificacao.id (BetterAuth)         |
| identifier | TEXT         | Não  | —                 |       | Chave de busca do token (ex.: e-mail) | Verificacao.identifier (BetterAuth) |
| value      | TEXT         | Não  | —                 |       | Valor do token temporário             | Verificacao.value (BetterAuth)      |
| expiresAt  | TIMESTAMP(3) | Não  | —                 |       | Expiração do token                    | Verificacao.expiresAt (BetterAuth)  |
| createdAt  | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |       | Data de criação                       | Verificacao.createdAt (BetterAuth)  |
| updatedAt  | TIMESTAMP(3) | Não  | —                 |       | Data da última alteração (Prisma)     | Verificacao.updatedAt (BetterAuth)  |

Índices: `(identifier)`; `(expiresAt)`.

## Autorização

### usuario_papeis

Associação entre `Usuario` e `Papel`. Chave primária composta, que impede o mesmo papel repetido para o mesmo usuário.

| Coluna    | Tipo         | Nulo | Default | Chave                                        | Descrição                  | Origem                      |
| --------- | ------------ | ---- | ------- | -------------------------------------------- | -------------------------- | --------------------------- |
| usuarioId | UUID         | Não  | —       | PK (usuarioId, papel) · FK → users · CASCADE | Usuário que recebe o papel | Usuario.papeis (associação) |
| papel     | Papel (enum) | Não  | —       | PK (usuarioId, papel)                        | Papel do usuário           | Papel                       |

Índices: `PK (usuarioId, papel)`; `(papel)`.

## Domínio

### instituicoes

| Coluna         | Tipo                     | Nulo | Default           | Chave                 | Descrição                                                | Origem                               |
| -------------- | ------------------------ | ---- | ----------------- | --------------------- | -------------------------------------------------------- | ------------------------------------ |
| id             | UUID                     | Não  | —                 | PK                    | Identificador da instituição                             | Instituicao.id                       |
| nome           | VARCHAR(180)             | Não  | —                 | UK                    | Nome da instituição, único                               | Instituicao.nome                     |
| ativo          | BOOLEAN                  | Não  | true              |                       | Instituição ativa; desativação lógica                    | Instituicao.ativo                    |
| status         | StatusInstituicao (enum) | Não  | 'PENDENTE'        |                       | Situação da solicitação: PENDENTE, APROVADA ou REJEITADA | Instituicao.status                   |
| cnpj           | VARCHAR(18)              | Sim  | —                 |                       | CNPJ informado na solicitação                            | Instituicao.cnpj                     |
| email          | VARCHAR(180)             | Sim  | —                 |                       | E-mail de contato                                        | Instituicao.email                    |
| telefone       | VARCHAR(32)              | Sim  | —                 |                       | Telefone de contato                                      | Instituicao.telefone                 |
| responsavel    | VARCHAR(180)             | Sim  | —                 |                       | Nome do responsável                                      | Instituicao.responsavel              |
| endereco       | TEXT                     | Sim  | —                 |                       | Endereço                                                 | Instituicao.endereco                 |
| descricao      | TEXT                     | Sim  | —                 |                       | Descrição da instituição                                 | Instituicao.descricao                |
| contratoUrl    | TEXT                     | Sim  | —                 |                       | Endereço do contrato enviado                             | Instituicao.contratoUrl              |
| motivoRejeicao | TEXT                     | Sim  | —                 |                       | Motivo informado na rejeição                             | Instituicao.motivoRejeicao           |
| aprovadoEm     | TIMESTAMP(3)             | Sim  | —                 |                       | Momento da aprovação                                     | Instituicao.aprovadoEm               |
| solicitanteId  | UUID                     | Sim  | —                 | FK → users · SET NULL | Usuário que fez a solicitação                            | Instituicao.solicitante (associação) |
| aprovadoPorId  | UUID                     | Sim  | —                 | FK → users · SET NULL | Usuário que aprovou ou rejeitou                          | Instituicao.aprovadoPor (associação) |
| configuracoes  | JSONB                    | Sim  | —                 |                       | Configuração livre, lida inteira                         | Instituicao.configuracoes            |
| createdAt      | TIMESTAMP(3)             | Não  | CURRENT_TIMESTAMP |                       | Data de criação                                          | Instituicao.createdAt                |
| updatedAt      | TIMESTAMP(3)             | Não  | —                 |                       | Data da última alteração (Prisma)                        | Instituicao.updatedAt                |

Índices: `UK (nome)`; `(status)`; `(ativo)`.

### tipos_etapa

| Coluna        | Tipo         | Nulo | Default | Chave                                                    | Descrição                             | Origem                             |
| ------------- | ------------ | ---- | ------- | -------------------------------------------------------- | ------------------------------------- | ---------------------------------- |
| id            | UUID         | Não  | —       | PK                                                       | Identificador do tipo de etapa        | TipoEtapa.id                       |
| codigo        | VARCHAR(64)  | Não  | —       | UK (instituicaoId, codigo)                               | Código do tipo, único por instituição | TipoEtapa.codigo                   |
| nome          | VARCHAR(120) | Não  | —       |                                                          | Nome exibido                          | TipoEtapa.nome                     |
| cor           | VARCHAR(32)  | Não  | 'gray'  |                                                          | Cor de exibição                       | TipoEtapa.cor                      |
| ativo         | BOOLEAN      | Não  | true    |                                                          | Tipo ativo; desativação lógica        | TipoEtapa.ativo                    |
| instituicaoId | UUID         | Não  | —       | UK (instituicaoId, codigo) · FK → instituicoes · CASCADE | Instituição dona do tipo              | TipoEtapa.instituicao (associação) |

Índices: `UK (instituicaoId, codigo)`. Não possui `createdAt` nem `updatedAt`.

### filas

| Coluna                 | Tipo                   | Nulo | Default           | Chave                       | Descrição                                                                      | Origem                        |
| ---------------------- | ---------------------- | ---- | ----------------- | --------------------------- | ------------------------------------------------------------------------------ | ----------------------------- |
| id                     | UUID                   | Não  | —                 | PK                          | Identificador da fila                                                          | Fila.id                       |
| nome                   | VARCHAR(180)           | Não  | —                 |                             | Nome da fila                                                                   | Fila.nome                     |
| tipoAtendimento        | TipoAtendimento (enum) | Não  | —                 |                             | ONLINE, PRESENCIAL ou HIBRIDO                                                  | Fila.tipoAtendimento          |
| ativa                  | BOOLEAN                | Não  | true              |                             | Fila ativa; desativação lógica                                                 | Fila.ativa                    |
| prioridadesHabilitadas | BOOLEAN                | Não  | false             |                             | Aceita senhas com prioridade                                                   | Fila.prioridadesHabilitadas   |
| fidelidadeHabilitada   | BOOLEAN                | Não  | false             |                             | Aceita prioridade de fidelidade                                                | Fila.fidelidadeHabilitada     |
| tempoMaximoAtendimento | INTEGER                | Sim  | —                 |                             | Tempo máximo de atendimento (inteiro positivo; unidade não definida no schema) | Fila.tempoMaximoAtendimento   |
| modoQrCode             | ModoQrCode (enum)      | Não  | 'ROTATIVO'        |                             | Modo do QR Code: ROTATIVO ou FIXO                                              | Fila.modoQrCode               |
| tempoExibicaoMin       | INTEGER                | Sim  | —                 |                             | Minutos de exibição do QR Code (validado entre 1 e 60)                         | Fila.tempoExibicaoMin         |
| tempoExpiracaoMin      | INTEGER                | Sim  | —                 |                             | Minutos até a expiração do QR Code; nulo = calculado pelo serviço              | Fila.tempoExpiracaoMin        |
| toleranciaMin          | INTEGER                | Sim  | —                 |                             | Minutos de tolerância após a validade; nulo = calculado pelo serviço           | Fila.toleranciaMin            |
| tempoAlertaSegundos    | INTEGER                | Sim  | —                 |                             | Tempo de alerta, em segundos (validado entre 5 e 120; padrão 30 no Zod)        | Fila.tempoAlertaSegundos      |
| instituicaoId          | UUID                   | Não  | —                 | FK → instituicoes · CASCADE | Instituição dona da fila                                                       | Fila.instituicao (associação) |
| createdAt              | TIMESTAMP(3)           | Não  | CURRENT_TIMESTAMP |                             | Data de criação                                                                | Fila.createdAt                |
| updatedAt              | TIMESTAMP(3)           | Não  | —                 |                             | Data da última alteração (Prisma)                                              | Fila.updatedAt                |

Índices: `(instituicaoId)`; `(instituicaoId, ativa)`.

### mesas

| Coluna | Tipo         | Nulo | Default | Chave                                      | Descrição                      | Origem                 |
| ------ | ------------ | ---- | ------- | ------------------------------------------ | ------------------------------ | ---------------------- |
| id     | UUID         | Não  | —       | PK                                         | Identificador da mesa          | Mesa.id                |
| numero | VARCHAR(32)  | Não  | —       | UK (filaId, numero)                        | Número da mesa, único por fila | Mesa.numero            |
| nome   | VARCHAR(120) | Sim  | —       |                                            | Nome descritivo da mesa        | Mesa.nome              |
| ativa  | BOOLEAN      | Não  | true    |                                            | Mesa ativa                     | Mesa.ativa             |
| filaId | UUID         | Não  | —       | UK (filaId, numero) · FK → filas · CASCADE | Fila da mesa                   | Mesa.fila (associação) |

Índices: `UK (filaId, numero)`. Não possui `createdAt` nem `updatedAt`.

### senhas

| Coluna          | Tipo                   | Nulo | Default           | Chave                       | Descrição                                                                       | Origem                           |
| --------------- | ---------------------- | ---- | ----------------- | --------------------------- | ------------------------------------------------------------------------------- | -------------------------------- |
| id              | UUID                   | Não  | —                 | PK                          | Identificador da senha                                                          | Senha.id                         |
| numero          | INTEGER                | Sim  | —                 |                             | Número sequencial da senha                                                      | Senha.numero                     |
| posicao         | INTEGER                | Não  | —                 |                             | Posição na fila                                                                 | Senha.posicao                    |
| status          | StatusSenha (enum)     | Não  | —                 |                             | AGUARDANDO, EM_ATENDIMENTO, CANCELADA ou FINALIZADA                             | Senha.status                     |
| prioridade      | PrioridadeSenha (enum) | Sim  | —                 |                             | LEGAL, FIDELIDADE ou NORMAL; nulo quando não se aplica                          | Senha.prioridade                 |
| prioridadePeso  | INTEGER                | Não  | 3                 |                             | Peso de ordenação: o menor é chamado primeiro (LEGAL 1, FIDELIDADE 2, demais 3) | Senha.prioridadePeso             |
| nomeCidadao     | VARCHAR(180)           | Sim  | —                 |                             | Nome do cidadão no atendimento presencial sem conta                             | Senha.nomeCidadao                |
| presencial      | BOOLEAN                | Não  | false             |                             | Senha emitida presencialmente                                                   | Senha.presencial                 |
| mesa            | VARCHAR(32)            | Sim  | —                 |                             | Identificação da mesa que atende (texto, sem FK)                                | Senha.mesa                       |
| mesaNome        | VARCHAR(120)           | Sim  | —                 |                             | Snapshot do nome da mesa                                                        | Senha.mesaNome                   |
| tokenPublico    | VARCHAR(64)            | Sim  | —                 | UK                          | Token público de acompanhamento da senha                                        | Senha.tokenPublico               |
| filaId          | UUID                   | Não  | —                 | FK → filas · CASCADE        | Fila da senha                                                                   | Senha.fila (associação)          |
| instituicaoId   | UUID                   | Não  | —                 | FK → instituicoes · CASCADE | Instituição da senha                                                            | Senha.instituicao (associação)   |
| usuarioId       | UUID                   | Sim  | —                 | FK → users · SET NULL       | Cidadão dono da senha                                                           | Senha.usuario (associação)       |
| operadorId      | UUID                   | Sim  | —                 | FK → users · SET NULL       | Operador que atende a senha                                                     | Senha.operador (associação)      |
| createdAt       | TIMESTAMP(3)           | Não  | CURRENT_TIMESTAMP |                             | Data de criação                                                                 | Senha.createdAt                  |
| updatedAt       | TIMESTAMP(3)           | Não  | —                 |                             | Data da última alteração (Prisma)                                               | Senha.updatedAt                  |
| flowId          | UUID                   | Sim  | —                 | FK → flows · SET NULL       | Flow de atendimento, se houver                                                  | Senha.flow (associação)          |
| flowStepAtualId | UUID                   | Sim  | —                 | FK → flow_steps · SET NULL  | Step atual do flow                                                              | Senha.flowStepAtual (associação) |

Índices: `UK (tokenPublico)`; `(filaId, status, prioridadePeso, posicao)`; `(usuarioId, filaId, status)`; `(instituicaoId, numero)`; `(operadorId)`; `(flowId)`.

### qr_codes

| Coluna        | Tipo         | Nulo | Default           | Chave                | Descrição                          | Origem                   |
| ------------- | ------------ | ---- | ----------------- | -------------------- | ---------------------------------- | ------------------------ |
| id            | UUID         | Não  | —                 | PK                   | Identificador do QR Code           | QrCode.id                |
| codigo        | VARCHAR(64)  | Não  | —                 | UK                   | Código do QR Code, único           | QrCode.codigo            |
| validoAte     | TIMESTAMP(3) | Não  | —                 |                      | Fim da validade                    | QrCode.validoAte         |
| toleranciaAte | TIMESTAMP(3) | Não  | —                 |                      | Fim da tolerância, após a validade | QrCode.toleranciaAte     |
| ativo         | BOOLEAN      | Não  | true              |                      | QR Code ativo                      | QrCode.ativo             |
| filaId        | UUID         | Não  | —                 | FK → filas · CASCADE | Fila do QR Code                    | QrCode.fila (associação) |
| createdAt     | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                      | Data de criação                    | QrCode.createdAt         |
| updatedAt     | TIMESTAMP(3) | Não  | —                 |                      | Data da última alteração (Prisma)  | QrCode.updatedAt         |

Índices: `UK (codigo)`; `(filaId, ativo)`; `(validoAte)`.

### auditorias

Trilha append-only: a tabela só recebe inserções. Não tem `updatedAt`.

| Coluna        | Tipo         | Nulo | Default           | Chave                        | Descrição                                                             | Origem                             |
| ------------- | ------------ | ---- | ----------------- | ---------------------------- | --------------------------------------------------------------------- | ---------------------------------- |
| id            | UUID         | Não  | —                 | PK                           | Identificador do registro de auditoria                                | Auditoria.id                       |
| acao          | VARCHAR(64)  | Não  | —                 |                              | Ação realizada                                                        | Auditoria.acao                     |
| entidade      | VARCHAR(64)  | Não  | —                 |                              | Nome da entidade afetada                                              | Auditoria.entidade                 |
| entidadeId    | UUID         | Sim  | —                 |                              | Id do registro afetado; sem FK, pois o registro pode não existir mais | Auditoria.entidadeId               |
| dados         | JSONB        | Sim  | —                 |                              | Detalhes da ação                                                      | Auditoria.dados                    |
| createdAt     | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                              | Momento da ação                                                       | Auditoria.createdAt                |
| instituicaoId | UUID         | Sim  | —                 | FK → instituicoes · SET NULL | Instituição relacionada                                               | Auditoria.instituicao (associação) |
| usuarioId     | UUID         | Sim  | —                 | FK → users · SET NULL        | Usuário que realizou a ação                                           | Auditoria.usuario (associação)     |

Índices: `(instituicaoId, createdAt)`; `(entidade, entidadeId)`; `(usuarioId)`.

### landing_pages

| Coluna    | Tipo         | Nulo | Default           | Chave | Descrição                                   | Origem                |
| --------- | ------------ | ---- | ----------------- | ----- | ------------------------------------------- | --------------------- |
| id        | UUID         | Não  | —                 | PK    | Identificador do documento                  | LandingPage.id        |
| chave     | VARCHAR(64)  | Não  | —                 | UK    | Chave da configuração; o padrão é 'default' | LandingPage.chave     |
| conteudo  | JSONB        | Não  | —                 |       | Documento da landing page, lido inteiro     | LandingPage.conteudo  |
| createdAt | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |       | Data de criação                             | LandingPage.createdAt |
| updatedAt | TIMESTAMP(3) | Não  | —                 |       | Data da última alteração (Prisma)           | LandingPage.updatedAt |

Índices: `UK (chave)`.

## Comunicação

### notificacoes

Persistidas e não excluídas pela aplicação. A tabela é a fonte de verdade; WebSocket e push são efeitos colaterais.

| Coluna        | Tipo                   | Nulo | Default           | Chave                        | Descrição                         | Origem                               |
| ------------- | ---------------------- | ---- | ----------------- | ---------------------------- | --------------------------------- | ------------------------------------ |
| id            | UUID                   | Não  | —                 | PK                           | Identificador da notificação      | Notificacao.id                       |
| tipo          | NotificacaoTipo (enum) | Não  | —                 |                              | Tipo da notificação (16 valores)  | Notificacao.tipo                     |
| titulo        | VARCHAR(180)           | Não  | —                 |                              | Título                            | Notificacao.titulo                   |
| mensagem      | TEXT                   | Não  | —                 |                              | Texto da mensagem                 | Notificacao.mensagem                 |
| dados         | JSONB                  | Sim  | —                 |                              | Dados auxiliares da notificação   | Notificacao.dados                    |
| lida          | BOOLEAN                | Não  | false             |                              | Marcada como lida pelo usuário    | Notificacao.lida                     |
| usuarioId     | UUID                   | Não  | —                 | FK → users · CASCADE         | Destinatário                      | Notificacao.usuario (associação)     |
| instituicaoId | UUID                   | Sim  | —                 | FK → instituicoes · SET NULL | Instituição relacionada           | Notificacao.instituicao (associação) |
| createdAt     | TIMESTAMP(3)           | Não  | CURRENT_TIMESTAMP |                              | Data de criação                   | Notificacao.createdAt                |
| updatedAt     | TIMESTAMP(3)           | Não  | —                 |                              | Data da última alteração (Prisma) | Notificacao.updatedAt                |

Índices: `(usuarioId, lida)`; `(usuarioId, createdAt DESC)`.

### chat_mensagens

| Coluna    | Tipo         | Nulo | Default           | Chave                 | Descrição                          | Origem                            |
| --------- | ------------ | ---- | ----------------- | --------------------- | ---------------------------------- | --------------------------------- |
| id        | UUID         | Não  | —                 | PK                    | Identificador da mensagem          | ChatMensagem.id                   |
| texto     | TEXT         | Não  | —                 |                       | Texto da mensagem                  | ChatMensagem.texto                |
| nome      | VARCHAR(180) | Não  | —                 |                       | Snapshot do nome do autor no envio | ChatMensagem.nome                 |
| senhaId   | UUID         | Não  | —                 | FK → senhas · CASCADE | Senha do atendimento               | ChatMensagem.senha (associação)   |
| usuarioId | UUID         | Não  | —                 | FK → users · CASCADE  | Autor da mensagem                  | ChatMensagem.usuario (associação) |
| createdAt | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                       | Momento do envio                   | ChatMensagem.createdAt            |

Índices: `(senhaId, createdAt)`. Não possui `updatedAt`.

## Flow de atendimento

### flows

| Coluna        | Tipo         | Nulo | Default           | Chave                       | Descrição                         | Origem                        |
| ------------- | ------------ | ---- | ----------------- | --------------------------- | --------------------------------- | ----------------------------- |
| id            | UUID         | Não  | —                 | PK                          | Identificador do flow             | Flow.id                       |
| nome          | VARCHAR(180) | Não  | —                 |                             | Nome do flow                      | Flow.nome                     |
| descricao     | TEXT         | Sim  | —                 |                             | Descrição                         | Flow.descricao                |
| ativo         | BOOLEAN      | Não  | true              |                             | Flow ativo; desativação lógica    | Flow.ativo                    |
| instituicaoId | UUID         | Não  | —                 | FK → instituicoes · CASCADE | Instituição dona do flow          | Flow.instituicao (associação) |
| createdAt     | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                             | Data de criação                   | Flow.createdAt                |
| updatedAt     | TIMESTAMP(3) | Não  | —                 |                             | Data da última alteração (Prisma) | Flow.updatedAt                |

Índices: `(instituicaoId, ativo)`.

### flow_steps

| Coluna            | Tipo         | Nulo | Default | Chave                                     | Descrição                                                | Origem                     |
| ----------------- | ------------ | ---- | ------- | ----------------------------------------- | -------------------------------------------------------- | -------------------------- |
| id                | UUID         | Não  | —       | PK                                        | Identificador do step                                    | FlowStep.id                |
| nome              | VARCHAR(180) | Não  | —       |                                           | Nome do step                                             | FlowStep.nome              |
| ordem             | INTEGER      | Não  | —       | UK (flowId, ordem)                        | Ordem do step dentro do flow                             | FlowStep.ordem             |
| tipo              | VARCHAR(64)  | Não  | —       |                                           | Tipo livre da etapa (ex.: TRIAGEM, ATENDIMENTO)          | FlowStep.tipo              |
| tempoMedioMinutos | INTEGER      | Sim  | —       |                                           | Tempo médio de permanência, em minutos                   | FlowStep.tempoMedioMinutos |
| permiteBifurcacao | BOOLEAN      | Não  | false   |                                           | Permite mais de uma transição de saída                   | FlowStep.permiteBifurcacao |
| ativo             | BOOLEAN      | Não  | true    |                                           | Step ativo; desativado em vez de apagado quando já usado | FlowStep.ativo             |
| flowId            | UUID         | Não  | —       | UK (flowId, ordem) · FK → flows · CASCADE | Flow dono do step                                        | FlowStep.flow (associação) |
| filaId            | UUID         | Não  | —       | FK → filas · RESTRICT                     | Fila vinculada ao step                                   | FlowStep.fila (associação) |

Índices: `UK (flowId, ordem)`; `(filaId)`. Não possui `createdAt` nem `updatedAt`.

### flow_transitions

| Coluna     | Tipo                     | Nulo | Default      | Chave                                                 | Descrição                       | Origem                               |
| ---------- | ------------------------ | ---- | ------------ | ----------------------------------------------------- | ------------------------------- | ------------------------------------ |
| id         | UUID                     | Não  | —            | PK                                                    | Identificador da transição      | FlowTransition.id                    |
| tipo       | FlowTransicaoTipo (enum) | Não  | 'SEQUENCIAL' |                                                       | SEQUENCIAL, CONDICIONAL ou ALTA | FlowTransition.tipo                  |
| label      | VARCHAR(120)             | Sim  | —            |                                                       | Rótulo da transição             | FlowTransition.label                 |
| flowId     | UUID                     | Não  | —            | FK → flows · CASCADE                                  | Flow dono da transição          | FlowTransition.flow (associação)     |
| fromStepId | UUID                     | Não  | —            | UK (fromStepId, toStepId) · FK → flow_steps · CASCADE | Step de origem                  | FlowTransition.fromStep (associação) |
| toStepId   | UUID                     | Não  | —            | UK (fromStepId, toStepId) · FK → flow_steps · CASCADE | Step de destino                 | FlowTransition.toStep (associação)   |

Índices: `UK (fromStepId, toStepId)`; `(flowId)`. Não possui `createdAt` nem `updatedAt`.

### senha_flow_trackings

| Coluna      | Tipo                   | Nulo | Default           | Chave                      | Descrição                            | Origem                                   |
| ----------- | ---------------------- | ---- | ----------------- | -------------------------- | ------------------------------------ | ---------------------------------------- |
| id          | UUID                   | Não  | —                 | PK                         | Identificador do acompanhamento      | SenhaFlowTracking.id                     |
| status      | SenhaFlowStatus (enum) | Não  | 'EM_ANDAMENTO'    |                            | EM_ANDAMENTO, CONCLUIDO ou CANCELADO | SenhaFlowTracking.status                 |
| senhaId     | UUID                   | Não  | —                 | UK · FK → senhas · CASCADE | Senha acompanhada; uma por senha     | SenhaFlowTracking.senha (associação)     |
| flowId      | UUID                   | Não  | —                 | FK → flows · CASCADE       | Flow acompanhado                     | SenhaFlowTracking.flow (associação)      |
| stepAtualId | UUID                   | Sim  | —                 | FK → flow_steps · SET NULL | Step atual da senha                  | SenhaFlowTracking.stepAtual (associação) |
| createdAt   | TIMESTAMP(3)           | Não  | CURRENT_TIMESTAMP |                            | Data de criação                      | SenhaFlowTracking.createdAt              |
| updatedAt   | TIMESTAMP(3)           | Não  | —                 |                            | Data da última alteração (Prisma)    | SenhaFlowTracking.updatedAt              |

Índices: `UK (senhaId)`; `(flowId, status)`.

### senha_flow_historicos

Histórico append-only dos steps por onde a senha passou. Guarda snapshots (`stepNome`, `filaId`) para preservar o registro se o step ou a fila mudarem depois.

| Coluna     | Tipo         | Nulo | Default           | Chave                               | Descrição                             | Origem                                   |
| ---------- | ------------ | ---- | ----------------- | ----------------------------------- | ------------------------------------- | ---------------------------------------- |
| id         | UUID         | Não  | —                 | PK                                  | Identificador do registro             | SenhaFlowHistorico.id                    |
| stepNome   | VARCHAR(180) | Não  | —                 |                                     | Snapshot do nome do step na entrada   | SenhaFlowHistorico.stepNome              |
| filaId     | VARCHAR(64)  | Não  | —                 |                                     | Snapshot do id da fila; texto, sem FK | SenhaFlowHistorico.filaId                |
| entradaEm  | TIMESTAMP(3) | Não  | CURRENT_TIMESTAMP |                                     | Momento de entrada no step            | SenhaFlowHistorico.entradaEm             |
| saidaEm    | TIMESTAMP(3) | Sim  | —                 |                                     | Momento de saída do step              | SenhaFlowHistorico.saidaEm               |
| resultado  | VARCHAR(120) | Sim  | —                 |                                     | Resultado do step                     | SenhaFlowHistorico.resultado             |
| observacao | TEXT         | Sim  | —                 |                                     | Observação do operador                | SenhaFlowHistorico.observacao            |
| trackingId | UUID         | Não  | —                 | FK → senha_flow_trackings · CASCADE | Acompanhamento ao qual pertence       | SenhaFlowHistorico.tracking (associação) |
| stepId     | UUID         | Não  | —                 | FK → flow_steps · RESTRICT          | Step percorrido                       | SenhaFlowHistorico.step (associação)     |
| operadorId | UUID         | Sim  | —                 | FK → users · SET NULL               | Operador responsável pelo step        | SenhaFlowHistorico.operador (associação) |

Índices: `(trackingId)`. Não possui `createdAt` nem `updatedAt`; a entrada é `entradaEm`.

## Enums

Cada enum vira um tipo do banco (`CREATE TYPE ... AS ENUM`), reutilizável entre tabelas. Valor fora da lista é recusado no `INSERT`.

| Enum              | Valores                                                                                                                                                                                                                                                                                       | Usado em                    |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| Papel             | ADMIN_PLATAFORMA, ADMIN_INSTITUICAO, OPERADOR, USUARIO_FINAL                                                                                                                                                                                                                                  | usuario_papeis.papel        |
| StatusInstituicao | PENDENTE, APROVADA, REJEITADA                                                                                                                                                                                                                                                                 | instituicoes.status         |
| TipoAtendimento   | ONLINE, PRESENCIAL, HIBRIDO                                                                                                                                                                                                                                                                   | filas.tipoAtendimento       |
| ModoQrCode        | ROTATIVO, FIXO                                                                                                                                                                                                                                                                                | filas.modoQrCode            |
| StatusSenha       | AGUARDANDO, EM_ATENDIMENTO, CANCELADA, FINALIZADA                                                                                                                                                                                                                                             | senhas.status               |
| PrioridadeSenha   | LEGAL, FIDELIDADE, NORMAL                                                                                                                                                                                                                                                                     | senhas.prioridade           |
| NotificacaoTipo   | SENHA_CHAMADA, SENHA_FINALIZADA, SENHA_CANCELADA, SENHA_PROXIMA, SENHA_NOVA_NA_FILA, SENHA_TRANSFERIDA, FILA_CRIADA, FILA_REMOVIDA, FILA_ATUALIZADA, SOLICITACAO_INSTITUICAO, INSTITUICAO_APROVADA, INSTITUICAO_REJEITADA, USUARIO_CRIADO, SISTEMA, SENHA_ENCAMINHADA_FLOW, SENHA_MOVIDA_FILA | notificacoes.tipo           |
| FlowTransicaoTipo | SEQUENCIAL, CONDICIONAL, ALTA                                                                                                                                                                                                                                                                 | flow_transitions.tipo       |
| SenhaFlowStatus   | EM_ANDAMENTO, CONCLUIDO, CANCELADO                                                                                                                                                                                                                                                            | senha_flow_trackings.status |

Veja também: [README](README.md) · [schema.sql](schema.sql) · [modelo de classes](../05-modelagem/classes/README.md)
