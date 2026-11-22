# Visão do produto

Fábrica de Software I · Milestone 1 · visão, problema, objetivos e partes interessadas

Esta pasta parte da RFP 2 da disciplina (Gestão de Oficinas Mecânicas, cliente Antônio Júnior) e chega à visão do Bancada. Ela define o problema, os objetivos, o escopo e quem participa. Não traz requisitos: esses estão registrados como issues no GitLab do projeto e são detalhados em [03-requisitos](../03-requisitos/README.md).

## Problema

Uma oficina faz todo o controla clientes, veículos, orçamentos e ordens de serviço em papel e em planilhas soltas. Não há registro confiável de quem aprovou um orçamento, nem de quando e por qual meio a aprovação chegou. Consultar o histórico de um veículo que já passou pela oficina é difícil. Também falta controle sobre a ocupação dos boxes, a ociosidade dos mecânicos e o estoque de peças.

## Visão do produto

O Bancada é um sistema web de gestão de oficina mecânica de carros. A secretária autenticada recebe o cliente no balcão, verifica se ele veio trazer ou buscar um veículo e mantém o cadastro de clientes e veículos. O mecânico analisa o veículo e monta o orçamento; somente depois da aprovação do dono do veículo o orçamento vira uma ordem de serviço (OS), designada a um box e a um mecânico. Cada mudança de status do orçamento e da OS fica registrada com data, hora e autor, o que forma o histórico do veículo. O estoque de peças avisa quando uma peça atinge o nível mínimo definido pela oficina.

## Objetivos

Os objetivos são qualitativos. As metas numéricas (tempo de resposta, volume de atendimentos, horário de disponibilidade) ainda estão na lista de perguntas da [RFP](rfp.md), seção 12.

