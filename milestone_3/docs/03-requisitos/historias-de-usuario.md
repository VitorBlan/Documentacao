# Histórias de usuário

Milestone 1 · Fábrica de Software I · Unidade de requisitos

Este documento reúne as 30 histórias de usuário registradas como issues no GitLab do projeto, agrupadas pelo requisito funcional de origem (RF-01 a RF-07). Os critérios de aceite estão em Gherkin, no padrão completo `Dado` / `Quando` / `Então` / `E`. As divergências estão na seção [Divergências](#divergências), no fim do documento.

## Índice

| US | Título | Ator | RF | UC | MoSCoW | Tamanho | Issue |
|---|---|---|---|---|---|---|---|
| [US-001](#us-001-autenticação-da-secretária) | Autenticação da secretária | Secretária | RF-01 | (incluído em todos) | Must | M | [#5](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/5) |
| [US-002](#us-002-registro-de-retirada) | Registro de retirada | Secretária | RF-01 | UC-013 | Should | M | [#6](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/6) |
| [US-003](#us-003-cadastro-do-cliente) | Cadastro do cliente | Secretária | RF-01 | UC-001 | Must | M | [#7](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/7) |
| [US-004](#us-004-atualização-cadastral-do-cliente) | Atualização cadastral do cliente | Secretária | RF-01 | UC-002 | Should | M | [#8](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/8) |
| [US-005](#us-005-relatórios-e-controle-de-acesso) | Relatórios e controle de acesso | Secretária | RF-01 | Nenhum | Should | M | [#9](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/9) |
| [US-006](#us-006-cadastro-do-carro) | Cadastro do carro | Secretária | RF-01 | UC-004 | Must | M | [#10](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/10) |
| [US-007](#us-007-atualização-cadastral-do-carro) | Atualização cadastral do carro | Secretária | RF-01 | UC-005 | Should | M | [#11](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/11) |
| [US-008](#us-008-cadastro-da-os) | Cadastro da OS | Secretária | RF-01 | Nenhum | Must | M | [#12](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/12) |
| [US-009](#us-009-atualização-da-os) | Atualização da OS | Secretária | RF-01 | Nenhum | Should | M | [#14](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/14) |
| [US-010](#us-010-listagem-de-clientes-cadastrados) | Listagem de clientes cadastrados | Secretária | RF-01 | UC-003 | Should | M | [#16](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/16) |
| [US-011](#us-011-listagem-de-carros-cadastrados) | Listagem de carros cadastrados | Secretária | RF-01 | UC-008 | Should | M | [#18](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/18) |
| [US-012](#us-012-listagem-de-os) | Listagem de OS | Secretária | RF-01 | Nenhum | Should | M | [#19](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/19) |
| [US-013](#us-013-visualizar-perfil-do-cliente) | Visualizar perfil do cliente | Vendedora | RF-01 | UC-011 | Should | M | [#20](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/20) |
| [US-014](#us-014-visualização-das-informações-do-carro) | Visualização das informações do carro | Vendedora | RF-01 | UC-012 | Should | M | [#21](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/21) |
| [US-015](#us-015-visualização-das-informações-de-uma-os) | Visualização das informações de uma OS | Secretária | RF-01 | Nenhum | Should | M | [#23](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/23) |
| [US-016](#us-016-montar-o-orçamento-da-os) | Montar o orçamento da OS | Mecânico | RF-02 | UC-006 | Must | M | [#22](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/22) |
| [US-017](#us-017-registrar-a-decisão-do-cliente-sobre-o-orçamento) | Registrar a decisão do cliente sobre o orçamento | Secretária | RF-02 | Nenhum | Must | M | [#24](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/24) |
| [US-018](#us-018-consultar-o-histórico-de-status-do-orçamento) | Consultar o histórico de status da OS | Gestor da oficina | RF-02 | Nenhum | Should | S | [#25](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/25) |
| [US-019](#us-019-cadastrar-uma-nova-peça-no-estoque) | Cadastrar uma nova peça no estoque | Funcionário de estoque | RF-05 | UC-017 | Must | M | [#31](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/31) |
| [US-020](#us-020-visualizar-os-detalhes-de-uma-peça) | Visualizar os detalhes de uma peça | Funcionário de atendimento | RF-05 | UC-021 | Must | S | [#32](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/32) |
| [US-021](#us-021-editar-as-informações-de-uma-peça) | Editar as informações de uma peça | Funcionário de atendimento | RF-05 | UC-022 | Must | S | [#33](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/33) |
| [US-022](#us-022-excluir-uma-peça-do-estoque) | Excluir uma peça do estoque | Funcionário de atendimento | RF-05 | Nenhum | Must | S | [#34](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/34) |
| [US-023](#us-023-filtrar-e-buscar-peças-no-estoque) | Filtrar e buscar peças no estoque | Funcionário de atendimento | RF-05 | Nenhum | Must | M | [#35](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/35) |
| [US-024](#us-024-ser-alertado-quando-o-estoque-de-uma-peça-estiver-baixo) | Ser alertado quando o estoque de uma peça estiver baixo | Funcionário de atendimento | RF-05 | Nenhum | Must | S | [#36](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/36) |
| [US-026](#us-026-filtrar-orçamentos-por-status) | Filtrar | Secretária | RF-02 | Nenhum | Should | M | [#37](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/37) |
| [US-027](#us-027-consultar-o-histórico-de-status-da-os) | Consultar o histórico de status da OS | Gestor da oficina | RF-06 | Nenhum | Must | M | [#39](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/39) |
| [US-028](#us-028-alterar-os-status-de-uma-os) | Alterar os status de uma OS | Secretária | RF-06 | UC-009, UC-010, UC-014 | Must | M | [#40](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/40) |
| [US-029](#us-029-consultar-histórico-do-veículo-por-placa) | Consultar histórico do veículo por placa | Secretária | RF-04 | Nenhum | Must | M | [#41](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/41) |
| [US-030](#us-030-ver-orientação-inicial-ao-abrir-a-tela-de-histórico) | Ver orientação inicial ao abrir a tela de histórico | Secretária | RF-04 | Nenhum | Could | M | [#50](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/50) |
| [#42](#42-organizar-minha-agenda-de-boxes) | Organizar minha agenda de boxes | Mecânico | RF-03 | Nenhum | Must | M | [#42](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/42) |

A coluna UC reproduz os vínculos do GitLab (título das issues de caso de uso). Os vínculos que parecem incorretos estão na divergência 8 de [requisitos-funcionais.md](requisitos-funcionais.md#divergências).

Não existe US-025 no GitLab. A história de agenda de boxes está registrada como "US -20", número que já pertence à US-020; por isso é citada pela issue (#42). Ver [Divergências](#divergências), D-01.

## Convenções

- **Formato.** **Como** `<ator>`, **quero** `<ação>`, **para** `<benefício>`. O texto é o da issue, com correção ortográfica.
- **Prioridade.** MoSCoW convertido da label `prio::` (P1 = Must, P2 = Should, P3 = Could). Tamanho conforme a label `size::`.
- **Critérios de aceite.** Gherkin em português: `Funcionalidade`, `Cenário`, `Dado`, `Quando`, `Então`, `E`. Quando o critério da issue começava pelo resultado, ou colocava uma ação no `Então`, os passos foram reorganizados no padrão, sem mudar o sentido.
- **Tags.** `@proposta`: cenário escrito pela equipe de documentação, porque a issue não tem critério de aceite ou o critério registrado não é verificável; deve ser revisado e copiado para a issue. `@divergencia`: cenário que contradiz o banco ou uma decisão do grupo; a divergência correspondente está no fim do documento.
- **Ator.** Pela premissa do Milestone 1, só a secretária (ACT-01) tem login. Mecânico (ACT-02) e gestor da oficina (ACT-03) aparecem nas histórias como partes interessadas.

## RF-01 Atendimento

### US-001 Autenticação da secretária

**Como** funcionária de atendimento (secretária), **quero** ter acesso privilegiado ao sistema, **para** cadastrar carros e clientes e modificar o status de clientes, OS e carros.

- Issue: [#5](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/5) · RF-01 · UC: autenticação incluída em todos os casos de uso · Must · M

```gherkin
Funcionalidade: Autenticação da secretária

  Cenário: login com credenciais de usuário privilegiado
    Dado que tenho o login e a senha de um usuário privilegiado
    Quando informo essas credenciais na tela de login
    Então sou redirecionada para a seção de funcionários
    E as operações privilegiadas ficam liberadas (cadastro, atualização de informações do cliente etc.)

  Cenário: login com informação faltando ou incorreta
    Dado que o login ou a senha de um usuário privilegiado está faltando ou incorreto
    Quando tento fazer login no sistema
    Então o login falha

  Cenário: login de usuário não privilegiado
    Dado que tenho o login e a senha de um usuário não privilegiado
    Quando tento fazer login no sistema
    Então o login falha
```

### US-002 Registro de retirada

**Como** secretária, **quero** modificar o status do veículo para retirado, **para** que o veículo seja liberado da oficina para o cliente.

- Issue: [#6](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/6) · RF-01 · UC-013 · Should · M

```gherkin
Funcionalidade: Registro de retirada do veículo

  @divergencia
  Cenário: retirada com dados corretos
    Dado que estou autenticada
    E o cliente que vai retirar o veículo, cadastrado previamente, informou seus dados corretamente
    Quando modifico o status do veículo para retirado
    Então o status do veículo é alterado para retirado
    E o cliente pode se dirigir ao carro e retirá-lo

  Cenário: dados do veículo incorretos
    Dado que estou autenticada
    E o cliente informou os dados do veículo incorretamente
    Quando tento buscar o veículo no sistema
    Então o pedido de retirada é negado

  @proposta
  Cenário: retirada por pessoa diferente da que entregou o veículo
    Dado que estou autenticada
    E o veículo foi entregue na oficina por outra pessoa
    Quando tento registrar a retirada
    Então a retirada é negada
    E vejo o aviso de que o veículo só pode ser retirado por quem o entregou
```

O terceiro cenário vem da regra aprovada na reunião de 26/08. O primeiro depende de um status do veículo que o banco não tem (D-08).

### US-003 Cadastro do cliente

**Como** secretária, **quero** registrar o cliente para a elaboração da OS ou a retirada do veículo, **para** saber quem apresentou e quem retirou o veículo.

- Issue: [#7](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/7) · RF-01 · UC-001 · Must · M

```gherkin
Funcionalidade: Cadastro de cliente

  Cenário: cadastro com dados válidos
    Dado que estou autenticada
    E tenho as informações pessoais do cliente que trouxe o veículo: nome completo, CPF, telefone celular e endereço (rua, número, bairro e CEP)
    Quando preencho a seção de cadastro de cliente e confirmo
    Então um novo cliente é cadastrado na oficina

  Cenário: CPF já cadastrado
    Dado que estou autenticada
    E tenho as informações pessoais de um cliente cujo CPF já está no sistema
    Quando tento cadastrá-lo na seção de cadastro de cliente
    Então o cadastramento falha
    E nenhum cliente novo é cadastrado
```

O banco também exige e-mail único, data de nascimento e contato preferencial (`pessoa`), campos que a história não cita.

### US-004 Atualização cadastral do cliente

**Como** secretária, **quero** atualizar as informações de um cliente no sistema, **para** que, quando o serviço for concluído ou por uma questão de marketing, eu possa entrar em contato por um telefone ou e-mail correto.

- Issue: [#8](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/8) · RF-01 · UC-002 · Should · M

```gherkin
Funcionalidade: Atualização cadastral do cliente

  Cenário: atualização de dados desatualizados
    Dado que estou autenticada
    E há informações desatualizadas do cliente no sistema
    Quando informo as novas informações do cliente e confirmo
    Então as informações do cliente são atualizadas

  Cenário: CPF já cadastrado para outro cliente
    Dado que estou autenticada
    E o novo CPF informado já consta como cadastrado no sistema
    Quando tento atualizar as informações do cliente
    Então a atualização é negada
    E é emitido um aviso de conflito de CPF
```

### US-005 Relatórios e controle de acesso

**Como** secretária, **quero** uma interface de busca de clientes, **para** pesquisar clientes já cadastrados.

- Issue: [#9](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/9) · RF-01 · UC: nenhum · Should · M

```gherkin
Funcionalidade: Busca de clientes cadastrados

  Cenário: busca pelo nome do cliente
    Dado que preciso das informações cadastradas de um cliente
    Quando pesquiso o nome do cliente na seção de busca de clientes
    Então encontro os dados do cliente já cadastrado
```

O título da issue fala em relatórios e controle de acesso, mas o conteúdo trata de busca de clientes (D-04).

### US-006 Cadastro do carro

**Como** secretária, **quero** registrar o carro do cliente para a elaboração da OS ou a retirada do veículo, **para** saber qual veículo foi apresentado e retirado pelo cliente.

- Issue: [#10](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/10) · RF-01 · UC-004 · Must · M

```gherkin
Funcionalidade: Cadastro de veículo

  Cenário: cadastro com placa nova
    Dado que estou autenticada
    E o cadastro do cliente já foi realizado
    E tenho as informações do carro: placa, marca, modelo e ano
    Quando preencho a seção de cadastro de veículo e confirmo
    Então um novo veículo é cadastrado no sistema

  Cenário: placa já cadastrada
    Dado que estou autenticada
    E o cadastro do cliente já foi realizado
    E tenho a marca, o modelo, o ano e uma placa de veículo já cadastrada
    Quando tento cadastrar um novo veículo com essa placa
    Então o cadastramento falha
    E é emitido um alerta de placa já cadastrada
```

### US-007 Atualização cadastral do carro

**Como** secretária, **quero** atualizar as informações do carro do cliente, **para** saber qual veículo foi apresentado e retirado pelo cliente e corrigir possíveis erros críticos.

- Issue: [#11](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/11) · RF-01 · UC-005 · Should · M

```gherkin
Funcionalidade: Atualização cadastral do veículo

  Cenário: atualização com dados válidos
    Dado que estou autenticada
    E o cadastro do cliente já foi realizado
    E tenho as informações a serem atualizadas do carro
    Quando preencho a seção de atualização do veículo e confirmo
    Então as informações do veículo são atualizadas no sistema

  Cenário: placa já cadastrada em outro veículo
    Dado que estou autenticada
    E as informações a serem atualizadas incluem uma placa que já está cadastrada
    Quando tento atualizar as informações do veículo
    Então a atualização é negada
    E é emitido um alerta de placa já cadastrada
```

### US-008 Cadastro da OS

**Como** secretária, **quero** registrar uma ordem de serviço, **para** que ela fique guardada no sistema e possa ser visualizada e atualizada pelo mecânico chefe.

- Issue: [#12](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/12) · RF-01 · UC: nenhum · Must · M

```gherkin
Funcionalidade: Cadastro de ordem de serviço

  @divergencia
  Cenário: cadastro com a descrição do problema
    Dado que estou autenticada
    Quando abro a seção de cadastro de OS, preencho a descrição do problema e confirmo
    Então uma nova OS é registrada no sistema
    E o campo situação fica, por padrão, como "em andamento"

  Cenário: descrição do problema em branco
    Dado que estou autenticada
    Quando tento cadastrar uma nova OS sem preencher a descrição do problema
    Então o cadastro não é efetivado
    E é emitido um alerta de campo obrigatório não preenchido
```

Pela regra do grupo, a OS só nasce de um orçamento aprovado, com status inicial `aberta` (D-07).

### US-009 Atualização da OS

**Como** secretária, **quero** atualizar as informações de uma OS, **para** que eventuais erros possam ser corrigidos.

- Issue: [#14](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/14) · RF-01 · UC: nenhum · Should · M

```gherkin
Funcionalidade: Atualização de ordem de serviço

  Cenário: atualização com campos obrigatórios preenchidos
    Dado que estou autenticada
    E tenho as novas informações a serem registradas
    Quando preencho os campos obrigatórios e confirmo a atualização
    Então as informações da OS são atualizadas

  Cenário: carro que já tem OS registrada
    Dado que estou autenticada
    E o carro já tem uma OS registrada
    Quando tento atualizar as informações da OS
    Então a atualização é negada
    E é emitido um aviso de conflito de mais de uma OS para o mesmo carro

  Cenário: descrição da OS em branco
    Dado que estou autenticada
    Quando tento atualizar a OS sem preencher a descrição
    Então a atualização é negada
    E é emitido um aviso de ausência de campo obrigatório
```

### US-010 Listagem de clientes cadastrados

**Como** secretária, **quero** visualizar a lista de clientes, **para** saber quais clientes estão cadastrados e poder realizar ações em cada registro.

- Issue: [#16](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/16) · RF-01 · UC-003 · Should · M

```gherkin
Funcionalidade: Listagem de clientes

  Cenário: abrir a lista de clientes
    Dado que estou autenticada
    Quando abro a seção de lista de clientes
    Então os clientes cadastrados são listados na tela
    E tenho acesso à visualização do perfil e à atualização das informações de cada cliente
```

### US-011 Listagem de carros cadastrados

**Como** secretária, **quero** visualizar a lista de carros, **para** saber quais carros estão cadastrados e poder realizar ações em cada registro.

- Issue: [#18](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/18) · RF-01 · UC-008 · Should · M

```gherkin
Funcionalidade: Listagem de veículos

  Cenário: abrir a lista de carros
    Dado que estou autenticada
    Quando abro a seção de lista de carros
    Então os carros cadastrados são listados na tela
    E tenho acesso à visualização e à atualização das informações de cada carro
```

### US-012 Listagem de OS

**Como** secretária, **quero** visualizar a lista de OS, **para** saber quais OS estão cadastradas e poder realizar ações em cada registro.

- Issue: [#19](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/19) · RF-01 · UC: nenhum · Should · M

```gherkin
Funcionalidade: Listagem de ordens de serviço

  Cenário: abrir a lista de OS
    Dado que estou autenticada
    Quando abro a seção de lista de OS
    Então as OS cadastradas são listadas na tela
    E tenho acesso à visualização e à atualização das informações de cada OS
```

### US-013 Visualizar perfil do cliente

**Como** vendedora, **quero** visualizar o perfil do cliente, **para** saber as informações cadastradas e os carros vinculados a ele, além de poder atualizar suas informações, caso necessário.

- Issue: [#20](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/20) · RF-01 · UC-011 · Should · M

```gherkin
Funcionalidade: Perfil do cliente

  Cenário: abrir o perfil de um cliente
    Dado que estou autenticada
    Quando abro a seção de visualização do perfil do cliente
    Então vejo as informações do cliente
    E vejo os carros vinculados a ele
    E posso atualizar o perfil, se necessário
```

### US-014 Visualização das informações do carro

**Como** vendedora, **quero** visualizar o perfil de um carro, **para** saber as informações cadastradas para ele.

- Issue: [#21](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/21) · RF-01 · UC-012 · Should · M

```gherkin
Funcionalidade: Informações do veículo

  Cenário: abrir as informações de um carro
    Dado que estou autenticada
    Quando abro a seção de visualização do perfil do carro
    Então vejo as informações do carro
    E posso acessar a atualização das informações desse carro, se necessário
```

### US-015 Visualização das informações de uma OS

**Como** secretária, **quero** visualizar as informações de uma OS cadastrada, **para** saber as informações registradas para ela.

- Issue: [#23](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/23) · RF-01 · UC: nenhum · Should · M

```gherkin
Funcionalidade: Informações da ordem de serviço

  Cenário: abrir as informações de uma OS
    Dado que estou autenticada
    Quando abro a seção de visualização de uma OS cadastrada
    Então vejo as informações da OS
    E posso acessar a atualização dessas informações
```

## RF-02 Orçamento e aprovação do serviço

### US-016 Montar o orçamento da OS

**Como** mecânico, **quero** lançar no orçamento os serviços e as peças de que o veículo precisa, com quantidade e valor, **para** que o balcão apresente ao cliente um valor com origem técnica, e não um chute.

- Issue: [#22](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/22) · RF-02 · UC-006 · Must · M

```gherkin
Funcionalidade: Montagem do orçamento

  @divergencia
  Cenário: lançar serviços e peças
    Dado uma OS "Aberta" com o problema relatado registrado na entrada
    Quando lanço os serviços e as peças previstas, cada um com quantidade e valor unitário
    Então a OS passa a "Orçada", com o total e a divisão entre peças e mão de obra
    E o orçamento fica registrado com o meu nome e a data/hora
```

No banco, "aberto" e "orçado" são status do orçamento, não da OS (D-06).

### US-017 Registrar a decisão do cliente sobre o orçamento

**Como** secretária, **quero** registrar se o cliente aprovou ou recusou o orçamento, e por qual meio ele respondeu, **para** que nenhum serviço comece sem autorização e a oficina tenha como comprovar depois.

- Issue: [#24](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/24) · RF-02 · UC: nenhum (sugerido: UC-009 e UC-010) · Must · M

```gherkin
Funcionalidade: Decisão do cliente sobre o orçamento

  @divergencia
  Cenário: recusa do cliente
    Dado uma OS "Orçada"
    Quando registro a recusa do cliente com o motivo
    Então a OS passa a "Recusada"
    E o veículo entra na lista de liberação para devolução

  Cenário: aprovação sem orçamento
    Dado uma OS ainda "Aberta", sem orçamento
    Quando tento registrar uma aprovação
    Então a ação é recusada, porque não há valor a aprovar

  @proposta
  Cenário: aprovação do cliente
    Dado um orçamento "orçado"
    Quando registro a aprovação do cliente e o meio da resposta (presencial, telefone ou mensagem)
    Então o orçamento passa a "aprovado"
    E o histórico grava o meio da resposta, o meu nome e a data/hora
    E a OS pode ser criada a partir desse orçamento
```

A issue não tem o cenário de aprovação, que é o caminho principal da história (D-05).

### US-018 Consultar o histórico de status do orçamento

**Como** gestor da oficina, **quero** ver cada transição de status da OS com autor e data/hora, **para** saber onde o atendimento travou quando o cliente reclama da demora.

- Issue: [#25](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/25) · RF-02 · UC: nenhum · Should · S

```gherkin
Funcionalidade: Histórico de status do orçamento

  Cenário: orçamento que já passou por vários status
    Dado uma OS que já passou por Aberta, Orçada e Aprovada
    Quando abro o histórico da OS
    Então vejo cada transição em ordem, com status anterior, novo status, autor e data/hora
    E a transição de aprovação mostra também o meio pelo qual o cliente respondeu

  Cenário: orçamento recém-aberto
    Dado uma OS recém-aberta
    Quando abro o histórico
    Então vejo apenas o registro da abertura, e não uma tela vazia sem explicação

  Cenário: tentativa de alterar o histórico
    Dado uma transição já gravada no histórico
    Quando tento alterá-la ou removê-la
    Então a ação é recusada, porque o histórico não é editável
```

O título da issue fala em OS, mas os status citados (Aberta, Orçada, Aprovada) são do orçamento; por isso o título desta seção usa "orçamento". A US-027 tem o mesmo título (D-02). No protótipo, esta história é a tela principal de Orçamentos.

### US-026 Filtrar orçamentos por status

**Como** secretária, **quero** filtrar os orçamentos já criados por status (aberto, orçado, aprovado), **para** encontrar meus orçamentos já criados.

- Issue: [#37](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/37) · RF-02 · UC: nenhum · Should · M

```gherkin
Funcionalidade: Filtro de orçamentos por status

  Cenário: filtro registrado na issue
    Dado um orçamento já criado
    Quando procuro pelos status
    Então posso filtrar entre aberto, orçado e aprovado

  @proposta
  Cenário: filtrar por um status
    Dado que existem orçamentos com status aberto, orçado, aprovado e recusado
    Quando seleciono o status "orçado" no filtro
    Então vejo apenas os orçamentos com status orçado

  @proposta
  Cenário: status sem orçamentos
    Dado que nenhum orçamento está com status recusado
    Quando seleciono o status "recusado" no filtro
    Então vejo a mensagem de que não há orçamentos com esse status
```

A issue diz "filtrar as OS", mas se refere a orçamentos, e não cita o status recusado, que o protótipo tem (D-06).

## RF-03 Distribuição do trabalho entre boxes e mecânicos

### #42 Organizar minha agenda de boxes

**Como** mecânico, **quero** saber como será a minha rotina de trabalho, **para** organizar minha agenda de boxes.

- Issue: [#42](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/42) (título "US -20") · RF-03 · UC: nenhum · Must · M

```gherkin
Funcionalidade: Agenda dos boxes

  Cenário: critério registrado na issue
    Dado que preciso saber em quais carros vou trabalhar
    Quando chega uma ordem de serviço
    Então uma ordem de serviço é designada a mim

  @proposta
  Cenário: designar box e mecânico a uma OS
    Dado uma OS aberta, aguardando designação
    E um box livre
    Quando a secretária designa o box e o mecânico para a OS
    Então a OS aparece na agenda do mecânico, com o box designado

  @proposta
  Cenário: box ocupado
    Dado um box com uma OS em execução
    Quando a secretária tenta designar outra OS para esse box
    Então a designação é recusada
    E o box aparece como indisponível
```

## RF-04 Histórico do veículo

### US-029 Consultar histórico do veículo por placa

**Como** secretária, **quero** consultar o histórico de um veículo informando a placa, **para** ver rapidamente os atendimentos, a quilometragem e os valores registrados na oficina.

- Issue: [#41](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/41) · RF-04 · UC: nenhum · Must · M

```gherkin
Funcionalidade: Histórico do veículo por placa

  Cenário: placa cadastrada
    Dado que estou na tela de Histórico do veículo
    Quando informo a placa "RTA4C21" e clico em Consultar
    Então vejo os dados do veículo (modelo, proprietário, cliente desde)
    E vejo a última quilometragem, o total de atendimentos e a data do último atendimento
    E vejo a lista de ordens de serviço vinculadas, das mais recentes para as mais antigas

  Cenário: placa não cadastrada
    Dado que informei uma placa que não está cadastrada no sistema
    Quando clico em Consultar
    Então vejo a mensagem "Placa não encontrada"
    E vejo a indicação para conferir o número ou cadastrar o veículo em Cadastros › Veículos

  Cenário: placa em branco
    Dado que estou na tela de Histórico do veículo
    Quando clico em Consultar sem informar nenhuma placa
    Então a consulta não é realizada
    E recebo a indicação de que o campo placa precisa ser preenchido
```

### US-030 Ver orientação inicial ao abrir a tela de histórico

**Como** secretária, **quero** ver instruções claras de como buscar quando abro a tela de histórico do veículo, **para** saber o que fazer sem precisar perguntar a alguém.

- Issue: [#50](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/50) (título "US - 030") · RF-04 · UC: nenhum · Could · M

```gherkin
Funcionalidade: Orientação inicial do histórico do veículo

  Cenário: primeira abertura da tela
    Dado que abro a tela de Histórico do veículo sem ter feito nenhuma consulta nesta sessão
    Quando a tela carrega
    Então vejo o campo de placa vazio, com o exemplo de formato "Ex.: ABC1D23"
    E vejo a mensagem "Nenhuma consulta realizada"

  Cenário: limpar a placa após uma consulta
    Dado que já tenho o resultado de uma consulta anterior na tela
    Quando limpo o campo de placa
    Então a tela volta ao estado inicial, sem os resultados da consulta anterior
```

O texto da issue registra a prioridade como "Desejável (a confirmar com o cliente)".

## RF-05 Controle de estoque de peças

As seis histórias de estoque não têm critério de aceite no GitLab. Os cenários abaixo são propostas derivadas do texto da RF-05 e das restrições da tabela `peca` (D-03).

### US-019 Cadastrar uma nova peça no estoque

**Como** funcionário de estoque, **quero** cadastrar uma nova peça informando nome, código, quantidade, valor unitário e fornecedor, **para** manter o estoque atualizado com o que a oficina realmente tem disponível.

- Issue: [#31](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/31) · RF-05 · UC-017 · Must · M

```gherkin
Funcionalidade: Cadastro de peça

  @proposta
  Cenário: cadastro com dados válidos
    Dado que estou autenticada
    E tenho o nome, o código, a categoria, a quantidade, o valor unitário e o fornecedor da peça
    Quando preencho o cadastro de peça e confirmo
    Então a peça é cadastrada no estoque
    E passa a aparecer na listagem de peças

  @proposta
  Cenário: código já cadastrado
    Dado que estou autenticada
    E o código informado já pertence a outra peça
    Quando tento cadastrar a peça
    Então o cadastro é recusado
    E vejo o aviso de código já cadastrado

  @proposta
  Cenário: quantidade negativa
    Dado que estou autenticada
    Quando tento cadastrar uma peça com quantidade menor que zero
    Então o cadastro é recusado
    E vejo o aviso de que a quantidade não pode ser negativa
```

### US-020 Visualizar os detalhes de uma peça

**Como** funcionário de atendimento, **quero** abrir os detalhes de uma peça cadastrada, com seu histórico de alterações, **para** conferir quantidade, valor e fornecedor antes de usá-la numa OS ou informar o cliente.

- Issue: [#32](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/32) · RF-05 · UC-021 · Must · S

```gherkin
Funcionalidade: Detalhes da peça

  @proposta
  Cenário: abrir os detalhes de uma peça
    Dado que estou autenticada
    E a peça está cadastrada no estoque
    Quando abro os detalhes da peça
    Então vejo nome, código, categoria, quantidade, estoque mínimo, valor unitário e fornecedor
    E vejo o histórico de alterações da peça
```

### US-021 Editar as informações de uma peça

**Como** funcionário de atendimento, **quero** corrigir ou atualizar os dados de uma peça já cadastrada, mantendo visível o que mudou, **para** manter o estoque condizente com a realidade (preço reajustado, novo fornecedor etc.) sem perder o histórico da peça.

- Issue: [#33](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/33) · RF-05 · UC-022 · Must · S

```gherkin
Funcionalidade: Edição de peça

  @proposta
  Cenário: reajuste de preço
    Dado que estou autenticada
    E a peça está cadastrada com valor unitário de R$ 80,00
    Quando altero o valor unitário para R$ 92,00 e confirmo
    Então a peça passa a ter o valor de R$ 92,00
    E o histórico da peça registra o valor anterior, o novo valor, o meu nome e a data/hora

  @proposta
  Cenário: código de outra peça
    Dado que estou autenticada
    Quando altero o código da peça para um código que já pertence a outra peça
    Então a alteração é recusada
    E vejo o aviso de código já cadastrado
```

### US-022 Excluir uma peça do estoque

**Como** funcionário de atendimento, **quero** remover do estoque uma peça que a oficina parou de usar, **para** não poluir a listagem com itens que não fazem mais sentido, sem perder o rastro de peças já usadas em OS antigas.

- Issue: [#34](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/34) · RF-05 · UC: nenhum · Must · S

```gherkin
Funcionalidade: Exclusão de peça

  @proposta
  Cenário: excluir peça já usada em OS antiga
    Dado que estou autenticada
    E a peça foi usada em uma OS concluída
    Quando excluo a peça do estoque
    Então a peça deixa de aparecer na listagem de peças
    E a OS concluída continua mostrando a peça utilizada
```

O banco atende a história por exclusão lógica (`peca_ativo`).

### US-023 Filtrar e buscar peças no estoque

**Como** funcionário de atendimento, **quero** filtrar as peças por nome, código ou categoria, combinando mais de um critério, **para** encontrar rapidamente o item de que preciso sem rolar a lista inteira.

- Issue: [#35](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/35) · RF-05 · UC: nenhum · Must · M

```gherkin
Funcionalidade: Filtro de peças

  @proposta
  Cenário: combinar categoria e nome
    Dado que estou autenticada
    E há peças cadastradas em várias categorias
    Quando filtro pela categoria "Freios" e pelo nome "pastilha"
    Então vejo apenas as peças da categoria Freios cujo nome contém "pastilha"

  @proposta
  Cenário: filtro sem resultado
    Dado que estou autenticada
    Quando filtro por um código que não existe no estoque
    Então vejo a mensagem de que nenhuma peça foi encontrada
```

### US-024 Ser alertado quando o estoque de uma peça estiver baixo

**Como** funcionário de atendimento, **quero** ser avisado quando a quantidade de uma peça atingir o nível mínimo definido, **para** providenciar a reposição antes que ela falte numa OS.

- Issue: [#36](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/36) · RF-05 · UC: nenhum · Must · S

```gherkin
Funcionalidade: Alerta de estoque baixo

  @proposta
  Cenário: quantidade chega ao mínimo
    Dado que estou autenticada
    E a peça tem estoque mínimo de 5 unidades e 6 unidades em estoque
    Quando uma unidade da peça é usada em uma OS
    Então a peça passa a aparecer com o alerta de estoque baixo

  @proposta
  Cenário: quantidade acima do mínimo
    Dado que estou autenticada
    E a peça tem estoque mínimo de 5 unidades e 20 unidades em estoque
    Quando abro a listagem de peças
    Então a peça aparece sem alerta de estoque baixo
```

## RF-06 Ordem de serviço

### US-027 Consultar o histórico de status da OS

**Como** gestor da oficina, **quero** ver cada transição de status da OS (aberta → em execução → concluída), **para** acompanhar o andamento do serviço.

- Issue: [#39](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/39) · RF-06 · UC: nenhum · Must · M

```gherkin
Funcionalidade: Histórico de status da OS

  Cenário: critério registrado na issue
    Dado uma OS que já passou por aberta, em execução e concluída
    Quando abro o histórico da OS
    Então vejo em que status ela se encontra

  @proposta
  Cenário: transições em ordem
    Dado uma OS que já passou por aberta, em execução e concluída
    Quando abro o histórico da OS
    Então vejo cada transição em ordem, com status anterior, novo status, autor e data/hora
```

A issue não traz o "para" da história. O benefício acima foi completado pela equipe de documentação.

### US-028 Alterar os status de uma OS

**Como** secretária, **quero** poder alterar o status de uma OS, **para** poder acompanhar o andamento dela.

- Issue: [#40](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/40) · RF-06 · UC-009, UC-010, UC-014 (vínculos a revisar) · Must · M

```gherkin
Funcionalidade: Alteração de status da OS

  Cenário: critério registrado na issue
    Dado que preciso alterar o status de uma OS
    Quando ela está aprovada, em execução ou concluída
    Então posso alterar o status

  @proposta
  Cenário: iniciar a execução
    Dado uma OS aberta, com box e mecânico designados
    Quando altero o status para "em execução"
    Então a OS passa a "em execução"
    E o histórico grava o novo status, o meu nome e a data/hora de início

  @proposta
  Cenário: iniciar a execução sem box ou mecânico
    Dado uma OS aberta, sem box ou sem mecânico designado
    Quando tento alterar o status para "em execução"
    Então a alteração é recusada
    E vejo o aviso de que é preciso designar box e mecânico

  @proposta
  Cenário: concluir o serviço
    Dado uma OS em execução
    Quando altero o status para "concluída"
    Então a OS passa a "concluída"
    E o histórico grava o novo status, o meu nome e a data/hora de conclusão
    E o box fica livre para uma nova designação
```

## Critérios INVEST

| Critério | Histórias que precisam de ajuste | Motivo |
|---|---|---|
| Independente | US-008, US-009 | Tratam da OS como se ela nascesse no balcão, sem o orçamento (RF-02) |
| Negociável | (nenhuma) | Todas descrevem a necessidade, sem impor solução técnica |
| Valiosa | US-005 | O título não corresponde ao valor descrito |
| Estimável | US-019 a US-024, #42 | Sem critério de aceite registrado, a estimativa fica imprecisa |
| Pequena | US-001 | Mistura login, permissão por perfil e redirecionamento; pode ser dividida |
| Testável | US-019 a US-024, US-026, US-027, US-028, #42 | Os critérios registrados não descrevem resultado observável; ver os cenários `@proposta` |

## Divergências

- **D-01 Numeração.** US-26, US-27 e US-28 estão sem o zero à esquerda, e "US - 030" tem espaços. A "US -20" (#42) repete o número da US-020 (detalhes da peça). Como não existe US-025, a sugestão é renomear a #42 para US-025.
- **D-02 Títulos repetidos.** A US-018 (RF-02) e a US-027 (RF-06) têm o mesmo título, "Consultar o histórico de status da OS". Sugestão: renomear a US-018 para "Consultar o histórico de status do orçamento".
- **D-03 Histórias sem critério de aceite.** US-019 a US-024 não têm critério de aceite. Os cenários `@proposta` deste documento podem ser copiados para as issues.
- **D-04 Título incoerente.** A US-005 chama-se "Relatórios e Controle de Acesso", mas descreve uma busca de clientes. O relatório está na RF-07, e o controle de acesso na US-001.
- **D-05 Caminho principal ausente.** A US-017 não tem o cenário de aprovação do orçamento, só a recusa e a aprovação inválida.
- **D-06 Status do orçamento tratados como status da OS.** US-016, US-017, US-018 e US-026 falam em "OS Aberta, Orçada, Aprovada, Recusada". Pela decisão do grupo e pelo banco (`ck_orca_status`), esses status são do orçamento. A US-026 também deixa de fora o status recusado.
- **D-07 OS criada no balcão.** A US-008 cria a OS direto, com situação "em andamento". O banco só aceita OS de orçamento aprovado (`tg_os_bi_orcamento_aprovado`), com status inicial `aberta`. A US-009 recusa uma segunda OS para o mesmo carro, regra que o banco não tem (a unicidade é por orçamento, `uk_os_orcamento`).
- **D-08 Status do veículo.** A US-002 muda o "status do veículo para retirado", mas o banco não tem status do veículo, nem registro de quem entregou o carro (regra da reunião de 26/08).
- **D-09 Histórico de alterações da peça.** A US-020 e a US-021 pedem o histórico de alterações da peça. O banco não tem tabela de histórico de peças; só guarda a data da última atualização (`PECA_ATUALIZADO_EM`).
- **D-10 Atores fora do catálogo.** As histórias usam "vendedora" (US-013, US-014), "funcionário de estoque" (US-019) e "funcionário de atendimento" (US-020 a US-024). Pela premissa do Milestone 1, todos são a secretária (ACT-01). Sugestão: padronizar o ator nas issues.
- **D-11 Correções de texto.** O primeiro cenário da US-009 dizia "as informações do cliente são atualizadas"; foi corrigido para "da OS", conforme o objetivo da história. O cenário de login válido da US-001 aparece duas vezes na issue; foi mantido uma vez.
- **D-12 Histórias sem caso de uso.** Pelos títulos das issues do GitLab (coluna UC do índice), 16 das 30 histórias não têm caso de uso: US-001 (a autenticação só aparece como `<<include>>`), US-005, US-008, US-009, US-012, US-015, US-017, US-018, US-022, US-023, US-024, US-026, US-027, US-029, US-030 e a #42. A modelagem liga a US-008 ao UC-014 e a US-017 aos UC-009 e UC-010, cujas issues citam a US-028 (lacuna L-02), e a [matriz de rastreabilidade](../07-rastreabilidade/matriz.md) usa esses vínculos; por isso ela conta 14 histórias sem caso de uso. Vários diagramas de sequência também não têm caso de uso e usam `UC-000` (ver [backlog](backlog.md#cobertura-por-artefato)).

## Veja também

- [Levantamento e método](README.md)
- [Requisitos funcionais](requisitos-funcionais.md)
- [Requisitos não funcionais](requisitos-nao-funcionais.md)
- [Backlog](backlog.md)
- [Casos de uso](../05-modelagem/casos-de-uso/README.md)
- [Template de história de usuário](../templates/historia-de-usuario.md)
