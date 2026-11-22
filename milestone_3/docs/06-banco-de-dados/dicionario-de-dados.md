# Dicionário de dados

Contexto: Fábrica de Software I · projeto Bancada (Gestão de Oficina Mecânica) · modelagem relacional (MySQL 8, InnoDB).

Fonte: [schema.sql](schema.sql), na versão ajustada do DDL do Milestone 2, que acrescenta restrições `CHECK`, a coluna `orca_meio_resposta` e gatilhos de status. Foi conferido coluna a coluna com o banco `oficina_carro` criado a partir dele em um MySQL 8.0.46. Para as decisões de modelagem e a lista dos ajustes, veja o [README](README.md).

O banco tem 21 tabelas, 155 colunas, 27 chaves estrangeiras, 17 restrições `CHECK`, 16 chaves únicas além das primárias, 24 índices comuns, 1 coluna gerada e 5 gatilhos.

**Legenda**

- **Tipo**: como declarado no DDL. `BOOL` é sinônimo de `TINYINT(1)` no MySQL.
- **Nulo**: se a coluna aceita `NULL`.
- **Default**: valor de `DEFAULT` no banco. `—` = sem default. `AUTO_INCREMENT` = valor gerado pelo banco. `GERADA` = coluna gerada (`GENERATED ALWAYS AS ... STORED`). Nas colunas `*_ATUALIZADO_EM`, o `DEFAULT` é `CURRENT_TIMESTAMP` e a coluna tem `ON UPDATE CURRENT_TIMESTAMP`.
- **Chave**: `PK` (chave primária), `UK` (único, com a lista de colunas quando composto), `FK → tabela · RESTRICT` (chave estrangeira). O DDL não declara `ON DELETE` nem `ON UPDATE`; vale o padrão do InnoDB, `NO ACTION`, que se comporta como `RESTRICT`.
- **Origem**: classe e atributo do [modelo de classes](../05-modelagem/classes/README.md) de onde a coluna vem. `associação` indica coluna de chave estrangeira, e `embutido` indica coluna do endereço, que na classe `Endereco` é separado. `Auditoria do banco` indica coluna sem atributo na classe. Siglas `L-xx` e `N-xx` remetem às [lacunas](../07-rastreabilidade/lacunas.md) e às [divergências de nomes](../05-modelagem/ocl/divergencias-m2.md).

## Pessoas e acesso

### pessoa

Tabela de `Pessoa` e, por extensão, de `Cliente`: o cliente é a pessoa ligada a um veículo por `veiculo.pess_id`. O endereço da classe `Endereco` está embutido nas colunas `pess_cep` e `pess_en_*`; não há colunas de cidade e UF. A classe `Cliente` tem também `dataCadastro` e `ativo`, que não têm coluna: a data de cadastro pode corresponder a `PESS_CRIADO_EM`, o que precisa de confirmação.

| Coluna               | Tipo         | Nulo | Default           | Chave | Descrição                                                                        | Origem                                      |
| -------------------- | ------------ | ---- | ----------------- | ----- | -------------------------------------------------------------------------------- | ------------------------------------------- |
| pess_id              | INT          | Não  | AUTO_INCREMENT    | PK    | Identificador da pessoa                                                          | — (chave do modelo lógico)                  |
| pess_nome            | VARCHAR(50)  | Não  | —                 |       | Nome da pessoa                                                                   | Pessoa.nome                                 |
| pess_cpf             | VARCHAR(11)  | Não  | —                 | UK    | CPF, com 11 posições e sem máscara; único                                        | Pessoa.cpf                                  |
| pess_dt_nascimento   | DATE         | Não  | —                 |       | Data de nascimento                                                               | Pessoa.dataNascimento                       |
| pess_telefone        | VARCHAR(11)  | Não  | —                 |       | Telefone com DDD, até 11 posições; sem restrição de unicidade                    | Pessoa.telefone                             |
| pess_email           | VARCHAR(100) | Não  | —                 | UK    | E-mail; único                                                                    | Pessoa.email                                |
| pess_ct_preferencial | VARCHAR(100) | Não  | —                 |       | Contato preferencial: meio pelo qual a pessoa prefere ser procurada; texto livre | Pessoa.contatoPreferencial                  |
| pess_cep             | VARCHAR(8)   | Sim  | —                 |       | CEP do endereço, com 8 posições; opcional                                        | Endereco.cep (embutido)                     |
| pess_en_logradouro   | VARCHAR(45)  | Não  | —                 |       | Logradouro do endereço                                                           | Endereco.logradouro (embutido)              |
| pess_en_numero       | VARCHAR(6)   | Não  | —                 |       | Número do endereço, em texto de até 6 posições                                   | Endereco.numero (embutido)                  |
| pess_en_bairro       | VARCHAR(45)  | Não  | —                 |       | Bairro do endereço                                                               | Endereco.bairro (embutido)                  |
| pess_en_complemento  | VARCHAR(100) | Sim  | —                 |       | Complemento do endereço; opcional                                                | Endereco.complemento (embutido)             |
| pess_observacoes     | VARCHAR(500) | Sim  | —                 |       | Observações sobre a pessoa; opcional                                             | Pessoa.observacoes                          |
| PESS_CRIADO_EM       | DATETIME     | Não  | CURRENT_TIMESTAMP |       | Data e hora de criação do registro                                               | Auditoria do banco (sem atributo na classe) |
| PESS_ATUALIZADO_EM   | DATETIME     | Não  | CURRENT_TIMESTAMP |       | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`              | Auditoria do banco (sem atributo na classe) |

Índices: `uk_pessoa_cpf` UK (pess_cpf); `uk_pessoa_email` UK (pess_email).

### funcao

Tabela de `Funcao`: cargo do funcionário, como secretária ou mecânico.

| Coluna             | Tipo        | Nulo | Default           | Chave | Descrição                                                           | Origem                                      |
| ------------------ | ----------- | ---- | ----------------- | ----- | ------------------------------------------------------------------- | ------------------------------------------- |
| funo_id            | INT         | Não  | AUTO_INCREMENT    | PK    | Identificador da função                                             | — (chave do modelo lógico)                  |
| funo_funcao        | VARCHAR(30) | Não  | —                 | UK    | Nome da função; único                                               | Funcao.nome                                 |
| FUNO_CRIADO_EM     | DATETIME    | Não  | CURRENT_TIMESTAMP |       | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| FUNO_ATUALIZADO_EM | DATETIME    | Não  | CURRENT_TIMESTAMP |       | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: `uk_funcao_funcao` UK (funo_funcao).

### funcionario

Tabela de `Funcionario`, especialização de `Pessoa`: a chave primária é a própria `pess_id` (relação 1:1 com `pessoa`). Login e senha são opcionais, mas só existem juntos (`ck_func_acesso`). A classe tem também `ativo`, que não tem coluna.

| Coluna                   | Tipo         | Nulo | Default           | Chave                      | Descrição                                                                                            | Origem                                       |
| ------------------------ | ------------ | ---- | ----------------- | -------------------------- | ---------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| pess_id                  | INT          | Não  | —                 | PK; FK → pessoa · RESTRICT | Pessoa que é funcionária; é chave primária e estrangeira ao mesmo tempo                              | Funcionario (herda Pessoa)                   |
| funo_id                  | INT          | Não  | —                 | FK → funcao · RESTRICT     | Função exercida pelo funcionário                                                                     | Funcionario.funcao (associação `exerce`)     |
| func_login               | VARCHAR(30)  | Sim  | —                 | UK                         | Login de acesso ao sistema; único. Nulo quando o funcionário não tem acesso                          | Funcionario.matricula (nome diferente; L-15) |
| func_senha               | VARCHAR(100) | Sim  | —                 |                            | Senha de acesso, até 100 posições; nula junto com o login. O banco não verifica se o valor é um hash | Funcionario.senha                            |
| func_hr_trabalho_entrada | TIME         | Sim  | —                 |                            | Horário de entrada do expediente; opcional                                                           | Funcionario.horaEntrada                      |
| func_hr_trabalho_saida   | TIME         | Sim  | —                 |                            | Horário de saída do expediente; opcional                                                             | Funcionario.horaSaida                        |
| FUNC_CRIADO_EM           | DATETIME     | Não  | CURRENT_TIMESTAMP |                            | Data e hora de criação do registro                                                                   | Auditoria do banco (sem atributo na classe)  |
| FUNC_ATUALIZADO_EM       | DATETIME     | Não  | CURRENT_TIMESTAMP |                            | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`                                  | Auditoria do banco (sem atributo na classe)  |

