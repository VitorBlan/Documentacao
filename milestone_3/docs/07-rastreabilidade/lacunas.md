# Lacunas e órfãos de rastreabilidade

Milestone 3 · Fábrica de Software I · Unidade de rastreabilidade

Responde às quatro perguntas da revisão cruzada (UML × protótipo × requisitos), lista os órfãos nos dois sentidos e registra cada lacuna com ação e prazo. A tabela base está em [matriz.md](matriz.md).

Situação em 22/11/2026. Os vínculos e a cobertura de caso de uso, classe, sequência, atividade e estado foram revistos com os documentos de [05-modelagem](../05-modelagem/README.md). A conferência de tela, de tabela e das issues do GitLab é a de 15/11/2026 (issues do [projeto2](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2), arquivo do Figma "Gestão de Oficina Mecânica" e banco do Milestone 2, `milestone_2/ddl.sql`) e não foi repetida. As lacunas ainda não têm issue aberta. Os prazos são sugeridos para antes da monitoria do Milestone 3 (01/12/2026); os de 20/11/2026 já passaram e precisam ser reprogramados.

## Resumo das quatro perguntas

| Pergunta | Resposta | Situação |
|---|---|---|
| Toda US tem UC? | Não. 16 das 30 histórias têm caso de uso e 14 não têm. A modelagem liga a US-008 e a US-017 aos UC-014, UC-009 e UC-010, mas o título das issues no GitLab cita a US-028. | Não |
| Todo dado do wireframe existe em classe? | Quase todos. Cinco dados de tela não têm atributo, o histórico de status do orçamento só existe no diagrama de apoio, e duas telas contradizem o requisito. | Parcial |
| Todo cenário principal tem sequência? | Não. 16 dos 23 casos de uso têm diagrama, e três desses diagramas (DS-004, DS-018 e DS-019) não desenham o que a issue descreve. Os de peças, distribuidoras (lista e informações) e relatórios não têm. RF-03 e RF-04 não têm caso de uso nem sequência. | Não |
| Toda classe com status tem máquina de estados? | Em parte. OrdemServico tem DE-001; Orcamento tem DE-002 e DE-003, que se contradizem; Box não tem diagrama. | Parcial |

## 1. Órfãos

### 1.1 Histórias sem elo

| Elo ausente | Histórias | Qtde |
|---|---|---|
| Caso de uso | US-001, US-005, US-009, US-012, US-015, US-018, US-022, US-023, US-024, US-026, US-027, US-029, US-030, #42 | 14 |
| Sequência | US-001, US-005, US-009, US-018, US-019 a US-024, US-026, US-027, US-029, US-030, #42 | 15 |
| Atividade ou estado | US-001, US-005, US-009, US-015, US-018, US-019 a US-024, US-026, US-027, US-029, US-030, #42 | 16 |
| Tela | US-002, US-008, US-009, US-027 | 4 |
| Teste | Todas | 30 |

