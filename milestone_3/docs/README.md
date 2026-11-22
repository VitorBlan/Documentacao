# Bancada: índice da documentação do projeto
s
MMilestones 1, 2 e 3 · Fábrica de Software I · Engenharia de Software · IFRO, Campus Vilhena · 2026/2

Este é o ponto de entrada da documentação do projeto. Aqui ficam o contexto do projeto, a equipe, o cliente, os catálogos de identificadores e o mapa de todos os artefatos. Os artefatos dos milestones anteriores ficam na raiz do repositório: o PM Canvas do Milestone 1 em `milestone_1/` e o banco do Milestone 2 em `milestone_2/` (`ddl.sql` e o modelo do MySQL Workbench).

## Projeto

O Bancada é um sistema web de gestão de oficina mecânica de carros, construído a partir da RFP 2 da disciplina. A secretária autenticada recebe o cliente no balcão, verifica se ele veio trazer ou buscar um veículo e mantém o cadastro de clientes e veículos. O mecânico analisa o veículo e monta o orçamento; somente depois da aprovação do dono do veículo o orçamento vira uma ordem de serviço (OS), designada a um box e a um mecânico. Cada mudança de status do orçamento e da OS fica registrada com data, hora e autor, o que forma o histórico do veículo. O estoque de peças avisa quando uma peça atinge o nível mínimo.

O projeto ainda não tem código da aplicação. As fontes de verdade são três, nesta ordem:

1. **Issues do GitLab** ([projeto2](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2)): requisitos, histórias, casos de uso e diagramas, cada um em uma issue.
2. **Arquivo do Figma** "[Gestão de Oficina Mecânica](https://www.figma.com/design/EHj0Js5KtOJ88TRQd5fTJI/Gest%C3%A3o-de-Oficina-Mec%C3%A2nica?node-id=3-36)": site map, IFML, protótipo, casos de uso, classes, sequência, banco e atividades e estados.
3. **Banco do Milestone 2** (`milestone_2/ddl.sql`): tabelas, restrições e gatilhos em MySQL.

Quando um documento diverge dessas fontes, a divergência é registrada na seção Divergências do próprio documento.

## Equipe

| Integrante   | Usuário no GitLab | Frentes registradas no GitLab                                                                                                                        |
| ------------ | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hugo Brandão | `hugo-brandao`    | RF-01 e histórias US-001 a US-015; catálogo de casos de uso; diagramas de sequência e de atividades; DER; issues de documentação (DOC-001 a DOC-008) |
| Vitor Blan   | `vitor-blan`      | RF-02, RF-03, RF-06 e RF-07; RNF-01 a RNF-05; PM Canvas; IFML; DE-001; documentação das pastas 01, 02, 03 e 07                                       |
| Cauã Rodrigo | `caua-silva`      | RF-05 e histórias US-019 a US-024; ata da reunião de levantamento                                                                                    |
| João Andrade | `joao-andrade`    | RF-04 e histórias US-029 e US-030                                                                                                                    |

## Cliente

Antônio Júnior, em nome de uma oficina mecânica de carros. A oficina controla ordens de serviço em papel e em planilhas soltas, não consegue comprovar quem aprovou cada serviço e não acompanha a ocupação dos boxes nem o estoque de peças. A RFP completa está em [01-visao/rfp.md](01-visao/rfp.md).

## Atores

| ID     | Ator              | Papel no sistema                                                 | Quem é                                                                   |
| ------ | ----------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------ |
| ACT-01 | Secretária        | Único perfil com login no Milestone 1 (`funcionario.func_login`) | Atende no balcão e registra clientes, veículos, orçamentos, OS e estoque |
| ACT-02 | Mecânico          | Sem login; gravado na OS (`ordem_servico.meca_id`)               | Monta o orçamento e executa a OS no box designado                        |
| ACT-03 | Gestor da oficina | Sem login                                                        | Acompanha o status das OS e o uso dos boxes                              |

