# Diagramas de atividades

Milestone 3 · Modelagem comportamental (Fábrica de Software I)

Os diagramas de atividades mostram o fluxo de trabalho entre os atores e o sistema, em raias. Cada raia é um responsável: a Secretária (ACT-01), o Estoquista nos fluxos de distribuidora e a raia Sistema, que representa o Bancada. Os diagramas de consulta, que só envolvem navegação, têm apenas a raia da Secretária. As mudanças de status que alguns fluxos provocam estão em [Máquinas de estados](../estados/README.md).

São 16 diagramas, de DA-001 a DA-016, desenhados na página "08 - Diagrama de Atividades e Estados" do arquivo do Figma "Gestão de Oficina Mecânica". Cada diagrama tem uma issue no GitLab, com os cenários em Gherkin, e é ligado a um diagrama de sequência pelo padrão de título `DA-0XX | DS-0YY - <nome>`.

## Notação

A notação segue a descrição da issue DOC-008 do GitLab.

| Elemento | Representação | Uso nos diagramas |
|---|---|---|
| Nó inicial e final | Círculo preenchido e círculo com anel | Todos os diagramas têm um nó inicial. DA-005 (três), DA-006, DA-007, DA-008 e DA-010 (dois cada) têm um nó final por desfecho |
| Ação | Retângulo arredondado, com verbo no infinitivo | Cada passo da Secretária, do Estoquista ou do Sistema |
| Decisão | Losango, com saídas rotuladas por guarda (`sim` e `não`) | Validações do Sistema: campo obrigatório, CNPJ já cadastrado, orçamento aprovado e demais |
| Raia | Faixa que agrupa as ações por responsável | Secretária, Estoquista e Sistema |
| Bifurcação e união | Barra grossa, para fluxos paralelos | Não usada: nenhum fluxo do Bancada tem etapas paralelas |

## Catálogo