Índices: `ix_funcao_funcionario` (funo_id); `uk_funcionario_login` UK (func_login).

Restrição `ck_func_acesso`: `(func_login IS NULL) = (func_senha IS NULL)`.

## Veículos

### carro_marca

Tabela de `CarroMarca`: marca do veículo.

| Coluna             | Tipo        | Nulo | Default           | Chave | Descrição                                                           | Origem                                      |
| ------------------ | ----------- | ---- | ----------------- | ----- | ------------------------------------------------------------------- | ------------------------------------------- |
| cama_id            | INT         | Não  | AUTO_INCREMENT    | PK    | Identificador da marca                                              | — (chave do modelo lógico)                  |
| cama_marca         | VARCHAR(30) | Não  | —                 | UK    | Nome da marca; único                                                | CarroMarca.nome                             |
| CAMA_CRIADO_EM     | DATETIME    | Não  | CURRENT_TIMESTAMP |       | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| CAMA_ATUALIZADO_EM | DATETIME    | Não  | CURRENT_TIMESTAMP |       | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: `uk_cama_marca` UK (cama_marca).

### carro_modelo

Tabela de `CarroModelo`. Cada modelo pertence a uma marca, e o par marca e modelo é único. A classe tem `anoInicio` e `anoFim`, que não têm coluna.

| Coluna             | Tipo         | Nulo | Default           | Chave                                                  | Descrição                                                           | Origem                                      |
| ------------------ | ------------ | ---- | ----------------- | ------------------------------------------------------ | ------------------------------------------------------------------- | ------------------------------------------- |
| camo_id            | INT          | Não  | AUTO_INCREMENT    | PK                                                     | Identificador do modelo                                             | — (chave do modelo lógico)                  |
| camo_modelo        | VARCHAR(100) | Não  | —                 | UK (cama_id, camo_modelo)                              | Nome do modelo; único dentro da marca                               | CarroModelo.nome                            |
| cama_id            | INT          | Não  | —                 | UK (cama_id, camo_modelo); FK → carro_marca · RESTRICT | Marca do modelo                                                     | CarroModelo.marca (associação `tem`)        |
| CAMO_CRIADO_EM     | DATETIME     | Não  | CURRENT_TIMESTAMP |                                                        | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| CAMO_ATUALIZADO_EM | DATETIME     | Não  | CURRENT_TIMESTAMP |                                                        | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: `uk_camo_marca_modelo` UK (cama_id, camo_modelo).

### veiculo

Tabela de `Veiculo`. A quilometragem da classe não tem coluna aqui: o banco a registra no orçamento (`orcamento.orca_quilometragem`).

| Coluna             | Tipo        | Nulo | Default           | Chave                        | Descrição                                                           | Origem                                      |
| ------------------ | ----------- | ---- | ----------------- | ---------------------------- | ------------------------------------------------------------------- | ------------------------------------------- |
| veic_id            | INT         | Não  | AUTO_INCREMENT    | PK                           | Identificador do veículo                                            | — (chave do modelo lógico)                  |
| veic_placa         | VARCHAR(7)  | Não  | —                 | UK                           | Placa do veículo, com 7 posições; única                             | Veiculo.placa                               |
| veic_cor           | VARCHAR(20) | Não  | —                 |                              | Cor do veículo                                                      | Veiculo.cor                                 |
| veic_ano           | YEAR        | Sim  | —                 |                              | Ano do veículo (tipo `YEAR`); opcional                              | Veiculo.ano                                 |
| pess_id            | INT         | Não  | —                 | FK → pessoa · RESTRICT       | Cliente dono do veículo                                             | Veiculo.cliente (associação `possui`)       |
| camo_id            | INT         | Não  | —                 | FK → carro_modelo · RESTRICT | Modelo do veículo                                                   | Veiculo.modelo (associação `classifica`)    |
| VEIC_CRIADO_EM     | DATETIME    | Não  | CURRENT_TIMESTAMP |                              | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| VEIC_ATUALIZADO_EM | DATETIME    | Não  | CURRENT_TIMESTAMP |                              | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: `fk_veic_pessoa` (pess_id); `ix_camo_veiculo` (camo_id); `uk_veiculo_placa` UK (veic_placa). O índice `fk_veic_pessoa` foi criado pelo próprio MySQL para a chave estrangeira, porque o DDL não tem `CREATE INDEX` para `pess_id`.

## Orçamento

### servico

Tabela de `Servico`: tipo de serviço (mão de obra) cobrado no orçamento.