Os casos de uso do Figma usam também os atores Estoquista e Gerente, e o protótipo mostra telas com login de Almoxarifado e de Gerente. A decisão sobre outros perfis está pendente (lacuna L-14 de [07-rastreabilidade/lacunas.md](07-rastreabilidade/lacunas.md)).

## Milestones

| Milestone   | Apresentação | Foco                                                                       | Pastas principais                                                      |
| ----------- | ------------ | -------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Milestone 1 | 15/09/2026   | Visão do produto, requisitos, histórias, site map e protótipo              | `01-visao/`, `03-requisitos/`, `04-prototipo/`                         |
| Milestone 2 | 20/10/2026   | Casos de uso, classes, sequência, IFML e banco                             | `05-modelagem/`, `06-banco-de-dados/`                                  |
| Milestone 3 | 08/12/2026   | Atividades e estados, processo, rastreabilidade, qualidade e entrega final | `02-processo/`, `07-rastreabilidade/`, `08-qualidade/`, `09-entregas/` |

As monitorias acontecem uma semana antes de cada apresentação (08/09, 13/10 e 01/12/2026). Cada entrega de milestone tem um resumo em `09-entregas/`.

## Documentos

Status: "Concluído" indica documento já adaptado ao Bancada; "Em elaboração", documento sendo escrito; "Pendente" indica que o arquivo ainda tem o conteúdo do modelo da disciplina e precisa ser refeito.