Os vínculos com caso de uso (UC) e história (US) seguem o [catálogo de casos de uso](../../README.md#catálogo-de-casos-de-uso) e a [matriz de rastreabilidade](../../07-rastreabilidade/matriz.md). `UC-000` é o código usado no GitLab para diagrama de sequência sem caso de uso no catálogo.

| DA | Fluxo | Ator | DS | UC | US | Issue |
|---|---|---|---|---|---|---|
| DA-001 | Cadastro de cliente | Secretária | DS-001 | UC-001 | US-003 | [#100](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/100) |
| DA-002 | Atualização das informações do cliente | Secretária | DS-005 | UC-002 | US-004 | [#101](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/101) |
| DA-003 | Cadastro de um veículo | Secretária | DS-002 | UC-004 | US-006 | [#102](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/102) |
| DA-004 | Cadastro de um orçamento | Secretária | DS-003 | UC-006 | US-016 | [#103](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/103) |
| DA-005 | Cadastro de OS | Secretária | DS-004 | UC-014 | US-008 | [#104](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/104) |
| DA-006 | Atualização das informações do veículo | Secretária | DS-006 | UC-005 | US-007 | [#105](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/105) |
| DA-007 | Atualização das informações do orçamento | Secretária | DS-007 | UC-000 | Nenhuma | [#106](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/106) |
| DA-008 | Listagem de clientes cadastrados | Secretária | DS-009 | UC-003 | US-010 | [#108](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/108) |
| DA-009 | Cadastro de distribuidora | Estoquista | DS-010 | UC-015 | US-000 | [#109](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/109) |
| DA-010 | Atualização das informações da distribuidora | Estoquista | DS-011 | UC-016 | US-000 | [#110](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/110) |
| DA-011 | Listagem de veículos cadastrados | Secretária | DS-014 | UC-008 | US-011 | [#113](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/113) |
| DA-012 | Visualização das informações de um veículo | Secretária | DS-015 | UC-012 | US-014 | [#114](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/114) |
| DA-013 | Listagem de orçamentos cadastrados | Secretária | DS-016 | UC-007 | US-000 | [#115](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/115) |
| DA-014 | Listagem de OSs cadastradas | Secretária | DS-017 | UC-000 | US-012 | [#116](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/116) |
| DA-015 | Visualização do perfil do cliente | Secretária | DS-018 | UC-011 | US-013 | [#117](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/117) |
| DA-016 | Visualização das informações do orçamento | Secretária | DS-019 | UC-000 | Nenhuma | [#118](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/118) |

## Clientes

### Cadastro de cliente (DA-001)

![Diagrama de atividades com raias Secretária e Sistema, do recebimento dos documentos do cliente ao registro e à lista de clientes](<DA-001 _ DS-001 - Cadastro de cliente.svg>)

Pontos de controle do fluxo:

- Preparação: a Secretária recebe as informações e os documentos do cliente, verifica as informações e só então preenche o formulário de cadastro.
- Validação: o Sistema valida os dados do formulário. Se a validação falhar, o fluxo volta ao preenchimento do formulário. Os dados verificados são os campos obrigatórios e a unicidade de CPF, e-mail e telefone.
- Desfecho: com os dados válidos, o Sistema registra o cliente e mostra a lista de clientes cadastrados.

### Atualização das informações do cliente (DA-002)

![Diagrama de atividades com raias Secretária e Sistema, da tela de edição do cliente à atualização do cadastro](<DA-002 _ DS-005 - Atualização das informações do cliente.svg>)

Pontos de controle do fluxo:

- Edição: a Secretária acessa a tela de edição de um cliente já cadastrado, altera as informações necessárias e clica em "Atualizar".
- Validação: o Sistema valida o formulário. Se falhar, a Secretária volta à edição. CPF, e-mail e telefone não podem pertencer a outro cliente.
- Desfecho: com os dados válidos, o Sistema atualiza o cadastro e mostra a lista de clientes cadastrados.

### Listagem de clientes cadastrados (DA-008)

![Diagrama de atividades com raias Secretária e Sistema, do acesso à listagem à exibição da lista ou do aviso de que não há clientes](<DA-008 _ DS-009 - Listagem de clientes cadastrados.svg>)

Pontos de controle do fluxo:

- Acesso: a Secretária abre a opção "Clientes" do menu, a partir de qualquer tela.
- Busca: o Sistema busca os clientes cadastrados. Se houver, exibe a lista. Se não houver, informa que não há clientes cadastrados.
- Desfechos: o diagrama tem dois nós finais, um para a lista exibida e outro para o aviso de lista vazia.

### Visualização do perfil do cliente (DA-015)

![Diagrama de atividades com a raia da Secretária, da lista de clientes à visualização das informações do perfil do cliente](<DA-015 _ DS-018 - Visualizar perfil do cliente.svg>)

Pontos de controle do fluxo:

- Seleção: a partir da lista de clientes cadastrados, a Secretária clica na linha do cliente desejado.
- Desfecho: o Bancada redireciona para a página com as informações do cliente. O fluxo é só de consulta e não tem decisão.

## Veículos

### Cadastro de um veículo (DA-003)

![Diagrama de atividades com raias Secretária e Sistema, da verificação da documentação ao registro do veículo e à notificação de sucesso](<DA-003 _ DS-002 - Cadastro de um veículo.svg>)

Pontos de controle do fluxo:

- Preparação: a Secretária recebe as informações e os documentos, verifica a documentação do veículo e preenche o formulário de cadastro.
- Validação: o Sistema valida os dados do formulário. Se falhar, o fluxo volta ao preenchimento.
- Desfecho: com os dados válidos, o Sistema registra o veículo e devolve a notificação de sucesso.

### Atualização das informações do veículo (DA-006)

![Diagrama de atividades com raias Secretária e Sistema, da edição do veículo à verificação da placa e ao salvamento das alterações](<DA-006 _ DS-006 - Atualizar informações do veículo.svg>)

Pontos de controle do fluxo:

- Edição: a Secretária acessa o cadastro do veículo, o Sistema exibe os dados, e a Secretária edita as informações e solicita a atualização.
- Placa: o Sistema verifica a placa informada. Se ela pertencer a outro veículo, notifica que a placa pertence a um veículo já cadastrado e encerra sem salvar.
- Desfecho: se a placa não pertencer a outro veículo, o Sistema salva as alterações e notifica o sucesso da atualização.

### Listagem de veículos cadastrados (DA-011)

![Diagrama de atividades com a raia da Secretária, do acesso à lista de veículos à sua visualização](<DA-011 _ DS-014 - Listagem de veículos cadastrados.svg>)

Pontos de controle do fluxo:

- Acesso: a Secretária abre a opção "Veículos" do menu, a partir de qualquer tela, e visualiza a lista de veículos cadastrados.
- O fluxo é só de consulta e não tem decisão.

### Visualização das informações de um veículo (DA-012)

![Diagrama de atividades com a raia da Secretária, da lista de veículos à visualização das informações do veículo selecionado](<DA-012 _ DS-015 - Visualizar as informações de um veículo.svg>)

Pontos de controle do fluxo:

- Seleção: a partir da lista de veículos cadastrados, a Secretária clica na linha do veículo desejado.
- Desfecho: o Bancada redireciona para a página com as informações do veículo escolhido.

## Orçamentos

### Cadastro de um orçamento (DA-004)

![Diagrama de atividades com raias Secretária e Sistema, da placa do veículo ao registro do orçamento com status Em aberto](<DA-004 _DS-003 - Cadastro de um orçamento.svg>)

Pontos de controle do fluxo:

- Solicitação: a Secretária informa a placa do veículo, preenche o formulário do orçamento e solicita o cadastro.
- Campos obrigatórios: se algum campo obrigatório estiver vazio, o Sistema notifica "Campo obrigatório não preenchido" e o fluxo volta ao formulário. Pela issue, os campos obrigatórios são a placa e o valor.
- Orçamento em aberto: se o veículo já tiver um orçamento em aberto, o Sistema notifica "Veículo já possui orçamento aberto" e o fluxo volta ao formulário. Cada veículo tem no máximo um orçamento em aberto.
- Desfecho: passadas as duas verificações, o Sistema registra o orçamento com status "Em aberto".

### Atualização das informações do orçamento (DA-007)

![Diagrama de atividades com raias Secretária e Sistema, da placa do veículo à verificação de orçamento ativo e à atualização do orçamento](<DA-007  DS-007 - Atualizar informações do orçamento.svg>)

Pontos de controle do fluxo:

- Solicitação: a Secretária informa a placa do veículo e os dados do cliente e solicita a atualização do orçamento.
- Orçamento ativo: o Sistema verifica se o veículo tem orçamento ativo, isto é, com status "Em aberto" ou "Orçada". Se tiver, notifica que o veículo já tem orçamento ativo e encerra sem atualizar.
- Desfecho: se o veículo não tiver orçamento ativo, o Sistema atualiza as informações do orçamento e notifica o sucesso da atualização.
- Este diagrama não tem caso de uso no catálogo (`UC-000`). Ver [Divergências](#divergências).

### Listagem de orçamentos cadastrados (DA-013)

![Diagrama de atividades com a raia da Secretária, do acesso à lista de orçamentos à visualização das informações de um orçamento](<DA-013 _ DS-016 - Listagem de orçamentos cadastrados.svg>)

Pontos de controle do fluxo:

- Acesso: a Secretária abre a opção "Orçamento" do menu lateral, a partir de qualquer tela, e visualiza a lista de orçamentos cadastrados.
- Ver [Divergências](#divergências): o diagrama desenhado também inclui a seleção de um orçamento, que pertence ao DA-016.

### Visualização das informações do orçamento (DA-016)

![Diagrama de atividades com a raia da Secretária, da lista de orçamentos à visualização das informações do orçamento selecionado](<DA-016 _ DS-019 - Visualizar as informações do orçamento.svg>)

Pontos de controle do fluxo:

- Seleção: a partir da lista de orçamentos cadastrados, a Secretária clica na linha do orçamento desejado.
- Desfecho: o Bancada redireciona para a página com as informações do orçamento.
- Este diagrama não tem caso de uso nem história no catálogo (`UC-000`).

## Ordens de serviço

### Cadastro de OS (DA-005)

![Diagrama de atividades com raias Secretária e Sistema, da seleção do orçamento ao registro da OS com status Aprovada ou à notificação de negação](<DA-005_DS-004 - Cadastrado de OS.svg>)

Pontos de controle do fluxo:

- Solicitação: a Secretária seleciona o orçamento, preenche o formulário da OS e solicita o cadastro.
- Orçamento aprovado: a OS só nasce de um orçamento aprovado. Se o orçamento não estiver aprovado, o Sistema notifica que o orçamento não foi aprovado e encerra sem registrar.
- Formulário completo: com o orçamento aprovado, o Sistema verifica se o formulário está completo. Se não estiver, notifica a negação do cadastro e encerra sem registrar.
- Desfecho: com as duas condições atendidas, o Sistema registra a OS com status "Aprovada". O diagrama tem três nós finais, um por desfecho.

### Listagem de OSs cadastradas (DA-014)

![Diagrama de atividades com uma raia, do acesso à lista à visualização das informações](<DA-014 _ DS-017 - Listagem de OSs cadastradas.svg>)

Pontos de controle do fluxo:

- Acesso: pela issue, a Secretária abre a opção "Ordens de Serviço" do menu, a partir de qualquer tela, e visualiza a lista de OSs cadastradas.
- Este diagrama não tem caso de uso no catálogo (`UC-000`). Ver [Divergências](#divergências): o diagrama desenhado ainda descreve orçamentos, não OSs.

## Distribuidoras

### Cadastro de distribuidora (DA-009)

![Diagrama de atividades com raias Estoquista e Sistema, do preenchimento dos dados à validação de campos e do CNPJ e ao registro da distribuidora](<DA-009 _ DS-010 - Cadastrar distribuidora.svg>)

Pontos de controle do fluxo:

- Solicitação: o Estoquista preenche os dados da distribuidora (razão social, CNPJ, telefone, e-mail, logradouro e bairro) e clica em "Cadastrar".
- Campos obrigatórios: se algum campo não estiver preenchido, o Sistema exibe a mensagem de campo obrigatório e o fluxo volta ao formulário.
- CNPJ: se o CNPJ já estiver cadastrado, o Sistema exibe "CNPJ já cadastrado" e o fluxo volta ao formulário.
- Desfecho: passadas as duas verificações, o Sistema registra a distribuidora e exibe "Distribuidora cadastrada com sucesso".

### Atualização das informações da distribuidora (DA-010)

![Diagrama de atividades com raias Estoquista e Sistema, da edição dos dados da distribuidora à verificação do CNPJ e ao salvamento](<DA-010 _ DS-011 - Atualizar informações da distribuidora.svg>)

Pontos de controle do fluxo:

- Edição: o Estoquista acessa o cadastro da distribuidora, o Sistema exibe os dados, e o Estoquista edita os dados e clica em "Atualizar".
- CNPJ: o Sistema verifica o CNPJ informado. Se ele pertencer a outra distribuidora, notifica que o CNPJ já está cadastrado e encerra sem salvar.
- Desfecho: se o CNPJ não pertencer a outra distribuidora, o Sistema salva as alterações e notifica o sucesso da atualização.

## Divergências

Diferenças entre os diagramas e as demais fontes do projeto (issues do GitLab, catálogo de casos de uso e matriz de rastreabilidade). Onde há diferença, a issue do GitLab é a fonte de verdade, e o texto acima segue a issue.

1. DA-013 inclui as ações "Selecionar orçamento" e "Visualizar as informações do orçamento", que são o fluxo do DA-016. A issue #115 só prevê a abertura do menu e a exibição da lista.
2. DA-014 descreve orçamentos ("Acessar lista de orçamentos", "Selecionar orçamento"), e a issue #116 trata de OSs. O fluxo correto é abrir "Ordens de Serviço" no menu e visualizar a lista de OSs.
3. DA-015 tem a ação "Visualizar a lista de orçamentos clientes". O texto correto é a lista de clientes cadastrados, como na issue #117.
4. DA-014, DA-015 e DA-016 ainda têm "texto" como nome da raia, no lugar de Secretária.
5. A raia Estoquista (DA-009 e DA-010) não consta no catálogo de atores (ACT-01 a ACT-03). A decisão sobre outros perfis está pendente (lacuna L-14 de [lacunas.md](../../07-rastreabilidade/lacunas.md)).
6. DA-007, DA-014 e DA-016 ligam-se a diagramas de sequência com `UC-000`, isto é, sem caso de uso no catálogo. O DA-007 e o DA-016 também não têm história de usuário.
7. DA-005 liga-se ao UC-014, que o GitLab vincula à US-028. O vínculo parece incorreto: a história de cadastro da OS é a US-008. Ver a divergência 8 de [requisitos-funcionais.md](../../03-requisitos/requisitos-funcionais.md#divergências).
8. O nome da raia Sistema varia entre os diagramas: "Sistema de cadastro de clientes" (DA-001), "Sistema de atualização das informações de clientes" (DA-002), "Sistema de cadastro de veículo" (DA-003) e "Sistema" nos demais.

## Veja também

- [Máquinas de estados](../estados/README.md): os estados e as transições que os fluxos de orçamento e de OS provocam.
- [Casos de uso](../casos-de-uso/README.md): UC-001 a UC-016, aos quais os diagramas se ligam.
- [Diagramas de sequência](../sequencia/README.md): troca de mensagens de cada DS citado no catálogo.
- [Matriz de rastreabilidade](../../07-rastreabilidade/matriz.md): a cadeia requisito, história, caso de uso, sequência e atividade.
- [Modelagem](../README.md): visão geral da unidade.