| Coluna             | Tipo          | Nulo | Default           | Chave | Descrição                                                                | Origem                                      |
| ------------------ | ------------- | ---- | ----------------- | ----- | ------------------------------------------------------------------------ | ------------------------------------------- |
| serv_id            | INT           | Não  | AUTO_INCREMENT    | PK    | Identificador do serviço                                                 | — (chave do modelo lógico)                  |
| serv_nome          | VARCHAR(100)  | Não  | —                 | UK    | Nome do serviço; único                                                   | Servico.nome                                |
| serv_descricao     | VARCHAR(255)  | Sim  | —                 |       | Descrição do serviço; opcional                                           | Servico.descricao                           |
| serv_valor         | DECIMAL(10,2) | Não  | —                 |       | Valor da mão de obra do serviço; não pode ser negativo (`ck_serv_valor`) | Servico.valorMaoDeObra (nome diferente)     |
| SERV_CRIADO_EM     | DATETIME      | Não  | CURRENT_TIMESTAMP |       | Data e hora de criação do registro                                       | Auditoria do banco (sem atributo na classe) |
| SERV_ATUALIZADO_EM | DATETIME      | Não  | CURRENT_TIMESTAMP |       | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`      | Auditoria do banco (sem atributo na classe) |

Índices: `uk_servico_nome` UK (serv_nome).

Restrição `ck_serv_valor`: `serv_valor >= 0`.

### orcamento

Tabela de `Orcamento`. O valor (`orca_valor`) é gravado, não calculado: nenhuma restrição o liga à soma dos itens. A classe tem também `numero` e `observacoes`, que não têm coluna; o identificador do banco é `orca_id`. O status nasce `aberto` e muda só na ordem aberto, orçado e, depois, aprovado ou recusado (gatilhos `tg_orca_bi_status_inicial` e `tg_orca_bu_transicao`); a recusa só é aceita depois de `orçado`, e a lacuna L-05 registra a dúvida sobre recusar a partir de `aberto`. Aprovar ou recusar exige o meio da resposta (`ck_orca_decisao`).

| Coluna             | Tipo          | Nulo | Default           | Chave                       | Descrição                                                                                                                                                                                                                                                 | Origem                                              |
| ------------------ | ------------- | ---- | ----------------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| orca_id            | INT           | Não  | AUTO_INCREMENT    | PK                          | Identificador do orçamento                                                                                                                                                                                                                                | — (chave do modelo lógico)                          |
| orca_valor         | DECIMAL(10,2) | Não  | 0.00              |                             | Valor do orçamento, gravado pela aplicação; zero por padrão; não pode ser negativo (`ck_orca_valores`)                                                                                                                                                    | Orcamento.valorTotal                                |
| orca_status        | VARCHAR(20)   | Não  | 'aberto'          |                             | Status do orçamento: `aberto`, `orçado`, `aprovado` ou `recusado` (`ck_orca_status`). Nasce `aberto` e muda só na ordem aberto, orçado, aprovado ou recusado                                                                                              | Orcamento.status                                    |
| orca_meio_resposta | VARCHAR(20)   | Sim  | —                 |                             | Meio pelo qual o cliente respondeu: `presencial`, `telefone` ou `mensagem` (`ck_orca_meio`). Nulo até a resposta; obrigatório quando o status é `aprovado` ou `recusado` (`ck_orca_decisao`)                                                              | Sem atributo na classe (RF-02; L-07)                |
| orca_formato       | VARCHAR(20)   | Não  | —                 |                             | Formato do orçamento, como o campo "Formato" da tela "Montar o orçamento"; texto obrigatório, sem lista de valores. O meio da resposta do cliente tem coluna própria, `orca_meio_resposta`. O significado de `orca_formato` precisa de confirmação (L-07) | Sem atributo na classe (L-07)                       |
| orca_quilometragem | INT           | Não  | —                 |                             | Quilometragem do veículo, registrada no orçamento; não pode ser negativa (`ck_orca_valores`)                                                                                                                                                              | Orcamento.quilometragem                             |
| veic_id            | INT           | Não  | —                 | FK → veiculo · RESTRICT     | Veículo orçado                                                                                                                                                                                                                                            | Orcamento.veiculo (associação `recebe`)             |
| func_id            | INT           | Não  | —                 | FK → funcionario · RESTRICT | Funcionário que elaborou o orçamento                                                                                                                                                                                                                      | Orcamento.funcionario (associação `elabora`)        |
| ORCA_CRIADO_EM     | DATETIME      | Não  | CURRENT_TIMESTAMP |                             | Data e hora de criação do registro                                                                                                                                                                                                                        | Orcamento.dataEmissao (data de criação do registro) |
| ORCA_ATUALIZADO_EM | DATETIME      | Não  | CURRENT_TIMESTAMP |                             | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`                                                                                                                                                                                       | Auditoria do banco (sem atributo na classe)         |

Índices: `ix_funcionario_orcamento` (func_id); `ix_orcamento_01` (orca_status); `ix_orcamento_02` (ORCA_CRIADO_EM); `ix_veiculo_orcamento` (veic_id).

Restrição `ck_orca_status`: `orca_status IN ('aberto', 'orçado', 'aprovado', 'recusado')`.

Restrição `ck_orca_meio`: `orca_meio_resposta IS NULL OR orca_meio_resposta IN ('presencial', 'telefone', 'mensagem')`.

Restrição `ck_orca_decisao`: `orca_status NOT IN ('aprovado', 'recusado') OR orca_meio_resposta IS NOT NULL`.

Restrição `ck_orca_valores`: `orca_valor >= 0 AND orca_quilometragem >= 0`.

Gatilho `tg_orca_bi_status_inicial` (`BEFORE INSERT`): recusa o orçamento criado com status diferente de `aberto`. Em caso de recusa, devolve o erro 45000 com a mensagem "O orçamento nasce com o status aberto."

Gatilho `tg_orca_bu_transicao` (`BEFORE UPDATE`): só aceita a mudança de `aberto` para `orçado` e a de `orçado` para `aprovado` ou `recusado`. Em caso de recusa, devolve o erro 45000 com a mensagem "Transição de status do orçamento inválida."

### orcamento_peca

Tabela de `ItemOrcamentoPeca`: peças do orçamento. A chave primária composta impede repetir a mesma peça no mesmo orçamento.

