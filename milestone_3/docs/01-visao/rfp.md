# RFP 2: gestão de oficinas mecânicas

Fábrica de Software I · Milestone 1 · documento de partida

> **Documento de partida da disciplina.** A RFP 2 foi apresentada pelo professor, com o cliente Antônio Júnior. Este texto consolida o contexto e as necessidades como o grupo as registrou no PM Canvas do Milestone 1 e nas issues do GitLab. Ele traz necessidades, não requisitos prontos: cada necessidade é transformada em requisitos, casos de uso e histórias de usuário no GitLab.

## 1. Solicitante

Antônio Júnior, cliente do projeto, em nome de uma oficina mecânica de carros. A oficina atende clientes no balcão, distribui o trabalho entre boxes e mecânicos e mantém um estoque de peças.

## 2. Contexto

A oficina controla ordens de serviço em papel e em planilhas soltas. Não há registro de quem autorizou cada serviço, o que dificulta comprovar que o cliente aprovou o gasto. Consultar o histórico de um veículo que já passou pela oficina é difícil. A oficina também não acompanha a ociosidade dos funcionários e dos boxes, nem o estoque de peças.

O cliente quer um sistema moderno de gestão que registre o atendimento do balcão até a entrega do veículo.

## 3. Objeto da contratação

Um sistema web para:

- atender o cliente no balcão e manter o cadastro de clientes e veículos;
- montar o orçamento e registrar a decisão do dono do veículo;
- abrir e acompanhar a ordem de serviço até a conclusão;
- distribuir o trabalho entre boxes e mecânicos;
- consultar o histórico de cada veículo pela placa;
- controlar o estoque de peças;
- consultar o relatório das ordens de serviço.

## 4. Necessidades do cliente

As necessidades estão em linguagem de negócio. A coluna "Registrado como" indica o requisito que o grupo abriu no GitLab para cada uma.

| Código | Necessidade                                                                                                      | Registrado como                |
| ------ | ---------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| N01    | Só pessoas autorizadas operam o sistema, e cada registro identifica quem o fez.                                  | RF-01, RNF-02                  |
| N02    | A secretária recebe o cliente, verifica se ele veio trazer ou buscar um veículo e registra a retirada.           | RF-01                          |
| N03    | Os dados de clientes e veículos ficam cadastrados e podem ser atualizados.                                       | RF-01                          |
| N04    | O orçamento separa peças e mão de obra, e o serviço só começa depois que o dono do veículo aprova.               | RF-02                          |
| N05    | A oficina consegue comprovar quando e por qual meio o cliente aprovou ou recusou o orçamento.                    | RF-02                          |
| N06    | Cada OS aprovada vai para um box e um mecânico, e um box atende um veículo por vez.                              | RF-03                          |
| N07    | A oficina sabe quanto tempo cada veículo ficou parado.                                                           | RF-03                          |
| N08    | Qualquer funcionário autorizado consulta o que já foi feito em um veículo, pela placa.                           | RF-04                          |
| N09    | A oficina sabe o que tem em estoque e é avisada quando uma peça atinge o mínimo.                                 | RF-05                          |
| N10    | A OS percorre um ciclo de status claro, com histórico de cada mudança.                                           | RF-06                          |
| N11    | A oficina consulta um relatório das ordens de serviço, com cliente, funcionário, mecânico, valor e datas.        | RF-07                          |
| N12    | O sistema é simples para a secretária, responde rápido, fica disponível no horário da oficina e não perde dados. | RNF-01, RNF-03, RNF-04, RNF-05 |

## 5. Usuários

| Perfil            | Quem é                                       | O que faz no sistema                                                              |
| ----------------- | -------------------------------------------- | --------------------------------------------------------------------------------- |
| Secretária        | Funcionária do balcão, com cadastro especial | Atende o cliente, cadastra clientes e veículos, registra orçamentos, OS e estoque |
| Mecânico          | Funcionário que executa o serviço            | Analisa o veículo e monta o orçamento; executa a OS no box designado              |
| Gestor da oficina | Responsável pela oficina                     | Acompanha o status das OS e o uso dos boxes                                       |

