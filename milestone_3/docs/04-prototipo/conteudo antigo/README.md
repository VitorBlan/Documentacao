> **Conteúdo antigo.** Esta pasta guarda o protótipo do modelo da disciplina (FilaCidadã), deixado pelo professor, apenas como referência. Ele não descreve o Bancada. O protótipo do Bancada está no [README da pasta 04-prototipo](../README.md).

# Protótipo do FilaCidadã

Fábrica de Software I · Protótipo do sistema (UC-01 a UC-15) · Milestones 2 e 3

Este diretório reúne o protótipo de interface do FilaCidadã: o mapa do site, o inventário de telas por perfil, os modelos IFML das duas interações principais e a revisão de usabilidade com as heurísticas de Nielsen. A fonte das telas é o código da API (`src/routes`, `src/services`, `src/constants/roles.js`), não a intuição do time.

## Índice

| Artefato | Conteúdo |
|---|---|
| [site-map.md](site-map.md) | Hierarquia do site em até 3 níveis, perfil de acesso por área e tabela tela para US |
| [site-map.svg](site-map.svg) | Diagrama da hierarquia do site |
| [ifml/README.md](ifml/README.md) | Notação IFML usada, legenda e descrição dos dois modelos |
| [ifml/ifml-emitir-senha.svg](ifml/ifml-emitir-senha.svg) | IFML da emissão de senha online (2 páginas) |
| [ifml/ifml-chamar-senha.svg](ifml/ifml-chamar-senha.svg) | IFML do chamado e atendimento de senha (2 páginas) |
| [wireframes/pub/README.md](wireframes/pub/README.md) | Wireframes das telas públicas (T-PUB) |
| [wireframes/cidadao/README.md](wireframes/cidadao/README.md) | Wireframes do cidadão (T-CID) |
| [wireframes/operador/README.md](wireframes/operador/README.md) | Wireframes do operador (T-OPE) |
| [wireframes/admin-instituicao/README.md](wireframes/admin-instituicao/README.md) | Wireframes do administrador da instituição (T-ADI) |
| [wireframes/admin-plataforma/README.md](wireframes/admin-plataforma/README.md) | Wireframes do administrador da plataforma (T-ADP) |

## Protótipo navegável