| Coluna              | Tipo          | Nulo | Default           | Chave                                            | Descrição                                                                      | Origem                                       |
| ------------------- | ------------- | ---- | ----------------- | ------------------------------------------------ | ------------------------------------------------------------------------------ | -------------------------------------------- |
| orca_id             | INT           | Não  | —                 | PK (orca_id, peca_id); FK → orcamento · RESTRICT | Orçamento do item                                                              | ItemOrcamentoPeca (composição com Orcamento) |
| peca_id             | INT           | Não  | —                 | PK (orca_id, peca_id); FK → peca · RESTRICT      | Peça do item                                                                   | ItemOrcamentoPeca.peca (associação)          |
| orpe_quantidade     | INT           | Não  | —                 |                                                  | Quantidade da peça no orçamento; maior que zero (`ck_orpe_valores`)            | ItemOrcamentoPeca.quantidade                 |
| orpe_preco_unitario | DECIMAL(10,2) | Não  | —                 |                                                  | Preço unitário da peça no orçamento; não pode ser negativo (`ck_orpe_valores`) | ItemOrcamentoPeca.valorUnitario              |
| ORPE_CRIADO_EM      | DATETIME      | Não  | CURRENT_TIMESTAMP |                                                  | Data e hora de criação do registro                                             | Auditoria do banco (sem atributo na classe)  |
| ORPE_ATUALIZADO_EM  | DATETIME      | Não  | CURRENT_TIMESTAMP |                                                  | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`            | Auditoria do banco (sem atributo na classe)  |

Índices: `ix_peca_orpe` (peca_id).

Restrição `ck_orpe_valores`: `orpe_quantidade > 0 AND orpe_preco_unitario >= 0`.

### orcamento_servico

Tabela de `ItemOrcamentoServico`: mão de obra do orçamento, com o valor cobrado. A classe tem também `quantidade`, que não tem coluna (N-07). A chave primária composta impede repetir o mesmo serviço no mesmo orçamento.

| Coluna             | Tipo          | Nulo | Default           | Chave                                            | Descrição                                                                        | Origem                                                                      |
| ------------------ | ------------- | ---- | ----------------- | ------------------------------------------------ | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| orca_id            | INT           | Não  | —                 | PK (orca_id, serv_id); FK → orcamento · RESTRICT | Orçamento do item                                                                | ItemOrcamentoServico (composição com Orcamento)                             |
| serv_id            | INT           | Não  | —                 | PK (orca_id, serv_id); FK → servico · RESTRICT   | Serviço do item                                                                  | ItemOrcamentoServico.servico (associação)                                   |
| orse_valor         | DECIMAL(10,2) | Não  | —                 |                                                  | Valor cobrado pelo serviço no orçamento; não pode ser negativo (`ck_orse_valor`) | ItemOrcamentoServico.valorUnitario (a classe tem também `quantidade`; N-07) |
| ORSE_CRIADO_EM     | DATETIME      | Não  | CURRENT_TIMESTAMP |                                                  | Data e hora de criação do registro                                               | Auditoria do banco (sem atributo na classe)                                 |
| ORSE_ATUALIZADO_EM | DATETIME      | Não  | CURRENT_TIMESTAMP |                                                  | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`              | Auditoria do banco (sem atributo na classe)                                 |

Índices: `ix_serv_orse` (serv_id).

Restrição `ck_orse_valor`: `orse_valor >= 0`.

### orcamento_historico

Tabela de `HistoricoStatusOrcamento`. Cada linha registra um status assumido pelo orçamento, com o funcionário e a data e hora. Guarda só o novo status. Nenhum gatilho a preenche: a gravação é da aplicação.

| Coluna         | Tipo        | Nulo | Default           | Chave                       | Descrição                                   | Origem                                                          |
| -------------- | ----------- | ---- | ----------------- | --------------------------- | ------------------------------------------- | --------------------------------------------------------------- |
| orhi_id        | INT         | Não  | AUTO_INCREMENT    | PK                          | Identificador do registro                   | — (chave do modelo lógico)                                      |
| orhi_status    | VARCHAR(20) | Não  | —                 |                             | Novo status do orçamento (`ck_orhi_status`) | HistoricoStatusOrcamento.statusNovo                             |
| orca_id        | INT         | Não  | —                 | FK → orcamento · RESTRICT   | Orçamento que mudou de status               | HistoricoStatusOrcamento (associação `registra` com Orcamento)  |
| func_id        | INT         | Não  | —                 | FK → funcionario · RESTRICT | Funcionário responsável pela mudança        | HistoricoStatusOrcamento.funcionario (associação `responsavel`) |
| ORHI_CRIADO_EM | DATETIME    | Não  | CURRENT_TIMESTAMP |                             | Data e hora da mudança de status            | HistoricoStatusOrcamento.dataHora                               |

Índices: `ix_funcionario_orhi` (func_id); `ix_orcamento_orhi` (orca_id, ORHI_CRIADO_EM).

Restrição `ck_orhi_status`: `orhi_status IN ('aberto', 'orçado', 'aprovado', 'recusado')`.

Tem só `ORHI_CRIADO_EM`; não há `ORHI_ATUALIZADO_EM`. A classe tem também `statusAnterior`, que não tem coluna (N-11).

## Ordem de serviço e boxes

### box

Tabela de `Box`. Não tem coluna de status: a ocupação é derivada das OS em execução (`ordem_servico.os_box_ativo`).

| Coluna            | Tipo             | Nulo | Default           | Chave | Descrição                                                           | Origem                                      |
| ----------------- | ---------------- | ---- | ----------------- | ----- | ------------------------------------------------------------------- | ------------------------------------------- |
| box_id            | INT              | Não  | AUTO_INCREMENT    | PK    | Identificador do box                                                | — (chave do modelo lógico)                  |
| box_numero        | TINYINT UNSIGNED | Não  | —                 | UK    | Número do box, de 0 a 255; único                                    | Box.numero                                  |
| BOX_CRIADO_EM     | DATETIME         | Não  | CURRENT_TIMESTAMP |       | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| BOX_ATUALIZADO_EM | DATETIME         | Não  | CURRENT_TIMESTAMP |       | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: `uk_box_numero` UK (box_numero).

### ordem_servico

Tabela de `OrdemServico`. Só nasce de um orçamento aprovado (gatilho `tg_os_bi_orcamento_aprovado`), com o status `aberta` (`tg_os_bi_status_inicial`), e há no máximo uma por orçamento (`uk_os_orcamento`). O orçamento da OS não pode ser trocado, e o status muda só na ordem aberta, em execução, concluída (`tg_os_bu_transicao`). A coluna gerada `os_box_ativo` garante uma única OS em execução por box (`uk_os_box_ativo`). A classe tem também `numero`, que não tem coluna; o identificador do banco é `os_id`.

