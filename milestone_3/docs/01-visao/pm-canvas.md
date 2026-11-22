# PM Canvas do Bancada

O PM Canvas resume o projeto em uma página. A versão em grade está em [pm-canvas.svg](pm-canvas.svg). Este texto traz o que está em cada bloco, com as referências a requisitos e casos de uso e a fonte de cada fato.

![PM Canvas do Bancada em grade de 11 blocos](pm-canvas.svg)

## Justificativas

Por que fazer:

- O cliente precisa de um sistema moderno para gerir a oficina mecânica.
- Há pouco controle da ociosidade de funcionários e boxes.
- Há pouco controle do estoque de peças.
- Orçamentos e ordens de serviço ficam em papel e planilhas, sem registro de quem aprovou e sem histórico por veículo.

## Objetivos

| Código | Objetivo                                                | Requisitos    | Casos de uso                            |
| ------ | ------------------------------------------------------- | ------------- | --------------------------------------- |
| OBJ-01 | Controlar o acesso da secretária                        | RF-01, RNF-02 | Sem UC no catálogo                      |
| OBJ-02 | Atender no balcão e cadastrar clientes e veículos       | RF-01         | UC-001 a UC-005, UC-008, UC-011, UC-012 |
| OBJ-03 | Orçar antes do serviço e registrar a decisão do cliente | RF-02         | UC-006, UC-007, UC-009, UC-010          |
| OBJ-04 | Acompanhar a OS e o histórico de status                 | RF-06         | UC-013, UC-014                          |
| OBJ-05 | Distribuir o trabalho entre boxes e mecânicos           | RF-03         | Sem UC no catálogo                      |
| OBJ-06 | Controlar o estoque de peças                            | RF-05         | UC-015 a UC-022                         |
| OBJ-07 | Manter o histórico do veículo pela placa                | RF-04         | Sem UC no catálogo                      |
| OBJ-08 | Emitir o relatório da OS                                | RF-07         | UC-023                                  |

Metas de prazo: Milestone 1 em 15/09/2026, Milestone 2 em 20/10/2026 e Milestone 3 em 08/12/2026. As metas numéricas do produto (tempo de resposta, disponibilidade) ainda não foram definidas e estão na lista de perguntas da [RFP](rfp.md).

## Benefícios

- Interface moderna de gestão da oficina.
- Controle da ociosidade de funcionários e boxes (RF-03).
- Controle do estoque, com aviso quando a peça atinge o mínimo (RF-05).
- Histórico de orçamentos e OS, com autor e data/hora de cada mudança (RF-02, RF-06).
- Tempo de permanência do veículo na oficina, calculado pelo início e pela conclusão do serviço (RF-03).

## Produto

- Bancada: sistema web de gestão de oficina mecânica de carros.
- Protótipo navegável no Figma (arquivo "Gestão de Oficina Mecânica").
- Banco relacional MySQL: script e modelos em [06-banco-de-dados](../06-banco-de-dados/README.md).
- Modelagem UML (casos de uso, sequência, atividades e estados) e IFML no Figma.
- Issues, milestones e quadro Kanban no GitLab.
- Documentação em Markdown nesta pasta `docs/`.

## Requisitos (macro)

O detalhamento (histórias e critérios de aceite em Gherkin) está nas issues do GitLab e em [03-requisitos](../03-requisitos/README.md). Aqui, a cobertura por requisito:

| Requisito                                  | Conteúdo                                                                    | Histórias                     |
| ------------------------------------------ | --------------------------------------------------------------------------- | ----------------------------- |
| RF-01 Atendimento                          | Login da secretária, balcão, clientes, veículos e retirada                  | US-001 a US-015               |
| RF-02 Orçamento e aprovação do serviço     | Montagem item a item, aprovação ou recusa, meio da resposta                 | US-016, US-017, US-018, US-26 |
| RF-03 Distribuição entre boxes e mecânicos | Designação, um veículo por box, início e conclusão                          | US-20                         |
| RF-04 Histórico do veículo                 | Consulta das OS anteriores pela placa                                       | US-029, US-030                |
| RF-05 Controle de estoque de peças         | Cadastro, consulta, edição, exclusão, filtro e aviso de mínimo              | US-019 a US-024               |
| RF-06 Ordem de serviço                     | Ciclo de status e histórico de cada mudança                                 | US-27, US-28                  |
| RF-07 Relatório da OS                      | Cliente, funcionário, mecânico, valor total e datas                         | Sem história                  |
| RNF-01 a RNF-05                            | Usabilidade, segurança, desempenho, disponibilidade e integridade dos dados | Sem história                  |