No Milestone 1, apenas a secretária tem login no sistema.

## 6. Restrições

- **Prazo:** as entregas seguem o calendário da disciplina (ver seção 10).
- **Gestão do projeto:** issues, milestones e quadro Kanban no GitLab da disciplina, com gestão coletiva do quadro e as métricas Cycle Time, Lead Time, Throughput, WIP e Rework Rate.
- **Software:** sem licença paga. O grupo usa GitLab institucional, Figma no plano Education e MySQL.

## 7. Fora do escopo

Nenhuma exclusão foi formalizada com o cliente até esta versão. Os itens com decisão pendente estão na seção 12.

## 8. Critérios de aceitação gerais

- Nenhuma OS é criada a partir de um orçamento que não esteja aprovado.
- Toda mudança de status de orçamento e de OS fica registrada com data, hora e autor.
- Um box com OS em execução não recebe outra designação.
- A consulta pela placa mostra o histórico das OS do veículo, ou indica que é a primeira visita.
- Os valores de desempenho e disponibilidade são definidos com o cliente, com base nas respostas da seção 12.

## 9. Entregáveis esperados

- Visão do produto, PM Canvas e glossário.
- Requisitos funcionais, não funcionais, histórias de usuário e métricas do processo.
- Wireframes, protótipo navegável e mapa do site.
- Modelagem: casos de uso, sequência, atividades, estados e classes, além do IFML.
- Modelo de dados (DER e script de criação).
- Matriz de rastreabilidade entre visão, requisitos e modelos.

## 10. Calendário

| Data       | Evento                          |
| ---------- | ------------------------------- |
| 08/09/2026 | Milestone 1, monitoria          |
| 15/09/2026 | Milestone 1, apresentação       |
| 13/10/2026 | Milestone 2, monitoria          |
| 20/10/2026 | Milestone 2, apresentação       |
| 01/12/2026 | Milestone 3, monitoria          |
| 08/12/2026 | Milestone 3, apresentação final |

Fonte: PM Canvas do Milestone 1 ([pm-canvas.md](pm-canvas.md)) e milestones do grupo no GitLab.

## 11. Como o grupo será avaliado

Checklist de entrega do Milestone 3, conforme o modelo de issue do GitLab:

| Item                        | Pontos |
| --------------------------- | ------ |
| Modelagem do banco de dados | 10     |
| Diagramas UML               | 20     |
| Processos                   | 5      |
| Documentação do projeto     | 40     |
| Apresentação                | 15     |
| Autoavaliação               | 5      |
| Nota do cliente             | 5      |

## 12. Perguntas em aberto ao cliente

Enquanto não houver resposta, a equipe registra a premissa adotada.

1. Qual tempo de resposta é aceitável nas consultas de histórico, estoque e relatórios (RNF-03)?
2. Qual é o horário de funcionamento da oficina que define a disponibilidade exigida (RNF-04)?
3. Quantos boxes e quantos mecânicos a oficina tem hoje?
4. O nível mínimo de estoque é definido por peça ou por categoria, e quem o altera?
5. Por quais meios o cliente responde ao orçamento, além de presencial, telefone e mensagem?
6. O pagamento do serviço (inclusive parcelado) entra no sistema?
7. O cadastro de distribuidoras é necessário, ou basta o nome do fornecedor na peça?
8. Mecânico e gestor da oficina terão login em uma etapa futura?
9. Por quanto tempo o histórico do veículo e das OS deve ser guardado?

## Veja também

- [Visão do produto](README.md)
- [PM Canvas](pm-canvas.md)
- [Glossário](glossario.md)
- [Requisitos funcionais](../03-requisitos/requisitos-funcionais.md)
- [Histórias de usuário](../03-requisitos/historias-de-usuario.md)