| Código | Objetivo                                                           | Requisitos    | Casos de uso                                           |
| ------ | ------------------------------------------------------------------ | ------------- | ------------------------------------------------------ |
| OBJ-01 | Controlar o acesso da secretária ao sistema                        | RF-01, RNF-02 | Sem UC no catálogo (ver [Divergências](#divergências)) |
| OBJ-02 | Atender no balcão e manter o cadastro de clientes e veículos       | RF-01         | UC-001 a UC-005, UC-008, UC-011, UC-012                |
| OBJ-03 | Orçar o serviço e registrar a decisão do cliente antes da execução | RF-02         | UC-006, UC-007, UC-009, UC-010                         |
| OBJ-04 | Acompanhar a OS e o histórico de status                            | RF-06         | UC-013, UC-014                                         |
| OBJ-05 | Distribuir o trabalho entre boxes e mecânicos                      | RF-03         | Sem UC no catálogo                                     |
| OBJ-06 | Controlar o estoque de peças e seus fornecedores                   | RF-05         | UC-015 a UC-022                                        |
| OBJ-07 | Manter o histórico do veículo, consultado pela placa               | RF-04         | Sem UC no catálogo                                     |
| OBJ-08 | Emitir o relatório da ordem de serviço                             | RF-07         | UC-023                                                 |

## Escopo

**Dentro do escopo:** os requisitos RF-01 a RF-07 e RNF-01 a RNF-05 registrados no GitLab, o catálogo de casos de uso (UC-001 a UC-023) e os objetivos acima.

**Fora do escopo:** nenhuma exclusão foi formalizada com o cliente até esta versão. O banco do Milestone 2 ([06-banco-de-dados](../06-banco-de-dados/README.md)) traz as tabelas `pagamento` e `pagamento_parcelas`, que não correspondem a nenhum requisito; a decisão sobre elas está na [RFP](rfp.md), seção 12.

## Partes interessadas

| Quem                      | Código | Papel no sistema                                        | Interesse principal                                                          |
| ------------------------- | ------ | ------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Secretária                | ACT-01 | Único perfil com login no Milestone 1                   | Atender no balcão e registrar orçamentos, OS e estoque sem papel             |
| Mecânico                  | ACT-02 | Aparece nas histórias (US-20); sem login no Milestone 1 | Saber qual OS e qual box estão designados a ele                              |
| Gestor da oficina         | ACT-03 | Aparece nas histórias (US-27); sem login no Milestone 1 | Acompanhar o status das OS e o uso dos boxes                                 |
| Antônio Júnior            |        |                                                         | Cliente do projeto; define as regras de negócio e valida as entregas         |
| Clientes da oficina       |        |                                                         | Donos dos veículos; aprovam ou recusam o orçamento                           |
| Equipe de desenvolvimento |        |                                                         | Hugo Brandão, Vitor Blan, Cauã Rodrigo e João Andrade; produzem os artefatos |
| Professor avaliador       |        |                                                         | Valida os artefatos a cada milestone                                         |

## Casos de uso do catálogo

Os títulos seguem as issues do GitLab, que são a referência de rastreabilidade do grupo (padrão `UC-0YY - US-0XX - <nome>`). No Milestone 1, todos os casos de uso são realizados pela secretária (ACT-01).

| Código | Caso de uso                               | História de origem | Ator principal |
| ------ | ----------------------------------------- | ------------------ | -------------- |
| UC-001 | Cadastro de cliente                       | US-003             | ACT-01         |
| UC-002 | Atualizar informações do cliente          | US-004             | ACT-01         |
| UC-003 | Visualizar lista de clientes cadastrados  | US-010             | ACT-01         |
| UC-004 | Cadastro um veículo                       | US-006             | ACT-01         |
| UC-005 | Atualização informações do veículo        | US-007             | ACT-01         |
| UC-006 | Cadastrar orçamento                       | US-016             | ACT-01         |
| UC-007 | Visualizar lista de orçamentos feitos     | US-000             | ACT-01         |
| UC-008 | Visualizar lista de veículos cadastrados  | US-011             | ACT-01         |
| UC-009 | Mudar o status do orçamento para recusado | US-028             | ACT-01         |
| UC-010 | Mudar o status do orçamento para aprovado | US-028             | ACT-01         |
| UC-011 | Visualizar perfil do cliente              | US-013             | ACT-01         |
| UC-012 | Visualizar informações do veículo         | US-014             | ACT-01         |
| UC-013 | Mudar status de uma OS para concluída     | US-002             | ACT-01         |
| UC-014 | Criar OS                                  | US-028             | ACT-01         |
| UC-015 | Cadastrar distribuidora                   | US-000             | ACT-01         |
| UC-016 | Atualizar informações da distribuidora    | US-000             | ACT-01         |
| UC-017 | Cadastrar peça                            | US-019             | ACT-01         |
| UC-018 | Visualizar lista de distribuidoras        | US-000             | ACT-01         |
| UC-019 | Visualizar informações da distribuidora   | US-000             | ACT-01         |
| UC-020 | Visualizar lista de peças                 | US-000             | ACT-01         |
| UC-021 | Visualizar informações da peça            | US-020             | ACT-01         |
| UC-022 | Atualizar informações da peça             | US-021             | ACT-01         |
| UC-023 | Visualizar lista de relatórios            | US-000             | ACT-01         |

`US-000` indica caso de uso ainda sem história vinculada.

## Documentos desta pasta

- [PM Canvas](pm-canvas.md), com o desenho em grade em [pm-canvas.svg](pm-canvas.svg).
- [RFP do cliente](rfp.md), ponto de partida, sem requisitos prontos.
- [Glossário](glossario.md), termos do domínio em ordem alfabética.

## Divergências

Diferenças entre os requisitos registrados no GitLab e o banco do Milestone 2 ([06-banco-de-dados](../06-banco-de-dados/README.md)). A frente responsável deve decidir o ajuste.

1. **Ciclo de status da OS.** A issue RF-06 descreve o ciclo `Aprovada → em execução → concluída`. O banco (`ck_os_status`) usa `aberta`, `em execução` e `concluída`, e a regra decidida pelo grupo é que `aprovado` pertence ao orçamento, não à OS. O texto da RF-06 precisa ser atualizado.
2. **Meio da resposta do cliente.** A RF-02 exige registrar o meio pelo qual a aprovação chegou (presencial, telefone ou mensagem). O banco não tem coluna com esse nome; a coluna `orca_formato` pode cumprir esse papel, mas não tem restrição de valores. Confirmar com o responsável pelo banco.
3. **Distribuidora e pagamento sem requisito.** O banco tem `distribuidora`, `peca_distribuidora`, `pagamento` e `pagamento_parcelas`. A RF-05 cita apenas "fornecedor" da peça, e os casos de uso de distribuidora (UC-015, UC-016, UC-018 e UC-019) apontam para `US-000`. Pagamento não aparece em nenhum requisito.
4. **Autenticação sem caso de uso.** A RF-01 e a US-001 tratam da autenticação da secretária, mas o catálogo não tem UC de login. A RNF-02 fala em "matrícula e senha", e o banco guarda o acesso em `func_login` e `func_senha`.
5. **Atores sem login.** Mecânico (US-20) e gestor da oficina (US-27) aparecem nas histórias, mas a premissa do Milestone 1 é que só a secretária acessa o sistema. Os códigos ACT-02 e ACT-03 identificam partes interessadas, não perfis de acesso.
6. **Numeração das histórias.** Há histórias fora do padrão `US-0XX` (US-26, US-27, US-28, "US -20" e "US - 030"). A "US -20" (agenda de boxes) usa o mesmo número da US-020 (detalhes da peça), e a US-018 e a US-27 têm o mesmo título.

## Veja também

- [PM Canvas](pm-canvas.md)
- [RFP do cliente](rfp.md)
- [Glossário](glossario.md)
- [Processo](../02-processo/README.md)
- [Requisitos](../03-requisitos/README.md)
- [Modelagem](../05-modelagem/README.md)