| Coluna           | Tipo         | Nulo | Default           | Chave                         | Descrição                                                                                                                                                                       | Origem                                                |
| ---------------- | ------------ | ---- | ----------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| os_id            | INT          | Não  | AUTO_INCREMENT    | PK                            | Identificador da OS                                                                                                                                                             | — (chave do modelo lógico)                            |
| os_detalhes      | VARCHAR(500) | Sim  | —                 |                               | Detalhes do serviço a executar; opcional                                                                                                                                        | OrdemServico.detalhes                                 |
| os_inicio        | DATETIME     | Sim  | —                 |                               | Data e hora de início da execução; obrigatória quando a OS está `em execução` ou `concluída` (`ck_os_execucao`)                                                                 | Sem atributo na classe (L-07)                         |
| os_fim           | DATETIME     | Sim  | —                 |                               | Data e hora de conclusão; obrigatória quando a OS está `concluída` (`ck_os_conclusao`) e não pode ser anterior ao início (`ck_os_periodo`)                                      | OrdemServico.dataConclusao                            |
| os_status        | VARCHAR(20)  | Não  | 'aberta'          |                               | Status da OS: `aberta`, `em execução` ou `concluída` (`ck_os_status`). Nasce `aberta` e muda só na ordem aberta, em execução, concluída                                         | OrdemServico.status                                   |
| orca_id          | INT          | Não  | —                 | UK; FK → orcamento · RESTRICT | Orçamento aprovado que originou a OS; único; não pode ser trocado depois                                                                                                        | OrdemServico.orcamento (associação `gera`)            |
| box_id           | INT          | Sim  | —                 | FK → box · RESTRICT           | Box designado; obrigatório quando a OS está `em execução` ou `concluída` (`ck_os_execucao`); nulo até a designação                                                              | OrdemServico.box (associação `ocupa`)                 |
| meca_id          | INT          | Sim  | —                 | FK → funcionario · RESTRICT   | Mecânico designado (um funcionário); obrigatório quando a OS está `em execução` ou `concluída` (`ck_os_execucao`); nulo até a designação                                        | OrdemServico.mecanico (associação `mecanico`)         |
| func_id          | INT          | Não  | —                 | FK → funcionario · RESTRICT   | Funcionário que registra a OS; obrigatório. O mecânico designado fica em `meca_id`                                                                                              | Sem associação na classe (ver Divergências no README) |
| os_box_ativo     | INT          | Sim  | GERADA            | UK                            | Coluna gerada (`STORED`): recebe `box_id` quando `os_status` é `em execução` e `NULL` nos demais casos. A chave única `uk_os_box_ativo` impede duas OS em execução no mesmo box | — (regra da RF-03)                                    |
| OS_CRIADO_EM     | DATETIME     | Não  | CURRENT_TIMESTAMP |                               | Data e hora de criação da OS                                                                                                                                                    | OrdemServico.dataAbertura                             |
| OS_ATUALIZADO_EM | DATETIME     | Não  | CURRENT_TIMESTAMP |                               | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`                                                                                                             | Auditoria do banco (sem atributo na classe)           |

Índices: `ix_box_os` (box_id); `ix_funcionario_os` (func_id); `ix_mecanico_os` (meca_id); `uk_os_box_ativo` UK (os_box_ativo); `uk_os_orcamento` UK (orca_id).

Restrição `ck_os_status`: `os_status IN ('aberta', 'em execução', 'concluída')`.

Restrição `ck_os_execucao`: `os_status = 'aberta' OR (box_id IS NOT NULL AND meca_id IS NOT NULL AND os_inicio IS NOT NULL)`.

Restrição `ck_os_conclusao`: `os_status <> 'concluída' OR os_fim IS NOT NULL`.

Restrição `ck_os_periodo`: `os_inicio IS NULL OR os_fim IS NULL OR os_fim >= os_inicio`.

Gatilho `tg_os_bi_orcamento_aprovado` (`BEFORE INSERT`): recusa a criação da OS quando o status do orçamento não é `aprovado`. Em caso de recusa, devolve o erro 45000 com a mensagem "A ordem de serviço só pode ser criada a partir de um orçamento aprovado."

Gatilho `tg_os_bi_status_inicial` (`BEFORE INSERT`): recusa a OS criada com status diferente de `aberta`; roda depois de `tg_os_bi_orcamento_aprovado`. Em caso de recusa, devolve o erro 45000 com a mensagem "A OS nasce com o status aberta."

Gatilho `tg_os_bu_transicao` (`BEFORE UPDATE`): recusa a troca do orçamento da OS e só aceita a mudança de `aberta` para `em execução` e a de `em execução` para `concluída`. Em caso de recusa, devolve o erro 45000 com as mensagens "A OS não pode trocar de orçamento." ou "Transição de status da OS inválida."

### ordem_servico_peca

Sem classe correspondente: o diagrama de classes liga as peças só ao orçamento. A tabela guarda as peças usadas na OS, com quantidade e preço unitário. A chave primária composta impede repetir a peça na mesma OS.

| Coluna              | Tipo          | Nulo | Default           | Chave                                              | Descrição                                                               | Origem                                      |
| ------------------- | ------------- | ---- | ----------------- | -------------------------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------- |
| os_id               | INT           | Não  | —                 | PK (os_id, peca_id); FK → ordem_servico · RESTRICT | OS que usa a peça                                                       | Sem classe (ver Divergências no README)     |
| peca_id             | INT           | Não  | —                 | PK (os_id, peca_id); FK → peca · RESTRICT          | Peça usada na OS                                                        | Sem classe (ver Divergências no README)     |
| ospe_qt_peca        | INT           | Não  | —                 |                                                    | Quantidade usada da peça; maior que zero (`ck_ospe_valores`)            | Sem classe (ver Divergências no README)     |
| ospe_preco_unitario | DECIMAL(10,2) | Não  | —                 |                                                    | Preço unitário da peça na OS; não pode ser negativo (`ck_ospe_valores`) | Sem classe (ver Divergências no README)     |
| OSPE_CRIADO_EM      | DATETIME      | Não  | CURRENT_TIMESTAMP |                                                    | Data e hora de criação do registro                                      | Auditoria do banco (sem atributo na classe) |
| OSPE_ATUALIZADO_EM  | DATETIME      | Não  | CURRENT_TIMESTAMP |                                                    | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`     | Auditoria do banco (sem atributo na classe) |

Índices: `ix_peca_ospe` (peca_id).

Restrição `ck_ospe_valores`: `ospe_qt_peca > 0 AND ospe_preco_unitario >= 0`.

### ordem_servico_historico

Tabela de `HistoricoStatusOS`. Cada linha registra um status assumido pela OS, com o funcionário e a data e hora. Guarda só o novo status. Nenhum gatilho a preenche: a gravação é da aplicação.