| Documento                                                                                    | Conteúdo                                                                            | Milestone | Status        |
| -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | --------- | ------------- |
| [01-visao/README.md](01-visao/README.md)                                                     | Visão do produto, objetivos, escopo, partes interessadas e catálogo de casos de uso | 1         | Concluído     |
| [01-visao/rfp.md](01-visao/rfp.md)                                                           | RFP 2: necessidades do cliente, calendário e perguntas em aberto                    | 1         | Concluído     |
| [01-visao/pm-canvas.md](01-visao/pm-canvas.md) e [.svg](01-visao/pm-canvas.svg)              | PM Canvas do projeto                                                                | 1         | Concluído     |
| [01-visao/glossario.md](01-visao/glossario.md)                                               | Termos do domínio, com a tabela ou a coluna correspondente no banco                 | 1         | Concluído     |
| [02-processo/README.md](02-processo/README.md)                                               | Processo de desenvolvimento (Kanban no GitLab)                                      | 3         | Em elaboração |
| [02-processo/bpmn/README.md](02-processo/bpmn/README.md)                                     | Processo atual da oficina (as-is) em BPMN                                           | 3         | Em elaboração |
| [02-processo/metricas/README.md](02-processo/metricas/README.md)                             | Métricas do quadro do GitLab                                                        | 3         | Em elaboração |
| [03-requisitos/README.md](03-requisitos/README.md)                                           | Elicitação, priorização, convenções e atores                                        | 1         | Concluído     |
| [03-requisitos/requisitos-funcionais.md](03-requisitos/requisitos-funcionais.md)             | RF-01 a RF-07, regras verificáveis e evidência no banco                             | 1         | Concluído     |
| [03-requisitos/requisitos-nao-funcionais.md](03-requisitos/requisitos-nao-funcionais.md)     | RNF-01 a RNF-05, com métrica e estado                                               | 1         | Concluído     |
| [03-requisitos/historias-de-usuario.md](03-requisitos/historias-de-usuario.md)               | As 30 histórias, com critérios de aceite em Gherkin                                 | 1         | Concluído     |
| [03-requisitos/backlog.md](03-requisitos/backlog.md)                                         | Backlog priorizado e cobertura de diagramas por caso de uso                         | 1         | Concluído     |
| [03-requisitos/rastreabilidade-requisitos.svg](03-requisitos/rastreabilidade-requisitos.svg) | Rastreabilidade entre RF, histórias, casos de uso e tabelas                         | 1         | Concluído     |
| [04-prototipo/README.md](04-prototipo/README.md)                                             | Índice do protótipo                                                                 | 1         | Concluído     |
| [04-prototipo/site-map.md](04-prototipo/site-map.md) e [.svg](04-prototipo/site-map.svg)     | Site map                                                                            | 1         | Concluído     |
| [04-prototipo/ifml/README.md](04-prototipo/ifml/README.md)                                   | Modelo IFML                                                                         | 2         | Concluído     |
| [04-prototipo/wireframes/README.md](04-prototipo/wireframes/README.md)                       | Telas do protótipo                                                                  | 1         | Concluído     |
| [05-modelagem/README.md](05-modelagem/README.md)                                             | Índice da modelagem UML                                                             | 2         | Pendente      |
| [05-modelagem/casos-de-uso/README.md](05-modelagem/casos-de-uso/README.md)                   | Diagrama e descrição dos casos de uso (UC-001 a UC-023)                             | 2         | Pendente      |
| [05-modelagem/classes/README.md](05-modelagem/classes/README.md)                             | Diagrama de classes do domínio                                                      | 2         | Concluído     |
| [05-modelagem/sequencia/README.md](05-modelagem/sequencia/README.md)                         | Diagramas de sequência (DS-001 a DS-020)                                            | 2         | Concluído     |
| [05-modelagem/atividades/README.md](05-modelagem/atividades/README.md)                       | Diagramas de atividades (DA-001 a DA-016)                                           | 3         | Concluído     |
| [05-modelagem/estados/README.md](05-modelagem/estados/README.md)                             | Diagramas de estados (DE-001 a DE-003)                                              | 3         | Concluído     |
| [05-modelagem/ocl/README.md](05-modelagem/ocl/README.md)                                     | Restrições em OCL                                                                   | 2         | Pendente      |
| [06-banco-de-dados/README.md](06-banco-de-dados/README.md)                                   | Índice do banco de dados                                                            | 2         | Pendente      |
| [06-banco-de-dados/dicionario-de-dados.md](06-banco-de-dados/dicionario-de-dados.md)         | Dicionário de dados                                                                 | 2         | Pendente      |
| [06-banco-de-dados/schema.sql](06-banco-de-dados/schema.sql)                                 | Esquema SQL (versão final de `milestone_2/ddl.sql`)                                 | 2         | Pendente      |
| [07-rastreabilidade/matriz.md](07-rastreabilidade/matriz.md)                                 | Matriz de rastreabilidade, uma linha por história                                   | 3         | Concluído     |
| [07-rastreabilidade/lacunas.md](07-rastreabilidade/lacunas.md)                               | As quatro perguntas da revisão cruzada, órfãos e lacunas com ação                   | 3         | Concluído     |
| [07-rastreabilidade/malha-rastreabilidade.svg](07-rastreabilidade/malha-rastreabilidade.svg) | Malha de rastreabilidade, com a cadeia da US-003 como exemplo                       | 3         | Concluído     |
| [08-qualidade/monitorias.md](08-qualidade/monitorias.md)                                     | Registro das monitorias                                                             | 3         | Pendente      |
| [08-qualidade/retrospectiva-swebok.md](08-qualidade/retrospectiva-swebok.md)                 | Retrospectiva com base no SWEBOK                                                    | 3         | Pendente      |
| [08-qualidade/retrospectiva-final.md](08-qualidade/retrospectiva-final.md)                   | Retrospectiva final do projeto                                                      | 3         | Pendente      |
| [09-entregas/milestone-1.md](09-entregas/milestone-1.md)                                     | Entrega do Milestone 1                                                              | 1         | Pendente      |
| [09-entregas/milestone-2.md](09-entregas/milestone-2.md)                                     | Entrega do Milestone 2                                                              | 2         | Pendente      |
| [09-entregas/milestone-3.md](09-entregas/milestone-3.md)                                     | Entrega do Milestone 3                                                              | 3         | Pendente      |
| [templates/](templates/)                                                                     | Modelos de caso de uso, história, requisito e ADR                                   | Apoio     | Pendente      |
| [adr/README.md](adr/README.md)                                                               | Registros de decisões de arquitetura                                                | Apoio     | Pendente      |
| [CHANGELOG.md](CHANGELOG.md)                                                                 | Histórico de mudanças da documentação                                               | Apoio     | Pendente      |
| [CONTRIBUTING.md](CONTRIBUTING.md)                                                           | Fluxo de contribuição no GitLab                                                     | Apoio     | Pendente      |

