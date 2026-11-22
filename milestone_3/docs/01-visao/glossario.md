# Glossário

Fábrica de Software I · Milestone 1 · termos do domínio e do projeto, em ordem alfabética

Os termos seguem o domínio do Bancada. A coluna "No banco" aponta para a tabela, a coluna ou a restrição do script do banco ([schema.sql](../06-banco-de-dados/schema.sql)). Valores de status aparecem como estão gravados no banco.

| Termo | Significado | No banco |
|---|---|---|
| Bancada | Nome do sistema de gestão da oficina mecânica | Banco `oficina_carro` |
| Box | Espaço da oficina onde um veículo é atendido. Recebe um veículo por vez | Tabela `box` |
| Box ativo | Box ocupado por uma OS com status `em execução`. O banco impede duas OS em execução no mesmo box | Coluna gerada `os_box_ativo`, chave única `uk_os_box_ativo` |
| Categoria da peça | Agrupamento usado para filtrar as peças no estoque | Coluna `peca_categoria` |
| Cliente | Dono do veículo atendido pela oficina. Aprova ou recusa o orçamento | Tabela `pessoa`, ligada ao veículo por `veiculo.pess_id` |
| Contato preferencial | Meio pelo qual o cliente prefere ser procurado | Coluna `pess_ct_preferencial` |
| Distribuidora | Fornecedor das peças do estoque | Tabelas `distribuidora` e `peca_distribuidora` |
| Estoque mínimo | Quantidade a partir da qual o sistema avisa que a peça está acabando. É definido pela oficina | Coluna `peca_estoque_minimo` |
| Função | Cargo do funcionário, como secretária ou mecânico | Tabela `funcao` |
| Funcionário | Pessoa que trabalha na oficina. Tem login apenas se tiver acesso ao sistema | Tabelas `pessoa` e `funcionario`, restrição `ck_func_acesso` |
| Gestor da oficina (ACT-03) | Responsável que acompanha o status das OS e o uso dos boxes. Sem login no Milestone 1 | Não há perfil próprio |
| Histórico do orçamento | Registro de cada mudança de status do orçamento, com o funcionário e a data/hora | Tabela `orcamento_historico` |
| Histórico da OS | Registro de cada mudança de status da OS, com o funcionário e a data/hora | Tabela `ordem_servico_historico` |
| Histórico do veículo | Conjunto das OS de um veículo, consultado pela placa (RF-04) | Consulta sobre `ordem_servico`, `orcamento` e `veiculo` |
| Issue | Item de trabalho do GitLab. Cada requisito, história, caso de uso e diagrama é uma issue | Não se aplica |
| Mão de obra | Serviço cobrado no orçamento, separado das peças | Tabelas `servico` e `orcamento_servico` |
| Marca | Fabricante do veículo | Tabela `carro_marca` |
| Mecânico (ACT-02) | Funcionário que analisa o veículo, monta o orçamento e executa a OS no box designado. Sem login no Milestone 1 | Coluna `meca_id` em `ordem_servico` |
| Milestone | Etapa de entrega da disciplina. O projeto tem três: 15/09, 20/10 e 08/12/2026 | Não se aplica |
| Modelo | Modelo do veículo, ligado a uma marca. Também indica com quais modelos uma peça é compatível | Tabelas `carro_modelo` e `peca_modelo` |
| Monitoria | Revisão dos artefatos com o professor, uma semana antes de cada apresentação | Não se aplica |
| Orçamento | Proposta de serviço para um veículo, com peças e mão de obra. Passa por `aberto` e `orçado` e termina em `aprovado` ou `recusado` | Tabela `orcamento`, restrição `ck_orca_status` |
| Ordem de serviço (OS) | Serviço autorizado, criado a partir de um orçamento aprovado. Passa por `aberta`, `em execução` e `concluída` | Tabela `ordem_servico`, restrição `ck_os_status`, gatilho `tg_os_bi_orcamento_aprovado` |
| Pagamento | Registro do pagamento de uma OS, à vista ou parcelado. Ainda sem requisito | Tabelas `pagamento` e `pagamento_parcelas` |
| Peça | Item do estoque usado nos serviços, com código, preço e quantidade | Tabela `peca` |
| Placa | Identificação do veículo, usada como referência no histórico. É única | Coluna `veic_placa`, chave única `uk_veiculo_placa` |
| Quilometragem | Quilometragem do veículo registrada no orçamento | Coluna `orca_quilometragem` |
| Retirada | Momento em que o cliente busca o veículo na oficina, registrado pela secretária (US-002) | Não há tabela própria |
| Secretária (ACT-01) | Funcionária do balcão com cadastro especial. Único perfil com login no Milestone 1 | Tabela `funcionario` com `func_login` |
| Status | Situação atual de um orçamento ou de uma OS. Cada mudança grava autor e data/hora | Colunas `orca_status` e `os_status` |
| Veículo | Carro de um cliente atendido pela oficina | Tabela `veiculo` |

## Veja também

- [Visão do produto](README.md)
- [PM Canvas](pm-canvas.md)
- [RFP do cliente](rfp.md)
- [Dicionário de dados](../06-banco-de-dados/dicionario-de-dados.md)
- [Classes](../05-modelagem/classes/classes.svg)
