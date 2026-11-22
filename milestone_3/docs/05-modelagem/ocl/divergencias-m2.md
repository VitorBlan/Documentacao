# Divergências de nomes: OCL, glossário e diagrama de classes

Milestone 2 · Unidade: Engenharia de Software (Fábrica de Software I) · Modelagem do domínio

Esta tabela compara os nomes usados nas [restrições OCL](README.md) com os nomes do glossário e dos requisitos, que citam as tabelas e colunas do banco do Milestone 2, e com o catálogo de classes definido em [classes/README.md](../classes/README.md): classes em português, no singular. Esta tabela não foi conferida com o script do banco; por isso os nomes de coluna vêm só do [glossário](../../01-visao/glossario.md) e de [requisitos-funcionais.md](../../03-requisitos/requisitos-funcionais.md), e os que não aparecem nesses documentos não foram conferidos.

| # | Nome no OCL e nas classes | Nome no glossário e no banco | Razão |
|---|---|---|---|
| N-01 | `Cliente` (herda `Pessoa`) | Tabela `pessoa`, ligada ao veículo por `veiculo.pess_id` | No banco, o cliente é uma pessoa que tem veículo. Mantidos `Pessoa` e `Cliente`, como no diagrama |
| N-02 | `Funcionario.matricula` e `senha` | `funcionario.func_login` e `func_senha` | A classe e a tela usam "Matrícula", e o banco usa "login". Lacuna L-15: escolher um termo e corrigir classe, tela ou banco |
| N-03 | `Peca.tipo` | `peca_categoria`, e "Categoria" nas telas | Lacuna L-15. A OCL usa o nome da classe; o filtro de peças da US-023 fala em categoria |
| N-04 | `Peca.codigo`, `nome`, `estoque`, `estoqueMinimo`, `preco` | `peca_codigo`, `peca_nome`, `peca_estoque`, `peca_estoque_minimo`, `peca_preco` | Mesmo sentido; a história chama o preço de "valor unitário" |
| N-05 | `Orcamento.valorTotal`, `quilometragem`, `status` | `orca_valor`, `orca_quilometragem`, `orca_status` | Mesmo sentido. A coluna `orca_formato` (meio da resposta do cliente) não tem atributo na classe (lacuna L-07) |
| N-06 | `ItemOrcamentoPeca` com `quantidade` e `valorUnitario` | Tabela `orcamento_peca`, colunas `orpe_quantidade` e `orpe_preco_unitario` | Mesmo sentido |
| N-07 | `ItemOrcamentoServico` com `quantidade` e `valorUnitario` | Tabela `orcamento_servico`, coluna `orse_valor`; a RF-02 não cita quantidade | A classe tem quantidade e valor unitário, e o requisito cita só o valor. Confirmar com o banco; `INV-06` usa a quantidade da classe |
| N-08 | `OrdemServico.dataAbertura`, `dataConclusao`, `mecanico`, `box` | `OS_CRIADO_EM`, `os_fim`, `meca_id`, `box_id`; `os_inicio` sem atributo | O início da execução não tem atributo na classe (lacuna L-07). "Mecânico" é o `Funcionario` designado, e não uma classe |
| N-09 | Literais sem acento: `orcado`, `emExecucao`, `concluida` | Valores com acento: `orçado`, `em execução`, `concluída` | Identificador OCL não leva acento nem espaço. Os valores gravados são os do glossário. `StatusBox` (`livre` e `ocupado`) ainda precisa de confirmação |
| N-10 | `Compatibilidade` e `PecaDistribuidora` | `peca_modelo` e `peca_distribuidora` | Nomes de classe em português, com o termo do negócio; o banco usa o par de tabelas ligadas |
| N-11 | `HistoricoStatusOS` e `HistoricoStatusOrcamento`, com `statusAnterior` e `statusNovo` | `ordem_servico_historico` e `orcamento_historico`, com `oshi_status` e `orhi_status` | As classes guardam o status anterior e o novo, e o banco só o novo. `HistoricoStatusOrcamento` está só no diagrama de apoio (lacuna L-06) |
| N-12 | `Funcionario` e a associação `funcao` | Tabelas `funcionario` e `funcao` | Secretária e Mecânico são valores de `Funcao` e atores (ACT-01 e ACT-02), e não classes. O Estoquista e o Gerente aparecem nos diagramas, mas não no catálogo de atores (lacuna L-14) |
| N-13 | Participantes `:OS`, `:Orçamento` e `:Cliente` nos diagramas de sequência | Classes `OrdemServico`, `Orcamento` e `Cliente` | Os diagramas de sequência usam nomes abreviados ou com acento. Ver a divergência 11 de [sequência](../sequencia/README.md#divergências) |
| N-14 | Sem classe: `Pagamento` | Tabelas `pagamento` e `pagamento_parcelas` | Nenhum requisito trata do pagamento (lacuna L-16). Nenhuma restrição OCL foi escrita |

Sem divergência de nome: `Veiculo`, `CarroMarca`, `CarroModelo`, `Servico`, `Funcao`, `Box` e `Distribuidora` têm o mesmo nome (em minúsculas, com sublinhado) no glossário e no catálogo de classes.

Veja também: [Restrições OCL](README.md) · [Classes](../classes/README.md) · [Glossário](../../01-visao/glossario.md) · [Lacunas](../../07-rastreabilidade/lacunas.md) · [Convenções de identificação e nomes](../../README.md#convenções-de-identificação)
