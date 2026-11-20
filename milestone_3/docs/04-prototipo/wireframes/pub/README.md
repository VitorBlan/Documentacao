# Wireframes públicos (T-PUB)

Fábrica de Software I · Protótipo em alta fidelidade · Desktop 1440×900 · Acesso sem sessão

Estes wireframes mostram a única tela do Bancada acessada sem sessão: o login. O Bancada não tem página inicial pública nem autocadastro, porque só os funcionários da oficina usam o sistema.

## Telas

| Tela | Arquivo | Caso de uso | US | Área |
|---|---|---|---|---|
| T-PUB-01 Login: entrar no sistema | [01-login-entrar-no-sistema.svg](01-login-entrar-no-sistema.svg) | Todos (inclusão) | US-001 | Login |

Os casos de uso reproduzem os vínculos registrados nas issues do GitLab, conforme o índice de [historias-de-usuario.md](../../../03-requisitos/historias-de-usuario.md#índice).

## Entrar no sistema

**T-PUB-01 · Login: entrar no sistema**

![Tela de login: painel escuro à esquerda com a marca Bancada e os indicadores veículos na oficina, boxes ativos e mecânicos hoje; à direita, formulário com Matrícula, Senha, opção Manter conectada, link Esqueci a senha, botão Entrar e aviso de acesso restrito por perfil](01-login-entrar-no-sistema.svg)

## Regras de comportamento

Cada linha é um cenário Gherkin das histórias. A coluna Situação indica se o wireframe mostra o comportamento: **Desenhado**, **Parcial** (a tela existe, mas falta parte do cenário), **Não desenhado** ou **Divergente** (a tela contradiz o cenário). Cenários marcados `@proposta` foram escritos pela equipe de documentação e ainda não estão nas issues.

| Cenário | US | Telas | Situação | Observação |
|---|---|---|---|---|
| Login com credenciais de usuário privilegiado | US-001 | T-PUB-01 → T-SEC-01 | Desenhado | Após "Entrar", a secretária chega ao Painel |
| Login com informação faltando ou incorreta | US-001 | — | Não desenhado | O IFML-01 prevê a mensagem de erro sob o formulário, mas o estado de erro não foi exportado |
| Login de usuário não privilegiado | US-001 | T-PUB-01 | Parcial | A tela informa que o acesso é restrito por perfil, mas não há estado de recusa desenhado |

## Divergências

| ID | Divergência | Telas | Tratamento proposto |
|---|---|---|---|
| W-01 | O link "Esqueci a senha" e a opção "Manter conectada neste terminal" não têm história de usuário. | T-PUB-01 | Registrar a história de recuperação de senha ou retirar o link do protótipo |
| W-02 | A história fala em "login"; a tela pede "Matrícula". | T-PUB-01 | Registrar no glossário que o login da secretária é a matrícula do funcionário |

## Veja também

- [Wireframes: todos os perfis](../README.md)
- [Índice das telas](../indice-das-telas.md)
- [Site map](../../site-map.md)
- [Protótipo: revisão de usabilidade e pendências](../../README.md)
- [IFML](../../ifml/README.md)
- [Histórias de usuário](../../../03-requisitos/historias-de-usuario.md)
