# Sistema de Gerenciamento de Naves Sucateiras

Autor:

- **João Vicente** – RM 556561

Este projeto implementa um **sistema de gerenciamento de naves sucateiras** baseado em filas de tarefas priorizadas e controle de status de frota, utilizando:

- **Classes orientadas a objetos** para modelar naves e tarefas;
- **Ordenação por prioridade** na fila de tarefas;
- **Controle de histórico** para monitorar o uso de cada nave;
- **Menu interativo recursivo** para navegação entre as operações do sistema.

## Justificativa do Projeto

Considerei a importância do acúmulo de lixo orbital no passar do tempo, especialmente para um aumento de exploração econômica exoplanetária, como algo importante a se ter em mente. O [Efeito Kessler](https://pt.wikipedia.org/wiki/S%C3%ADndrome_de_Kessler) é uma bomba-relógio para um aumento da exploração espacial além do que é feito hoje (manutenção de satélites de transmissão/observação, vôos orbitais/suborbitais, e missões científicas), e embora muitos avanços tenham sido atingidos para diminuição do problema, ele continua sendo "acumulado". Esse trabalho é baseado numa [série de ficção científica](https://pt.wikipedia.org/wiki/Planetes), onde astronautas contratados são responsáveis por lidar com lixo espacial a bordo de pequenas naves-engenheiras.

---

## 1. Formulação do Problema

### Objetivo

Gerenciar uma frota de **30 naves sucateiras** que atendem uma **fila de tarefas** priorizadas, garantindo que:

- Tarefas sejam alocadas para naves disponíveis;
- Naves tenham um expediente com limite de missões cumpridas;
- O histórico das operações seja mantido para consulta.

### Entradas

O sistema solicita a inserção de tarefas pelo operador:

- **Tipo da tarefa**: descrição textual da operação;
- **Prioridade** da tarefa: valor inteiro de 1 a 10.

### Saídas

#### Naves sucateiras
- **Listagem total da frota**, com dados de cada nave;
- **Busca específica** por registro único (`regid`) ou por status (dentro ou fora do expediente);
#### Tarefas
- **Listagem total de tarefas** ordenada por prioridade;
#### Histórico
- **Listagen de Histórico de tarefas concluídas**, incluindo `regid` da nave alocada e status de conclusão.

---

## 2. Visão Geral da Solução

Fluxo principal:

1. O programa inicia com `testeSimulado()`;
2. O sistema gera automaticamente uma lista com **30 naves** (status padrão: `"atracada"`, ou seja, antes de começar o expediente);
3. O sistema exibe um **menu recursivo** com opções:
   - (1) Consultar naves;
   - (2) Operar com tarefas/histórico;
   - (3) Terminar teste.
4. Durante o processamento de uma tarefa, o sistema:
   - Busca a próxima nave disponível via `processamentoListaNaves`;
   - Aloca essa nave à tarefa por `regid` e atualiza seu status para `"no expediente"`;
   - Após 5 tarefas concluídas, marca a nave como `"fora do expediente"`;
   - Registra a tarefa como concluída e anexa ao histórico via `updateHistoricoTarefas`.

---

## 3. Descrição Resumida das Estruturas

### Classe `NaveSucateira`

- Representa uma nave da frota com `naveregid` (identificador numérico aleatório entre 1000 e 9999) e `navestatus`.
- Métodos: `updateNaveStatus`, `printNave`.

### Classe `Tarefa`

- Representa uma tarefa com `tarefatipo`, `tarefaprio`, `naveregid` de uma nave alocada e `tarefastatus`.
- Métodos: `updateTarefaStatus`, `updateTarefaNave`, `readTarefa`.

### `testeSimulado()`

Função principal que encapsula toda a lógica do sistema. Contém:

- `criarListaNaves` — inicializa as 30 naves com `regid` aleatórios;
- `listarTodasNaves` — imprime todas as naves da frota;
- `buscarRegid` — localiza uma nave pelo identificador;
- `listarPorStatus` — filtra naves por status (`"atracada"`, `"no expediente"`, `"fora do expediente"`);
- `processamentoListaNaves` — retorna o `regid` da próxima nave disponível;
- `updateListaNaves` — atualiza o status de uma nave conforme sua presença no histórico;
- `adicionarTarefa` — coleta entradas do operador, insere uma tarefa na fila, e reordena a fila;
- `resolverTarefa` — aloca uma nave à tarefa de maior prioridade e registra no histórico;
- `lerTarefas` — exibe todas as tarefas pendentes;
- `lerHistoricoTarefas` — exibe todo o histórico;
- `updateHistoricoTarefas` / `checkHistoricoRegid` — gerenciam o histórico.

---

## 4. Controle de Expediente

O sistema acompanha quantas vezes cada nave aparece no histórico de tarefas concluídas:

- Enquanto `checkHistoricoRegid(naveregid) < 5` → nave permanece `"no expediente"`;
- A partir de 5 tarefas registradas → nave é marcada como `"fora do expediente"` e não recebe novas alocações até o histórico remover suas tarefas mais antigas.

Isso garante **rotatividade e controle de desgaste** da frota ao longo das operações.

---
