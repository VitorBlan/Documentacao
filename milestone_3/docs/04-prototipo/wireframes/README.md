# Wireframes

Fábrica de Software I · Protótipo em alta fidelidade · Desktop 1440×900 · Milestones 1 a 3

Índice dos wireframes do Bancada, exportados em SVG da página "[03 · Protótipo](https://www.figma.com/design/EHj0Js5KtOJ88TRQd5fTJI/Gest%C3%A3o-de-Oficina-Mec%C3%A2nica?node-id=3-36)" do Figma. Cada pasta corresponde a um perfil de acesso do [site map](../site-map.md) e tem um README com a tabela tela, caso de uso e história de usuário, as telas na ordem do fluxo, as regras de comportamento tiradas dos cenários Gherkin e as divergências encontradas.

| Perfil | Pasta | Telas (SVG) | Formato |
|---|---|---|---|
| Público (sem sessão) | [pub/](pub/README.md) | 1 | 1440x900 desktop |
| Secretária (ACT-01) | [secretaria/](secretaria/README.md) | 57 | 1440x900 desktop |
| Estoquista | [estoquista/](estoquista/README.md) | 30 (17 vigentes e 13 de versões anteriores) | 1440x900 desktop |
| Gerente | [gerente/](gerente/README.md) | 1 | 1440x900 desktop |

Total: 89 telas. A pasta [rotulos/](rotulos/README.md) guarda os títulos das seções da página do Figma, que não são telas. O [índice das telas](indice-das-telas.md) liga cada arquivo ao quadro de origem no Figma.

Convenção: telas numeradas na ordem do fluxo dentro de cada perfil (T-PUB, T-SEC, T-EST, T-GER), com o arquivo `NN-nome-da-tela.svg`, sem espaços nem acentos. Exemplo: `T-SEC-04` é o arquivo `secretaria/04-cadastro-de-cliente-informacoes-conflitantes.svg`. Dentro da pasta da secretária, as telas seguem a ordem das áreas do site map: Painel, Clientes, Veículos, Orçamentos, Ordens de serviço e Agenda dos boxes.

## Cobertura dos cenários de aceite

Os READMEs das pastas conferem cada cenário Gherkin das histórias contra as telas. Resultado em 20/11/2026:

| Perfil | Desenhado | Parcial | Não desenhado | Divergente |
|---|---|---|---|---|
| Público (sem sessão) | 1 | 1 | 1 | 0 |
| Secretária (ACT-01) | 24 | 11 | 11 | 2 |
| Estoquista | 4 | 4 | 3 | 0 |
| Gerente | 0 | 1 | 0 | 0 |
| **Total** | **29** | **17** | **15** | **2** |

O cenário de conclusão da US-028 aparece duas vezes na secretária, porque as Ordens de serviço e a Agenda dos boxes o atendem. A maior parte dos cenários não desenhados são estados de erro (placa duplicada, código duplicado, campo obrigatório em branco). As duas divergências são a OS sem formulário próprio (US-008) e o CPF bloqueado na edição do cliente (US-004).

O protótipo do modelo da disciplina (FilaCidadã), com as pastas por perfil deixadas pelo professor, está guardado em [conteudo antigo](<../conteudo antigo/README.md>).

## Veja também

- [Índice do protótipo e revisão de usabilidade](../README.md)
- [Site map](../site-map.md)
- [IFML](../ifml/README.md)
- [Histórias de usuário](../../03-requisitos/historias-de-usuario.md)
