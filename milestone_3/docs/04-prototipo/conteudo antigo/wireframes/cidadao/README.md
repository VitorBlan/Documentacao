# Wireframes do cidadão (T-CID)

Milestone 2 · Protótipo do FilaCidadã, versão 0.2 (Fábrica de Software I)

Wireframes de média fidelidade das telas do cidadão autenticado (USUARIO_FINAL), em 390x844 (mobile). Escala de cinza com um acento azul; a ação principal de cada tela é o botão azul preenchido. Telas raiz têm navegação inferior (Filas, Minha senha, Histórico, Perfil); telas de fluxo têm cabeçalho com voltar.

## Telas

| ID | Arquivo | Tela | UC | RF | US | Endpoints principais |
|---|---|---|---|---|---|---|
| T-CID-01 | [01-filas-disponiveis.svg](01-filas-disponiveis.svg) | Filas disponíveis | UC-08 | RF-025, RF-036 | US-020, US-021 | GET /filas; GET /filas/:id/senhas/contagem |
| T-CID-02 | [02-fila-emissao-online.svg](02-fila-emissao-online.svg) | Fila e emissão de senha online | UC-08 | RF-034, RF-037 | US-020, US-021 | POST /filas/:filaId/senhas |
| T-CID-03 | [03-entrada-qr-code.svg](03-entrada-qr-code.svg) | Entrada por QR Code | UC-08 | RF-035, RF-034 | US-022 | GET /qrcodes/buscar/:codigo; POST /filas/:filaId/senhas |
| T-CID-04 | [04-minha-senha.svg](04-minha-senha.svg) | Minha senha: status, posição e cancelamento | UC-15 | RF-059, RF-060 | US-029, US-038 | GET /senhas/:id; POST /senhas/:id/cancelar |
| T-CID-05 | [05-minhas-senhas.svg](05-minhas-senhas.svg) | Minhas senhas (histórico) | UC-15 | RF-058 | US-038 | GET /senhas (page, limit, statusIn) |
| T-CID-06 | [06-chat-da-senha.svg](06-chat-da-senha.svg) | Chat da senha | UC-15 | RF-063 | US-040 | GET e POST /senhas/:senhaId/chat |
| T-CID-07 | [07-notificacoes.svg](07-notificacoes.svg) | Notificações | UC-15 | RF-064, RF-065 | US-041 | GET /notificacoes; PATCH /notificacoes/:id/lida; PATCH /notificacoes/ler-todas |
| T-CID-08 | [08-solicitar-instituicao.svg](08-solicitar-instituicao.svg) | Solicitar cadastro de instituição | UC-03 | RF-009, RF-010 | US-007 | POST /instituicoes/solicitar |
| T-CID-09 | [09-meu-perfil.svg](09-meu-perfil.svg) | Meu perfil | UC-02 | RF-005, RF-006, RF-007, RF-008 | US-004, US-005, US-006 | GET e PATCH /me; PATCH /me/senha; POST e DELETE /me/avatar; GET /timezones |
| T-CID-10 | [10-fila-vazia.svg](10-fila-vazia.svg) | Estado: fila vazia | UC-08 | RF-036 | US-020 | GET /filas/:id/senhas/contagem |
| T-CID-11 | [11-erro.svg](11-erro.svg) | Estado: erro ao emitir senha | UC-08 | RF-034 | US-020 | POST /filas/:filaId/senhas (409, 400, 500) |
| T-CID-12 | [12-sem-permissao.svg](12-sem-permissao.svg) | Estado: sem permissão | UC-15 | RF-059 | US-038 | GET /senhas/:id (403) |

## Notas de comportamento

- **T-CID-01.** Não há rota que liste instituições para o cidadão (`GET /instituicoes` exige ADMIN). A tela agrupa as filas de `GET /filas` por `instituicao.nome`. A listagem é paginada (padrão 10 itens), então o agrupamento precisa percorrer todas as páginas. Só filas ativas aparecem em destaque; filas com `ativa: false` ficam tracejadas e sem ação.
- **T-CID-02.** Prioridades mostradas só se a fila habilitar: "Prioritário" (`prioridadesHabilitadas`) e "Fidelidade" (`fidelidadeHabilitada`, não desenhada aqui por falta de espaço). Sem habilitação, a senha sai como Normal (RF-037). Usuário com senha AGUARDANDO ou EM_ATENDIMENTO na mesma fila recebe 409; a tela 11 mostra esse caso.
- **T-CID-03.** Só para filas `PRESENCIAL`: o código do QR é obrigatório e validado no servidor. Em filas ONLINE e HIBRIDO não há QR no código atual. Câmera negada cai no campo de código digitado.
- **T-CID-04.** Ação de cancelar só aparece com status AGUARDANDO (RF-060). O campo "Posição na fila" mostra o valor `posicao` gravado na emissão. Pelo RF-037 a posição não é reordenada depois, e o código só altera `posicao` ao mover senha no flow. Por isso a tela não promete posição ao vivo; "Aguardando agora" vem de `GET /filas/:id/senhas/contagem`.
- **T-CID-05.** Filtro por situação usa `statusIn` (ex.: `FINALIZADA,CANCELADA`). Paginação com `page` e `limit` (1 a 100).
- **T-CID-06.** Chat só existe com senha EM_ATENDIMENTO; fora disso a API responde 409 e a tela deixa o campo indisponível.
- **T-CID-07.** Atualiza ao vivo por `notificacao:nova`. "Ler todas" usa PATCH /notificacoes/ler-todas.
- **T-CID-08.** Campos iguais ao modelo `Instituicao`. A instituição nasce PENDENTE; o solicitante é avisado depois da análise.
- **T-CID-09.** Senha nova segue a regra de `PATCH /me/senha`: mínimo 8, com maiúscula, minúscula, número e caractere especial. Foto aceita JPEG, PNG, WebP e GIF até 50 MB (`AVATAR_MAX_SIZE_BYTES`).
- **T-CID-10 a 12 (estados de exceção).** Mensagens vêm de `message` na resposta (`CommonResponse`). 401 (sessão expirada) não aparece como tela própria aqui: redireciona para o login com aviso, conforme anotação da tela 12.

## Divergências

1. A lista oficial em [../README.md](../README.md) prevê 9 telas CID. Esta pasta tem 12: as 9 oficiais mais as 3 de exceção (T-CID-10 a 12), que a frente pediu explicitamente. O total de telas da lista oficial (42) e a contagem do perfil precisam ser atualizados pelo responsável do índice. Nomes e números das 9 telas oficiais foram seguidos.
2. Não há endpoint para o cidadão listar instituições (lacuna). T-CID-01 deriva a lista de `GET /filas`.
3. `posicao` não é recalculada quando outra senha é chamada; a tela 04 não exibe posição ao vivo. Se o time quiser posição real, é preciso contar as senhas de prioridade igual ou menor e em ordem anterior, ou criar endpoint.
4. A lista oficial atribuía a T-CID-03 (entrada por QR) a rota `GET /qrcodes/buscar/:codigo`. Esse endpoint localiza a fila sem login (RF-035); a emissão em si é `POST /filas/:filaId/senhas`, feita pelo cidadão (USUARIO_FINAL).
5. Critérios de elegibilidade de "Prioritário" não existem no código; a tela não os explica.

Veja também: [Protótipo (índice)](../README.md) · [Mapa do site](../../site-map.md) · [Wireframes públicos](../pub/README.md) · [Requisitos funcionais](../../../03-requisitos/requisitos-funcionais.md) · [Histórias de usuário](../../../03-requisitos/historias-de-usuario.md)
