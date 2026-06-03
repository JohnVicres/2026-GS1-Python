# Sistema de Gerenciamento de Naves Sucateiras

Integrantes:

- **Nome** – RM XXXXXX
- **Nome** – RM XXXXXX
- **Nome** – RM XXXXXX

Este projeto implementa um **sistema de gerenciamento de naves sucateiras** baseado em filas de tarefas priorizadas e controle de status de frota, utilizando:

- **Classes orientadas a objetos** para modelar naves e tarefas;
- **Ordenação por prioridade** na fila de tarefas;
- **Controle de histórico** para monitorar o uso de cada nave;
- **Menu interativo recursivo** para navegação entre as operações do sistema.

O código foi desenvolvido em função do tema da **Global Solution** e atende ao enunciado da disciplina, incluindo:

- Formulação do problema (entradas, saídas, objetivo);
- Estrutura de classes com encapsulamento e métodos de atualização;
- Controle de estado de frota com limite de tarefas por nave;
- Fila de tarefas ordenada por prioridade;
- Histórico das tarefas concluídas;
- Menu interativo com submenus e validação de entradas.

---

## 1. Formulação do Problema

### Objetivo

Gerenciar uma frota de **30 naves sucateiras** e uma fila de tarefas priorizadas, garantindo que:

- Cada tarefa seja alocada à próxima nave disponível;
- Naves com muitas missões acumuladas sejam retiradas do expediente;
- O histórico das operações seja mantido para consulta.

### Entradas

O sistema solicita ao operador:

- **Tipo da tarefa**: descrição textual da operação;
- **Prioridade** da tarefa: valor inteiro de 1 a 10.

### Saídas

- **Listagem da frota**, com status de cada nave;
- **Busca por registro** (`regid`) ou por status;
- **Fila de tarefas** ordenada por prioridade;
- **Histórico de tarefas concluídas**, com nave alocada e status final.

---

## 2. Visão Geral da Solução

Fluxo principal:

1. O operador executa `testeSimulado()`;
2. O sistema inicializa automaticamente uma lista com **30 naves** (status padrão: `"atracada"`);
3. O sistema exibe um **menu recursivo** com opções:
   - (1) Consultar naves;
   - (2) Operar com tarefas;
   - (3) Terminar teste.
4. Ao processar uma tarefa, o sistema:
   - Busca a próxima nave disponível via `processamentoListaNaves`;
   - Aloca a nave à tarefa e atualiza seu status para `"no expediente"`;
   - Após 5 tarefas concluídas, marca a nave como `"fora do expediente"`;
   - Registra a tarefa concluída no histórico via `updateHistoricoTarefas`.

---

## 3. Descrição Resumida das Estruturas

### `naveSucateira`

- Representa uma nave da frota com `naveregid` (identificador numérico aleatório entre 1000 e 9999) e `navestatus`.
- Métodos: `updateNaveStatus`, `printNave`.

### `tarefa`

- Representa uma tarefa com `tarefatipo`, `tarefaprio`, `naveregid` designada e `tarefastatus`.
- Métodos: `updateTarefaStatus`, `updateTarefaNave`, `readTarefa`.

### `testeSimulado()`

Função principal que encapsula toda a lógica do sistema. Contém:

- `criarListaNaves` — inicializa as 30 naves com `regid` aleatórios;
- `listarTodasNaves` — imprime todas as naves da frota;
- `buscarRegid` — localiza uma nave pelo identificador;
- `listarPorStatus` — filtra naves por status (`"atracada"`, `"no expediente"`, `"fora do expediente"`);
- `processamentoListaNaves` — retorna o `regid` da próxima nave disponível;
- `updateListaNaves` — atualiza o status de uma nave conforme o histórico;
- `adicionarTarefa` — coleta entradas do operador e insere tarefa ordenada na fila;
- `resolverTarefa` — aloca uma nave à tarefa de maior prioridade e registra no histórico;
- `lerTarefas` — exibe todas as tarefas pendentes;
- `lerHistoricoTarefas` / `updateHistoricoTarefas` / `checkHistoricoRegid` — gerenciam o histórico de operações.

---

## 4. Controle de Expediente

O sistema acompanha quantas vezes cada nave aparece no histórico de tarefas concluídas:

- Enquanto `checkHistoricoRegid(naveregid) < 5` → nave permanece `"no expediente"`;
- A partir de 5 tarefas registradas → nave é marcada como `"fora do expediente"` e não recebe novas alocações.

Isso garante **rotatividade e controle de desgaste** da frota ao longo das operações.

---

## 5. Como Executar o Projeto

1. Abrir o código em um **Jupyter Notebook** ou **Google Colab**;
2. Garantir que as bibliotecas abaixo estão instaladas:
   - `random` (padrão Python)
   - `time` (padrão Python)
3. Executar a célula com todo o código da solução;
4. Rodar:

```python
testeSimulado()
```

5. Seguir o menu interativo para:
   - consultar a frota de naves;
   - adicionar e resolver tarefas;
   - visualizar o histórico de operações.

---

## 6. Estrutura do Repositório

```
.
├── GS1.ipynb     # Notebook com todo o código da solução
└── README.md     # Visão geral, problema, estruturas e execução
```
