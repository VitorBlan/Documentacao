# Modelagem

Milestone 2 e Milestone 3 · Fábrica de Software I · Unidade de modelagem (UML) · Visão geral dos diagramas do Bancada

Este diretório reúne os diagramas que descrevem o Bancada, o sistema de gestão da oficina mecânica, antes do código. Cada diagrama responde a uma pergunta específica. Se uma pergunta já é respondida por um texto curto, o diagrama não se justifica. Os diagramas foram desenhados no arquivo do Figma "Gestão de Oficina Mecânica" (páginas 04, 05, 06 e 08), e este diretório guarda as exportações em SVG e a descrição de cada um.

## Critério para manter um diagrama

Seguimos a linha de Martin Fowler em *UML Distilled*: a UML serve sobretudo como **esboço** para comunicar uma ideia, não como documentação completa. Daí as regras deste projeto:

1. **Um diagrama, uma pergunta.** Cada arquivo responde a uma questão (quem faz o quê, quais são os estados, em que ordem as mensagens trafegam). Diagrama que tenta responder tudo não responde nada.
2. **Só o que é difícil de ver no texto.** Uma lista de três atores não precisa de diagrama; a interação entre quatro partes com dez mensagens precisa.
3. **Notação mínima.** Usamos só os elementos que a pergunta exige. Um caso de uso não ganha atributos, e uma classe não ganha operações que nenhum caso de uso usa.
4. **Coerente com os requisitos.** O projeto ainda não tem código da aplicação. Quando um diagrama contradiz a issue do GitLab ou uma decisão do grupo registrada nos requisitos, a issue e a decisão prevalecem, e o diagrama é corrigido. As divergências encontradas ficam registradas no documento de cada diagrama, e não escondidas.
5. **Rastreável.** Todo diagrama cita os casos de uso (UC-0YY) que cobre, e os identificadores seguem a cadeia RF → US → UC → DS → DA ou DE. Assim é possível ver se algum caso de uso ficou sem modelo.

## Ordem de leitura

1. [Casos de uso](casos-de-uso/README.md): o escopo, quem são os atores e o que o sistema faz. Ponto de partida.
2. [Classes](classes/README.md): o vocabulário do domínio (Pessoa, Cliente, Veiculo, Orcamento, OrdemServico, Box, Peca e demais). Serve para ler o restante sem ambiguidade.
3. [Sequência](sequencia/README.md): a ordem das mensagens entre a Secretária ou o Estoquista e os objetos do sistema, nos casos de uso de cadastro, atualização, mudança de status e consulta.
4. [Atividades](atividades/README.md): o fluxo de decisão, em raias, de processos com desvios, como a validação do cadastro ou a criação da OS a partir do orçamento aprovado.
5. [Estados](estados/README.md): como a OS e o orçamento mudam de status, e quais transições são proibidas.
6. [OCL](ocl/README.md): as regras que não cabem no desenho, escritas como restrições formais, e as divergências de nomes.

## Diagramas

| Diagrama | Arquivos | Pergunta que responde | Quantidade |
|---|---|---|---|
| Casos de uso | [casos-de-uso/README.md](casos-de-uso/README.md) | Quem faz o quê, e qual é a fronteira do sistema | 1 diagrama geral e 6 diagramas por assunto |
| Classes | [classes.svg](classes/classes.svg) e [classes-apoio.svg](classes/classes-apoio.svg) | Quais são os conceitos do domínio e como se relacionam | 1 principal (19 classes) e 1 de apoio |
| Sequência | [sequencia/README.md](sequencia/README.md) | Em que ordem as partes trocam mensagens em um caso de uso | 20 (DS-001 a DS-020) |
| Atividades | [atividades/README.md](atividades/README.md) | Quais decisões e desvios existem em um processo | 16 (DA-001 a DA-016) |
| Estados | [estados/README.md](estados/README.md) | Quais estados a OS e o orçamento assumem e quais transições são permitidas | 3 (DE-001 a DE-003) |
| OCL | [ocl/README.md](ocl/README.md) e [ocl/divergencias-m2.md](ocl/divergencias-m2.md) | Quais regras são restrições formais e onde os nomes divergem | Texto, sem desenho |

## Mapa de casos de uso para diagramas

