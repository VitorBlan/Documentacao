# Site Map

Fábrica de Software I · Protótipo do sistema · Milestones 2 e 3

O site map organiza as telas do Bancada, o sistema de gestão de oficina mecânica da RFP 2, em três níveis: a Home, as áreas de cada perfil de acesso e as telas de cada área. Ele indica quem acessa cada área e qual história de usuário justifica cada tela. O diagrama foi desenhado na página "01 · Site Map" do arquivo do Figma "[Gestão de Oficina Mecânica](https://www.figma.com/design/EHj0Js5KtOJ88TRQd5fTJI/Gest%C3%A3o-de-Oficina-Mec%C3%A2nica?node-id=1108-3313)" e exportado em [site-map.svg](site-map.svg).

![Site Map do Bancada](site-map.svg)

## Níveis da hierarquia

| Nível | Elemento | Exemplo |
|---|---|---|
| 1 | Home: ponto de entrada após o login | HOME |
| 2 | Área: item do menu lateral, com o perfil que a acessa | Clientes (Secretária) |
| 3 | Tela: página da área, nomeada pela ação que a secretária, o estoquista ou o gerente realiza | Cadastrar cliente |

O Login não aparece como nó: ele é o estado inicial da Home para quem ainda não está autenticado (ver [IFML-01](ifml/README.md#ifml-01-acesso-e-painel)). As ações de confirmação e de exclusão são estados ou janelas dentro das telas do nível 3, e não nós próprios.

## Perfis de acesso

| Perfil | Cor no diagrama | Áreas | Situação das telas |
|---|---|---|---|
| Secretária | Verde | Painel, Clientes, Carros, Orçamentos, OS e Agenda dos Box | Todas as áreas têm wireframes |
| Estoquista | Roxo | Painel, Peças e Estoque e Distribuidora | Peças e Estoque e Distribuidora têm wireframes; o Painel do estoquista não tem |
| Gerente | Laranja | Relatórios | Uma tela desenhada (lista de relatórios) |

No Milestone 1, a secretária era o único perfil com login (ver [atores](../03-requisitos/README.md#atores)), e o estoquista e o gerente apareciam como perfis futuros. O site map atual distribui as áreas entre os três perfis. As telas e os modelos IFML concentram-se na secretária, porque é ela quem mais usa o sistema no dia a dia da oficina.

## Hierarquia

- HOME
  - **Secretária**
    - Painel
    - Clientes: Cadastrar cliente · Atualizar informações do cliente · Ver informações do cliente · Listar clientes cadastrados
    - Carros: Cadastrar carro · Atualizar informações do carro · Ver informações do carro · Listar carros cadastrados · Consultar histórico do veículo
    - Orçamentos: Criar orçamento · Atualizar informações do orçamento · Ver informações do orçamento · Listar orçamentos cadastrados
    - OS: Criar OS · Atualizar informações da OS · Ver informações da OS · Listar OS cadastradas
    - Agenda dos Box: Lista de box · Finalizar serviço do box
  - **Estoquista**
    - Painel
    - Peças e Estoque: Cadastrar peça · Atualizar informações da peça · Ver informações da peça · Listar peças cadastradas
    - Distribuidora: Cadastrar distribuidora · Atualizar informações da distribuidora · Ver informações da distribuidora · Listar distribuidoras cadastradas
  - **Gerente**
    - Relatórios: Lista de relatórios · Ver informações de um relatório

Total: 10 áreas (6 da secretária, 3 do estoquista e 1 do gerente) e 29 telas de nível 3. Os dois Painéis não têm filhos: a área é a própria tela.

## Tela, história e wireframe

Cada linha liga um nó do site map aos wireframes que o desenham, às histórias de [historias-de-usuario.md](../03-requisitos/historias-de-usuario.md) que o justificam e ao modelo IFML que descreve a navegação. Os números dos wireframes seguem o [índice das telas](wireframes/indice-das-telas.md). Quando uma célula indica lacuna, a pendência correspondente está no [README do protótipo](README.md#pendências).

### Secretária

| Área | Tela (nível 3) | Wireframes | US | IFML |
|---|---|---|---|---|
| (Home) | Login, estado inicial | T-PUB-01 | US-001 | IFML-01 |
| Painel | Painel (tela da própria área) | T-SEC-01 | Sem US própria; o bloco "Prontos para retirada" atende parcialmente a US-002 | IFML-01 |
| Clientes | Listar clientes cadastrados | T-SEC-02, T-SEC-06 | US-010, US-005 (busca) | IFML-02 |
| Clientes | Cadastrar cliente | T-SEC-03, T-SEC-04, T-SEC-05 | US-003 | IFML-02 |
| Clientes | Ver informações do cliente | T-SEC-07 | US-013 | IFML-02 |
| Clientes | Atualizar informações do cliente | T-SEC-08, T-SEC-09, T-SEC-10 | US-004 | IFML-02 |
| Carros | Listar carros cadastrados | T-SEC-11, T-SEC-14 | US-011 | IFML-03 |
| Carros | Cadastrar carro | T-SEC-12, T-SEC-13 | US-006 | IFML-03 |
| Carros | Ver informações do carro | T-SEC-15 | US-014 | IFML-03 |
| Carros | Atualizar informações do carro | T-SEC-16 | US-007 | IFML-03 |
| Carros | Consultar histórico do veículo | T-SEC-17 a T-SEC-21 | US-029, US-030 | IFML-06 |
| Orçamentos | Listar orçamentos cadastrados | T-SEC-22, T-SEC-23, T-SEC-27, T-SEC-34 a T-SEC-43 | US-018, US-026 | IFML-05 |
| Orçamentos | Criar orçamento | T-SEC-24, T-SEC-25, T-SEC-26 | US-016 | IFML-05 |
| Orçamentos | Ver informações do orçamento | T-SEC-28, T-SEC-33 | US-016 | IFML-05 |
| Orçamentos | Atualizar informações do orçamento | T-SEC-29, T-SEC-30, T-SEC-31, T-SEC-32 | US-016, US-017 | IFML-05 |
| OS | Listar OS cadastradas | T-SEC-44 a T-SEC-47, T-SEC-50, T-SEC-51 | US-012 | IFML-04 |
| OS | Criar OS | Sem formulário próprio: a OS é aberta pela ação "Iniciar execução" do orçamento aprovado (T-SEC-33) | US-008 | IFML-04 |
| OS | Ver informações da OS | T-SEC-48, T-SEC-52 | US-015, US-027 | IFML-04 |
| OS | Atualizar informações da OS | T-SEC-48, T-SEC-49 (finalizar a OS) | US-009, US-028 | IFML-04 |
| Agenda dos Box | Lista de box | T-SEC-53, T-SEC-55 | #42 | Sem modelo |
| Agenda dos Box | Finalizar serviço do box | T-SEC-54, T-SEC-55 | #42 | Sem modelo |
| Agenda dos Box | Designar box e mecânico (fora do site map, ver SM-02) | T-SEC-56, T-SEC-57 | #42 | Sem modelo |

### Estoquista

| Área | Tela (nível 3) | Wireframes | US | IFML |
|---|---|---|---|---|
| Painel | Painel do estoquista (tela da própria área) | Sem wireframe | Sem US (lacuna) | Sem modelo |
| Peças e Estoque | Listar peças cadastradas | T-EST-01, T-EST-02, T-EST-03 | US-023 | IFML-07 |
| Peças e Estoque | Cadastrar peça | T-EST-04 (a redesenhar), T-EST-05 | US-019 | IFML-07 |
| Peças e Estoque | Ver informações da peça | T-EST-06; exclusão em T-EST-09 e T-EST-10 | US-020, US-022 | IFML-07 |
| Peças e Estoque | Atualizar informações da peça | T-EST-07, T-EST-08 | US-021 | IFML-07 |
| Peças e Estoque | Alertas de estoque (fora do site map, ver SM-03) | T-EST-11 | US-024 | IFML-07 |
| Distribuidora | Listar distribuidoras cadastradas | T-EST-12 | Sem US (lacuna) | Sem modelo |
| Distribuidora | Cadastrar distribuidora | T-EST-13, T-EST-14, T-EST-15 | Sem US (lacuna) | Sem modelo |
| Distribuidora | Ver informações da distribuidora | T-EST-16 | Sem US (lacuna) | Sem modelo |
| Distribuidora | Atualizar informações da distribuidora | T-EST-17 | Sem US (lacuna) | Sem modelo |

Os wireframes T-EST-18 a T-EST-30 são versões anteriores das telas de peças (a versão por marca e modelo de carro e a primeira versão da RF-05). Eles ficam na pasta como registro do redesenho e não fazem parte do site map.

### Gerente

| Área | Tela (nível 3) | Wireframes | US | IFML |
|---|---|---|---|---|
| Relatórios | Lista de relatórios | T-GER-01 | Sem US; o RF-07 e o UC-023 cobrem o relatório da OS | Sem modelo |
| Relatórios | Ver informações de um relatório | Sem wireframe | Sem US (lacuna) | Sem modelo |

## Divergências entre o site map e os wireframes

| ID | Divergência | Tratamento proposto |
|---|---|---|
| SM-01 | O Painel do estoquista e a tela "Ver informações de um relatório" estão no site map, mas não têm wireframe. | Desenhar as duas telas ou marcá-las no diagrama como telas previstas. |
| SM-02 | A tela "Designar box e mecânico" (T-SEC-56 e T-SEC-57) existe nos wireframes, mas não aparece na área Agenda dos Box. | Incluir o nó "Designar box e mecânico" na área Agenda dos Box. |
| SM-03 | A tela de alertas de estoque (T-EST-11, US-024) não tem nó próprio. | Incluir o nó "Ver alertas de estoque" em Peças e Estoque, ou ligar o alerta ao Painel do estoquista. |
| SM-04 | A área Distribuidora (T-EST-12 a T-EST-17) não tem história de usuário. Pelo material da disciplina, tela sem história é escopo sem justificativa. | Registrar a história no GitLab e substituir "US-xxx" nos nomes dos quadros do Figma. |
| SM-05 | Nos wireframes, a área de relatórios (T-GER-01) mostra o menu completo da secretária, embora o site map a reserve ao gerente. Da mesma forma, o cadastro de peça (T-EST-04) mostra o menu da secretária para o usuário do almoxarifado. | Desenhar o menu lateral de cada perfil apenas com as áreas que o site map lhe atribui. |
| SM-06 | No diagrama, a Agenda dos Box e a Distribuidora estão no singular, e o último nó do gerente diz "Ver informações em um relatórios". | Corrigir os rótulos no Figma: "Agenda dos boxes", "Distribuidoras" e "Ver informações de um relatório". |

## Veja também

- [Protótipo: índice da pasta](README.md)
- [IFML: fluxos de interação](ifml/README.md)
- [Wireframes](wireframes/README.md) e [índice das telas](wireframes/indice-das-telas.md)
- [Histórias de usuário](../03-requisitos/historias-de-usuario.md)
- [Matriz de rastreabilidade](../07-rastreabilidade/matriz.md)
