# Mapa do site

Fábrica de Software I · Protótipo do sistema · Milestones 2 e 3

O mapa do site organiza as telas do FilaCidadã por área, a partir do perfil de acesso de cada ator. Ele descreve o que cada perfil enxerga e a qual caso de uso cada tela responde. Os IDs das telas seguem a convenção `T-<perfil>-NN` definida no [README do protótipo](README.md).

## Níveis da hierarquia

| Nível | Elemento | Exemplo |
|---|---|---|
| Raiz | Sistema | FilaCidadã |
| 1 | Área, uma por perfil de acesso | Cidadão (CID) |
| 2 | Seção, agrupamento de telas da área | Filas e emissão |
| 3 | Tela, com ID `T-<perfil>-NN` | `T-CID-02` Fila e emissão de senha online |

## Hierarquia

- FilaCidadã
  - Público (PUB)
    - Entrada: `T-PUB-01` Página inicial
    - Acesso: `T-PUB-02` Login, `T-PUB-03` Cadastro, `T-PUB-04` Recuperar senha, `T-PUB-05` Sessão expirada
    - Acompanhamento: `T-PUB-06` Acompanhar senha por código
  - Cidadão (CID)
    - Filas e emissão: `T-CID-01` Filas disponíveis, `T-CID-02` Fila e emissão online, `T-CID-03` Entrada por QR Code
    - Senhas: `T-CID-04` Minha senha, `T-CID-05` Minhas senhas, `T-CID-06` Chat da senha
    - Conta e avisos: `T-CID-07` Notificações, `T-CID-09` Meu perfil
    - Instituições: `T-CID-08` Solicitar cadastro de instituição
  - Operador (OPE)
    - Painel: `T-OPE-01` Painel do operador, `T-OPE-02` Fila em atendimento
    - Atendimento: `T-OPE-03` Detalhe da senha, `T-OPE-04` Finalizar ou cancelar, `T-OPE-06` Fluxo da senha, `T-OPE-07` Chat da senha
    - Emissão presencial: `T-OPE-05` Emitir senha presencial
  - Administrador da Instituição (ADI)
    - Painel: `T-ADI-01` Painel da instituição e relatório
    - Filas: `T-ADI-02` Lista de filas, `T-ADI-03` Formulário de fila
    - Etapas e fluxos: `T-ADI-04` Tipos de etapa, `T-ADI-05` Lista de fluxos, `T-ADI-06` Editor de fluxo
    - Instituição: `T-ADI-07` Dados da instituição
    - Usuários: `T-ADI-08` Lista de usuários, `T-ADI-09` Novo usuário, `T-ADI-10` Detalhe do usuário e papéis
    - QR Codes: `T-ADI-11` QR Codes da instituição
    - Auditoria: `T-ADI-12` Auditoria da instituição
  - Administrador da Plataforma (ADP)
    - Painel: `T-ADP-01` Painel da plataforma
    - Solicitações: `T-ADP-02` Solicitações de instituição, `T-ADP-03` Análise da solicitação
    - Instituições: `T-ADP-04` Lista de instituições, `T-ADP-05` Detalhe e edição da instituição
    - Usuários: `T-ADP-06` Lista de usuários da plataforma, `T-ADP-07` Detalhe do usuário e papéis
    - Auditoria: `T-ADP-08` Auditoria da plataforma

Telas de Notificações (`T-CID-07`) e Meu perfil (`T-CID-09`) são desenhadas uma vez, na área do cidadão, e valem para todo perfil autenticado.

![Diagrama da hierarquia do site](site-map.svg)

## Perfil de acesso por área

Cada célula indica se o perfil da coluna acessa a área. "Parcial" indica que só algumas telas da área são acessíveis, e a observação diz quais.

| Área | Público (sem sessão) | Cidadão | Operador | Administrador da Instituição | Administrador da Plataforma |
|---|---|---|---|---|---|
| Público (PUB) | sim | sim | sim | sim | sim |
| Cidadão (CID) | não | sim | parcial (CID-07, CID-08, CID-09) | parcial (CID-07, CID-08, CID-09) | parcial (CID-07, CID-08, CID-09) |
| Operador (OPE) | não | não | sim | sim (herda o acesso do operador) | não (sem ações de atendimento) |
| Administrador da Instituição (ADI) | não | não | parcial (ADI-01, ADI-02, ADI-03, ADI-05 leitura, ADI-11) | sim | parcial (ADI-01, ADI-02 leitura, ADI-04) |
| Administrador da Plataforma (ADP) | não | não | não | não | sim |

Fonte: `authorize(...)` em `src/routes/*.js` e as regras de escopo nos services. Quando a tabela divergir do código, prevalece o código.

Regras de acesso observadas no código e refletidas nas telas:

- Emissão de senha online (`POST /filas/:filaId/senhas`) é exclusiva do cidadão (`USUARIO_FINAL`).
- Chamar, rechamar e finalizar senha são permitidos a operador e administrador da instituição. O administrador da plataforma não executa essas ações.
- Assumir senha é exclusivo do administrador da instituição.
- Cancelar senha não usa `authorize`. A permissão é verificada no service: o dono da senha, o administrador da instituição da senha ou o administrador da plataforma podem cancelar, e só com status AGUARDANDO.
- Consultar uma senha por ID (`GET /senhas/:id`) é permitido ao dono, ao administrador da instituição da senha e ao administrador da plataforma.

## Tabela tela para US

Cada tela aponta para as histórias de [historias-de-usuario.md](../03-requisitos/historias-de-usuario.md) que ela realiza. Onde a célula indica lacuna, nenhuma história cobre a tela, e a pendência PN-01 do README registra o que falta.