Esta tabela é a referência para o levantamento de cobertura: um caso de uso sem nenhum diagrama aparece com "—" nas três colunas. Os nomes dos casos de uso são os do catálogo ([casos-de-uso/README.md](casos-de-uso/README.md#catálogo)). Todos os casos de uso estão no diagrama de casos de uso e na especificação.

| UC | Caso de uso | Sequência | Atividades | Estados |
|---|---|---|---|---|
| UC-001 | Cadastro de cliente | DS-001 | DA-001 | — |
| UC-002 | Atualizar informações do cliente | DS-005 | DA-002 | — |
| UC-003 | Visualizar lista de clientes cadastrados | DS-009 | DA-008 | — |
| UC-004 | Cadastro de um veículo | DS-002 | DA-003 | — |
| UC-005 | Atualizar informações do veículo | DS-006 | DA-006 | — |
| UC-006 | Cadastrar orçamento | DS-003 | DA-004 | — |
| UC-007 | Visualizar lista de orçamentos feitos | DS-016 | DA-013 | — |
| UC-008 | Visualizar lista de veículos cadastrados | DS-014 | DA-011 | — |
| UC-009 | Mudar o status do orçamento para recusado | DS-013 | — | DE-003 |
| UC-010 | Mudar o status do orçamento para aprovado | DS-012 | — | DE-002 |
| UC-011 | Visualizar perfil do cliente | DS-018 | DA-015 | — |
| UC-012 | Visualizar informações do veículo | DS-015 | DA-012 | — |
| UC-013 | Mudar status de uma OS para concluída | DS-008 | — | DE-001 |
| UC-014 | Criar OS | DS-004 | DA-005 | — |
| UC-015 | Cadastrar distribuidora | DS-010 | DA-009 | — |
| UC-016 | Atualizar informações da distribuidora | DS-011 | DA-010 | — |
| UC-017 a UC-023 | Peças, lista e informações de distribuidora e lista de relatórios | — | — | — |

O diagrama de classes cobre todos os casos de uso, porque as classes são o vocabulário comum. Quatro diagramas de sequência e três de atividades não têm caso de uso no catálogo (`UC-000`): DS-007, DS-017, DS-019 e DS-020, e DA-007, DA-014 e DA-016. Os UC-017 a UC-023 são tratados só no texto de especificação, e a falta de diagrama de sequência está registrada como lacuna L-03 em [lacunas.md](../07-rastreabilidade/lacunas.md). Os casos de uso de consulta e de listagem têm diagramas curtos, porque o fluxo é só a navegação.

## Convenções comuns

- **Identificadores.** UC-0YY (caso de uso), DS-0XX (sequência), DA-0XX (atividades), DE-0XX (estados). Os títulos dos quadros do Figma e das issues do GitLab seguem os padrões `DS-0XX | UC-0YY - <nome>`, `DA-0XX | DS-0YY` e `DE-0XX | DS-0YY`. `UC-000` indica diagrama sem caso de uso no catálogo, e `US-000`, caso de uso sem história.
- **Nomes de classes.** Em português, no singular e sem acento, como na [lista oficial de classes](classes/README.md#lista-oficial-de-classes). Sequência, estados e OCL usam esses nomes.
- **Status.** O orçamento passa por `aberto`, `orçado`, `aprovado` e `recusado`. A OS passa por `aberta`, `em execução` e `concluída`, e só nasce de um orçamento aprovado. Os literais são os do [glossário](../01-visao/glossario.md).
- **Autenticação.** Todo caso de uso inclui "Autenticar" (`<<include>>`). Os diagramas partem do funcionário já autenticado.

## Divergências

1. **Dois conjuntos de numeração de casos de uso.** Os diagramas por assunto do Figma são titulados "UC-01" a "UC-06", e o catálogo usa UC-001 a UC-023. Ver a divergência 2 em [casos de uso](casos-de-uso/README.md#divergências).
2. **Diagramas incompletos ou fora do padrão.** O DS-004, o DS-018 e o DS-019 têm conteúdo trocado ou incompleto; DS-012 e DS-013 existem em duas versões; vários arquivos têm nome fora do padrão. O detalhe está nas divergências de [sequência](sequencia/README.md#divergências) e de [estados](estados/README.md#divergências).
3. **Cobertura parcial.** Não há diagramas de sequência, de atividades ou de estados para peças, distribuidoras (lista e informações) e relatórios (UC-017 a UC-023), nem para o `Box`, que tem `status` sem máquina de estados (lacuna L-13).
4. **Classe de histórico do orçamento só no apoio.** `HistoricoStatusOrcamento` está em `classes-apoio.svg`, e não no diagrama principal (lacuna L-06).

## Veja também

- [Casos de uso](casos-de-uso/README.md)
- [Classes](classes/README.md)
- [Requisitos funcionais](../03-requisitos/requisitos-funcionais.md)
- [Histórias de usuário](../03-requisitos/historias-de-usuario.md)
- [Matriz de rastreabilidade](../07-rastreabilidade/matriz.md)
- [Lacunas](../07-rastreabilidade/lacunas.md)
- [Glossário](../01-visao/glossario.md)
- [Processo: fluxo as-is](../02-processo/bpmn/README.md)