A RF-07 também não tem sequência nem atividade. Ela entra na contagem da [matriz](matriz.md#cobertura-da-cadeia), que tem 31 linhas.

Em relação a 15/11/2026, a US-008 e a US-017 deixaram a lista de casos de uso, a US-008 deixou a de sequência, e a US-008 e a US-016 deixaram a de atividade ou estado, porque os diagramas correspondentes (DS-004, DA-005 e DA-004) passaram a ter conteúdo e o vínculo com o caso de uso foi corrigido na modelagem.

### 1.2 Itens sem requisito (sentido inverso)

| Item | Onde está | Situação |
|---|---|---|
| Distribuidoras | Classes `Distribuidora` e `PecaDistribuidora`; UC-015, UC-016, UC-018, UC-019; DS-010, DS-011, DA-009, DA-010; seis telas "US-xxx"; tabelas `distribuidora` e `peca_distribuidora` | Lacuna L-17: modelado de ponta a ponta, sem história |
| Atualizar informações do orçamento | DS-007 (`UC-000`) e DA-007 | Lacuna L-17: nenhuma história pede a edição do orçamento |
| Visualizar informações do orçamento | DS-019 e DA-016 (`UC-000`, sem história) | Lacuna L-17: nenhuma história pede essa consulta, e o DS-019 ainda não a desenha (L-18) |
| Pagamento | Tabelas `pagamento` e `pagamento_parcelas`; card "Relatório de Faturamento" | Lacuna L-16: sem requisito e sem classe |
| Filtros como casos de uso | Oito filtros `<<extend>>` no diagrama de casos de uso | Aceito: estendem casos de uso existentes. Quatro têm história (nome, US-005; status do orçamento, US-026; categoria e fornecedor da peça, US-023). Quatro não têm: CPF, placa, modelo e data de criação do orçamento |
| Sete relatórios além da RF-07 | Tela "Relatórios - tela principal" (ocupação dos boxes, faturamento, peças, clientes, mecânicos, veículos, orçamentos) | Lacuna L-16: a RF-07 trata só do relatório de OS |
| "Esqueci a senha" e "Manter conectada" | Tela de login | Lacuna L-16: sem requisito |
| Classe `Endereco` e `Funcao` | Diagrama de classes | Aceito: aparecem nas telas de cliente e no cabeçalho (cargo do usuário) |

Das 20 classes da [lista oficial](../05-modelagem/classes/README.md#lista-oficial-de-classes), 19 aparecem em pelo menos uma linha da matriz. A exceção é `Distribuidora`, que só aparece na primeira linha da tabela acima, por não ter história. A versão de 15/11/2026 deste documento afirmava que todas apareciam, o que não valia para `Distribuidora`.

## 2. As quatro perguntas em detalhe

### 2.1 Toda US tem UC?

- Não: 16 das 30 histórias têm caso de uso no catálogo.
- As 14 sem caso de uso estão na tabela 1.1. Entre elas estão a autenticação (US-001), o histórico do veículo (US-029, US-030), a agenda dos boxes (#42) e três histórias de estoque (US-022 a US-024). O catálogo de casos de uso lista as 14 em [Funcionalidades sem caso de uso](../05-modelagem/casos-de-uso/README.md#funcionalidades-sem-caso-de-uso). Lacuna L-01.
- Três casos de uso têm título de issue com a história errada: UC-009 e UC-010 (recusar e aprovar orçamento) e UC-014 (criar OS) citam a US-028, que trata do status da OS. A história correta é a US-017 nos dois primeiros e a US-008 no UC-014, e a modelagem já usa esses vínculos. UC-007 e UC-020 citam `US-000`, embora a US-026 e a US-023 tratem dessas listas. Lacuna L-02.
- Toda US tem RF? Sim: o título de cada história cita o RF de origem.

### 2.2 Todo dado do wireframe existe em classe?

Foram lidos os textos de 16 telas principais do protótipo: login, cadastro e detalhe de cliente e de veículo, montagem e visualização de orçamento, lista e detalhe de OS, agenda dos boxes (principal, designar e concluir), histórico do veículo, cadastro e detalhe de peça e relatórios. Botões, menu e textos de ajuda foram descartados. Cada dado foi procurado nas classes do diagrama de domínio.

Dados que existem nas classes: identificação e contato do cliente (`Pessoa`, `Cliente`, `Endereco`), placa, marca, modelo, cor, ano e quilometragem (`Veiculo`, `CarroModelo`, `CarroMarca`), valor e status do orçamento (`Orcamento`), status, mecânico e box da OS (`OrdemServico`, `Box`), código, preço, estoque e estoque mínimo da peça (`Peca`), compatibilidade com modelos (`Compatibilidade`), preço de compra (`PecaDistribuidora`) e matrícula do funcionário (`Funcionario`).

Dados de tela sem atributo de classe:

- **Formato do orçamento** ("Formato: Valor fixo", tela "Montar o orçamento"). `Orcamento` não tem o atributo; o banco tem `orca_formato`. O meio da resposta do cliente, exigido pela RF-02, também não tem atributo. Lacuna L-07.
- **Início da execução e tempo no box** ("Iniciado às 08:20 · 3h 05min no box", agenda dos boxes). `OrdemServico` tem `dataAbertura` e `dataConclusao`, mas não o início da execução; o banco tem `os_inicio`. Lacuna L-07.
- **Retirada do veículo** ("Retirado em", "Retirado por", detalhe do carro). Nenhuma classe guarda a retirada. Lacuna L-07.
- **Histórico de alterações** (aba do detalhe do cliente; pedido também na US-020 e na US-021 para a peça). Não há classe de histórico de cliente nem de peça. Lacuna L-07.
- **Situação ativa da peça** (exclusão lógica da US-022). `Peca` não tem `ativo`; o banco tem `peca_ativo`. Lacuna L-07.

Dado de tela com classe só no diagrama de apoio:

- **Histórico de status do orçamento** (tela inicial de Orçamentos, US-018). `HistoricoStatusOrcamento` foi desenhada em `classes-apoio.svg`, com os atributos `statusAnterior`, `statusNovo` e `dataHora`, propostos por analogia com `HistoricoStatusOS`. Falta incluí-la no diagrama principal, se o grupo preferir, e confirmar se a tabela `orcamento_historico` guarda o status anterior. Lacuna L-06.

Tabela sem classe: `ordem_servico_peca`, citada nos requisitos para o histórico do veículo, não tem classe; o diagrama liga as peças só ao orçamento. Lacuna L-07.

Divergências entre tela e requisito:

- **Orçamento sem itens.** A tela "Montar o orçamento" registra um valor fixo. A RF-02, a US-016 e as classes `ItemOrcamentoPeca` e `ItemOrcamentoServico` pedem o orçamento item a item, com peças e mão de obra separadas. Lacuna L-10.
- **OS com status "Aprovada".** A lista de OS ("Status: em aprovada") e o componente "Badge (aprovada)" usam "Aprovada" para a OS. O ciclo decidido pelo grupo é `aberta → em execução → concluída`. O DA-005 registra a OS como "Aprovada" e o DS-004 desenha `iniciarExecucao`. Lacuna L-11.

Diferenças de nome: a tela e o banco usam "Categoria" da peça (`peca_categoria`), e a classe usa `tipo`; a tela e a classe usam "Matrícula", e o banco usa `func_login`. O comparativo completo, com 14 diferenças entre OCL e classes, de um lado, e glossário e banco, do outro, está em [divergencias-m2.md](../05-modelagem/ocl/divergencias-m2.md). Lacuna L-15.

Resposta: parcial.

### 2.3 Todo cenário principal tem sequência?

| Casos de uso | Diagramas | Situação |
|---|---|---|
| UC-001 a UC-005, UC-008, UC-011, UC-012 (clientes e veículos) | DS-001, DS-005, DS-009, DS-002, DS-006, DS-014, DS-018, DS-015 | Sim. O DS-018 não desenha o perfil do cliente (L-18) |
| UC-006, UC-007, UC-009, UC-010 (orçamento) | DS-003, DS-016, DS-013, DS-012 | Sim. Em 15/11/2026, a issue DS-016 (#92) seguia aberta, embora o diagrama exista no Figma |
| UC-013, UC-014 (OS) | DS-008, DS-004 | Sim. O DS-004 não desenha o cadastro da OS, e o DS-008 não valida o status de origem (L-18) |
| UC-015, UC-016 (distribuidora) | DS-010, DS-011 | Sim |
| UC-017, UC-020, UC-021, UC-022 (peças) | Nenhum | Não |
| UC-018, UC-019 (distribuidora) | Nenhum | Não |
| UC-023 (relatórios) | Nenhum | Não |
| RF-03 agenda dos boxes, RF-04 histórico do veículo | Nenhum, e sem caso de uso | Não |

Resultado: 16 dos 23 casos de uso têm diagrama. Há ainda quatro diagramas sem caso de uso no catálogo (DS-007, DS-017, DS-019 e DS-020). Em 15/11/2026, o DS-020 estava desenhado no Figma sem título ("secr-fina-os"). Lacunas L-03 e L-04.

Existir o diagrama não significa que ele desenhe o que a issue descreve. O DS-004 não mostra a criação da OS nem a guarda de orçamento aprovado (issue #81); o DS-018 ainda tem a estrutura da agenda dos boxes (issue #94); o DS-019 mostra o fluxo de clientes (issue #96); o DS-007 usa `aberta`, `aprovada` e `recusada` e deixa o orçamento `orçado` sem ramo; o DS-008 não tem guarda de status nem o registro de funcionário, data e hora. Lacuna L-18.

### 2.4 Toda classe com status tem máquina de estados?

| Classe | Atributo | Diagrama | Situação |
|---|---|---|---|
| OrdemServico | `status : StatusOS` | DE-001 · DS-008 Mudar status de uma OS | Coberta. O DE-001 desenha o ciclo inteiro, e o UC-013 trata só da conclusão |
| Orcamento | `status : StatusOrcamento` | DE-002 (aprovado) e DE-003 (recusado), ambos com conteúdo | Parcial. O DE-002 permite recusar a partir de "Em aberto" e traz uma nota de pendência; o DE-003 só permite recusar a partir de "Orçada". Os nomes dos estados variam. Lacuna L-05 |
| Box | `status : StatusBox` | Nenhum | Não. Lacuna L-13. O OCL descreve a regra nos invariantes INV-09, INV-10 e INV-11, e `StatusBox` (`livre` e `ocupado`) é proposta |

`HistoricoStatusOS` e `HistoricoStatusOrcamento` guardam `statusAnterior` e `statusNovo`, mas são registros de transição, não objetos com ciclo de vida. Aceito sem diagrama.

Resposta: parcial.

## 3. Lacunas e ações

A coluna Situação refere-se aos documentos de modelagem em 22/11/2026. "A conferir" indica item cujo estado no GitLab ou no Figma não foi consultado de novo.

| ID | Elo | Lacuna | Ação | Prazo | Situação |
|---|---|---|---|---|---|
| L-01 | UC | 14 histórias sem caso de uso (tabela 1.1) | Criar casos de uso para autenticação, histórico do veículo, agenda dos boxes, exclusão e alerta de peças, ou registrar a história no UC existente mais próximo | 24/11/2026 | Aberta. A US-008 e a US-017 saíram da lista |
| L-02 | UC | Título das issues com a história errada (UC-007, UC-009, UC-010, UC-014, UC-020) | Corrigir o título das issues #60, #62, #63, #67 e #73 no GitLab, para refletir os vínculos que a modelagem já usa | 20/11/2026 | A conferir; prazo vencido |
| L-03 | Sequência | UC-017 a UC-023 sem diagrama de sequência, de atividades nem de estados; RF-03 e RF-04 sem diagrama | Desenhar ao menos o cadastro de peça (UC-017), a designação de box (RF-03) e a consulta por placa (RF-04), que têm decisões no fluxo | 28/11/2026 | Aberta |
| L-04 | Sequência | DS-020 sem título no Figma em 15/11/2026; arquivo `Ds-020_UC-000-...` fora do padrão; issues DS-016 e DS-020 abertas | Dar o título `DS-020 \| UC-000 - Visualização das informações da OS` ao quadro e fechar as issues | 20/11/2026 | A conferir; prazo vencido |
| L-05 | Atividade e estado | O DE-002 permite recusar a partir de "Em aberto", o DE-003 não; o DE-002 traz nota de pendência sobre `criar`, `orçar` e `recusar`; os nomes dos estados variam ("Em aberto" e "Em aberta"); o DA-005 registra a OS como "Aprovada". Os DA-004, DA-005, DA-007 e DA-009 e o DE-002, que estavam vazios em 15/11/2026, hoje têm conteúdo | Confirmar com o grupo as transições do orçamento, alinhar o DE-002 ao DE-003 e padronizar os nomes dos estados | 28/11/2026 | Em parte: os diagramas existem, falta alinhá-los |
| L-06 | Classe | `HistoricoStatusOrcamento` só no diagrama de apoio, com atributos propostos por analogia; as tabelas de histórico podem guardar só o novo status (RF-02 e RF-06) | Incluir a classe no diagrama principal ou aceitar o diagrama de apoio; confirmar os atributos com o banco | 24/11/2026 | Em parte |
| L-07 | Classe | Dados de tela sem atributo: formato e meio da resposta do orçamento, início da execução da OS, retirada do veículo, histórico de alterações de cliente e peça, peça ativa; tabela `ordem_servico_peca` sem classe | Acrescentar os atributos e a classe, ou retirar os dados das telas | 24/11/2026 | Aberta |
| L-08 | Tela | Telas sem identificador; "US-012" no quadro de relatórios; "US-xxx - RF-xx" em distribuidoras e na versão anterior de peças | Corrigir os nomes dos quadros conforme o RF e a história; avaliar um código `T-NN` por tela | 24/11/2026 | A conferir |
| L-09 | Tela | US-002, US-008, US-009 e US-027 sem tela | Desenhar a retirada e o histórico de status da OS; decidir se US-008 e US-009 continuam válidas (D-07 de historias-de-usuario.md) | 28/11/2026 | A conferir |
| L-10 | Regra | Tela de orçamento com valor fixo, sem itens de peças e mão de obra | Alinhar a tela à RF-02 ou registrar com o cliente que o orçamento é por valor fixo | 24/11/2026 | Aberta |
| L-11 | Regra | Status "Aprovada" na lista de OS, no componente de badge e no DA-005; o DS-004 desenha `iniciarExecucao` | Trocar por `aberta` nas telas e no componente; corrigir o DA-005 e o DS-004 | 24/11/2026 | Aberta |
| L-12 | Teste | Não há código nem testes; nenhum critério de aceite é verificado automaticamente | Registrar no backlog; quando houver aplicação, um teste por cenário Gherkin | Fora do Milestone 3 | Aceita por ora |
| L-13 | Estado | `Box` tem `status` sem máquina de estados | Desenhar o DE do box (livre ↔ ocupado) ou trocar o atributo por booleano | 28/11/2026 | Aberta |
| L-14 | Ator | Telas com login de Almoxarifado e de Gerente; diagramas com Estoquista e Gerente, que não têm código de ator; premissa de um só perfil com login. O Gerente pode corresponder ao Gestor da oficina (ACT-03), o que precisa de confirmação | Decidir com o cliente os perfis de login (pergunta 8 da RFP) e alinhar telas, atores e histórias | 24/11/2026 | Aberta |
| L-15 | Nome | `Peca.tipo` × "Categoria"; `matricula` × `func_login`; demais diferenças em [divergencias-m2.md](../05-modelagem/ocl/divergencias-m2.md) | Escolher um termo para cada caso e corrigir classe, tela ou banco | 24/11/2026 | Aberta |
| L-16 | Escopo | Pagamento, sete relatórios e funções do login sem requisito | Criar requisito ou retirar do protótipo e do banco | 24/11/2026 | Aberta |
| L-17 | Requisito | Distribuidoras, edição do orçamento e visualização das informações do orçamento modeladas sem história | Criar histórias na RF-05 e na RF-02 | 24/11/2026 | Aberta |
| L-18 | Sequência, atividade e estado | Diagrama sem o conteúdo da issue: DS-004, DS-018, DS-019; DS-007 com status do orçamento diferentes dos do banco; DS-008 sem guarda de status; DA-013 a DA-016 com fluxos trocados e, do DA-014 ao DA-016, raia chamada "texto"; DE-001 vinculado ao UC-013, mas com o ciclo inteiro da OS | Redesenhar DS-004, DS-018 e DS-019; corrigir DA-013 a DA-016; incluir a guarda de status e o registro de funcionário, data e hora no DS-008 | 28/11/2026 | Aberta |
| L-19 | Padrão | Títulos de quadro e nomes de arquivo fora do padrão (DS-011, DS-012, DS-014 e DS-015 sem título; `Ds-008`, `ds-010` e `Ds-020`); participantes `:OS` e `:Orçamento` em vez dos nomes de classe; raia Sistema com nomes variados; quadros "UC-01" a "UC-06" confundem-se com o catálogo "UC-001" a "UC-023"; arquivo único do DS-012 e do DS-013 redundante | Padronizar nomes de quadro, de arquivo e de participante conforme as convenções do [índice de modelagem](../05-modelagem/README.md#convenções-comuns); renomear os quadros dos grupos para "Diagrama 1 · Clientes" e assim por diante | 28/11/2026 | Aberta |

## 4. Itens aceitos, sem ação

- Filtros modelados como `<<extend>>` de casos de uso existentes.
- Classes `Endereco` e `Funcao`, que aparecem nas telas.
- `HistoricoStatusOS` e `HistoricoStatusOrcamento` sem máquina de estados, por serem registros de transição.
- Colunas de auditoria do banco (`*_CRIADO_EM`, `*_ATUALIZADO_EM`), que ficam fora do modelo de domínio.

## Veja também

- [Matriz de rastreabilidade](matriz.md)
- [Malha de rastreabilidade (diagrama)](malha-rastreabilidade.svg)
- [Requisitos funcionais](../03-requisitos/requisitos-funcionais.md)
- [Histórias de usuário](../03-requisitos/historias-de-usuario.md)
- [Backlog](../03-requisitos/backlog.md)
- [Modelagem (índice)](../05-modelagem/README.md)
- [Casos de uso](../05-modelagem/casos-de-uso/README.md)
- [Classes](../05-modelagem/classes/README.md)
- [Sequência](../05-modelagem/sequencia/README.md)
- [Atividades](../05-modelagem/atividades/README.md)
- [Estados](../05-modelagem/estados/README.md)
- [OCL](../05-modelagem/ocl/README.md)