## Árvore

```text
milestone_3/docs/
├── README.md                  este índice
├── CHANGELOG.md               histórico da documentação por milestone
├── CONTRIBUTING.md            fluxo no GitLab: issues, labels e critério de pronto
├── 01-visao/                  visão do produto, RFP, PM Canvas e glossário
├── 02-processo/               processo, BPMN as-is e métricas do GitLab
├── 03-requisitos/             RF, RNF, histórias, backlog e rastreabilidade
├── 04-prototipo/              site map, IFML e telas
├── 05-modelagem/              UML: casos de uso, classes, sequência, atividades, estados e OCL
├── 06-banco-de-dados/         modelos conceitual e lógico, dicionário e schema.sql
├── 07-rastreabilidade/        matriz, lacunas e malha de rastreabilidade
├── 08-qualidade/              monitorias e retrospectivas
├── 09-entregas/               uma entrega por milestone
├── templates/                 modelos de caso de uso, história, requisito e ADR
└── adr/                       decisões de arquitetura numeradas
```

## Catálogo de casos de uso

Os identificadores abaixo são os das issues do GitLab (padrão `UC-0YY - US-0XX - <nome>`) e aparecem com o mesmo sentido em todos os documentos. `US-000` indica caso de uso ainda sem história vinculada.

| ID     | Caso de uso                               | História no GitLab |
| ------ | ----------------------------------------- | ------------------ |
| UC-001 | Cadastro de cliente                       | US-003             |
| UC-002 | Atualizar informações do cliente          | US-004             |
| UC-003 | Visualizar lista de clientes cadastrados  | US-010             |
| UC-004 | Cadastro de um veículo                    | US-006             |
| UC-005 | Atualizar informações do veículo          | US-007             |
| UC-006 | Cadastrar orçamento                       | US-016             |
| UC-007 | Visualizar lista de orçamentos feitos     | US-000             |
| UC-008 | Visualizar lista de veículos cadastrados  | US-011             |
| UC-009 | Mudar o status do orçamento para recusado | US-028             |
| UC-010 | Mudar o status do orçamento para aprovado | US-028             |
| UC-011 | Visualizar perfil do cliente              | US-013             |
| UC-012 | Visualizar informações do veículo         | US-014             |
| UC-013 | Mudar status de uma OS para concluída     | US-002             |
| UC-014 | Criar OS                                  | US-028             |
| UC-015 | Cadastrar distribuidora                   | US-000             |
| UC-016 | Atualizar informações da distribuidora    | US-000             |
| UC-017 | Cadastrar peça                            | US-019             |
| UC-018 | Visualizar lista de distribuidoras        | US-000             |
| UC-019 | Visualizar informações da distribuidora   | US-000             |
| UC-020 | Visualizar lista de peças                 | US-000             |
| UC-021 | Visualizar informações da peça            | US-020             |
| UC-022 | Atualizar informações da peça             | US-021             |
| UC-023 | Visualizar lista de relatórios            | US-000             |