| ID | Tela | UC | US | Acesso |
|---|---|---|---|---|
| `T-PUB-01` | Página inicial (landing page) | — | — | Público |
| `T-PUB-02` | Login | UC-01 | US-002 | Público |
| `T-PUB-03` | Cadastro de cidadão | UC-01 | US-001 | Público |
| `T-PUB-04` | Recuperar senha | UC-01 | US-003 | Público |
| `T-PUB-05` | Sessão expirada (código 498) | UC-01 | US-002 | Público (estado de erro) |
| `T-PUB-06` | Acompanhar senha por código | UC-15 | US-039 | Público |
| `T-CID-01` | Filas disponíveis | UC-08 | US-020 | Cidadão |
| `T-CID-02` | Fila e emissão de senha online | UC-08 | US-020, US-021 | Cidadão |
| `T-CID-03` | Entrada por QR Code | UC-08 | US-022, US-033 | Cidadão |
| `T-CID-04` | Minha senha: status, posição e cancelamento | UC-15 | US-038, US-029 | Cidadão (dono da senha) |
| `T-CID-05` | Minhas senhas | UC-15 | US-038 | Cidadão |
| `T-CID-06` | Chat da senha (cidadão) | UC-15 | US-040 | Cidadão (dono da senha) |
| `T-CID-07` | Notificações | UC-15 | US-041 | Qualquer autenticado |
| `T-CID-08` | Solicitar cadastro de instituição | UC-03 | US-007 | Qualquer autenticado |
| `T-CID-09` | Meu perfil | UC-02 | US-004, US-005, US-006 | Qualquer autenticado |
| `T-OPE-01` | Painel do operador (meu atendimento) | UC-10 | US-024, US-026 | Operador, Admin Instituição |
| `T-OPE-02` | Fila em atendimento (senhas aguardando) | UC-10 | US-024, US-025 | Operador, Admin Instituição |
| `T-OPE-03` | Detalhe da senha: chamar, rechamar e assumir | UC-10 | US-026, US-027 | Operador, Admin Instituição |
| `T-OPE-04` | Finalizar ou cancelar atendimento | UC-11 | US-028, US-029 | Operador, Admin Instituição |
| `T-OPE-05` | Emitir senha presencial | UC-09 | US-023 | Operador, Admin Instituição |
| `T-OPE-06` | Fluxo da senha: ingressar, encaminhar e dar alta | UC-10, UC-11 | US-030 | Operador, Admin Instituição |
| `T-OPE-07` | Chat da senha (operador) | UC-10 | US-040 (parcial: história só do cidadão) | Operador, Admin Instituição |
| `T-ADI-01` | Painel da instituição e relatório | UC-13 | US-035, US-036 | Admin Instituição, Operador (leitura) |
| `T-ADI-02` | Lista de filas | UC-06 | US-016 | Admin Instituição, Operador |
| `T-ADI-03` | Formulário de fila (criar, editar e excluir) | UC-06 | US-015, US-016, US-017 | Admin Instituição, Operador (criar e editar) |
| `T-ADI-04` | Tipos de etapa da instituição | UC-07 | US-018, US-019 | Admin Instituição |
| `T-ADI-05` | Lista de fluxos | UC-07 | sem US (lacuna) | Admin Instituição, Operador (leitura) |
| `T-ADI-06` | Editor de fluxo | UC-07 | sem US (lacuna); US-030 cobre só a operação do flow | Admin Instituição |
| `T-ADI-07` | Dados da instituição | UC-03 | sem US (lacuna) | Admin Instituição |
| `T-ADI-08` | Lista de usuários da instituição | UC-05 | US-011 | Admin Instituição |
| `T-ADI-09` | Novo usuário da instituição | UC-05 | US-012 | Admin Instituição |
| `T-ADI-10` | Detalhe do usuário e papéis | UC-05 | US-013, US-014 | Admin Instituição |
| `T-ADI-11` | QR Codes da instituição | UC-12 | US-031, US-032 | Admin Instituição, Operador (sem leitura de detalhe e exclusão) |
| `T-ADI-12` | Auditoria da instituição | UC-14 | US-037 | Admin Instituição |
| `T-ADP-01` | Painel da plataforma | UC-13 | US-034 | Admin Plataforma |
| `T-ADP-02` | Solicitações de instituição | UC-04 | US-008 | Admin Plataforma |
| `T-ADP-03` | Análise da solicitação: aprovar, rejeitar, reconsiderar e contrato | UC-04 | US-008, US-009, US-010 | Admin Plataforma |
| `T-ADP-04` | Lista de instituições | UC-04 | US-008 (parcial: lista sem história própria) | Admin Plataforma |
| `T-ADP-05` | Detalhe e edição da instituição | UC-04 | sem US (lacuna) | Admin Plataforma (PATCH também Admin Instituição) |
| `T-ADP-06` | Lista de usuários da plataforma | UC-05 | US-011 (parcial: ator é o administrador da instituição) | Admin Plataforma |
| `T-ADP-07` | Detalhe do usuário e papéis | UC-05 | US-013 (parcial: ator é o administrador da instituição) | Admin Plataforma |
| `T-ADP-08` | Auditoria da plataforma | UC-14 | US-037 (parcial: ator é o administrador da instituição) | Admin Plataforma |

## Veja também

- [Protótipo: índice e inventário de telas](README.md)
- [Diagrama do mapa do site](site-map.svg)
- [IFML: emissão e chamado de senha](ifml/README.md)
- [Historias de usuario](../03-requisitos/historias-de-usuario.md)
- [Casos de uso](../05-modelagem/casos-de-uso/README.md)