## Stakeholders

| Quem                       | Papel no projeto                                                        |
| -------------------------- | ----------------------------------------------------------------------- |
| Antônio Júnior             | Cliente; define as regras de negócio e valida as entregas               |
| Secretária (ACT-01)        | Único perfil com login no Milestone 1; atende no balcão e registra tudo |
| Mecânico (ACT-02)          | Monta o orçamento e executa a OS no box designado                       |
| Gestor da oficina (ACT-03) | Acompanha o status das OS e o uso dos boxes                             |
| Clientes da oficina        | Donos dos veículos; aprovam ou recusam o orçamento                      |
| Equipe de desenvolvimento  | Hugo Brandão, Vitor Blan, Cauã Rodrigo e João Andrade                   |
| Professor avaliador        | Valida os artefatos pelas milestones                                    |

## Riscos

| Risco                                                     | Resposta prevista                                                                                     |
| --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Desistência de um integrante                              | Cada artefato tem responsável registrado na issue; o restante da equipe redistribui as issues abertas |
| Desistência do cliente do curso e, portanto, do projeto   | Registrar as decisões já tomadas nas issues, para que o projeto continue com base nelas               |
| Mudanças de escopo pedidas pelo cliente                   | Registrar cada pedido como issue e decidir com o cliente em qual milestone entra                      |
| Indisponibilidade do cliente para reuniões                | Levar as perguntas em aberto por escrito e registrar a premissa adotada até a resposta                |
| Inexperiência da equipe com as ferramentas                | Dividir o trabalho em issues pequenas e revisar entre os integrantes antes de fechar                  |
| Falta de luz ou água no IFRO                              | Manter os artefatos no GitLab e no Figma, acessíveis fora do campus                                   |
| Banco maior que os requisitos (pagamento, distribuidoras) | Levar a decisão ao cliente; ver a [RFP](rfp.md), seção 12                                             |
| Dados de exemplo divergentes no protótipo                 | Revisar o protótipo contra o banco e os requisitos antes da apresentação                              |

## Premissas

- O cliente pode pedir novas funcionalidades ao longo do projeto.
- O cliente pode pedir mudanças em funcionalidades já prontas.
- O orçamento vem antes da OS: a OS só nasce de um orçamento aprovado.
- No Milestone 1, só a secretária tem login no sistema.
- O cliente responde as perguntas da RFP até o início da modelagem.

## Restrições

- Prazo: calendário da disciplina.
- Gestão no GitLab com Kanban coletivo e métricas obrigatórias.
- Ferramentas sem licença paga: GitLab institucional, Figma (Education) e MySQL.
- Banco relacional MySQL, definido no Milestone 2.
- Equipe fixa de quatro integrantes.

## Linha do tempo

| Quando     | O quê                                                                                        | Fonte                                                  |
| ---------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| 08/09/2026 | Milestone 1, monitoria                                                                       | Calendário da disciplina                               |
| 15/09/2026 | Milestone 1, apresentação: PM Canvas, requisitos e histórias, métricas, Site Map             | Calendário · milestone "Milestone 1" no GitLab         |
| 13/10/2026 | Milestone 2, monitoria                                                                       | Calendário da disciplina                               |
| 20/10/2026 | Milestone 2, apresentação: DER, casos de uso, diagramas de sequência e IFML                  | Calendário · milestone "Milestone 2" no GitLab         |
| 06/11/2026 | Modelo de documentação do professor incluído no repositório                                  | `git log`                                              |
| 01/12/2026 | Milestone 3, monitoria                                                                       | Calendário da disciplina                               |
| 08/12/2026 | Milestone 3, apresentação final: diagramas de atividades e estados e documentação do projeto | Calendário · milestone "Milestone 3 (Final)" no GitLab |

O canvas do Milestone 1 planejava wireframes, protótipo e Site Map no Milestone 2 e o DER no Milestone 3. A tabela acima segue o que foi registrado nas milestones do GitLab.

## Custos

- Estimativa total: R$ 5 mil (canvas do Milestone 1).
- GitLab, Figma (Education) e MySQL: sem custo de licença.
- Composição do valor (horas da equipe, infraestrutura, hospedagem): a detalhar.

## Veja também

- [Visão do produto](README.md)
- [RFP do cliente](rfp.md)
- [Glossário](glossario.md)
- [Processo](../02-processo/README.md)
- [Requisitos funcionais](../03-requisitos/requisitos-funcionais.md)
- [Casos de uso](../05-modelagem/casos-de-uso/casos-de-uso.svg)