Todos os casos de uso incluem "Autenticar" (`<<include>>`). Os vínculos de UC-009, UC-010 e UC-014 com a US-028 parecem incorretos; a correção está na divergência 8 de [requisitos-funcionais.md](03-requisitos/requisitos-funcionais.md#divergências).

## Convenções de identificação

| Prefixo            | Significado                                                           | Exemplo                                |
| ------------------ | --------------------------------------------------------------------- | -------------------------------------- |
| `RF-NN`            | Requisito funcional (agrupa histórias)                                | RF-02 Orçamento e aprovação do serviço |
| `RNF-NN`           | Requisito não funcional                                               | RNF-02 Segurança                       |
| `US-NNN`           | História de usuário                                                   | US-016 Montar o orçamento da OS        |
| `UC-NNN`           | Caso de uso                                                           | UC-006 Cadastrar orçamento             |
| `DS-NNN`           | Diagrama de sequência, ligado ao caso de uso                          | DS-003 \| UC-006                       |
| `DA-NNN`, `DE-NNN` | Diagrama de atividades ou de estados, ligado ao diagrama de sequência | DA-004 \| DS-003                       |
| `ACT-NN`           | Ator                                                                  | ACT-01 Secretária                      |
| `#N`               | Número da issue no GitLab                                             | #13                                    |

- **Telas:** ainda não têm código próprio. Os documentos citam o nome do quadro no Figma, no padrão `US-0XX - RF-0Y - <nome>`, com link (lacuna L-08).
- **Classes:** nomes do diagrama de domínio do Figma, em português e no singular (`Cliente`, `Veiculo`, `Orcamento`, `OrdemServico`, `Box`, `Peca`).
- **Labels do GitLab:** `prio::P1` a `P3` (convertidas em Must, Should e Could), `size::S`, `M` e `L`, `type::Feature` e `type::Documentation`, `workflow::`.

## Como ler a malha de rastreabilidade

A malha ([07-rastreabilidade/malha-rastreabilidade.svg](07-rastreabilidade/malha-rastreabilidade.svg)) mostra os elos entre os artefatos. Cada elo cita o anterior pelo identificador, e o título de cada issue carrega esse vínculo:

1. O **requisito** (RF-NN) agrupa **histórias** (US-NNN); o título da história cita o RF.
2. A história é realizada por um **caso de uso** (UC-NNN); o título do caso de uso cita a história.
3. O caso de uso usa **classes** e é descrito por um **diagrama de sequência** (DS-NNN), que cita o UC.
4. O diagrama de sequência é detalhado por um **diagrama de atividades ou de estados** (DA-NNN ou DE-NNN), que cita o DS.
5. A história aparece em uma ou mais **telas** do protótipo, e as classes viram **tabelas** do banco.

Cada issue de diagrama tem uma seção "Diagrama" com o link para o quadro no Figma, e o título do quadro tem link de volta para a issue. A [matriz](07-rastreabilidade/matriz.md) é a mesma informação em tabela, e [lacunas.md](07-rastreabilidade/lacunas.md) lista os elos que ainda faltam, com ação e prazo para antes da monitoria do Milestone 3.

## Divergências

Diferenças entre as fontes que afetam vários documentos. As listas completas estão em cada pasta.

1. **Ciclo de status da OS.** A issue RF-06 descreve `Aprovada → em execução → concluída`; o banco e a decisão do grupo usam `aberta → em execução → concluída`, e `aprovado` pertence ao orçamento. O protótipo ainda mostra OS "Aprovada" na listagem.
2. **Perfis de acesso.** A premissa do Milestone 1 é que só a secretária tem login, mas os casos de uso têm Estoquista e Gerente, e o protótipo tem telas de Almoxarifado e de Gerente.
3. **Histórias sem caso de uso.** 16 das 30 histórias não têm caso de uso no catálogo, entre elas a autenticação, o histórico do veículo e a agenda dos boxes.
4. **Sem código e sem testes.** O elo de teste da malha fica vazio até haver aplicação; os critérios de aceite em Gherkin são a base para os testes futuros.
5. **Pastas com conteúdo do modelo.** As pastas marcadas como "Pendente" ainda descrevem o sistema de exemplo da disciplina e precisam ser refeitas antes da entrega final.

Veja também: [Visão do produto](01-visao/README.md) · [RFP](01-visao/rfp.md) · [Requisitos](03-requisitos/README.md) · [Histórias de usuário](03-requisitos/historias-de-usuario.md) · [Matriz de rastreabilidade](07-rastreabilidade/matriz.md) · [Lacunas](07-rastreabilidade/lacunas.md) · [CHANGELOG](CHANGELOG.md) · [Como contribuir](CONTRIBUTING.md)
