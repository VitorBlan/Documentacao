# Requisitos não funcionais

Milestone 1 · Fábrica de Software I · Unidade de requisitos

Os requisitos não funcionais do Bancada são as issues RNF-01 a RNF-05 do GitLab, todas com `prio::P1` (Must) no Milestone 1. Para cada RNF, este documento traz o texto da issue, uma métrica verificável e a evidência no banco do Milestone 2 ([06-banco-de-dados](../06-banco-de-dados/README.md)) ou no protótipo. Os requisitos funcionais afetados estão em [requisitos-funcionais.md](requisitos-funcionais.md).

## Como ler

As issues descrevem os RNF em linguagem qualitativa. As métricas marcadas como "proposta" foram sugeridas pela equipe para tornar o requisito verificável e dependem das respostas do cliente à seção 12 da [RFP](../01-visao/rfp.md#12-perguntas-em-aberto-ao-cliente).

| Estado | Significado |
|---|---|
| Atendido | Há evidência no repositório de que o requisito é cumprido |
| Parcial | O banco ou o protótipo cobrem parte do requisito |
| Lacuna | Nada no repositório cumpre o requisito |
| Não medido | A meta depende da aplicação em funcionamento, que ainda não existe |

## Resumo

| ID | Tipo | Issue | Tamanho | Estado |
|---|---|---|---|---|
| RNF-01 | Usabilidade | [#45](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/45) | M | Não medido |
| RNF-02 | Segurança | [#46](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/46) | M | Parcial |
| RNF-03 | Desempenho | [#47](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/47) | M | Não medido |
| RNF-04 | Disponibilidade | [#48](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/48) | S | Não medido |
| RNF-05 | Integridade dos dados | [#49](https://gitlab.fslab.dev/fabrica-de-software-i-2026/projeto2/-/issues/49) | M | Parcial |

## RNF-01 Usabilidade

**Texto da issue.** A interface deve ser simples e intuitiva, permitindo que a secretária opere o sistema sem necessidade de treinamento técnico extenso.

| Campo | Conteúdo |
|---|---|
| Métrica (proposta) | Em teste com o protótipo navegável, a secretária conclui sem ajuda as tarefas de balcão (identificar o cliente, abrir o orçamento e registrar a decisão) em pelo menos 4 de cada 5 tentativas |
| Evidência | Protótipo no Figma com menu lateral fixo em todas as telas (Atendimento, Cadastros, Gestão), estados vazios e mensagens de erro desenhados (por exemplo, "Placa não encontrada" na US-029 e orientação inicial na US-030) |
| Requisitos afetados | RF-01, RF-02, RF-04, RF-05 |
| Estado | Não medido: não houve teste de usabilidade com usuários |

## RNF-02 Segurança

**Texto da issue.** O acesso ao sistema deve exigir autenticação (matrícula e senha), e toda ação relevante (aprovação de orçamento, mudança de status de OS) deve registrar o usuário e a data/hora responsáveis.

| Campo | Conteúdo |
|---|---|
| Métrica | Nenhuma tela além do login abre sem sessão válida; 100% das mudanças de status de orçamento e de OS geram um registro de histórico com funcionário e data/hora |
| Evidência | `funcionario.func_login` e `func_senha`, com `uk_funcionario_login` e `ck_func_acesso` (login e senha existem juntos); `orcamento_historico` e `ordem_servico_historico` com `func_id` e data de criação |
| Requisitos afetados | RF-01, RF-02, RF-06 |
| Estado | Parcial: o banco guarda credencial e autoria; não há aplicação que exija a sessão, e nada garante que a senha seja gravada como hash |

## RNF-03 Desempenho

**Texto da issue.** As operações de consulta (histórico do veículo, estoque, relatórios) devem responder dentro de um tempo aceitável definido pela equipe, mesmo com o crescimento do volume de dados da oficina.

| Campo | Conteúdo |
|---|---|
| Métrica (proposta) | Consultas de histórico por placa, de estoque e de relatório respondem em até 2 segundos em 95% das requisições, com a base povoada com o volume de cinco anos de atendimentos |
| Evidência | Índices nas colunas de consulta: `uk_veiculo_placa` (histórico por placa), `ix_orcamento_01` (status do orçamento), `ix_os_oshi` e `ix_orcamento_orhi` (históricos), `ix_box_os` e `ix_mecanico_os` |
| Requisitos afetados | RF-04, RF-05, RF-07 |
| Estado | Não medido: o tempo aceitável depende da pergunta 1 da RFP |

## RNF-04 Disponibilidade

**Texto da issue.** O sistema deve estar disponível durante o horário de funcionamento da oficina, com o mínimo possível de indisponibilidade.

| Campo | Conteúdo |
|---|---|
| Métrica (proposta) | Disponível em todo o horário de funcionamento da oficina; manutenções programadas só fora desse horário |
| Evidência | Nenhuma no repositório: não há ambiente de implantação definido |
| Requisitos afetados | Todos |
| Estado | Não medido: o horário de funcionamento depende da pergunta 2 da RFP |

## RNF-05 Integridade dos dados

**Texto da issue.** O sistema deve garantir a consistência dos dados de estoque, orçamentos e ordens de serviço, evitando duplicidade ou perda de informações em caso de falhas.

| Campo | Conteúdo |
|---|---|
| Métrica | Nenhum CPF, placa ou código de peça duplicado; estoque nunca negativo; nenhuma OS sem orçamento aprovado; orçamento e seus itens gravados na mesma transação |
| Evidência | Chaves únicas (`uk_pessoa_cpf`, `uk_veiculo_placa`, `uk_peca_codigo`, `uk_os_orcamento`), `ck_peca_estoque`, `ck_orca_status`, `ck_os_status`, gatilho `tg_os_bi_orcamento_aprovado`, chaves estrangeiras e tabelas InnoDB (transacionais) |
| Requisitos afetados | RF-01, RF-02, RF-05, RF-06 |
| Estado | Parcial: o banco impede duplicidade e estados inválidos; o uso de transação na gravação e a cópia de segurança dependem da aplicação e do ambiente |

## Lacunas

1. **Metas sem número.** RNF-01, RNF-03 e RNF-04 não têm valor verificável no GitLab. As métricas acima são propostas da equipe e precisam ser confirmadas com o cliente.
2. **Matrícula ou login.** A RNF-02 fala em "matrícula e senha"; o banco usa `func_login`. A equipe deve escolher um termo e atualizar a issue ou o glossário.
3. **Cópia de segurança e retenção.** Nenhum RNF trata de backup nem do tempo de guarda do histórico (pergunta 9 da RFP), embora a RNF-05 cite perda de dados em caso de falha.
4. **Classificação das issues.** As cinco issues de RNF usam `type::Feature`. O texto da RNF-01 termina com um caractere solto ("extenso.eu").

## Veja também

- [Requisitos funcionais](requisitos-funcionais.md)
- [Histórias de usuário](historias-de-usuario.md)
- [Backlog](backlog.md)
- [RFP do cliente](../01-visao/rfp.md)
- [Modelo de dados](../06-banco-de-dados/README.md)
- [Template de requisito](../templates/requisito.md)
