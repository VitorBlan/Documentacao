# Diagramas de sequência

Milestone 2 · Modelagem UML (Engenharia de Software, Fábrica de Software I)

Vinte diagramas de sequência, de DS-001 a DS-020, descrevem a troca de mensagens entre o ator e os objetos do Bancada em cada caso de uso de cadastro, atualização, mudança de status e consulta. Eles foram desenhados na página `diagrama-sequencia` do arquivo do Figma "Gestão de Oficina Mecânica", e cada um tem uma issue no GitLab, com os cenários em Gherkin. O projeto ainda não tem código da aplicação; por isso os participantes são os objetos propostos nos próprios diagramas (telas, objetos de domínio, serviços e repositórios), e não camadas de uma aplicação existente.

## Cenários e diagramas

Os vínculos com caso de uso (UC), requisito (RF) e história (US) seguem o [catálogo de casos de uso](../../README.md#catálogo-de-casos-de-uso), o [mapeamento RF → US → UC](../../03-requisitos/requisitos-funcionais.md#mapeamento-rf--us--uc) e a [matriz de rastreabilidade](../../07-rastreabilidade/matriz.md). `UC-000` é o código usado no GitLab para diagrama de sequência sem caso de uso no catálogo, e `US-000`, para caso de uso sem história vinculada.

| DS | Cenário | Ator | UC | RF | US | Issue |
|---|---|---|---|---|---|---|
| [DS-001](<DS-001 _ UC-001 - Cadastro de cliente.svg>) | Cadastro de cliente | Secretária | UC-001 | RF-01 | US-003 | [#78](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/78) |
| [DS-002](<DS-002 _ UC-004 - Cadastro de um veículo.svg>) | Cadastro de um veículo | Secretária | UC-004 | RF-01 | US-006 | [#79](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/79) |
| [DS-003](<DS-003 _ UC-006 - Cadastro de um orçamento.svg>) | Cadastro de um orçamento | Secretária | UC-006 | RF-02 | US-016 | [#80](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/80) |
| [DS-004](<DS-004 _ UC-014 - Cadastro de uma OS.svg>) | Cadastro de uma OS | Secretária | UC-014 | RF-06 | US-008 | [#81](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/81) |
| [DS-005](<DS-005 _ UC-002 - Atualizar informações do cliente.svg>) | Atualização das informações do cliente | Secretária | UC-002 | RF-01 | US-004 | [#82](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/82) |
| [DS-006](<DS-006 _ UC-005 - Atualizar informações do veículo.svg>) | Atualização das informações do veículo | Secretária | UC-005 | RF-01 | US-007 | [#83](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/83) |
| [DS-007](<DS-007 _ UC-000 - Atualizar informações do orçamento.svg>) | Atualização das informações do orçamento | Secretária | UC-000 | RF-02 | US-000 | [#84](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/84) |
| [DS-008](<Ds-008_UC-013-mudar status de uma OS.svg>) | Mudança de status de uma OS | Secretária | UC-013 | RF-06 | US-002, US-028 | [#85](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/85) |
| [DS-009](<DS-009 _ UC-003 - Listagem de clientes cadastrados.svg>) | Listagem de clientes cadastrados | Secretária | UC-003 | RF-01 | US-010 | [#86](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/86) |
| [DS-010](<ds-010_UC_015-cadasrar distribuidora.svg>) | Cadastro de distribuidora | Estoquista | UC-015 | RF-05 | US-000 | [#87](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/87) |
| [DS-011](<DS-011 _ UC-016 - Atualizar informações da distribuidora.svg>) | Atualização das informações da distribuidora | Estoquista | UC-016 | RF-05 | US-000 | [#88](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/88) |
| [DS-012](<DS-012 _ UC-010 - Alterar o status do orçamento para aprovado.svg>) | Mudança do status do orçamento para aprovado | Secretária | UC-010 | RF-02 | US-017 | [#89](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/89) |
| [DS-013](<DS-013 _ UC-009 - Mudar o status do orçamento para recusado.svg>) | Mudança do status do orçamento para recusado | Secretária | UC-009 | RF-02 | US-017 | [#90](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/90) |
| [DS-014](<DS-014 _ UC-008 - Listagem de veículos cadastrados.svg>) | Listagem de veículos cadastrados | Secretária | UC-008 | RF-01 | US-011 | [#91](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/91) |
| [DS-015](<DS-015 _ UC-012 - Visualizar informações do veículo.svg>) | Visualização das informações do veículo | Secretária | UC-012 | RF-01 | US-014 | [#95](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/95) |
| [DS-016](<DS-016 _ UC-007 - Listagem de orçamentos cadastrados.svg>) | Listagem de orçamentos cadastrados | Secretária | UC-007 | RF-02 | US-000 | [#92](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/92) |
| [DS-017](<DS-017 _ UC-000 - Listar ordens de serviço cadastradas.svg>) | Listagem de ordens de serviço cadastradas | Secretária | UC-000 | RF-01 | US-012 | [#93](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/93) |
| [DS-018](<DS-018 _ UC-011 - Visualizar perfil do cliente.svg>) | Visualização do perfil do cliente | Secretária | UC-011 | RF-01 | US-013 | [#94](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/94) |
| [DS-019](<DS-019 _ UC-000 - Visualização as informações do orçamento.svg>) | Visualização das informações do orçamento | Secretária | UC-000 | RF-02 | US-000 | [#96](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/96) |
| [DS-020](<Ds-020_UC-000-visualização das informaçoes da OS.svg>) | Visualização das informações da OS | Secretária | UC-000 | RF-01 | US-015 | [#97](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/97) |

Na coluna US, DS-004 usa a US-008 (cadastro da OS) e DS-012 e DS-013 usam a US-017 (decisão do cliente sobre o orçamento), porque o GitLab vincula UC-014, UC-010 e UC-009 à US-028, que trata do status da OS. Ver a divergência 8 de [requisitos-funcionais.md](../../03-requisitos/requisitos-funcionais.md#divergências). Os UC-000 de DS-007, DS-017, DS-019 e DS-020 não têm caso de uso no catálogo; as histórias de DS-017 e DS-020 (US-012 e US-015) vêm do [índice de histórias](../../03-requisitos/historias-de-usuario.md#índice).

Os UC-001 a UC-016 do catálogo têm diagrama de sequência. Os UC-017 a UC-023 (peças, lista e informações de distribuidora e lista de relatórios) ainda não têm.

## Convenções de leitura

- **Ator**: aparece à esquerda, com o papel do catálogo: a Secretária (ACT-01) na maioria dos diagramas e o Estoquista em DS-010 e DS-011. O Estoquista não consta no catálogo de atores (lacuna L-14 de [lacunas.md](../../07-rastreabilidade/lacunas.md)).
- **Autenticação**: todos os casos de uso incluem "Autenticar" (`<<include>>`). O login não é desenhado; os diagramas partem da secretária já autenticada.
- **Participantes**: da esquerda para a direita, o ator, os objetos de tela (`:TelaPrincipal`, `:Menu`, `:TelaClientes` e semelhantes), o objeto de domínio ou o serviço (`:Cliente`, `:Orcamento`, `:ServicoOS`) e, por último, o repositório (`:RepositorioCliente`, `:RepositorioOS`). Nos diagramas de listagem de orçamentos e de OS, o objeto de domínio faz a consulta sem repositório.
- **Setas**: seta cheia é chamada; seta tracejada é retorno. DS-010 traz a legenda no desenho. Alguns diagramas numeram as mensagens (DS-006 a DS-010, DS-013, DS-016, DS-017 e DS-019); o DS-011 numera só as duas primeiras, e os outros não numeram.
- **Fragmentos**: `alt` tem uma guarda por ramo, entre colchetes. Em DS-005, DS-006 e DS-011 há um `alt` dentro de outro, para a checagem de duplicidade depois da validação dos campos obrigatórios.
- **Validação**: a validação de campos obrigatórios é uma autochamada (`validarObrigatorios()` ou `validarCampos(dados)`). O erro volta pela cadeia até a tela, que exibe a mensagem à secretária.
- **Diagramas de consulta**: não têm `alt`. Mostram o caminho do clique no menu ou na lista até a exibição dos dados.

## Clientes

### Cadastro de cliente (DS-001)

![Sequência: cadastro de cliente, da Secretária ao RepositorioCliente, com ramos de dados válidos, campo obrigatório vazio e valor duplicado](<DS-001 _ UC-001 - Cadastro de cliente.svg>)

A Secretária acessa "Clientes" na `:TelaPrincipal`, que abre a `:ListaClientes`, e envia `cadastrarCliente(dados)` ao `:NovoCliente`. Três ramos:

- **Dados válidos**: o `:NovoCliente` chama `salvar(dados)` no `:RepositorioCliente`, e o resultado volta à tela com "cliente cadastrado".
- **Campo obrigatório vazio** (nome, nascimento, contatos ou endereço): retorna `erro("Campo obrigatório não preenchido")`.
- **Valor duplicado** (telefone, e-mail ou CPF já cadastrado): retorna `erro("Campo de valor único duplicado")`.

A tela principal devolve à secretária "Sucesso ou msg de erro".

### Atualização das informações do cliente (DS-005)

![Sequência: atualização do cliente, com validação de campos obrigatórios e checagem de CPF, e-mail e telefone de outro cliente](<DS-005 _ UC-002 - Atualizar informações do cliente.svg>)

Na `:TelaEditarCliente`, a Secretária chama `clicarAtualizar(dados)`, e o `:Cliente` executa `atualizar(dados)` e `validarObrigatorios()`.

- **Campo obrigatório vazio**: retorna `erro("Campo obrigatório não preenchido")`, e a tela exibe a mensagem.
- **Campos preenchidos**: o `:RepositorioCliente` responde a `existeOutroCom(cpf, email, telefone)`. Se algum valor pertence a outro cliente, retorna `erro("Informação já cadastrada")`. Se os dados são inéditos, `salvar(cliente)` grava a alteração, e a `:TelaClientes` mostra a lista de clientes.

### Listagem de clientes cadastrados (DS-009)

![Sequência: listagem de clientes, do clique no item Clientes do menu à exibição da lista](<DS-009 _ UC-003 - Listagem de clientes cadastrados.svg>)

A Secretária clica no item "Clientes" do `:Menu`, que abre a `:TelaClientes`. A tela chama `listarTodos()` no `:RepositorioCliente` e exibe a lista recebida.

### Visualização do perfil do cliente (DS-018)

![Sequência do quadro DS-018, ainda com a estrutura de agenda de boxes: TelaPrincipal, AgendaDosBoxes, OS e RepositorioOS](<DS-018 _ UC-011 - Visualizar perfil do cliente.svg>)

A issue #94 prevê que a Secretária, na listagem de clientes, clique em um cliente e seja levada à tela de um único cliente, onde vê as informações dele. O desenho do quadro não mostra esse fluxo: tem outros participantes e mensagens ainda sem nome. Ver [Divergências](#divergências).

## Veículos

### Cadastro de um veículo (DS-002)

![Sequência: cadastro de veículo, da Secretária ao RepositorioVeiculo, com ramos de dados válidos, campo obrigatório vazio e veículo já cadastrado](<DS-002 _ UC-004 - Cadastro de um veículo.svg>)

Mesma estrutura do DS-001, com `:ListaVeiculos`, `:NovoVeiculo` e `:RepositorioVeiculo`. A Secretária envia `cadastrarVeiculo(dados)`, e os ramos são:

- **Dados válidos**: `salvar(dados)` e retorno "veículo cadastrado".
- **Campo obrigatório vazio** (marca, modelo, cor, ano, placa ou CPF): `erro("Campo obrigatório não preenchido")`.
- **Veículo já cadastrado**: `erro("Campo de valor único duplicado")`.

A placa é única no banco (`uk_veiculo_placa`).

### Atualização das informações do veículo (DS-006)

![Sequência: atualização do veículo, com validação de campos obrigatórios e checagem de placa de outro veículo](<DS-006 _ UC-005 - Atualizar informações do veículo.svg>)

Na `:TelaEditarVeiculo`, `clicarAtualizar(dados)` leva ao `:Veiculo`, que valida os obrigatórios. Se algum está vazio, retorna `erro("Campo obrigatório não preenchido")`. Com os campos preenchidos, o `:RepositorioVeiculo` responde a `existeOutroComPlaca(placa)`: placa de outro veículo retorna `erro("Informação já cadastrada")`; caso contrário, `salvar(veiculo)` grava e a `:TelaVeiculos` mostra a lista.

### Listagem de veículos cadastrados (DS-014)

![Sequência: listagem de veículos, do clique em Veículos no menu à exibição da lista](<DS-014 _ UC-008 - Listagem de veículos cadastrados.svg>)

A Secretária chama `clicarMenuVeiculos()` na `:TelaPrincipal`, que abre a `:TelaVeiculos`. A tela chama `listar()` no `:RepositorioVeiculo` e exibe a lista.

### Visualização das informações do veículo (DS-015)

![Sequência: visualização do veículo, do clique na lista à tela de detalhe](<DS-015 _ UC-012 - Visualizar informações do veículo.svg>)

Da `:TelaListaVeiculos`, `clicarVeiculo(id)` abre a `:TelaDetalheVeiculo`, que chama `buscarPorId(id)` no `:RepositorioVeiculo` e exibe as informações do veículo.

## Orçamentos

### Cadastro de um orçamento (DS-003)

![Sequência: cadastro de orçamento, com ramos de dados válidos, campo obrigatório vazio e veículo com orçamento em aberto](<DS-003 _ UC-006 - Cadastro de um orçamento.svg>)

A Secretária acessa "Orçamentos", e a `:ListaOrcamentos` envia `criarOrcamento(dados)` ao `:NovoOrcamento`. Ramos:

- **Dados válidos**: `salvar(dados)` no `:RepositorioOrcamento` e retorno "orçamento cadastrado".
- **Campo obrigatório vazio** (placa, quilometragem, valor ou forma de pagamento): `erro("Campo obrigatório não preenchido")`.
- **Veículo com orçamento em aberto**: `erro("O veículo já tem um orçamento aberto")`.

### Atualização das informações do orçamento (DS-007)

![Sequência: atualização do orçamento, com ramos de campo obrigatório vazio, orçamento aprovado ou recusado e orçamento aberto](<DS-007 _ UC-000 - Atualizar informações do orçamento.svg>)

Na `:TelaEditarOrcamento`, `clicarAtualizar(dados)` leva ao `:Orcamento`, que valida os obrigatórios. Três ramos:

- **Campo obrigatório vazio**: `erro("Campo obrigatório não preenchido")`.
- **Status "aprovada" ou "recusada"**: `erro("O orçamento não pode ser alterado nesse status")`.
- **Campos preenchidos e status "aberta"**: `salvar(orcamento)` e abertura da lista de orçamentos.

### Mudança do status do orçamento para aprovado (DS-012)

![Sequência: aprovação do orçamento, com ramos de orçamento orçado e de orçamento em outro status](<DS-012 _ UC-010 - Alterar o status do orçamento para aprovado.svg>)

A Secretária chama `clicarAprovar(id)` na `:TelaOrcamento`, e o `:Orcamento` executa `aprovar()` e `validarStatus()`.

- **Status "orçada"**: `mudarStatus(id, "aprovada")` no `:RepositorioOrcamento`, e a tela exibe o orçamento com o status atualizado.
- **Status "aberta", "aprovada" ou "recusada"**: retorna `erro("O orçamento não pode ser aprovado nesse status")`.

### Mudança do status do orçamento para recusado (DS-013)

![Sequência: recusa do orçamento, com ramos de orçamento orçado e de orçamento em outro status](<DS-013 _ UC-009 - Mudar o status do orçamento para recusado.svg>)

A Secretária abre o orçamento a partir da `:TelaOrcamentos` e chama `clicarRecusar()` na `:TelaOrcamento`; o `:Orcamento` executa `recusar(id)`.

- **Status "orçada"**: `mudarStatus(id, "recusada")` e exibição do orçamento com o status atualizado.
- **Status "aberta", "aprovada" ou "recusada"**: retorna `erro("O orçamento não pode ser recusado nesse status")`.

Existe também um arquivo único para os dois casos de uso, [DS-012 e DS-013 - Mudar o status do orçamento](<DS-012 _ UC-010 e DS-013 _ UC-009 - Mudar o status do orçamento.svg>), com um `alt` entre `aprovar(id)` e `recusar(id)`. Ele não tem a validação de status e usa "aprovado" e "recusado". Os diagramas deste documento são os dois separados, que seguem as issues #89 e #90.

### Listagem de orçamentos cadastrados (DS-016)

![Sequência: listagem de orçamentos, do clique em Orçamentos no menu à exibição da lista](<DS-016 _ UC-007 - Listagem de orçamentos cadastrados.svg>)

A Secretária clica em "Orçamentos" no `:Menu`, que abre a `:TelaOrçamentos`. A tela chama `listarOrcamentos()` no `:Orçamento` e exibe a lista.

### Visualização das informações do orçamento (DS-019)

![Sequência do quadro DS-019, que mostra o fluxo de clientes e de perfil do cliente, e não o de orçamento](<DS-019 _ UC-000 - Visualização as informações do orçamento.svg>)

A issue #96 prevê que a Secretária, na listagem de orçamentos, clique em um orçamento e seja levada à tela de um único orçamento, onde vê as informações dele. O desenho do quadro mostra o fluxo de clientes e de perfil do cliente. Ver [Divergências](#divergências).

## Ordens de serviço

### Cadastro de uma OS (DS-004)

![Sequência do quadro DS-004: da lista de orçamentos, iniciarExecucao(id) muda o status da OS para em execução](<DS-004 _ UC-014 - Cadastro de uma OS.svg>)

A issue #81 prevê dois cenários: com os campos preenchidos e o orçamento aprovado, a OS é registrada; com o orçamento ainda não aprovado, o app mostra a mensagem de que o orçamento não foi aprovado. O desenho do quadro mostra outra coisa: a partir da `:ListaOrcamentos`, `iniciarExecucao(id)` leva ao `:OS`, que chama `mudarStatus(id, "em execução")` no `:RepositorioOS`. Ver [Divergências](#divergências).

### Mudança de status de uma OS (DS-008)

![Sequência: mudança do status da OS para Concluída, da tela de atualização ao RepositorioOS e redirecionamento à lista de OS](<Ds-008_UC-013-mudar status de uma OS.svg>)

Na `:TelaAtualizacaoOS`, a Secretária chama `atualizar(idOS, "Concluída")`. O `:ServicoOS` executa `mudarStatus(idOS, "Concluída")`, o `:RepositorioOS` responde a `atualizarStatus(idOS, "Concluída")`, e o serviço busca `listarOS()`. A tela redireciona a secretária para a lista de OS. O diagrama cobre só a mudança para "Concluída", como a issue #85, e não tem `alt`. A [máquina de estados da OS](../estados/README.md) (DE-001) define o ciclo `aberta → em execução → concluída`.

### Listagem de ordens de serviço cadastradas (DS-017)

![Sequência: listagem de OS, do clique em Ordens de serviço no menu à exibição da lista](<DS-017 _ UC-000 - Listar ordens de serviço cadastradas.svg>)

A Secretária clica em "Ordens de serviço" no `:Menu`, que abre a `:TelaOrdensServico`. A tela chama `listarTodas()` no `:OrdemServico` e exibe a lista de OS.

### Visualização das informações da OS (DS-020)

![Sequência: visualização da OS, do clique na lista à tela de detalhe](<Ds-020_UC-000-visualização das informaçoes da OS.svg>)

Da `:TelaListaOS`, `clicarOS(id)` abre a `:TelaDetalheOS`, que chama `buscarPorId(id)` no `:RepositorioOS` e exibe as informações da OS.

## Distribuidoras

### Cadastro de distribuidora (DS-010)

![Sequência: cadastro de distribuidora, do Estoquista ao RepositorioDistribuidora, com ramos de campo obrigatório em branco, CNPJ já cadastrado e cadastro válido](<ds-010_UC_015-cadasrar distribuidora.svg>)

O Estoquista envia `cadastrar(razaoSocial, cnpj, telefone, email, logradouro, bairro)` na `:TelaCadastroDistribuidora`. O `:ServicoDistribuidora` valida os campos e consulta `buscarPorCnpj(cnpj)` no `:RepositorioDistribuidora`. Ramos:

- **Campo obrigatório em branco**: `erro("Campo obrigatório não preenchido")`.
- **CNPJ já cadastrado**: `erro("CNPJ já cadastrado")`.
- **Campos preenchidos e CNPJ novo**: `salvar(distribuidora)`, `listarTodas()` e exibição da lista de distribuidoras.

### Atualização das informações da distribuidora (DS-011)

![Sequência: atualização da distribuidora, com validação de campos obrigatórios e checagem de CNPJ de outra distribuidora](<DS-011 _ UC-016 - Atualizar informações da distribuidora.svg>)

Na `:TelaEditarDistribuidora`, `clicarSalvar(dados)` leva à `:Distribuidora`, que valida os obrigatórios. Se algum está em branco, retorna `erro("Campo obrigatório não preenchido")`. Com os campos preenchidos, o `:RepositorioDistribuidora` responde a `existeOutraCom(id, cnpj)`: CNPJ de outra distribuidora retorna `erro("CNPJ já cadastrado")`; CNPJ disponível grava com `salvar(distribuidora)`, e a `:TelaDetalheDistribuidora` mostra as informações atualizadas.

## Mensagens de validação

As mensagens de erro dos diagramas se repetem. A tabela mostra onde cada uma aparece.

| Mensagem | Regra | Onde aparece |
|---|---|---|
| "Campo obrigatório não preenchido" | Algum campo obrigatório está vazio | DS-001, DS-002, DS-003, DS-005, DS-006, DS-007, DS-010, DS-011 |
| "Campo de valor único duplicado" | CPF, e-mail, telefone ou placa já cadastrado, no cadastro | DS-001, DS-002 |
| "Informação já cadastrada" | CPF, e-mail, telefone ou placa pertence a outro registro, na atualização | DS-005, DS-006 |
| "CNPJ já cadastrado" | CNPJ já pertence a uma distribuidora | DS-010, DS-011 |
| "O veículo já tem um orçamento aberto" | Cadastro de orçamento para veículo com orçamento em aberto | DS-003 |
| "O orçamento não pode ser alterado nesse status" | Atualização de orçamento aprovado ou recusado | DS-007 |
| "O orçamento não pode ser aprovado nesse status" | Aprovação de orçamento que não está orçado | DS-012 |
| "O orçamento não pode ser recusado nesse status" | Recusa de orçamento que não está orçado | DS-013 |

CPF e placa são únicos no banco (`uk_pessoa_cpf` e `uk_veiculo_placa`, RF-01).

## Divergências

Diferenças entre os diagramas e as demais fontes do projeto (issues do GitLab, catálogo de casos de uso e requisitos). Onde há diferença, a issue do GitLab é a fonte de verdade, e o texto acima segue a issue.

1. **DS-004 não mostra o cadastro da OS.** O quadro desenha `iniciarExecucao(id)` e `mudarStatus(id, "em execução")` a partir da lista de orçamentos, sem criar a OS e sem a guarda de orçamento aprovado. A issue #81 descreve o cadastro da OS e a recusa quando o orçamento não foi aprovado.
2. **DS-018 e DS-019 têm o conteúdo trocado ou incompleto.** O DS-018 (perfil do cliente) ainda tem a estrutura de agenda de boxes (`:AgendaDosBoxes`, `:OS`, `:RepositorioOS`), com mensagens chamadas "texto" e "cliclo em ()". O DS-019 (informações do orçamento) mostra o fluxo de clientes e do perfil do cliente. Nenhum dos dois desenha o que as issues #94 e #96 descrevem, e a visualização do orçamento não tem diagrama.
3. **UC-000 em quatro diagramas.** DS-007, DS-017, DS-019 e DS-020 usam `UC-000`, isto é, não têm caso de uso no catálogo. DS-007 e DS-019 também não têm história de usuário (`US-000`).
4. **DS-012 e DS-013 em duas versões.** Há os dois diagramas separados e um arquivo único, que não valida o status e usa "aprovado" e "recusado". Os separados usam "aprovada" e "recusada", e o banco usa `aprovado` e `recusado`.
5. **Status do orçamento no DS-007.** O diagrama e a issue #84 usam "aberta", "aprovada" e "recusada" para o orçamento, e o banco usa `aberto`, `orçado`, `aprovado` e `recusado`. O orçamento "orçada" não cai em nenhum ramo: nem é salvo, nem recebe a mensagem de erro.
6. **DS-008 não valida o status de origem.** A mudança para "Concluída" não tem guarda, e o diagrama não mostra o registro de funcionário, data e hora que a RF-06 exige. A [máquina de estados](../estados/README.md) só permite concluir uma OS "Em execução".
7. **DS-003 e a história do orçamento.** O diagrama pede placa, quilometragem, valor e forma de pagamento, e a issue #80 cita só placa e valor. A US-016 tem o mecânico como ator, e o diagrama usa a Secretária. O diagrama também não separa peças e mão de obra, como pede a RF-02.
8. **Mensagem de erro de duplicidade.** DS-001 e DS-002 usam "Campo de valor único duplicado", e DS-005 e DS-006 usam "Informação já cadastrada", que é o texto das issues.
9. **Issues com texto de outro diagrama.** A issue #79 (DS-002), de cadastro de veículo, traz os cenários de atualização do veículo, copiados do DS-006. A issue #91 (DS-014) tem a funcionalidade "Listar clientes cadastrados" para uma lista de veículos.
10. **Quadros sem título e sem link.** Os desenhos de DS-011, DS-012, DS-014 e DS-015 não têm o título `DS-0XX | UC-0YY - <nome>`, e DS-012 e DS-015 também não têm o link da issue.
11. **Nomes de participantes.** O objeto de domínio aparece como `:OS` (DS-004 e DS-018) e como `:OrdemServico` (DS-017), e com e sem acento (`:Orçamento` em DS-016, `:Orcamento` nos demais). O DS-019 tem participantes sem os dois-pontos (`Clientes`, `Telas Clientes`). A convenção do projeto é o nome da classe do diagrama de domínio, em português, no singular e sem acento (`Orcamento`, `OrdemServico`).
12. **Nomes de arquivo fora do padrão.** O padrão é `DS-0XX _ UC-0YY - <nome>.svg`. Fogem dele `Ds-008_UC-013-mudar status de uma OS.svg`, `ds-010_UC_015-cadasrar distribuidora.svg` (com "cadasrar") e `Ds-020_UC-000-visualização das informaçoes da OS.svg`. O DS-012 está como "Alterar o status", e o título da issue é "Mudar o status".
13. **Estoquista fora do catálogo de atores.** DS-010 e DS-011 usam o Estoquista, que não consta em ACT-01 a ACT-03 (lacuna L-14). Os casos de uso de distribuidora não têm história vinculada.
14. **Casos de uso sem diagrama.** UC-017 a UC-023 não têm diagrama de sequência.

## Veja também

- [Casos de uso](../casos-de-uso/README.md): UC-001 a UC-016, aos quais os diagramas se ligam.
- [Classes](../classes/README.md): classes `Cliente`, `Veiculo`, `Orcamento` e `OrdemServico`, que os objetos de domínio dos diagramas representam.
- [Atividades](../atividades/README.md): o fluxo de trabalho que cada DS detalha (DA-001 a DA-016).
- [Estados](../estados/README.md): os status de OS e orçamento alterados em DS-008, DS-012 e DS-013.
- [Requisitos funcionais](../../03-requisitos/requisitos-funcionais.md)
- [Matriz de rastreabilidade](../../07-rastreabilidade/matriz.md)
- [Modelagem](../README.md)
