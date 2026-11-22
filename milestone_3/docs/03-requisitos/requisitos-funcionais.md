# Requisitos funcionais

Milestone 1 · Fábrica de Software I · Unidade de requisitos

Os requisitos funcionais do Bancada são as issues RF-01 a RF-07 do GitLab. Cada RF agrupa histórias de usuário e casos de uso. Para cada RF, este documento traz o texto da issue, as regras verificáveis que dele decorrem e a evidência no banco do Milestone 2 ([06-banco-de-dados](../06-banco-de-dados/README.md)). A prioridade MoSCoW vem da label `prio::` (ver [README](README.md#priorização)).

## Como ler

A coluna "Situação" indica como cada regra está coberta hoje. Não há código da aplicação no repositório; por isso a evidência é o banco e o protótipo.

| Situação | Significado |
|---|---|
| Coberta pelo banco | Uma restrição, chave ou gatilho do banco garante a regra |
| Dados disponíveis | O banco guarda os dados; a regra depende da aplicação |
| Parcial | O banco cobre parte da regra |
| Lacuna | O banco não tem como cumprir a regra |

## RF-01 Atendimento

| Campo | Conteúdo |
|---|---|
| Issue | [#4](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/4) |
| Ator | ACT-01 Secretária |
| Prioridade | Must (`prio::P1`) · tamanho L |
| Milestone | Milestone 1 |
| Histórias | US-001 a US-015 |
| Casos de uso | UC-001 a UC-005, UC-008, UC-011, UC-012, UC-013 |

**Texto da issue.** O cliente é atendido por uma secretária autenticada. Para isso, a secretária deve possuir um cadastro especial (por ser uma secretária), estar logada no sistema e verificar com o cliente se ele veio trazer ou buscar um veículo. A secretária também pode atualizar as informações do cliente, caso seja necessário.

| Regra verificável | Evidência no banco | Situação |
|---|---|---|
| Só funcionário com login e senha acessa o sistema; login e senha existem juntos | `funcionario.func_login`, `funcionario.func_senha`, `uk_funcionario_login`, `ck_func_acesso` | Coberta pelo banco |
| O cargo de secretária é identificado no cadastro do funcionário | `funcao.funo_funcao`, `fk_func_funcao` | Dados disponíveis |
| O cliente é cadastrado com CPF único; CPF repetido é recusado | `pessoa.pess_cpf`, `uk_pessoa_cpf` | Coberta pelo banco |
| O veículo é cadastrado com placa única, ligado a um cliente; placa repetida é recusada | `veiculo.veic_placa`, `uk_veiculo_placa`, `veiculo.pess_id` | Coberta pelo banco |
| A atualização de cliente ou veículo não pode gerar CPF ou placa duplicados | `uk_pessoa_cpf`, `uk_veiculo_placa` | Coberta pelo banco |
| A secretária registra se o cliente veio trazer ou buscar o veículo | Sem tabela de entrada ou retirada | Lacuna |

## RF-02 Orçamento e aprovação do serviço

| Campo | Conteúdo |
|---|---|
| Issue | [#13](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/13) |
| Ator | ACT-02 Mecânico (monta o orçamento); ACT-01 Secretária (registra a decisão) |
| Prioridade | Must (`prio::P1`) · tamanho L |
| Milestone | Milestone 1 |
| Histórias | US-016, US-017, US-018, US-026 |
| Casos de uso | UC-006, UC-007, UC-009, UC-010 |

**Texto da issue.** Todo orçamento deve percorrer o ciclo Aberta → Orçada → Aprovada, ou Recusada após o orçamento. O orçamento é montado item a item, separando peças e mão de obra, e o serviço só pode iniciar depois de registrada a aprovação do dono do veículo, com o meio pelo qual a resposta chegou (presencial, telefone, mensagem). Cada mudança de status grava data/hora e autor.

| Regra verificável | Evidência no banco | Situação |
|---|---|---|
| O orçamento passa por `aberto` e `orçado` e termina em `aprovado` ou `recusado` | `orcamento.orca_status`, `ck_orca_status` | Coberta pelo banco (os valores) |
| As peças entram com quantidade e preço unitário; a mão de obra entra separada, com valor | `orcamento_peca` (`orpe_quantidade`, `orpe_preco_unitario`), `orcamento_servico` (`orse_valor`) | Coberta pelo banco |
| Nenhuma OS é criada sem orçamento aprovado, e cada orçamento gera no máximo uma OS | Gatilho `tg_os_bi_orcamento_aprovado`, `uk_os_orcamento` | Coberta pelo banco |
| A aprovação registra o meio da resposta: presencial, telefone ou mensagem | `orcamento.orca_formato`, sem restrição de valores | Parcial |
| Cada mudança de status grava o novo status, o funcionário e a data/hora | `orcamento_historico` (`orhi_status`, `func_id`, `ORHI_CRIADO_EM`) | Coberta pelo banco |
| A transição segue a ordem do ciclo (não se aprova orçamento ainda `aberto`) | Sem gatilho de transição | Lacuna |

## RF-03 Distribuição do trabalho entre boxes e mecânicos

| Campo | Conteúdo |
|---|---|
| Issue | [#15](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/15) |
| Ator | ACT-01 Secretária; ACT-02 Mecânico (recebe a designação) |
| Prioridade | Must (`prio::P1`) · tamanho L |
| Milestone | Milestone 1 |
| Histórias | "US -20" Organizar minha agenda de boxes (#42) |
| Casos de uso | Nenhum no catálogo |

**Texto da issue.** Toda OS aprovada deve ser designada a um box e a um mecânico responsável antes de entrar em execução. Um box comporta um veículo por vez: enquanto houver OS em execução nele, fica indisponível para novas designações. Início e conclusão de serviço gravam data/hora e mecânico, alimentando o tempo de permanência do veículo na oficina.

| Regra verificável | Evidência no banco | Situação |
|---|---|---|
| Um box tem no máximo uma OS `em execução` | Coluna gerada `os_box_ativo`, `uk_os_box_ativo` | Coberta pelo banco |
| A OS só entra `em execução` com box e mecânico definidos | `ordem_servico.box_id` e `meca_id` aceitam nulo; sem restrição | Lacuna |
| Início e conclusão gravam data/hora e mecânico | `ordem_servico.os_inicio`, `os_fim`, `meca_id`; `ordem_servico_historico` | Dados disponíveis |
| O tempo de permanência do veículo é calculado a partir do início e da conclusão | `os_inicio`, `os_fim` | Dados disponíveis |

## RF-04 Histórico do veículo

| Campo | Conteúdo |
|---|---|
| Issue | [#17](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/17) |
| Ator | ACT-01 Secretária |
| Prioridade | A definir (issue sem labels e sem milestone) |
| Milestone | Não atribuída |
| Histórias | US-029, US-030 |
| Casos de uso | Nenhum no catálogo |

**Texto da issue.** O sistema deve manter e permitir a consulta do histórico das ordens de serviço de cada veículo, utilizando a placa como referência. O histórico deve apresentar datas, quilometragem, serviços executados, peças utilizadas, valores e mecânico ou box responsável.

| Regra verificável | Evidência no banco | Situação |
|---|---|---|
| A consulta parte da placa, que identifica um único veículo | `uk_veiculo_placa` | Coberta pelo banco |
| O histórico mostra datas, quilometragem, serviços, peças, valores, mecânico e box de cada OS | `OS_CRIADO_EM`, `os_inicio`, `os_fim`, `orca_quilometragem`, `orcamento_servico`, `ordem_servico_peca`, `orca_valor`, `meca_id`, `box_id` | Dados disponíveis |

## RF-05 Controle de estoque de peças

| Campo | Conteúdo |
|---|---|
| Issue | [#30](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/30) |
| Ator | ACT-01 Secretária (nas histórias: funcionário de estoque ou de atendimento) |
| Prioridade | Must (`prio::P1`) · tamanho L |
| Milestone | Milestone 1 |
| Histórias | US-019 a US-024 |
| Casos de uso | UC-015 a UC-022 |

**Texto da issue.** O sistema deve permitir o cadastro, a consulta, a edição e a exclusão de peças no estoque, informando nome, código, quantidade disponível, valor unitário e fornecedor. A secretária ou o funcionário autenticado deve poder filtrar as peças por nome, código ou categoria, e o sistema deve indicar quando a quantidade de uma peça atingir o nível mínimo definido pela oficina.

| Regra verificável | Evidência no banco | Situação |
|---|---|---|
| A peça é cadastrada com nome, código único, quantidade e valor unitário | `peca` (`peca_nome`, `peca_codigo`, `peca_estoque`, `peca_preco`), `uk_peca_codigo` | Coberta pelo banco |
| A peça é ligada ao fornecedor | `peca_distribuidora` | Dados disponíveis |
| A quantidade em estoque nunca fica negativa | `ck_peca_estoque` | Coberta pelo banco |
| As peças podem ser filtradas por nome, código ou categoria | `peca_nome`, `peca_codigo`, `peca_categoria` | Dados disponíveis |
| O sistema avisa quando a quantidade chega ao mínimo definido | `peca_estoque_minimo` | Dados disponíveis |
| A exclusão não apaga o rastro de peças já usadas em OS antigas | `peca_ativo` (exclusão lógica) | Dados disponíveis |

## RF-06 Ordem de serviço

| Campo | Conteúdo |
|---|---|
| Issue | [#38](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/38) |
| Ator | ACT-01 Secretária; ACT-03 Gestor da oficina (consulta o histórico) |
| Prioridade | Must (`prio::P1`) · tamanho M |
| Milestone | Milestone 1 |
| Histórias | US-027, US-028 |
| Casos de uso | UC-013, UC-014 |

**Texto da issue.** Após a aprovação do cliente, o orçamento vira uma ordem de serviço, que passa para o ciclo da OS: Aprovada → em execução → concluída.

| Regra verificável | Evidência no banco | Situação |
|---|---|---|
| A OS nasce de um orçamento aprovado | Gatilho `tg_os_bi_orcamento_aprovado` | Coberta pelo banco |
| A OS passa por `aberta`, `em execução` e `concluída` | `ordem_servico.os_status`, `ck_os_status` | Coberta pelo banco (os valores; ver divergência 1) |
| Cada mudança de status da OS grava o status, o funcionário e a data/hora | `ordem_servico_historico` (`oshi_status`, `func_id`, `OSHI_CRIADO_EM`) | Coberta pelo banco |

## RF-07 Relatório da ordem de serviço

| Campo | Conteúdo |
|---|---|
| Issue | [#43](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/43) |
| Ator | ACT-01 Secretária |
| Prioridade | Must (`prio::P1`) · tamanho M |
| Milestone | Milestone 1 |
| Histórias | Nenhuma |
| Casos de uso | UC-023 |

**Texto da issue.** O sistema deve permitir ver o relatório da ordem de serviço, que deve conter o nome do cliente, o nome do funcionário, o nome do mecânico, o orçamento ou valor total e as datas de abertura e de fechamento.

| Regra verificável | Evidência no banco | Situação |
|---|---|---|
| O relatório mostra cliente, funcionário, mecânico, valor total, abertura e fechamento de cada OS | `veiculo.pess_id` → `pessoa`, `ordem_servico.func_id`, `meca_id`, `orcamento.orca_valor`, `OS_CRIADO_EM`, `os_fim` | Dados disponíveis |

## Mapeamento RF → US → UC

| RF | Nome | Histórias | Casos de uso | MoSCoW |
|---|---|---|---|---|
| RF-01 | Atendimento | US-001 a US-015 (15) | UC-001 a UC-005, UC-008, UC-011, UC-012, UC-013 | Must |
| RF-02 | Orçamento e aprovação do serviço | US-016, US-017, US-018, US-026 (4) | UC-006, UC-007, UC-009, UC-010 | Must |
| RF-03 | Distribuição do trabalho entre boxes e mecânicos | #42 (1) | Nenhum | Must |
| RF-04 | Histórico do veículo | US-029, US-030 (2) | Nenhum | A definir |
| RF-05 | Controle de estoque de peças | US-019 a US-024 (6) | UC-015 a UC-022 | Must |
| RF-06 | Ordem de serviço | US-027, US-028 (2) | UC-013, UC-014 | Must |
| RF-07 | Relatório da ordem de serviço | Nenhuma | UC-023 | Must |

Total: 7 RF, 30 histórias e 23 casos de uso. Must 6, a definir 1.

Os casos de uso estão agrupados pelo assunto do RF. Os vínculos de cada caso de uso com uma história específica seguem o GitLab e estão na coluna UC de [historias-de-usuario.md](historias-de-usuario.md#índice).

## Divergências

Diferenças entre as issues do GitLab, a ata da reunião com o cliente e o banco do Milestone 2. A equipe deve decidir cada ajuste.

1. **Ciclo de status da OS.** A RF-06 descreve `Aprovada → em execução → concluída`. O banco usa `aberta`, `em execução` e `concluída`, e a regra decidida pelo grupo é que `aprovado` pertence ao orçamento. O texto da RF-06 precisa ser atualizado.
2. **Status do orçamento escritos como status da OS.** A RF-02 usa "Aberta, Orçada, Aprovada, Recusada", e as histórias US-016 e US-017 falam em "OS Orçada" e "OS Recusada". No banco esses status são do orçamento (`aberto`, `orçado`, `aprovado`, `recusado`).
3. **Meio da resposta sem valores definidos.** A RF-02 exige registrar presencial, telefone ou mensagem. A coluna `orca_formato` aceita qualquer texto. Sugestão: restrição `CHECK` com os três valores.
4. **Box e mecânico não obrigatórios na execução.** A RF-03 exige box e mecânico antes de `em execução`. O banco permite OS `em execução` com `box_id` ou `meca_id` nulos.
5. **Achados da reunião sem requisito.** A ata de 26/08 traz regras que não viraram issue: peças trazidas pelo cliente (cobrança só da mão de obra), retirada do veículo apenas pela pessoa que o entregou, pagamento antes da entrega e tabela de preços alterada só pela gerência e pelo financeiro. O banco tem `pagamento` e `pagamento_parcelas`, sem requisito correspondente.
6. **Entrada e retirada sem registro.** A RF-01 pede verificar se o cliente veio trazer ou buscar o veículo, e a US-002 muda o "status do veículo para retirado". O banco não tem status do veículo nem registro de quem entregou ou retirou.
7. **Distribuidora sem história.** Os casos de uso UC-015, UC-016, UC-018 e UC-019 tratam de distribuidora e apontam para `US-000`. A RF-05 cita apenas "fornecedor".
8. **Vínculos UC → US incorretos.** UC-009 e UC-010 (recusar e aprovar orçamento) apontam para a US-028, que trata do status da OS; a história correspondente é a US-017. UC-014 (criar OS depois do orçamento aprovado) também aponta para a US-028, que não trata da criação da OS. UC-007 (lista de orçamentos) e UC-020 (lista de peças) apontam para `US-000`, embora a US-026 e a US-023 tratem dessas listas.
9. **RF-04 sem classificação.** A issue #17 não tem labels de prioridade, tamanho e tipo, nem milestone.
10. **RF-07 classificado como documentação.** A issue #43 tem `type::Documentation`, embora descreva funcionalidade, e não tem história vinculada. O caso de uso UC-023 aponta para `US-000`.
11. **Autenticação sem caso de uso.** A RF-01 e a US-001 tratam do login da secretária, mas o catálogo não tem UC de autenticação; os casos de uso apenas incluem "Autenticar" (`<<include>>`).
12. **OS criada sem orçamento.** A US-008 cadastra a OS direto no balcão, com situação padrão "em andamento". O banco exige orçamento aprovado (`tg_os_bi_orcamento_aprovado`) e usa `aberta` como padrão.

## Veja também

- [Levantamento e método](README.md)
- [Requisitos não funcionais](requisitos-nao-funcionais.md)
- [Histórias de usuário](historias-de-usuario.md)
- [Backlog](backlog.md)
- [Casos de uso](../05-modelagem/casos-de-uso/README.md)
- [Matriz de rastreabilidade](../07-rastreabilidade/matriz.md)
- [Rastreabilidade de requisitos (diagrama)](rastreabilidade-requisitos.svg)
