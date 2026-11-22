# Requisitos

Milestone 1 · Fábrica de Software I · Unidade de requisitos

Esta pasta registra o levantamento de requisitos do Bancada, o sistema de gestão de oficina mecânica da RFP 2 (cliente Antônio Júnior). A fonte oficial dos requisitos são as issues do GitLab do projeto ([projeto2](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2)). Os documentos desta pasta consolidam essas issues, apontam onde cada regra aparece no banco do Milestone 2 ([06-banco-de-dados](../06-banco-de-dados/README.md)) e registram as divergências encontradas.

## Técnicas de elicitação

| Técnica | Fonte | O que produziu |
|---|---|---|
| Análise documental | RFP 2 apresentada pelo professor ([rfp.md](../01-visao/rfp.md)) | Necessidades N01 a N12, usuários e restrições |
| Reunião com o cliente | Reunião de levantamento de 26/08/2026, gravada em áudio; ata na issue [#2](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/2) | Fluxo da OS, perfis de usuário, regras de estoque e de retirada do veículo |
| PM Canvas | Issue [#3](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/3) e [pm-canvas.md](../01-visao/pm-canvas.md) | Problema, objetivos e partes interessadas |
| Prototipação | Arquivo do Figma "Gestão de Oficina Mecânica" (telas de 1440 × 900) | Validação visual do fluxo de balcão, orçamento, OS, boxes, estoque e histórico do veículo |
| Modelagem do banco | [06-banco-de-dados](../06-banco-de-dados/README.md) | Restrições que confirmam ou contradizem os requisitos (unicidade, status, gatilhos) |

## Reunião com o cliente

| Data | Participantes | Principais achados |
|---|---|---|
| 26/08/2026, 20h40 às 20h55 | Equipe de desenvolvimento (conforme a ata) | Fluxo completo da OS: atendimento pela secretária, diagnóstico e orçamento pelo mecânico, autorização do cliente, execução, pagamento e entrega |
| | | Estoque próprio controlado pelo código da peça; peças trazidas pelo cliente cobram apenas a mão de obra, com garantia restrita ao serviço |
| | | Perfis citados: Secretária, Mecânico, Almoxarifado e Gerente/Financeiro; só a gerência e o financeiro alteram a tabela de preços dos serviços |
| | | O veículo só pode ser retirado pela mesma pessoa que o entregou; a plataforma principal é web |

Parte desses achados ainda não virou requisito. Os itens pendentes estão em [Divergências](requisitos-funcionais.md#divergências).

## Priorização

O grupo prioriza no GitLab com a label `prio::`. Para seguir o modelo da disciplina, a label foi convertida em MoSCoW:

| Label no GitLab | MoSCoW | Significado |
|---|---|---|
| `prio::P1` | Must | Sem o item, o fluxo principal (balcão, orçamento, OS) não funciona |
| `prio::P2` | Should | Agrega valor forte, mas o fluxo principal funciona sem ele |
| `prio::P3` | Could | Desejável; entra se houver tempo |
| (sem label) | A definir | Item sem prioridade registrada no GitLab |

O tamanho usa a label `size::` (S, M, L), copiada sem alteração.

## Validação com o cliente

A validação acontece nas apresentações de cada milestone, com o cliente e o professor (15/09, 20/10 e 08/12/2026). As perguntas que ainda dependem do cliente estão na seção 12 da [RFP](../01-visao/rfp.md#12-perguntas-em-aberto-ao-cliente). Enquanto não houver resposta, os documentos desta pasta registram a premissa adotada.

## Método de análise

1. **Inventário.** Todas as issues de requisito (RF, RNF) e de história (US) do GitLab foram lidas, com labels, milestone e texto completo.
2. **Agrupamento.** Cada história foi ligada ao RF citado no seu título. Cada caso de uso foi ligado à história citada no título da issue de UC (padrão `UC-0YY - US-0XX - <nome>`).
3. **Conferência com o banco.** Cada regra verificável foi procurada no banco do Milestone 2 ([06-banco-de-dados](../06-banco-de-dados/README.md)). Quando a regra existe, o documento cita a tabela, a coluna ou a restrição.
4. **Redação verificável.** O texto das issues foi preservado, com correção ortográfica. As regras de cada RF foram reescritas como itens verificáveis, sem mudar o sentido.
5. **Critérios de aceite.** Histórias sem critério de aceite, ou com critério fora do padrão `Dado` / `Quando` / `Então` / `E`, receberam um cenário marcado como `@proposta`, para revisão da equipe.
6. **Rastreabilidade.** As tabelas de mapeamento RF → US → UC alimentam o [backlog](backlog.md), o [diagrama de rastreabilidade](rastreabilidade-requisitos.svg) e a [matriz](../07-rastreabilidade/matriz.md).

## Convenções de identificação

Os identificadores são os das issues do GitLab, para manter a rastreabilidade entre os artefatos.

| Prefixo | Significado | Exemplo |
|---|---|---|
| `RF-NN` | Requisito funcional (agrupa histórias) | RF-02 Orçamento e aprovação do serviço |
| `RNF-NN` | Requisito não funcional | RNF-02 Segurança |
| `US-NNN` | História de usuário | US-016 Montar o orçamento da OS |
| `UC-NNN` | Caso de uso | UC-006 Cadastrar orçamento |
| `DS-NNN`, `DA-NNN`, `DE-NNN` | Diagramas de sequência, atividades e estados | DS-003, DA-004, DE-001 |
| `ACT-NN` | Ator | ACT-01 Secretária |
| `#N` | Número da issue no GitLab | #13 |

Algumas histórias estão fora do padrão `US-0XX` no GitLab (US-26, US-27, US-28, "US -20" e "US - 030"). Nesta pasta elas aparecem como US-026, US-027, US-028 e US-030; a "US -20" é citada pela issue (#42), porque o número 020 já pertence a outra história. A correção sugerida está em [Divergências](historias-de-usuario.md#divergências).

## Atores

| ID | Ator | Papel no sistema |
|---|---|---|
| ACT-01 | Secretária | Único perfil com login no Milestone 1 (`funcionario.func_login`). Realiza todos os casos de uso do catálogo |
| ACT-02 | Mecânico | Aparece nas histórias US-016 e #42. Sem login no Milestone 1; é gravado na OS (`ordem_servico.meca_id`) |
| ACT-03 | Gestor da oficina | Aparece nas histórias US-018 e US-027. Sem login no Milestone 1 |

As histórias também citam "funcionário de estoque", "funcionário de atendimento" e "vendedora", e os casos de uso citam "Estoquista" e "Gerente". Pela premissa do Milestone 1, todos esses papéis são exercidos pela secretária (ACT-01). A ata da reunião cita ainda Almoxarifado e Gerente/Financeiro como perfis futuros.

## Arquivos desta pasta

- [requisitos-funcionais.md](requisitos-funcionais.md): RF-01 a RF-07, regras verificáveis, mapeamento RF → US → UC e divergências.
- [requisitos-nao-funcionais.md](requisitos-nao-funcionais.md): RNF-01 a RNF-05, com métrica proposta e evidência.
- [historias-de-usuario.md](historias-de-usuario.md): as 30 histórias do GitLab, com critérios de aceite em Gherkin.
- [backlog.md](backlog.md): itens ordenados por prioridade, com o estado de cada artefato.
- [rastreabilidade-requisitos.svg](rastreabilidade-requisitos.svg): diagrama de rastreabilidade entre RF, US e UC.

## Veja também

- [Visão do produto](../01-visao/README.md)
- [RFP do cliente](../01-visao/rfp.md)
- [Glossário](../01-visao/glossario.md)
- [Casos de uso](../05-modelagem/casos-de-uso/README.md)
- [Protótipo e telas](../04-prototipo/README.md)
- [Matriz de rastreabilidade](../07-rastreabilidade/matriz.md)
