# Restrições OCL do domínio

Milestone 2 · Unidade: Engenharia de Software (Fábrica de Software I) · Modelagem do domínio

Este documento escreve em OCL as regras de negócio que o [diagrama de classes](../classes/README.md) não expressa. Cada restrição vem de um requisito funcional ([requisitos-funcionais.md](../../03-requisitos/requisitos-funcionais.md)), de uma história de usuário, de um cenário de diagrama de sequência ou de uma decisão do grupo. O projeto ainda não tem código da aplicação nem o `milestone_2/ddl.sql` no repositório; por isso a garantia no banco, na [tabela de rastreio](#tabela-de-rastreio), vem do que os requisitos citam (restrições, chaves únicas e gatilhos), e não de uma leitura do DDL. Onde os documentos do projeto divergem, a OCL segue a issue do GitLab e a decisão do grupo, e a diferença fica na seção de [divergências](#divergências).

## Convenções

- Sintaxe: OCL 2.4, em subconjunto (`context`, `inv`, `pre`, `post`, `@pre`, `result`, `allInstances()`, coleções `Set{...}`).
- Classes seguem a [lista oficial](../classes/README.md#lista-oficial-de-classes): português, no singular, sem acento. Os papéis de associação são os do diagrama de classes (`veiculos`, `orcamentos`, `itensPeca`, `ordemServico` e assim por diante), listados no [mapeamento](#mapeamento-para-classes-e-associações).
- Tipos dos atributos: `Texto`, `Inteiro`, `Decimal`, `Data`, `DataHora`, `Hora` e `Booleano`, como no diagrama. Atributo opcional ausente é testado com `oclIsUndefined()`; associação opcional (0..1) é testada com `isEmpty()` e `notEmpty()`.
- `agora : DataHora` é parâmetro das operações que dependem do relógio, para que a expressão não chame uma função que o modelo não define.
- Os literais das enumerações são escritos sem acento, para valerem como identificadores OCL: `StatusOrcamento::aberto`, `orcado`, `aprovado`, `recusado`; `StatusOS::aberta`, `emExecucao`, `concluida`; `StatusBox::livre`, `ocupado`. Os valores com acento (`orçado`, `em execução`, `concluída`) são os do [glossário](../../01-visao/glossario.md) e do banco. Ver [divergencias-m2.md](divergencias-m2.md), N-09.
- Os nomes de classe e de atributo que mudam entre diagrama, glossário e banco estão em [divergencias-m2.md](divergencias-m2.md).

### Mapeamento para classes e associações

| Classe OCL | Tabela (glossário e RF) | Associações usadas nas expressões |
|---|---|---|
| `Pessoa` (abstrata) | `pessoa` | `endereco` |
| `Cliente` (herda `Pessoa`) | `pessoa` | `veiculos` |
| `Funcionario` (herda `Pessoa`) | `funcionario` | `funcao` |
| `Funcao` | `funcao` | |
| `Veiculo` | `veiculo` | `cliente`, `modelo`, `orcamentos` |
| `CarroMarca`, `CarroModelo` | `carro_marca`, `carro_modelo` | `modelos`, `marca` |
| `Orcamento` | `orcamento` | `veiculo`, `funcionario`, `itensPeca`, `itensServico`, `ordemServico`, `historico` |
| `ItemOrcamentoPeca` | `orcamento_peca` | `peca` |
| `ItemOrcamentoServico` | `orcamento_servico` | `servico` |
| `Servico` | `servico` | |
| `OrdemServico` | `ordem_servico` | `orcamento`, `box`, `mecanico`, `historico` |
| `Box` | `box` | |
| `HistoricoStatusOS` | `ordem_servico_historico` | `funcionario` |
| `HistoricoStatusOrcamento` | `orcamento_historico` | `funcionario` |
| `Peca` | `peca` | |
| `Distribuidora` | `distribuidora` | `endereco` |
| `PecaDistribuidora` | `peca_distribuidora` | |
| `Compatibilidade` | `peca_modelo` | |

`Orcamento.historico` e `HistoricoStatusOrcamento` estão no [diagrama de apoio](../classes/classes-apoio.svg). O papel `historico` é o nome adotado aqui para a composição "registra" do diagrama.

## Invariantes

Cada invariante indica o contexto em que vale, a expressão e a consequência da violação. A garantia no banco e a origem estão na [tabela de rastreio](#tabela-de-rastreio).

### INV-01 CPF único

Vale para `Pessoa`. Duas pessoas não têm o mesmo CPF. O cadastro e a atualização de cliente recusam CPF repetido (US-003 e US-004).

```ocl
context Pessoa
inv CpfUnico:
  Pessoa.allInstances()->forAll(p | p <> self implies p.cpf <> self.cpf)
```

Consequência: dois clientes passam a representar a mesma pessoa, e o histórico do atendimento se divide entre eles.

### INV-02 E-mail e telefone únicos

Vale para `Pessoa`. O diagrama de sequência do cadastro (DS-001) recusa telefone, e-mail ou CPF já cadastrado com "Campo de valor único duplicado".

```ocl
context Pessoa
inv EmailUnico:
  Pessoa.allInstances()->forAll(p | p <> self implies p.email <> self.email)
inv TelefoneUnico:
  Pessoa.allInstances()->forAll(p | p <> self implies p.telefone <> self.telefone)
```

Consequência: a Secretária liga ou escreve para a pessoa errada. A unicidade do telefone só aparece nos diagramas (divergência D-08).

### INV-03 Placa única

Vale para `Veiculo`. A placa identifica um único veículo, e a consulta do histórico parte dela (RF-01 e RF-04).

```ocl
context Veiculo
inv PlacaUnica:
  Veiculo.allInstances()->forAll(v | v <> self implies v.placa <> self.placa)
```

Consequência: a consulta do histórico por placa (US-029) devolve veículos misturados.

### INV-04 Um orçamento aberto por veículo

Vale para `Orcamento`. Um veículo tem no máximo um orçamento `aberto`. O DS-003 recusa o cadastro com "O veículo já tem um orçamento aberto".

```ocl
context Orcamento
inv UmAbertoPorVeiculo:
  self.status = StatusOrcamento::aberto implies
    Orcamento.allInstances()->select(o |
      o.veiculo = self.veiculo and o.status = StatusOrcamento::aberto
    )->size() = 1
```

Consequência: dois orçamentos abertos para o mesmo veículo, e a Secretária não sabe qual aprovar.

### INV-05 Orçamento orçado tem itens

Vale para `Orcamento`. O orçamento é montado item a item, com peças e mão de obra separadas, e só deixa de ser `aberto` depois de lançados os itens (RF-02 e US-016).

```ocl
context Orcamento
inv OrcadoTemItens:
  self.status <> StatusOrcamento::aberto implies
    (self.itensPeca->notEmpty() or self.itensServico->notEmpty())
```

Consequência: um orçamento `orçado` ou `aprovado` sem nenhum item, ou seja, sem valor com origem técnica. A tela atual registra um valor fixo, sem itens (divergência D-05).

### INV-06 Total igual à soma dos itens

Vale para `Orcamento`. Depois de `aberto`, o valor total é a soma das peças e da mão de obra.

```ocl
context Orcamento
inv TotalConfere:
  self.status <> StatusOrcamento::aberto implies
    self.valorTotal =
      self.itensPeca->collect(i | i.quantidade * i.valorUnitario)->sum()
      + self.itensServico->collect(i | i.quantidade * i.valorUnitario)->sum()
```

Consequência: o balcão apresenta ao cliente um valor diferente do que foi lançado.

### INV-07 Uma OS por orçamento

Vale para `OrdemServico`. Cada orçamento gera no máximo uma OS (RF-02 e RF-06).

```ocl
context OrdemServico
inv UmaOSPorOrcamento:
  OrdemServico.allInstances()->select(o | o.orcamento = self.orcamento)->size() = 1
```

Consequência: o mesmo serviço seria executado e cobrado duas vezes.

### INV-08 A OS nasce de orçamento aprovado

Vale para `OrdemServico`. Nenhuma OS existe sem um orçamento aprovado. A regra é do grupo e vem da RF-02 e da RF-06.

```ocl
context OrdemServico
inv OrcamentoAprovado:
  self.orcamento.status = StatusOrcamento::aprovado
```

Consequência: um serviço começa sem a autorização do cliente.

### INV-09 Box e mecânico antes de executar

Vale para `OrdemServico`. A OS só deixa de estar `aberta` com box e mecânico definidos (RF-03 e US-028).

```ocl
context OrdemServico
inv BoxEMecanicoDefinidos:
  self.status <> StatusOS::aberta implies
    (self.box->notEmpty() and self.mecanico->notEmpty())
```

Consequência: uma OS em execução sem box nem responsável, e o painel não mostra onde o veículo está.

### INV-10 Um box, uma OS em execução

Vale para `OrdemServico`. Um box comporta um veículo por vez: enquanto houver OS `em execução` nele, não recebe outra (RF-03).

```ocl
context OrdemServico
inv BoxExclusivoEmExecucao:
  self.status = StatusOS::emExecucao implies
    OrdemServico.allInstances()->select(o |
      o.box = self.box and o.status = StatusOS::emExecucao
    )->size() = 1
```

Consequência: dois veículos no mesmo box ao mesmo tempo.

### INV-11 Situação do box coerente com as OS

Vale para `Box`. O box está `ocupado` exatamente quando há uma OS `em execução` nele.

```ocl
context Box
inv SituacaoCoerente:
  (self.status = StatusBox::ocupado) =
    OrdemServico.allInstances()->exists(o |
      o.box = self and o.status = StatusOS::emExecucao)
```

Consequência: um box aparece livre com veículo dentro, ou ocupado depois da conclusão. Os valores de `StatusBox` ainda precisam de confirmação (divergência D-04).

### INV-12 Datas da OS

Vale para `OrdemServico`. A OS `concluída` tem data de conclusão, e a conclusão não é anterior à abertura (RF-03: início e conclusão gravam data e hora).

```ocl
context OrdemServico
inv DatasCoerentes:
  (self.status = StatusOS::concluida implies not self.dataConclusao.oclIsUndefined())
  and (not self.dataConclusao.oclIsUndefined() implies self.dataConclusao >= self.dataAbertura)
```

Consequência: o tempo de permanência do veículo na oficina não pode ser calculado.

### INV-13 Histórico da OS registra quem e quando

Vale para `HistoricoStatusOS`. Cada mudança de status grava o funcionário e a data e hora (RF-06). O registro de criação da OS tem status anterior vazio.

```ocl
context HistoricoStatusOS
inv RegistroCompleto:
  self.funcionario->notEmpty()
  and not self.dataHora.oclIsUndefined()
  and (self.statusAnterior.oclIsUndefined() or self.statusAnterior <> self.statusNovo)
```

Consequência: o histórico mostra mudanças sem autor ou transições que não aconteceram.

### INV-14 Histórico do orçamento registra quem e quando

Vale para `HistoricoStatusOrcamento`. Mesma regra, para o orçamento (RF-02 e US-018).

```ocl
context HistoricoStatusOrcamento
inv RegistroCompleto:
  self.funcionario->notEmpty()
  and not self.dataHora.oclIsUndefined()
  and (self.statusAnterior.oclIsUndefined() or self.statusAnterior <> self.statusNovo)
```

Consequência: a oficina não tem como comprovar quem registrou a aprovação do cliente.

### INV-15 Código da peça único

Vale para `Peca`. O código identifica a peça (US-019 e US-021 recusam código repetido).

```ocl
context Peca
inv CodigoUnico:
  Peca.allInstances()->forAll(p | p <> self implies p.codigo <> self.codigo)
```

Consequência: o estoque baixa a peça errada.

### INV-16 Estoque nunca negativo

Vale para `Peca`. A quantidade em estoque não fica abaixo de zero, e o estoque mínimo também não (RF-05 e US-019).

```ocl
context Peca
inv EstoqueNaoNegativo:
  self.estoque >= 0 and self.estoqueMinimo >= 0
```

Consequência: o alerta de estoque baixo deixa de ser confiável.

### INV-17 CNPJ único

Vale para `Distribuidora`. O CNPJ identifica a distribuidora (DS-010 e DS-011 recusam com "CNPJ já cadastrado").

```ocl
context Distribuidora
inv CnpjUnico:
  Distribuidora.allInstances()->forAll(d | d <> self implies d.cnpj <> self.cnpj)
```

Consequência: a mesma distribuidora com dois cadastros, e a cotação de uma peça se divide entre eles.

### INV-18 Acesso ao sistema

Vale para `Funcionario`. Só funcionário com login e senha acessa o sistema, e os dois existem juntos (RF-01 e US-001). O login é único.

```ocl
context Funcionario
inv LoginESenhaJuntos:
  self.matricula.oclIsUndefined() = self.senha.oclIsUndefined()
inv LoginUnico:
  not self.matricula.oclIsUndefined() implies
    Funcionario.allInstances()->forAll(f | f <> self implies f.matricula <> self.matricula)
```

Consequência: um funcionário sem senha consegue entrar, ou dois funcionários dividem o mesmo login. Em `Funcionario`, `matricula` corresponde à coluna do login (divergência N-02).

## Operações

Cada operação traz `pre` (condição de entrada) e `post` (efeito). Em `@pre`, o valor vale antes da operação. `result` é o retorno. O ator de cada operação é o funcionário autenticado.

### OP-01 Orcamento::calcularTotal

```ocl
context Orcamento::calcularTotal() : Decimal
post SomaDosItens:
  result =
    self.itensPeca->collect(i | i.quantidade * i.valorUnitario)->sum()
    + self.itensServico->collect(i | i.quantidade * i.valorUnitario)->sum()
```

Observação: a operação só lê. A atualização de `valorTotal` e a mudança para `orçado` acontecem quando os itens são lançados (INV-05 e INV-06).

### OP-02 Orcamento::aprovar

```ocl
context Orcamento::aprovar(ator : Funcionario, agora : DataHora)
pre EstadoOrcado:
  self.status = StatusOrcamento::orcado
post StatusAprovado:
  self.status = StatusOrcamento::aprovado
post HistoricoGravado:
  self.historico->exists(h |
    h.statusAnterior = StatusOrcamento::orcado
    and h.statusNovo = StatusOrcamento::aprovado
    and h.funcionario = ator
    and h.dataHora = agora)
```

Observação: corresponde ao DS-012 e ao DE-002. Aprovar um orçamento `aberto`, `aprovado` ou `recusado` retorna "O orçamento não pode ser aprovado nesse status". A US-017 pede também o meio da resposta (presencial, telefone ou mensagem), que o modelo ainda não tem (divergência D-06).

### OP-03 Orcamento::recusar

```ocl
context Orcamento::recusar(ator : Funcionario, agora : DataHora)
pre EstadoOrcado:
  self.status = StatusOrcamento::orcado
post StatusRecusado:
  self.status = StatusOrcamento::recusado
post HistoricoGravado:
  self.historico->exists(h |
    h.statusAnterior = StatusOrcamento::orcado
    and h.statusNovo = StatusOrcamento::recusado
    and h.funcionario = ator
    and h.dataHora = agora)
post NaoGeraOS:
  self.ordemServico->isEmpty()
```

Observação: corresponde ao DS-013 e ao DE-003. `recusado` é final, e o orçamento recusado não gera OS. Retorna "O orçamento não pode ser recusado nesse status" fora de `orçado`.

### OP-04 OrdemServico::criar

```ocl
context OrdemServico::criar(orc : Orcamento, ator : Funcionario,
                            detalhes : Texto, agora : DataHora) : OrdemServico
pre OrcamentoAprovado:
  orc.status = StatusOrcamento::aprovado
pre SemOutraOS:
  orc.ordemServico->isEmpty()
post OSAberta:
  result.status = StatusOS::aberta
  and result.orcamento = orc
  and result.dataAbertura = agora
post HistoricoGravado:
  result.historico->exists(h |
    h.statusNovo = StatusOS::aberta
    and h.funcionario = ator
    and h.dataHora = agora)
```

Observação: corresponde ao UC-014 (issue #81). Com o orçamento não aprovado, o sistema notifica que o orçamento não foi aprovado e não registra a OS. O diagrama de classes não tem essa operação, e o DA-005 registra a OS como "Aprovada" (divergência D-07).

### OP-05 OrdemServico::designarBox

```ocl
context OrdemServico::designarBox(b : Box, m : Funcionario)
pre OSAberta:
  self.status = StatusOS::aberta
pre BoxLivre:
  OrdemServico.allInstances()->select(o |
    o.box = b and o.status = StatusOS::emExecucao
  )->isEmpty()
pre MecanicoAtivo:
  m.ativo
post BoxEMecanicoDefinidos:
  self.box = b and self.mecanico = m
post StatusInalterado:
  self.status = self.status@pre
```

Observação: corresponde às telas de designar box e mecânico (T-SEC-56 e T-SEC-57), em que os boxes em execução aparecem desabilitados. A designação não ocupa o box; a ocupação começa em `iniciar` (divergência D-09). A designação não tem caso de uso no catálogo.

### OP-06 OrdemServico::iniciar

```ocl
context OrdemServico::iniciar(ator : Funcionario, agora : DataHora)
pre EstadoAberta:
  self.status = StatusOS::aberta
pre BoxEMecanicoDesignados:
  self.box->notEmpty() and self.mecanico->notEmpty()
post EmExecucao:
  self.status = StatusOS::emExecucao
post BoxOcupado:
  self.box.status = StatusBox::ocupado
post HistoricoGravado:
  self.historico->exists(h |
    h.statusAnterior = StatusOS::aberta
    and h.statusNovo = StatusOS::emExecucao
    and h.funcionario = ator
    and h.dataHora = agora)
```

Observação: corresponde aos cenários "iniciar a execução" da US-028 e à transição do DE-001. Sem box ou mecânico, a alteração é recusada com o aviso de que é preciso designar os dois.

### OP-07 OrdemServico::concluir

```ocl
context OrdemServico::concluir(ator : Funcionario, agora : DataHora)
pre EstadoEmExecucao:
  self.status = StatusOS::emExecucao
post Concluida:
  self.status = StatusOS::concluida
  and self.dataConclusao = agora
post BoxLiberado:
  self.box.status = StatusBox::livre
post HistoricoGravado:
  self.historico->exists(h |
    h.statusAnterior = StatusOS::emExecucao
    and h.statusNovo = StatusOS::concluida
    and h.funcionario = ator
    and h.dataHora = agora)
```

Observação: corresponde ao DS-008, ao UC-013 e ao DE-001. O DS-008 não desenha a guarda da `pre EstadoEmExecucao`, que vem do DE-001.

### OP-08 Box::ocupar e Box::liberar

```ocl
context Box::ocupar()
pre BoxLivre:
  self.status = StatusBox::livre
post Ocupado:
  self.status = StatusBox::ocupado

context Box::liberar()
pre BoxOcupado:
  self.status = StatusBox::ocupado
post Livre:
  self.status = StatusBox::livre
```

Observação: `iniciar` chama `ocupar`, e `concluir` chama `liberar`. O `Box` tem `status` sem máquina de estados (lacuna L-13); estas duas operações são a regra mínima, até o grupo decidir entre desenhar o diagrama de estados do box e trocar o atributo por um booleano.

### OP-09 Peca::cadastrarEstoque, baixarEstoque e estoqueBaixo

```ocl
context Peca::cadastrarEstoque(qtd : Inteiro)
pre QuantidadePositiva:
  qtd > 0
post EstoqueAumentou:
  self.estoque = self.estoque@pre + qtd

context Peca::baixarEstoque(qtd : Inteiro)
pre QuantidadePositiva:
  qtd > 0
pre HaEstoque:
  qtd <= self.estoque
post EstoqueDiminuiu:
  self.estoque = self.estoque@pre - qtd

context Peca::estoqueBaixo() : Booleano
post AbaixoDoMinimo:
  result = (self.estoque <= self.estoqueMinimo)
```

Observação: a baixa de estoque protege INV-16. O sistema avisa quando a quantidade chega ao mínimo definido pela oficina (RF-05 e US-024); a regra "chega ao mínimo" foi escrita como `<=`.

### OP-10 Funcionario::autenticar

```ocl
context Funcionario::autenticar(login : Texto, senhaInformada : Texto) : Booleano
post CredenciaisConferem:
  result = (self.ativo
            and self.matricula = login
            and self.senha = senhaInformada)
```

Observação: a classe guarda a senha em um atributo, e o modelo não define como ela é protegida; essa decisão é de implementação. A autenticação é incluída (`<<include>>`) por todos os casos de uso.

## Tabela de rastreio

Expressão (resumida), regra, origem e a garantia no banco. A coluna "Garantia no banco" cita as restrições, as chaves únicas e os gatilhos que os requisitos e o glossário mencionam. "Não confirmada" quer dizer que o `milestone_2/ddl.sql` não está no repositório e nenhum documento cita a restrição. "Nenhuma" quer dizer que o documento afirma que o banco aceita a violação.

| Ref | Expressão (resumo) | Regra | Origem | Garantia no banco |
|---|---|---|---|---|
| INV-01 | `cpf` diferente em toda outra `Pessoa` | CPF único | RF-01, US-003, US-004 | `uk_pessoa_cpf` |
| INV-02 | `email` e `telefone` únicos | Contato único | DS-001, US-003 | E-mail: exigido pelo banco, segundo a US-003; telefone: não confirmada |
| INV-03 | `placa` diferente em todo outro `Veiculo` | Placa única | RF-01, RF-04, US-006 | `uk_veiculo_placa` |
| INV-04 | um `aberto` por `veiculo` | Orçamento aberto único | DS-003 | Não confirmada |
| INV-05 | `status <> aberto implies` itens | Orçamento item a item | RF-02, US-016 | Nenhuma: a tela registra valor fixo |
| INV-06 | `valorTotal` = soma dos itens | Total confere | RF-02, US-016 | Não confirmada |
| INV-07 | uma OS por `orcamento` | Uma OS por orçamento | RF-02, RF-06 | `uk_os_orcamento` |
| INV-08 | `orcamento.status = aprovado` | OS só de orçamento aprovado | RF-02, RF-06 | Gatilho `tg_os_bi_orcamento_aprovado` |
| INV-09 | `status <> aberta implies` box e mecânico | Box e mecânico antes de executar | RF-03, US-028 | Nenhuma: `box_id` e `meca_id` aceitam nulo |
| INV-10 | um `emExecucao` por `box` | Box exclusivo | RF-03 | Coluna gerada `os_box_ativo` e `uk_os_box_ativo` |
| INV-11 | `ocupado` se e só se há OS `emExecucao` | Situação do box | RF-03 | Não confirmada |
| INV-12 | `concluida implies dataConclusao` | Datas da OS | RF-03 | `os_fim` guarda a data; sem restrição citada |
| INV-13 | funcionário, data e hora, transição real | Histórico da OS | RF-06 | `ordem_servico_historico` (`oshi_status`, `func_id`, `OSHI_CRIADO_EM`); só o novo status |
| INV-14 | funcionário, data e hora, transição real | Histórico do orçamento | RF-02, US-018 | `orcamento_historico` (`orhi_status`, `func_id`, `ORHI_CRIADO_EM`); só o novo status |
| INV-15 | `codigo` diferente em toda outra `Peca` | Código único | RF-05, US-019 | `uk_peca_codigo` |
| INV-16 | `estoque >= 0` | Estoque não negativo | RF-05, US-019 | `ck_peca_estoque` (estoque); o mínimo não confirmada |
| INV-17 | `cnpj` único | CNPJ único | DS-010, DS-011 | Não confirmada |
| INV-18 | `matricula` e `senha` juntos; login único | Acesso ao sistema | RF-01, US-001 | `ck_func_acesso`, `uk_funcionario_login` |
| OP-02 | `aprovar`: `orcado` para `aprovado` | Decisão do cliente | RF-02, DS-012 | `ck_orca_status` (valores); sem gatilho de transição |
| OP-03 | `recusar`: `orcado` para `recusado` | Decisão do cliente | RF-02, DS-013 | `ck_orca_status` (valores); sem gatilho de transição |
| OP-04 | `criar`: OS `aberta` de orçamento aprovado | Nascimento da OS | RF-06, UC-014 | `tg_os_bi_orcamento_aprovado` |
| OP-05 a OP-07 | `designarBox`, `iniciar`, `concluir` | Ciclo da OS | RF-03, RF-06, US-028 | `ck_os_status` (valores); sem gatilho de transição |
| OP-09 | `baixarEstoque` não deixa o estoque negativo | Controle de estoque | RF-05 | `ck_peca_estoque` |

## Divergências

Onde os documentos do projeto não batem, a OCL segue a issue do GitLab e a decisão do grupo registrada nos requisitos. A tabela lista o que foi encontrado ao escrever as restrições.

| # | Documentação | Fonte divergente | Tratamento na OCL |
|---|---|---|---|
| D-01 | A issue da RF-06 diz que a OS passa por "Aprovada → em execução → concluída" | O banco (`ck_os_status`) e a decisão do grupo usam `aberta`, `em execução` e `concluída`; `aprovado` pertence ao orçamento | A OCL usa `aberta`, `emExecucao` e `concluida`. Ver [requisitos-funcionais.md](../../03-requisitos/requisitos-funcionais.md#divergências) |
| D-02 | RF-03: a OS só entra em execução com box e mecânico definidos | O banco aceita `box_id` e `meca_id` nulos, sem restrição | `INV-09` e `OP-06` impõem a regra; só a aplicação a garante. Falta uma restrição no banco |
| D-03 | RF-02: a transição segue a ordem do ciclo (não se aprova orçamento `aberto`) | O banco não tem gatilho de transição, só restrição nos valores | `OP-02` e `OP-03` têm `pre EstadoOrcado`, e `OP-05` a `OP-07` têm a guarda do estado de origem |
| D-04 | Glossário e lacuna L-13: o box tem `status` com `livre` e `ocupado` | O README antigo de classes falava em `LIVRE` e `EM EXECUÇÃO`; o `Box` não tem máquina de estados | `INV-11` e `OP-08` usam `livre` e `ocupado`. Confirmar os valores com o banco |
| D-05 | RF-02 e US-016: o orçamento é montado item a item, com peças e mão de obra separadas | A tela "Montar o orçamento" registra um valor fixo, sem itens (lacuna L-10) | `INV-05` e `INV-06` seguem a RF-02. Se o cliente aceitar o orçamento por valor fixo, as duas restrições deixam de valer |
| D-06 | US-017: a aprovação registra o meio da resposta (presencial, telefone ou mensagem) e a recusa, o motivo | O diagrama de classes não tem esses atributos; o banco tem `orca_formato`, sem restrição de valores | `OP-02` e `OP-03` não exigem o meio nem o motivo. Lacuna L-07 |
| D-07 | UC-014 (issue #81): a OS é criada a partir do orçamento aprovado | O DS-004 desenha `iniciarExecucao`, o DA-005 registra a OS como "Aprovada", e a classe `OrdemServico` não tem operação de criação | `OP-04` descreve a criação a partir do orçamento aprovado, com status `aberta`. Ver [sequência](../sequencia/README.md#divergências) |
| D-08 | DS-001: telefone, e-mail e CPF repetidos são recusados | Só o CPF (`uk_pessoa_cpf`) e a placa (`uk_veiculo_placa`) têm restrição citada nos requisitos; a US-003 diz que o banco exige e-mail único | `INV-02` inclui telefone e e-mail por causa do DS-001. A garantia no banco do telefone não está confirmada |
| D-09 | RF-03: o box fica indisponível enquanto houver OS em execução nele | A classe tem `designarBox` e `ocupar` separados; a tela mostra os boxes em execução desabilitados | A designação não ocupa o box; `iniciar` ocupa e `concluir` libera (`OP-05` a `OP-08`) |
| D-10 | RF-02 e RF-06: cada mudança de status grava o novo status, o funcionário e a data e hora | As classes de histórico têm `statusAnterior` e `statusNovo`, e o banco guarda só o novo status (`oshi_status`, `orhi_status`) | `INV-13` e `INV-14` exigem o status novo, e o anterior pode ser vazio na criação. Confirmar quando o DDL for incluído |
| D-11 | Lacuna L-16: o glossário e o banco têm Pagamento (`pagamento` e `pagamento_parcelas`) | Nenhum requisito e nenhuma classe trata dele | Sem restrição OCL. Nada a escrever até existir requisito |

Veja também: [Divergências de nomes (M2)](divergencias-m2.md) · [Classes](../classes/README.md) · [Casos de uso](../casos-de-uso/README.md) · [Estados](../estados/README.md) · [Sequência](../sequencia/README.md) · [Requisitos funcionais](../../03-requisitos/requisitos-funcionais.md) · [Glossário](../../01-visao/glossario.md) · [Lacunas](../../07-rastreabilidade/lacunas.md)
