# Wireframes das telas públicas (T-PUB)

Milestone 2 · Protótipo do FilaCidadã, versão 0.2 (Fábrica de Software I)

Wireframes de média fidelidade das telas acessíveis sem sessão: página inicial, login e cadastro de cidadão. Formato 1280x800 (desktop). Escala de cinza com um acento azul; a ação principal de cada tela é o botão azul preenchido.

## Telas

| ID | Arquivo | Tela | UC | RF | US | Endpoints |
|---|---|---|---|---|---|---|
| T-PUB-01 | [01-pagina-inicial.svg](01-pagina-inicial.svg) | Página inicial (landing page) | UC-03 | RF-011 | — | GET /landing-page |
| T-PUB-02 | [02-login.svg](02-login.svg) | Login | UC-01 | RF-002 | US-002 | POST /api/auth/sign-in/email |
| T-PUB-03 | [03-cadastro.svg](03-cadastro.svg) | Cadastro de cidadão | UC-01 | RF-001 | US-001 | POST /api/auth/sign-up/email; GET /timezones |

Ainda não desenhadas nesta entrega, mas listadas em [../../README.md](../../README.md) como T-PUB-04 (recuperar senha), T-PUB-05 (sessão expirada, 498) e T-PUB-06 (acompanhar senha por código, RF-061). Ficam para a próxima revisão do protótipo.

## Notas de comportamento

- **T-PUB-01.** Textos e blocos vêm de `GET /landing-page`. Os valores da prévia são os padrões de `LandingPageService.getDadosPadrao()`. Abaixo da dobra, a página segue a ordem do JSON: funcionalidades, passo a passo, para quem é, FAQ e chamada final. Os botões "Comece agora" e "Criar conta grátis" levam a T-PUB-03; "Entrar" leva a T-PUB-02.
- **T-PUB-02.** Sucesso: a sessão vem em cookie e no cabeçalho `set-auth-token`; o destino depende do papel, e o destino por papel ainda não está definido nos requisitos. Erro de credencial (401) mostra a faixa com "E-mail ou senha inválidos". Após muitas tentativas, o BetterAuth aplica bloqueio temporário; a tela mostra só a nota, sem número de tentativas, porque o código não fixa esse limite.
- **T-PUB-03.** Campos enviados: `name`, `email`, `password` e `fusoHorario` (lista de `GET /timezones`, padrão America/Manaus). `pais` é preenchido pelo padrão BR e não aparece. O campo "Confirmar senha" é só de interface. A conta é criada sem login automático (`autoSignIn: false`), então o sucesso leva à tela de login com a mensagem de conta criada. E-mail já cadastrado responde 422.

## Divergências

1. A lista oficial em [../../README.md](../../README.md) prevê 6 telas PUB. Esta entrega traz 3 (T-PUB-01 a 03), e os números 02 e 03 foram trocados para casar com a lista (login é 02, cadastro é 03).
2. A senha do cadastro segue só `minPasswordLength: 8` do BetterAuth. A regra de maiúscula, minúscula, número e caractere especial (`senhaRegex`) vale apenas para `PATCH /me/senha`. Quem cria conta pode escolher senha que depois não passaria na troca. Decisão para a frente de requisitos.
3. Fora do código: não há regra de elegibilidade para prioridade, então a tela não explica quem tem direito.

Veja também: [Protótipo (índice)](../../README.md) · [Mapa do site](../../site-map.md) · [Wireframes do cidadão](../cidadao/README.md) · [Requisitos funcionais](../../../03-requisitos/requisitos-funcionais.md)
