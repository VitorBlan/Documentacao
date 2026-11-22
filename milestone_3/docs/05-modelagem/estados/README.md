# Máquinas de estados

Milestone 3 · Modelagem comportamental (Fábrica de Software I)

Estes diagramas mostram o ciclo de vida dos objetos de domínio que têm um campo `status`: a ordem de serviço (OS) e o orçamento. Os estados são situações do objeto, e não ações ("Em execução", não "Executar"). As transições são nomeadas pelo evento que as dispara; a guarda aparece entre colchetes e vem dos cenários em Gherkin das issues do GitLab, porque o projeto ainda não tem código da aplicação. Nos diagramas do orçamento, a seta tracejada em vermelho indica uma transição bloqueada, que gera uma notificação de erro para a Secretária. O Box também tem `status`, mas não tem máquina de estados (lacuna L-13 de [lacunas.md](../../07-rastreabilidade/lacunas.md)). Segundo a revisão de classes com status, só a OS, o orçamento e o Box têm esse atributo (seção 2.4 de [lacunas.md](../../07-rastreabilidade/lacunas.md)).

Os diagramas foram desenhados na página "08 - Diagrama de Atividades e Estados" do arquivo do Figma "Gestão de Oficina Mecânica". Cada um tem uma issue no GitLab e se liga a um diagrama de sequência pelo padrão de título `DE-00X | DS-0YY - <nome>`.

## Catálogo