Protótipo no Figma: [PLACEHOLDER: link ficticio, substituir pelo arquivo real](https://www.figma.com/design/PLACEHOLDER-filacidada-prototipo)

O endereço acima é um marcador. Não existe arquivo Figma com esse identificador. Substituir pelo link real quando o arquivo for criado e compartilhado com a equipe e com o professor.

## Versões

| Versão | Data | Escopo | Situação |
|---|---|---|---|
| 0.1 | 2026-11-06 | Inventário de telas por perfil, mapa do site, IFML (2 modelos) e revisão heurística inicial | Rascunho para revisão |
| 0.2 | a definir | Wireframes SVG publicados em `wireframes/` para os cinco perfis | Planejada |
| 1.0 | a definir | Protótipo navegável no Figma, com os ajustes da revisão de usabilidade | Planejada |

## Perfis e convenção de telas

Os atores vêm de `src/constants/roles.js`. Cada papel tem um código de perfil usado nas telas:

| Código | Ator | Papel no código |
|---|---|---|
| PUB | Visitante e usuário sem sessão | Rotas públicas e rotas de autenticação |
| CID | ACT-01 Cidadão | `USUARIO_FINAL` |
| OPE | ACT-02 Operador | `OPERADOR` |
| ADI | ACT-03 Administrador da Instituição | `ADMIN_INSTITUICAO` |
| ADP | ACT-04 Administrador da Plataforma | `ADMIN_PLATAFORMA` |

Convenção de identificação: `T-<perfil>-NN`, com `NN` sequencial dentro do perfil, começando em 01. Exemplo: `T-CID-02` é a segunda tela do cidadão. Ao inserir ou remover uma tela, renumera-se o perfil a partir dela e atualizam-se todas as referências (mapa do site, IFML, wireframes e divergências), sem deixar vão na numeração. A mudança entra no quadro de versões.

Telas compartilhadas por perfis autenticados (Notificações `T-CID-07`, Meu perfil `T-CID-09`) são desenhadas uma vez, na pasta do cidadão, e reutilizadas pelos demais perfis.

Um perfil herda o acesso de outro quando o código permite. O administrador da instituição executa todas as ações do operador (`PAPEIS_ADMIN_INSTITUICAO` em `src/constants/roles.js`). Por isso as telas do operador também são acessíveis a ele, e o inventário indica isso na coluna Acesso de `site-map.md`.

## Inventário de telas

Total de telas previstas:

| Perfil | Telas | Pasta de wireframes |
|---|---|---|
| PUB · Público (login e acesso sem sessão) | 6 | `wireframes/pub/` |
| CID · Cidadão (USUARIO_FINAL) | 9 | `wireframes/cidadao/` |
| OPE · Operador (OPERADOR) | 7 | `wireframes/operador/` |
| ADI · Administrador da Instituição (ADMIN_INSTITUICAO) | 12 | `wireframes/admin-instituicao/` |
| ADP · Administrador da Plataforma (ADMIN_PLATAFORMA) | 8 | `wireframes/admin-plataforma/` |
| **Total** | **42** | |

Convenção de UC e US: cada tela aponta para o caso de uso (UC) que atende e para as histórias de [historias-de-usuario.md](../03-requisitos/historias-de-usuario.md) que ela realiza. Quando nenhuma história cobre a tela, a célula da coluna US mostra a lacuna (ver PN-01). A coluna Endpoints lista as rotas da API que a tela consome, para que o desenvolvimento e os wireframes falem da mesma coisa.

### PUB: Público (login e acesso sem sessão)

Pasta dos wireframes: [`wireframes/pub/`](wireframes/pub/README.md) · 6 telas

| ID | Tela | UC | US | Endpoints principais |
|---|---|---|---|---|
| `T-PUB-01` | Página inicial (landing page) | — | — | GET /landing-page |
| `T-PUB-02` | Login | UC-01 | US-002 | POST /api/auth/sign-in/email |
| `T-PUB-03` | Cadastro de cidadão | UC-01 | US-001 | POST /api/auth/sign-up/email |
| `T-PUB-04` | Recuperar senha | UC-01 | US-003 | Recuperação de senha do BetterAuth (requestPasswordReset) |
| `T-PUB-05` | Sessão expirada (código 498) | UC-01 | US-002 | Resposta 498 em qualquer rota autenticada |
| `T-PUB-06` | Acompanhar senha por código | UC-15 | US-039 | GET /publico/senhas/:token/status |

### CID: Cidadão (USUARIO_FINAL)

Pasta dos wireframes: [`wireframes/cidadao/`](wireframes/cidadao/README.md) · 9 telas

| ID | Tela | UC | US | Endpoints principais |
|---|---|---|---|---|
| `T-CID-01` | Filas disponíveis | UC-08 | US-020 | GET /filas, GET /filas/:id |
| `T-CID-02` | Fila e emissão de senha online | UC-08 | US-020, US-021 | GET /filas/:id/senhas/contagem, POST /filas/:filaId/senhas |
| `T-CID-03` | Entrada por QR Code | UC-08 | US-022, US-033 | GET /qrcodes/buscar/:codigo |
| `T-CID-04` | Minha senha: status, posição e cancelamento | UC-15 | US-038, US-029 | GET /senhas/:id, POST /senhas/:id/cancelar |
| `T-CID-05` | Minhas senhas | UC-15 | US-038 | GET /senhas |
| `T-CID-06` | Chat da senha (cidadão) | UC-15 | US-040 | GET e POST /senhas/:senhaId/chat |
| `T-CID-07` | Notificações | UC-15 | US-041 | GET /notificacoes, GET /notificacoes/nao-lidas, PATCH /notificacoes/:id/lida, PATCH /notificacoes/ler-todas |
| `T-CID-08` | Solicitar cadastro de instituição | UC-03 | US-007 | POST /instituicoes/solicitar |
| `T-CID-09` | Meu perfil | UC-02 | US-004, US-005, US-006 | GET e PATCH /me, PATCH /me/senha, POST e DELETE /me/avatar |

### OPE: Operador (OPERADOR)

Pasta dos wireframes: [`wireframes/operador/`](wireframes/operador/README.md) · 7 telas

| ID | Tela | UC | US | Endpoints principais |
|---|---|---|---|---|
| `T-OPE-01` | Painel do operador (meu atendimento) | UC-10 | US-024, US-026 | GET /senhas/meu-atendimento |
| `T-OPE-02` | Fila em atendimento (senhas aguardando) | UC-10 | US-024, US-025 | GET /senhas, GET /filas/:id/senhas/contagem |
| `T-OPE-03` | Detalhe da senha: chamar, rechamar e assumir | UC-10 | US-026, US-027 | GET /senhas/:id, POST /senhas/:id/chamar, POST /senhas/:id/rechamar, POST /senhas/:id/assumir (somente Admin Instituição) |
| `T-OPE-04` | Finalizar ou cancelar atendimento | UC-11 | US-028, US-029 | POST /senhas/:id/finalizar, POST /senhas/:id/cancelar |
| `T-OPE-05` | Emitir senha presencial | UC-09 | US-023 | POST /filas/:filaId/senhas/presencial |
| `T-OPE-06` | Fluxo da senha: ingressar, encaminhar e dar alta | UC-10, UC-11 | US-030 | GET /senhas/:id/flow/status, GET /senhas/:id/flow/transicoes, POST /flows/:id/senhas/:senhaId/ingressar, /encaminhar, /alta |
| `T-OPE-07` | Chat da senha (operador) | UC-10 | US-040 (parcial: história só do cidadão) | GET e POST /senhas/:senhaId/chat |

### ADI: Administrador da Instituição (ADMIN_INSTITUICAO)

Pasta dos wireframes: [`wireframes/admin-instituicao/`](wireframes/admin-instituicao/README.md) · 12 telas

| ID | Tela | UC | US | Endpoints principais |
|---|---|---|---|---|
| `T-ADI-01` | Painel da instituição e relatório | UC-13 | US-035, US-036 | GET /dashboard/instituicao, GET /senhas/stats, GET /senhas/stats/relatorio |
| `T-ADI-02` | Lista de filas | UC-06 | US-016 | GET /filas |
| `T-ADI-03` | Formulário de fila (criar, editar e excluir) | UC-06 | US-015, US-016, US-017 | POST /filas, PATCH /filas/:id, DELETE /filas/:id (excluir somente Admin Instituição) |
| `T-ADI-04` | Tipos de etapa da instituição | UC-07 | US-018, US-019 | GET e PUT /instituicoes/:instituicaoId/tipos-etapa |
| `T-ADI-05` | Lista de fluxos | UC-07 | sem US (lacuna) | GET /flows, GET /flows/:id |
| `T-ADI-06` | Editor de fluxo | UC-07 | sem US (lacuna); US-030 cobre só a operação do flow | POST /flows, PUT /flows/:id, PATCH /flows/:id/desativar, DELETE /flows/:id |
| `T-ADI-07` | Dados da instituição | UC-03 | sem US (lacuna) | GET e PATCH /instituicoes/:instituicaoId |
| `T-ADI-08` | Lista de usuários da instituição | UC-05 | US-011 | GET /instituicoes/:instituicaoId/usuarios |
| `T-ADI-09` | Novo usuário da instituição | UC-05 | US-012 | POST /instituicoes/:instituicaoId/usuarios |
| `T-ADI-10` | Detalhe do usuário e papéis | UC-05 | US-013, US-014 | GET, PATCH e DELETE /usuarios/:id |
| `T-ADI-11` | QR Codes da instituição | UC-12 | US-031, US-032 | GET /qrcodes, GET /qrcodes/:id, POST /qrcodes, POST /qrcodes/regenerar, PATCH /qrcodes/:id/desativar |
| `T-ADI-12` | Auditoria da instituição | UC-14 | US-037 | GET /auditorias |

### ADP: Administrador da Plataforma (ADMIN_PLATAFORMA)

Pasta dos wireframes: [`wireframes/admin-plataforma/`](wireframes/admin-plataforma/README.md) · 8 telas

| ID | Tela | UC | US | Endpoints principais |
|---|---|---|---|---|
| `T-ADP-01` | Painel da plataforma | UC-13 | US-034 | GET /dashboard/admin |
| `T-ADP-02` | Solicitações de instituição | UC-04 | US-008 | GET /instituicoes |
| `T-ADP-03` | Análise da solicitação: aprovar, rejeitar, reconsiderar e contrato | UC-04 | US-008, US-009, US-010 | PATCH /instituicoes/:id/aprovar, /rejeitar, /reconsiderar, POST /instituicoes/:id/contrato |
| `T-ADP-04` | Lista de instituições | UC-04 | US-008 (parcial: lista sem história própria) | GET /instituicoes |
| `T-ADP-05` | Detalhe e edição da instituição | UC-04 | sem US (lacuna) | GET, PATCH e DELETE /instituicoes/:id |
| `T-ADP-06` | Lista de usuários da plataforma | UC-05 | US-011 (parcial: ator é o administrador da instituição) | GET /usuarios |
| `T-ADP-07` | Detalhe do usuário e papéis | UC-05 | US-013 (parcial: ator é o administrador da instituição) | GET, PATCH e DELETE /usuarios/:id |
| `T-ADP-08` | Auditoria da plataforma | UC-14 | US-037 (parcial: ator é o administrador da instituição) | GET /auditorias |


## Revisão de usabilidade (heurísticas de Nielsen)

Escopo: inspeção heurística do protótipo planejado na versão 0.1 (inventário de telas, mapa do site e IFML). Não há wireframes finais nem teste com usuários nesta versão. Por isso esta revisão antecipa problemas e define critérios para os wireframes. Deve ser repetida quando os wireframes de `wireframes/` forem publicados.

Escala de severidade (Nielsen): 0 não é problema de usabilidade; 1 cosmético; 2 menor; 3 grande; 4 catástrofe.

### Avaliação por heurística

| # | Heurística | Aplicação no protótipo | Decisão aplicada |
|---|---|---|---|
| H1 | Visibilidade do status do sistema | A senha passa por AGUARDANDO, EM_ATENDIMENTO, CANCELADA e FINALIZADA. O cidadão precisa saber em que etapa está e qual é sua posição. | `T-CID-04` e `T-PUB-06` mostram status e posição. `T-OPE-02` atualiza a fila sem recarregar a tela. |
| H2 | Correspondência com o mundo real | Termos internos do código, como "alta", "encaminhar" e "rechamar", podem não fazer sentido para o operador. | Rótulos de ação em linguagem de atendimento. Ver UX-02. |
| H3 | Controle e liberdade do usuário | Cancelar senha só é permitido com status AGUARDANDO; a API responde 409 nos demais casos. Finalizar e cancelar são irreversíveis. | Botão desabilitado com o motivo em `T-CID-04`, em vez de oculto. Confirmação antes de finalizar ou cancelar em `T-OPE-04`. Ver UX-01. |
| H4 | Consistência e padrões | O mesmo conjunto de ações (chamar, rechamar, assumir) aparece no painel e no detalhe da senha. | Mesmo padrão de ação na lista e no detalhe. Ver UX-03. |
| H5 | Prevenção de erros | Formulários de fila (`T-ADI-03`) e de usuário (`T-ADI-09`) recebem dados que a API valida. | Validação de campos obrigatórios antes do envio. Pendência PN-02 sobre regra de senha duplicada. |
| H6 | Reconhecer em vez de lembrar | O cidadão não deveria digitar código de fila nem de senha de memória. | Entrada por QR Code em `T-CID-03`. Detalhe da senha em `T-OPE-03` mostra os dados sem exigir digitação. |
| H7 | Flexibilidade e eficiência de uso | O operador repete as mesmas ações o dia inteiro. | "Chamar próxima" em um toque em `T-OPE-01`. Atalhos de teclado ficam como melhoria futura, não como requisito. |
| H8 | Estética e design minimalista | Painéis administrativos tendem a acumular indicadores. | Wireframes em tons de cinza com um único acento azul. Máximo de quatro indicadores principais por painel. |
| H9 | Ajudar a reconhecer, diagnosticar e recuperar erros | A API devolve mensagem no campo `message`. A sessão expirada responde 498, não 401. | `T-PUB-05` explica a expiração e oferece entrar de novo. Erros de permissão (403) mostram o motivo, não uma tela vazia. |
| H10 | Ajuda e documentação | Não há ajuda contextual prevista na versão 0.1. | `T-PUB-01` apresenta o serviço. Ajuda contextual fica como melhoria futura, não implementada. |

### Achados abertos

| ID | Severidade | Achado | Tela afetada | Ação |
|---|---|---|---|---|
| UX-01 | 3 | O cidadão pode tentar cancelar uma senha que já está em atendimento e não entender por que não é possível. | `T-CID-04` | Mostrar o botão desabilitado com a razão ("Senha já está em atendimento"). |
| UX-02 | 2 | Os rótulos "alta", "encaminhar" e "ingressar" vêm do vocabulário do código. | `T-OPE-06` | Usar "Encerrar fluxo (dar alta)", "Encaminhar para etapa" e "Entrar no fluxo", com o termo técnico no glossário. |
| UX-03 | 3 | Assumir senha é permitido só ao administrador da instituição (`authorize` em `senhaRoutes.js`). Um operador que tentar recebe 403. | `T-OPE-03` | Mostrar a ação ao operador desabilitada, com o motivo "Disponível para o administrador da instituição". |
| UX-04 | 2 | Excluir fila é ação destrutiva e está no mesmo formulário de edição. | `T-ADI-03` | Separar a exclusão em área própria, com confirmação explícita. |

Prioridade: UX-01 e UX-03 devem estar resolvidos antes da versão 0.2. UX-02 e UX-04 podem ficar para a versão 1.0.

## Pendências

| ID | Pendência | Responsável sugerido |
|---|---|---|
| PN-01 | Cobrir as lacunas do inventário: configuração de fluxo (`T-ADI-05`, `T-ADI-06`), dados da instituição (`T-ADI-07`) e edição ou exclusão de instituição pela plataforma (`T-ADP-05`), além de histórias próprias para a lista de instituições (`T-ADP-04`), a lista e o detalhe de usuários da plataforma (`T-ADP-06`, `T-ADP-07`), a auditoria da plataforma (`T-ADP-08`) e o chat do operador (`T-OPE-07`). | Frente de histórias |
| PN-02 | Confirmar se a API impede duas senhas ativas do mesmo cidadão na mesma fila. Esta regra não foi confirmada no código lido e não aparece nas telas. | Frente de requisitos |
| PN-03 | Criar o arquivo Figma real e substituir o marcador de link. | Equipe |

## Divergências

Divergências entre o catálogo de casos de uso do enunciado e o código. Quando houve conflito, o protótipo seguiu o código.

| ID | Divergência | Tratamento no protótipo |
|---|---|---|
| D-01 | UC-15 "Acompanhar minha senha (meu atendimento)": no código, `GET /senhas/meu-atendimento` é do operador e do administrador da instituição, e mostra a senha que ele está atendendo. O acompanhamento do cidadão é `GET /senhas/:id` (dono da senha) e `GET /publico/senhas/:token/status` (sem login). | Atendimento do operador em `T-OPE-01` (UC-10). Acompanhamento do cidadão em `T-CID-04` e `T-PUB-06` (UC-15). |
| D-02 | Chat da senha (`/senhas/:senhaId/chat`) não tem UC no catálogo. | Mapeado para UC-15 (cidadão, `T-CID-06`) e UC-10 (operador, `T-OPE-07`). A frente de requisitos deve decidir se cria um UC. |
| D-03 | Fluxo da senha (`/flows`, `/senhas/:id/flow/*`) não tem UC próprio. | Configuração do fluxo em `T-ADI-05` e `T-ADI-06` (UC-07). Operação em `T-OPE-06` (UC-10 e UC-11). |
| D-04 | Edição (`PATCH /instituicoes/:id`) e exclusão (`DELETE /instituicoes/:id`) de instituição não têm UC. | Edição do administrador da instituição em `T-ADI-07` (UC-03). Edição e exclusão pela plataforma em `T-ADP-05` (UC-04). |
| D-05 | Notificações não têm UC. | Tela do cidadão `T-CID-07` (UC-15). Para o operador, as notificações ficam na tela compartilhada. |
| D-06 | Recuperação de senha existe no BetterAuth (`sendResetPassword` em `src/config/auth.js`), mas não está no UC-01, que cita cadastro, login e logout. | Tela `T-PUB-04` ligada a UC-01 como extensão. A frente de requisitos deve incluir o caso. |
| D-07 | Landing page (`GET /landing-page`) é pública e não tem UC. Não há rota de administração para editá-la. | Tela `T-PUB-01` sem UC. Não há tela de edição. |
| D-08 | Pelo código, o operador pode criar e editar filas (`POST` e `PATCH /filas`), e também gerenciar QR Codes. O catálogo não diz isso. | `T-ADI-03` e `T-ADI-11` indicam o acesso do operador. A exclusão de fila fica só com o administrador da instituição. |
| D-09 | Ação "assumir" da senha é restrita ao administrador da instituição. O operador não pode executá-la. | Ação desabilitada para o operador em `T-OPE-03`. Ver UX-03. |
| D-10 | Sessão expirada responde 498 (`src/utils/helpers/HttpStatusCodes.js`). O catálogo não tem caso de uso para esse estado. | Estado de erro `T-PUB-05`, ligado a UC-01. |

Sem divergência: os quatro atores (ACT-01 a ACT-04) correspondem aos papéis de `src/constants/roles.js`.

## Veja também

- [Mapa do site](site-map.md) e [diagrama](site-map.svg)
- [IFML: emissão e chamado de senha](ifml/README.md)
- [Historias de usuario](../03-requisitos/historias-de-usuario.md)
- [Requisitos funcionais](../03-requisitos/requisitos-funcionais.md)
- [Casos de uso](../05-modelagem/casos-de-uso/README.md)
- [Glossario](../01-visao/glossario.md)