| Coluna         | Tipo        | Nulo | Default           | Chave                         | Descrição                            | Origem                                                     |
| -------------- | ----------- | ---- | ----------------- | ----------------------------- | ------------------------------------ | ---------------------------------------------------------- |
| oshi_id        | INT         | Não  | AUTO_INCREMENT    | PK                            | Identificador do registro            | — (chave do modelo lógico)                                 |
| oshi_status    | VARCHAR(20) | Não  | —                 |                               | Novo status da OS (`ck_oshi_status`) | HistoricoStatusOS.statusNovo                               |
| os_id          | INT         | Não  | —                 | FK → ordem_servico · RESTRICT | OS que mudou de status               | HistoricoStatusOS (associação `registra` com OrdemServico) |
| func_id        | INT         | Não  | —                 | FK → funcionario · RESTRICT   | Funcionário responsável pela mudança | HistoricoStatusOS.funcionario (associação `responsavel`)   |
| OSHI_CRIADO_EM | DATETIME    | Não  | CURRENT_TIMESTAMP |                               | Data e hora da mudança de status     | HistoricoStatusOS.dataHora                                 |

Índices: `ix_funcionario_oshi` (func_id); `ix_os_oshi` (os_id, OSHI_CRIADO_EM).

Restrição `ck_oshi_status`: `oshi_status IN ('aberta', 'em execução', 'concluída')`.

Tem só `OSHI_CRIADO_EM`; não há `OSHI_ATUALIZADO_EM`. A classe tem também `statusAnterior`, que não tem coluna (N-11).

## Estoque

### peca

Tabela de `Peca`. A desativação lógica usa `peca_ativo`, a única do banco: as demais tabelas não têm coluna de exclusão lógica.