| DE | Objeto | Diagrama | DS | UC | US | Issue |
|---|---|---|---|---|---|---|
| DE-001 | Ordem de serviço | Mudar status de uma OS | DS-008 | UC-013 | US-002, US-028 | [#107](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/107) |
| DE-002 | Orçamento | Mudar o status do orçamento para aprovado | DS-012 | UC-010 | US-017 | [#111](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/111) |
| DE-003 | Orçamento | Mudar o status do orçamento para recusado | DS-013 | UC-009 | US-017 | [#112](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/112) |

O GitLab vincula UC-009 e UC-010 à US-028, que trata do status da OS. A história correspondente é a US-017, que é a usada na tabela (ver a divergência 8 de [requisitos-funcionais.md](../../03-requisitos/requisitos-funcionais.md#divergências)).

## Ordem de serviço (DE-001)

![Máquina de estados da OS: Aberta, Em execução e Concluída, com as transições criar OS, iniciar serviço e concluir serviço](<DE-001 _ DS-008 - Mudar status de uma OS.svg>)

O diagrama segue o ciclo da OS decidido pelo grupo: `aberta → em execução → concluída`. Os status de decisão do cliente (aprovado e recusado) pertencem ao orçamento e aparecem em DE-002 e DE-003.

| Origem | Evento | Guarda | Destino | Quem pode | Fonte |
|---|---|---|---|---|---|
| (inicial) | criar OS | O orçamento está aprovado | Aberta | Secretária | Diagrama; DA-005 |
| Aberta | iniciar serviço | Box e mecânico designados | Em execução | Não definido | Diagrama; RF-03 |
| Em execução | concluir serviço | Nenhuma | Concluída | Não definido | Diagrama |
| Concluída | (sem evento) | Nenhuma | (final) | Não definido | Diagrama |

Notas:

- Toda mudança de status da OS registra funcionário, data e hora (RF-06). A nota do diagrama cita a RF-06.
- A OS só nasce de um orçamento aprovado. Por isso a transição inicial tem a guarda "O orçamento está aprovado" e não existe estado de orçamento no ciclo da OS.
- A RF-03 exige box e mecânico designados antes de a OS entrar em execução. A guarda aparece na transição "iniciar serviço".
- Concluída é o único estado final: nenhuma transição sai dele.

## Orçamento (DE-002 e DE-003)

Os dois diagramas descrevem o mesmo objeto, o orçamento, e cada um destaca uma transição de decisão do cliente: aprovar (DE-002) e recusar (DE-003).

### Mudar o status do orçamento para aprovado (DE-002)

![Máquina de estados do orçamento com foco na aprovação: Em aberto, Orçada, Recusado e Aprovado, com as tentativas de aprovar bloqueadas em tracejado](<DE-002 _ DS-012 - Mudar o status do orçamento para aprovado.svg>)

### Mudar o status do orçamento para recusado (DE-003)

![Máquina de estados do orçamento com foco na recusa: Em aberta, Orçada, Aprovada e Recusada, com a recusa a partir de Em aberta bloqueada em tracejado](<DE-003 _ DS-013 - Mudar o status do orçamento para recusado.svg>)

Transições do orçamento, reunindo os dois diagramas. Os nomes dos estados seguem o DE-002 e o DA-004 ("Em aberto", "Orçada", "Aprovado", "Recusado"); a divergência 3 trata das variações de grafia.

| Origem | Evento | Guarda | Destino | Quem pode | Fonte |
|---|---|---|---|---|---|
| (inicial) | criar | Nenhuma | Em aberto | Secretária | Diagrama; DA-004 |
| Em aberto | orçar | Nenhuma | Orçada | Não definido | Diagrama |
| Orçada | aprovar / notificar sucesso | Status igual a Orçada | Aprovado | Secretária | Cenário 2 da issue #111 |
| Orçada | recusar / registrar motivo | Status igual a Orçada | Recusado | Secretária | Cenário 2 da issue #112 |
| Em aberto | aprovar (bloqueada) | Status diferente de Orçada | Permanece Em aberto, com notificação de erro | Secretária | Cenário 1 da issue #111 |
| Recusado | aprovar (bloqueada) | Status diferente de Orçada | Permanece Recusado, com notificação de erro | Secretária | Cenário 1 da issue #111 |
| Em aberto | recusar (bloqueada) | O orçamento ainda está Em aberto | Permanece Em aberto, com notificação de que a mudança não é permitida | Secretária | Cenário 1 da issue #112 |
| Aprovado | (sem evento) | Nenhuma | (final) | Não definido | Diagrama |
| Recusado | (sem evento) | Nenhuma | (final) | Não definido | Diagrama |

Notas:

- Só um orçamento Orçada pode ser aprovado ou recusado. Orçamentos Em aberto não podem ser decididos antes de serem orçados.
- Aprovado e Recusado são estados finais. O orçamento aprovado é a condição para criar a OS (ver [Diagramas de atividades](../atividades/README.md), DA-005).
- Cada mudança de status do orçamento grava o novo status, o funcionário e a data e hora (RF-02, US-018).

## Divergências

Diferenças entre os diagramas e as demais fontes do projeto. Onde há diferença, as issues do GitLab e as decisões do grupo registradas nos requisitos prevalecem, e as tabelas acima seguem as issues.

1. O DE-001 mistura status do orçamento (Orçada, Aprovada, Recusada) com os da OS e acrescenta Entregue. O ciclo da OS decidido pelo grupo é `aberta → em execução → concluída`, e `aprovado` e `recusado` pertencem ao orçamento (divergências 1 e 2 de [requisitos-funcionais.md](../../03-requisitos/requisitos-funcionais.md#divergências) e lacuna L-11 de [lacunas.md](../../07-rastreabilidade/lacunas.md)).
2. O DE-001 começa com "abrir OS" antes do orçamento. A regra do projeto é que a OS nasce de um orçamento aprovado, o que o DA-005 também mostra (a OS é registrada depois da verificação do orçamento aprovado).
3. O nome do primeiro estado varia: "Aberta" (DE-001), "Em aberto" (DE-002 e DA-004) e "Em aberta" (DE-003 e issue #112). Os estados finais também alternam entre masculino e feminino ("Aprovado" e "Recusado" no DE-002; "Aprovada" e "Recusada" no DE-003). No banco, os status do orçamento são `aberto`, `orçado`, `aprovado` e `recusado`.
4. O DE-002 desenha `recusar` de Em aberto para Recusado como transição permitida. O DE-003 e a issue #112 bloqueiam essa mudança, e só permitem recusar a partir de Orçada.
5. `criar` e `orçar` não têm cenário em nenhuma das issues. O DE-002 traz na própria figura uma nota de pendência pedindo para confirmar `criar`, `orçar` e `recusar` antes de fechar o diagrama.
6. A nota do DE-001 cita a RF-02 para o registro de data, hora e autor. Na RF-02 essa regra é do histórico do orçamento; a da OS está na RF-06.
7. O DE-001 está vinculado ao UC-013 (Mudar status de uma OS para concluída), mas desenha o ciclo inteiro da OS, não só a conclusão.

## Veja também

- [Diagramas de atividades](../atividades/README.md): o fluxo que dispara estas transições (DA-005 cria a OS e DA-007 atualiza o orçamento).
- [Diagrama de classes](../classes/README.md): classes `OrdemServico`, `Orcamento` e `HistoricoStatusOS` e seus atributos.
- [Casos de uso](../casos-de-uso/README.md): UC-009, UC-010 e UC-013, que disparam as transições.
- [Diagramas de sequência](../sequencia/README.md): troca de mensagens em DS-008, DS-012 e DS-013.
- [Modelagem](../README.md): visão geral da unidade.