| Coluna              | Tipo          | Nulo | Default           | Chave | Descrição                                                                                                         | Origem                                      |
| ------------------- | ------------- | ---- | ----------------- | ----- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| peca_id             | INT           | Não  | AUTO_INCREMENT    | PK    | Identificador da peça                                                                                             | — (chave do modelo lógico)                  |
| peca_codigo         | VARCHAR(30)   | Não  | —                 | UK    | Código da peça; único                                                                                             | Peca.codigo                                 |
| peca_nome           | VARCHAR(100)  | Não  | —                 |       | Nome da peça                                                                                                      | Peca.nome                                   |
| peca_categoria      | VARCHAR(50)   | Não  | —                 |       | Categoria da peça, usada para filtrar o estoque                                                                   | Peca.tipo (nome diferente; L-15)            |
| peca_descricao      | VARCHAR(255)  | Não  | —                 |       | Descrição da peça; obrigatória                                                                                    | Sem atributo na classe                      |
| peca_estoque        | INT           | Não  | 0                 |       | Quantidade em estoque; não pode ser negativa (`ck_peca_estoque`)                                                  | Peca.estoque                                |
| peca_estoque_minimo | INT           | Não  | 0                 |       | Estoque mínimo: a partir dele o sistema avisa que a peça está acabando; não pode ser negativo (`ck_peca_valores`) | Peca.estoqueMinimo                          |
| peca_preco          | DECIMAL(10,2) | Não  | —                 |       | Preço unitário da peça; não pode ser negativo (`ck_peca_valores`)                                                 | Peca.preco                                  |
| peca_ativo          | BOOL          | Não  | TRUE              |       | Indica se a peça está ativa; é a desativação lógica da peça. Verdadeiro por padrão                                | Sem atributo na classe (L-07)               |
| PECA_CRIADO_EM      | DATETIME      | Não  | CURRENT_TIMESTAMP |       | Data e hora de criação do registro                                                                                | Auditoria do banco (sem atributo na classe) |
| PECA_ATUALIZADO_EM  | DATETIME      | Não  | CURRENT_TIMESTAMP |       | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE`                                               | Auditoria do banco (sem atributo na classe) |

Índices: `ix_cate_peca` (peca_categoria); `ix_esto_peca` (peca_estoque); `ix_nome_peca` (peca_nome); `uk_peca_codigo` UK (peca_codigo).

Restrição `ck_peca_estoque`: `peca_estoque >= 0`.

Restrição `ck_peca_valores`: `peca_preco >= 0 AND peca_estoque_minimo >= 0`.

### peca_modelo

Tabela de `Compatibilidade`: liga a peça aos modelos de carro compatíveis. A classe tem `observacao`, que não tem coluna.

| Coluna             | Tipo     | Nulo | Default           | Chave                                               | Descrição                                                           | Origem                                       |
| ------------------ | -------- | ---- | ----------------- | --------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------- |
| peca_id            | INT      | Não  | —                 | PK (peca_id, camo_id); FK → peca · RESTRICT         | Peça compatível                                                     | Compatibilidade (associação com Peca)        |
| camo_id            | INT      | Não  | —                 | PK (peca_id, camo_id); FK → carro_modelo · RESTRICT | Modelo de carro com o qual a peça é compatível                      | Compatibilidade (associação com CarroModelo) |
| PEMO_CRIADO_EM     | DATETIME | Não  | CURRENT_TIMESTAMP |                                                     | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe)  |
| PEMO_ATUALIZADO_EM | DATETIME | Não  | CURRENT_TIMESTAMP |                                                     | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe)  |

Índices: `ix_camo_pemo` (camo_id).

### distribuidora

Tabela de `Distribuidora`, com o endereço embutido nas colunas `dist_cep` e `dist_en_*`. A classe tem `contatoPreferencial`, que não tem coluna.

| Coluna              | Tipo         | Nulo | Default           | Chave | Descrição                                                           | Origem                                      |
| ------------------- | ------------ | ---- | ----------------- | ----- | ------------------------------------------------------------------- | ------------------------------------------- |
| dist_id             | INT          | Não  | AUTO_INCREMENT    | PK    | Identificador da distribuidora                                      | — (chave do modelo lógico)                  |
| dist_nome           | VARCHAR(100) | Não  | —                 |       | Nome da distribuidora                                               | Distribuidora.razaoSocial (nome diferente)  |
| dist_cnpj           | VARCHAR(14)  | Não  | —                 | UK    | CNPJ, com 14 posições e sem máscara; único                          | Distribuidora.cnpj                          |
| dist_telefone       | VARCHAR(11)  | Não  | —                 | UK    | Telefone com DDD, até 11 posições; único                            | Distribuidora.telefone                      |
| dist_email          | VARCHAR(100) | Não  | —                 | UK    | E-mail; único                                                       | Distribuidora.email                         |
| dist_cep            | VARCHAR(8)   | Sim  | —                 |       | CEP do endereço, com 8 posições; opcional                           | Endereco.cep (embutido)                     |
| dist_en_logradouro  | VARCHAR(100) | Não  | —                 |       | Logradouro do endereço                                              | Endereco.logradouro (embutido)              |
| dist_en_numero      | VARCHAR(6)   | Não  | —                 |       | Número do endereço, em texto de até 6 posições                      | Endereco.numero (embutido)                  |
| dist_en_bairro      | VARCHAR(100) | Não  | —                 |       | Bairro do endereço                                                  | Endereco.bairro (embutido)                  |
| dist_en_complemento | VARCHAR(100) | Sim  | —                 |       | Complemento do endereço; opcional                                   | Endereco.complemento (embutido)             |
| dist_observacoes    | VARCHAR(500) | Sim  | —                 |       | Observações sobre a distribuidora; opcional                         | Distribuidora.observacoes                   |
| DIST_CRIADO_EM      | DATETIME     | Não  | CURRENT_TIMESTAMP |       | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| DIST_ATUALIZADO_EM  | DATETIME     | Não  | CURRENT_TIMESTAMP |       | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: `uk_distribuidora_cnpj` UK (dist_cnpj); `uk_distribuidora_email` UK (dist_email); `uk_distribuidora_telefone` UK (dist_telefone).

### peca_distribuidora

Tabela de `PecaDistribuidora`. Cada linha registra uma peça fornecida por uma distribuidora, com quantidade e custo unitário. Não há chave única em (`peca_id`, `dist_id`): o mesmo par pode aparecer mais de uma vez. A classe tem `dataCotacao`, que não tem coluna própria.

| Coluna              | Tipo          | Nulo | Default           | Chave                         | Descrição                                                           | Origem                                                                   |
| ------------------- | ------------- | ---- | ----------------- | ----------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| pedi_id             | INT           | Não  | AUTO_INCREMENT    | PK                            | Identificador do registro                                           | — (chave do modelo lógico)                                               |
| peca_id             | INT           | Não  | —                 | FK → peca · RESTRICT          | Peça fornecida                                                      | PecaDistribuidora (associação com Peca)                                  |
| dist_id             | INT           | Não  | —                 | FK → distribuidora · RESTRICT | Distribuidora que fornece a peça                                    | PecaDistribuidora (associação `fornece`)                                 |
| pedi_quantidade     | INT           | Não  | —                 |                               | Quantidade fornecida                                                | Sem atributo na classe                                                   |
| pedi_custo_unitario | DECIMAL(10,2) | Não  | —                 |                               | Custo unitário da peça                                              | PecaDistribuidora.precoCusto                                             |
| PEDI_CRIADO_EM      | DATETIME      | Não  | CURRENT_TIMESTAMP |                               | Data e hora de criação do registro                                  | PecaDistribuidora.dataCotacao (data de criação do registro; a confirmar) |
| PEDI_ATUALIZADO_EM  | DATETIME      | Não  | CURRENT_TIMESTAMP |                               | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe)                              |

Índices: `ix_dist_pedi` (dist_id); `ix_peca_pedi` (peca_id).

## Pagamento

### pagamento

Sem classe e sem requisito (L-16 e N-14). Registro do pagamento de uma OS, à vista ou parcelado, segundo o glossário. Não há chave única em `os_id`: uma OS pode ter mais de um pagamento.

| Coluna             | Tipo          | Nulo | Default           | Chave                         | Descrição                                                           | Origem                                      |
| ------------------ | ------------- | ---- | ----------------- | ----------------------------- | ------------------------------------------------------------------- | ------------------------------------------- |
| paga_id            | INT           | Não  | AUTO_INCREMENT    | PK                            | Identificador do pagamento                                          | — (chave do modelo lógico)                  |
| paga_metodo        | VARCHAR(45)   | Não  | —                 |                               | Método de pagamento; texto livre, sem lista de valores              | Sem classe (L-16)                           |
| paga_valor         | DECIMAL(10,2) | Não  | —                 |                               | Valor do pagamento                                                  | Sem classe (L-16)                           |
| paga_nota_fiscal   | VARCHAR(44)   | Sim  | —                 | UK                            | Número da nota fiscal, até 44 posições; opcional e único            | Sem classe (L-16)                           |
| paga_status        | VARCHAR(20)   | Não  | —                 |                               | Status do pagamento; texto livre, sem `CHECK`                       | Sem classe (L-16)                           |
| paga_parcelado     | BOOL          | Não  | FALSE             |                               | Indica se o pagamento é parcelado; falso por padrão                 | Sem classe (L-16)                           |
| paga_dt_pagamento  | DATETIME      | Sim  | —                 |                               | Data e hora do pagamento; opcional                                  | Sem classe (L-16)                           |
| os_id              | INT           | Não  | —                 | FK → ordem_servico · RESTRICT | OS paga                                                             | Sem classe (L-16)                           |
| PAGA_CRIADO_EM     | DATETIME      | Não  | CURRENT_TIMESTAMP |                               | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| PAGA_ATUALIZADO_EM | DATETIME      | Não  | CURRENT_TIMESTAMP |                               | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: `ix_os_pagamento` (os_id); `uk_pagamento_nf` UK (paga_nota_fiscal).

### pagamento_parcelas

Sem classe e sem requisito (L-16 e N-14). Parcelas de um pagamento; a chave primária é o pagamento e o número da parcela.

| Coluna             | Tipo             | Nulo | Default           | Chave                                                | Descrição                                                           | Origem                                      |
| ------------------ | ---------------- | ---- | ----------------- | ---------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------- |
| paga_id            | INT              | Não  | —                 | PK (paga_id, papa_numero); FK → pagamento · RESTRICT | Pagamento ao qual a parcela pertence                                | Sem classe (L-16)                           |
| papa_numero        | TINYINT UNSIGNED | Não  | —                 | PK (paga_id, papa_numero)                            | Número da parcela, de 0 a 255; faz parte da chave primária          | Sem classe (L-16)                           |
| papa_valor         | DECIMAL(10,2)    | Não  | —                 |                                                      | Valor da parcela                                                    | Sem classe (L-16)                           |
| papa_vencimento    | DATE             | Não  | —                 |                                                      | Data de vencimento da parcela                                       | Sem classe (L-16)                           |
| papa_dt_pagamento  | DATETIME         | Sim  | —                 |                                                      | Data e hora em que a parcela foi paga; opcional                     | Sem classe (L-16)                           |
| PAPA_CRIADO_EM     | DATETIME         | Não  | CURRENT_TIMESTAMP |                                                      | Data e hora de criação do registro                                  | Auditoria do banco (sem atributo na classe) |
| PAPA_ATUALIZADO_EM | DATETIME         | Não  | CURRENT_TIMESTAMP |                                                      | Data e hora da última alteração; o MySQL a atualiza a cada `UPDATE` | Auditoria do banco (sem atributo na classe) |

Índices: só os da chave primária.

## Listas de valores e restrições

O banco não usa `ENUM`. Os status são `VARCHAR(20)` com `CHECK`, e o MySQL recusa o valor fora da lista no `INSERT` e no `UPDATE`. A comparação segue a collation padrão do servidor (no MySQL 8.0, `utf8mb4_0900_ai_ci`), que ignora maiúsculas, minúsculas e acentos: o valor `ORCADO` passa pelo `CHECK` e é gravado como foi enviado.

| Tabela                  | Restrição         | Valores ou regra                                                |
| ----------------------- | ----------------- | --------------------------------------------------------------- |
| funcionario             | `ck_func_acesso`  | Login e senha são ambos nulos ou ambos preenchidos.             |
| orcamento               | `ck_orca_status`  | Status: aberto, orçado, aprovado, recusado.                     |
| orcamento               | `ck_orca_meio`    | Meio da resposta: presencial, telefone, mensagem ou nulo.       |
| orcamento               | `ck_orca_decisao` | Orçamento aprovado ou recusado tem o meio da resposta.          |
| orcamento               | `ck_orca_valores` | Valor e quilometragem não negativos.                            |
| orcamento_historico     | `ck_orhi_status`  | Status: aberto, orçado, aprovado, recusado.                     |
| orcamento_peca          | `ck_orpe_valores` | Quantidade maior que zero e preço unitário não negativo.        |
| orcamento_servico       | `ck_orse_valor`   | Valor não negativo.                                             |
| ordem_servico           | `ck_os_status`    | Status: aberta, em execução, concluída.                         |
| ordem_servico           | `ck_os_execucao`  | OS em execução ou concluída tem box, mecânico e data de início. |
| ordem_servico           | `ck_os_conclusao` | OS concluída tem data de fim.                                   |
| ordem_servico           | `ck_os_periodo`   | A data de fim não é anterior à de início.                       |
| ordem_servico_peca      | `ck_ospe_valores` | Quantidade maior que zero e preço unitário não negativo.        |
| ordem_servico_historico | `ck_oshi_status`  | Status: aberta, em execução, concluída.                         |
| peca                    | `ck_peca_estoque` | O estoque não pode ser negativo.                                |
| peca                    | `ck_peca_valores` | Preço e estoque mínimo não negativos.                           |
| servico                 | `ck_serv_valor`   | Valor não negativo.                                             |

Colunas de texto sem lista de valores: `pess_ct_preferencial`, `orca_formato`, `paga_metodo` e `paga_status`.

## Gatilhos

Todos os gatilhos recusam a operação com `SIGNAL SQLSTATE '45000'`, que o MySQL devolve como o erro 1644. Eles tratam da ordem dos status e da origem da OS. Nenhum preenche as tabelas de histórico: essa gravação é da aplicação.

| Tabela        | Gatilho                       | Momento         | Regra                                                                                                                         |
| ------------- | ----------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| orcamento     | `tg_orca_bi_status_inicial`   | `BEFORE INSERT` | Recusa o orçamento criado com status diferente de `aberto`.                                                                   |
| orcamento     | `tg_orca_bu_transicao`        | `BEFORE UPDATE` | Só aceita a mudança de `aberto` para `orçado` e a de `orçado` para `aprovado` ou `recusado`.                                  |
| ordem_servico | `tg_os_bi_orcamento_aprovado` | `BEFORE INSERT` | Recusa a criação da OS quando o status do orçamento não é `aprovado`.                                                         |
| ordem_servico | `tg_os_bi_status_inicial`     | `BEFORE INSERT` | Recusa a OS criada com status diferente de `aberta`; roda depois de `tg_os_bi_orcamento_aprovado`.                            |
| ordem_servico | `tg_os_bu_transicao`          | `BEFORE UPDATE` | Recusa a troca do orçamento da OS e só aceita a mudança de `aberta` para `em execução` e a de `em execução` para `concluída`. |

## Resumo das chaves estrangeiras

| Tabela                  | Coluna  | Tabela referenciada | Coluna referenciada | Restrição                  |
| ----------------------- | ------- | ------------------- | ------------------- | -------------------------- |
| carro_modelo            | cama_id | carro_marca         | cama_id             | `fk_camo_marca`            |
| funcionario             | funo_id | funcao              | funo_id             | `fk_func_funcao`           |
| funcionario             | pess_id | pessoa              | pess_id             | `fk_func_pessoa`           |
| orcamento               | func_id | funcionario         | pess_id             | `fk_orcamento_funcionario` |
| orcamento               | veic_id | veiculo             | veic_id             | `fk_orcamento_veiculo`     |
| orcamento_historico     | func_id | funcionario         | pess_id             | `fk_orhi_funcionario`      |
| orcamento_historico     | orca_id | orcamento           | orca_id             | `fk_orhi_orcamento`        |
| orcamento_peca          | orca_id | orcamento           | orca_id             | `fk_orpe_orcamento`        |
| orcamento_peca          | peca_id | peca                | peca_id             | `fk_orpe_peca`             |
| orcamento_servico       | orca_id | orcamento           | orca_id             | `fk_orse_orcamento`        |
| orcamento_servico       | serv_id | servico             | serv_id             | `fk_orse_servico`          |
| ordem_servico           | box_id  | box                 | box_id              | `fk_os_box`                |
| ordem_servico           | func_id | funcionario         | pess_id             | `fk_os_funcionario`        |
| ordem_servico           | meca_id | funcionario         | pess_id             | `fk_os_mecanico`           |
| ordem_servico           | orca_id | orcamento           | orca_id             | `fk_os_orcamento`          |
| ordem_servico_historico | func_id | funcionario         | pess_id             | `fk_oshi_funcionario`      |
| ordem_servico_historico | os_id   | ordem_servico       | os_id               | `fk_oshi_os`               |
| ordem_servico_peca      | os_id   | ordem_servico       | os_id               | `fk_ospe_os`               |
| ordem_servico_peca      | peca_id | peca                | peca_id             | `fk_ospe_peca`             |
| pagamento               | os_id   | ordem_servico       | os_id               | `fk_pagamento_os`          |
| pagamento_parcelas      | paga_id | pagamento           | paga_id             | `fk_papa_pagamento`        |
| peca_distribuidora      | dist_id | distribuidora       | dist_id             | `fk_pedi_distribuidora`    |
| peca_distribuidora      | peca_id | peca                | peca_id             | `fk_pedi_peca`             |
| peca_modelo             | camo_id | carro_modelo        | camo_id             | `fk_pemo_modelo`           |
| peca_modelo             | peca_id | peca                | peca_id             | `fk_pemo_peca`             |
| veiculo                 | camo_id | carro_modelo        | camo_id             | `fk_veic_modelo`           |
| veiculo                 | pess_id | pessoa              | pess_id             | `fk_veic_pessoa`           |

Todas as chaves estrangeiras usam a regra padrão (`NO ACTION`, equivalente a `RESTRICT`): o banco não apaga nem altera uma linha que ainda é referenciada.

Veja também: [README](README.md) · [schema.sql](schema.sql) · [modelo de classes](../05-modelagem/classes/README.md)
