# Task Tracker CLI

Aplicação de linha de comando (CLI) para criar, atualizar, excluir e
acompanhar tarefas pessoais.

Este projeto é baseado no desafio **Task Tracker** do
[roadmap.sh](https://roadmap.sh/projects/task-tracker). O objetivo é
praticar fundamentos de desenvolvimento de software, especialmente
entrada de dados via linha de comando, manipulação do sistema de
arquivos, persistência em JSON, validação e organização de código.

> **Nível sugerido pelo roadmap.sh:** iniciante / CLI / Backend.

------------------------------------------------------------------------

## 1. Objetivo do projeto

Construir uma aplicação executada diretamente pelo terminal que permita
gerenciar uma lista de tarefas.

A aplicação deverá permitir:

-   adicionar tarefas;
-   atualizar tarefas existentes;
-   excluir tarefas;
-   marcar tarefas como `in-progress`;
-   marcar tarefas como `done`;
-   listar todas as tarefas;
-   listar somente tarefas concluídas;
-   listar somente tarefas pendentes;
-   listar somente tarefas em andamento.

As tarefas deverão ser persistidas em um arquivo JSON localizado no
diretório atual do projeto.

A aplicação deve funcionar mesmo quando o arquivo JSON ainda não
existir: nesse caso, o arquivo deverá ser criado automaticamente.

------------------------------------------------------------------------

## 2. Requisitos oficiais traduzidos

O projeto original estabelece os seguintes requisitos:

### 2.1 Interface de linha de comando

A aplicação deve ser executada pelo terminal e receber comandos e dados
por argumentos da linha de comando.

Os argumentos devem ser **posicionais**.

Exemplos:

``` bash
task-cli add "Comprar leite"
task-cli update 1 "Comprar leite e pão"
task-cli delete 1
```

------------------------------------------------------------------------

### 2.2 Persistência em JSON

As tarefas devem ser armazenadas em um arquivo JSON no diretório atual.

Exemplo:

``` text
tasks.json
```

Se o arquivo não existir, a aplicação deverá criá-lo automaticamente.

A aplicação deverá utilizar o módulo nativo de manipulação do sistema de
arquivos da linguagem escolhida.

------------------------------------------------------------------------

### 2.3 Não utilizar bibliotecas externas

O projeto não deve utilizar bibliotecas ou frameworks externos para
implementar a aplicação.

É permitido utilizar somente recursos nativos da linguagem escolhida.

Exemplos:

-   Python: `sys`, `json`, `os`, `pathlib`, `datetime`, etc.
-   JavaScript/Node.js: `process`, `fs`, `path`, `Date`, etc.
-   Go: pacotes da biblioteca padrão.

Não utilizar frameworks ou bibliotecas externas para CLI, persistência
ou banco de dados.

------------------------------------------------------------------------

### 2.4 Tratamento de erros

A aplicação deve tratar erros e casos extremos de forma adequada.

Exemplos:

-   comando inexistente;
-   quantidade incorreta de argumentos;
-   descrição vazia;
-   ID inexistente;
-   ID inválido;
-   arquivo JSON inválido;
-   arquivo sem tarefas;
-   tentativa de atualizar uma tarefa que não existe;
-   tentativa de excluir uma tarefa que não existe;
-   status inválido.

------------------------------------------------------------------------

# 3. Modelo de dados

Cada tarefa deve possuir obrigatoriamente as seguintes propriedades:

  -------------------------------------------------------------------------
  Campo            Tipo                        Obrigatório Descrição
  ---------------- ----------------- --------------------- ----------------
  `id`             inteiro                             Sim Identificador
                                                           único da tarefa

  `description`    string                              Sim Descrição da
                                                           tarefa

  `status`         string                              Sim Estado atual da
                                                           tarefa

  `createdAt`      datetime/string                     Sim Data e hora de
                                                           criação

  `updatedAt`      datetime/string                     Sim Data e hora da
                                                           última
                                                           atualização
  -------------------------------------------------------------------------

Os únicos valores permitidos oficialmente para `status` são:

``` text
todo
in-progress
done
```

------------------------------------------------------------------------

## 3.1 Exemplo de uma tarefa

``` json
{
  "id": 1,
  "description": "Comprar leite",
  "status": "todo",
  "createdAt": "2026-09-28T21:00:00",
  "updatedAt": "2026-09-28T21:00:00"
}
```

------------------------------------------------------------------------

## 3.2 Exemplo do arquivo completo

``` json
[
  {
    "id": 1,
    "description": "Comprar leite",
    "status": "todo",
    "createdAt": "2026-09-28T21:00:00",
    "updatedAt": "2026-09-28T21:00:00"
  },
  {
    "id": 2,
    "description": "Estudar Python",
    "status": "in-progress",
    "createdAt": "2026-09-28T18:00:00",
    "updatedAt": "2026-09-28T20:30:00"
  },
  {
    "id": 3,
    "description": "Fazer exercícios",
    "status": "done",
    "createdAt": "2026-09-27T10:00:00",
    "updatedAt": "2026-09-28T19:00:00"
  }
]
```

------------------------------------------------------------------------

# 4. Estados das tarefas

Uma tarefa pode estar em um dos três estados:

``` text
todo
in-progress
done
```

### `todo`

A tarefa foi criada, mas ainda não foi iniciada.

Exemplo:

``` json
{
  "id": 1,
  "description": "Estudar Python",
  "status": "todo"
}
```

### `in-progress`

A tarefa está sendo executada.

Exemplo:

``` json
{
  "id": 1,
  "description": "Estudar Python",
  "status": "in-progress"
}
```

### `done`

A tarefa foi concluída.

Exemplo:

``` json
{
  "id": 1,
  "description": "Estudar Python",
  "status": "done"
}
```

------------------------------------------------------------------------

# 5. Comandos obrigatórios

A aplicação deverá implementar os seguintes comandos.

## 5.1 Adicionar tarefa

Sintaxe:

``` bash
task-cli add "<descrição>"
```

Exemplo:

``` bash
task-cli add "Comprar mantimentos"
```

Resultado esperado:

``` text
Task added successfully (ID: 1)
```

Ao adicionar uma tarefa:

1.  gerar um novo ID;
2.  armazenar a descrição;
3.  definir `status` como `todo`;
4.  definir `createdAt`;
5.  definir `updatedAt`;
6.  salvar a tarefa no JSON.

Exemplo:

``` json
{
  "id": 1,
  "description": "Comprar mantimentos",
  "status": "todo",
  "createdAt": "2026-09-28T21:00:00",
  "updatedAt": "2026-09-28T21:00:00"
}
```

------------------------------------------------------------------------

# 6. Atualizar tarefa

Sintaxe:

``` bash
task-cli update <id> "<nova descrição>"
```

Exemplo:

``` bash
task-cli update 1 "Comprar mantimentos e produtos de limpeza"
```

A atualização deverá:

1.  localizar a tarefa pelo `id`;
2.  substituir a descrição;
3.  manter o `status`;
4.  manter `createdAt`;
5.  atualizar `updatedAt`;
6.  salvar novamente o JSON.

### Importante

Ao atualizar uma tarefa, **não** deve ser alterado o `createdAt`.

Somente `updatedAt` deve ser atualizado entre os timestamps.

------------------------------------------------------------------------

# 7. Excluir tarefa

Sintaxe:

``` bash
task-cli delete <id>
```

Exemplo:

``` bash
task-cli delete 1
```

A tarefa correspondente ao ID deverá ser removida do arquivo JSON.

Exemplo:

Antes:

``` json
[
  {
    "id": 1,
    "description": "Comprar leite",
    "status": "todo"
  },
  {
    "id": 2,
    "description": "Estudar Python",
    "status": "todo"
  }
]
```

Depois:

``` json
[
  {
    "id": 2,
    "description": "Estudar Python",
    "status": "todo"
  }
]
```

O ID excluído não deve permanecer no arquivo.

------------------------------------------------------------------------

# 8. Marcar como em andamento

Sintaxe:

``` bash
task-cli mark-in-progress <id>
```

Exemplo:

``` bash
task-cli mark-in-progress 1
```

A tarefa deverá passar para:

``` text
in-progress
```

O `updatedAt` também deverá ser atualizado.

------------------------------------------------------------------------

# 9. Marcar como concluída

Sintaxe:

``` bash
task-cli mark-done <id>
```

Exemplo:

``` bash
task-cli mark-done 1
```

A tarefa deverá passar para:

``` text
done
```

O `updatedAt` também deverá ser atualizado.

------------------------------------------------------------------------

# 10. Listar todas as tarefas

Sintaxe:

``` bash
task-cli list
```

Esse comando deverá mostrar todas as tarefas existentes,
independentemente do status.

Exemplo:

``` text
[1] Comprar mantimentos - todo
[2] Estudar Python - in-progress
[3] Fazer exercícios - done
```

A forma exata de apresentação fica a critério da implementação.

------------------------------------------------------------------------

# 11. Listar tarefas concluídas

Sintaxe:

``` bash
task-cli list done
```

Deve retornar somente tarefas com:

``` text
status = done
```

------------------------------------------------------------------------

# 12. Listar tarefas pendentes

Sintaxe:

``` bash
task-cli list todo
```

Deve retornar somente tarefas com:

``` text
status = todo
```

O desafio utiliza `todo` para representar as tarefas que ainda não foram
concluídas.

------------------------------------------------------------------------

# 13. Listar tarefas em andamento

Sintaxe:

``` bash
task-cli list in-progress
```

Deve retornar somente tarefas com:

``` text
status = in-progress
```

------------------------------------------------------------------------

# 14. Resumo dos comandos

  Comando              Função
  -------------------- --------------------------
  `add`                Criar tarefa
  `update`             Alterar descrição
  `delete`             Excluir tarefa
  `mark-in-progress`   Marcar como em andamento
  `mark-done`          Marcar como concluída
  `list`               Listar todas
  `list done`          Listar concluídas
  `list todo`          Listar pendentes
  `list in-progress`   Listar em andamento

------------------------------------------------------------------------

# 15. Regras de negócio

## 15.1 ID deve ser único

Cada tarefa deve possuir um ID único.

Exemplo:

``` text
1
2
3
4
```

Duas tarefas não podem possuir o mesmo ID.

Uma estratégia simples é utilizar:

``` text
maior ID existente + 1
```

Se não existir nenhuma tarefa:

``` text
ID = 1
```

------------------------------------------------------------------------

## 15.2 Nova tarefa começa como `todo`

Toda tarefa criada através de:

``` bash
task-cli add
```

deve começar com:

``` text
status = todo
```

------------------------------------------------------------------------

## 15.3 `createdAt`

`createdAt` representa o momento em que a tarefa foi criada.

Esse valor não deve ser alterado posteriormente.

------------------------------------------------------------------------

## 15.4 `updatedAt`

`updatedAt` representa a última alteração feita na tarefa.

Deve ser atualizado quando a tarefa sofrer alterações, por exemplo:

-   descrição modificada;
-   status alterado.

------------------------------------------------------------------------

## 15.5 Descrição obrigatória

Não deve ser possível criar uma tarefa sem descrição.

Exemplo inválido:

``` bash
task-cli add ""
```

A aplicação deve apresentar uma mensagem de erro amigável.

------------------------------------------------------------------------

## 15.6 ID inexistente

Caso seja solicitado:

``` bash
task-cli update 999 "Nova descrição"
```

e a tarefa `999` não exista, a aplicação deve informar que a tarefa não
foi encontrada.

O programa não deve quebrar.

------------------------------------------------------------------------

# 16. Tratamento de erros

O objetivo não é apenas fazer os comandos funcionarem em condições
perfeitas.

A aplicação deve lidar com entradas inválidas.

## 16.1 Comando inexistente

Entrada:

``` bash
task-cli qualquer-coisa
```

Saída sugerida:

``` text
Unknown command: qualquer-coisa
```

------------------------------------------------------------------------

## 16.2 Argumentos insuficientes

Entrada:

``` bash
task-cli add
```

Saída sugerida:

``` text
Error: task description is required.
```

------------------------------------------------------------------------

## 16.3 ID inválido

Entrada:

``` bash
task-cli delete abc
```

Saída sugerida:

``` text
Error: ID must be a valid number.
```

------------------------------------------------------------------------

## 16.4 Tarefa inexistente

Entrada:

``` bash
task-cli delete 999
```

Saída sugerida:

``` text
Error: Task with ID 999 not found.
```

------------------------------------------------------------------------

## 16.5 Status inválido

Entrada:

``` bash
task-cli list finished
```

Saída sugerida:

``` text
Error: invalid status. Use: todo, in-progress or done.
```

------------------------------------------------------------------------

## 16.6 JSON inválido

Caso o arquivo esteja corrompido ou contenha JSON inválido, a aplicação
deve tratar a exceção adequadamente.

Não deve simplesmente apresentar um traceback não tratado ao usuário.

------------------------------------------------------------------------

# 17. Estrutura sugerida do projeto

A estrutura abaixo é uma sugestão. O desafio não exige uma estrutura
específica.

## Python

``` text
task-tracker/
│
├── task-cli.py
├── tasks.json
├── README.md
└── .gitignore
```

Uma estrutura mais organizada:

``` text
task-tracker/
│
├── src/
│   ├── cli.py
│   ├── task_manager.py
│   └── storage.py
│
├── tasks.json
├── README.md
└── .gitignore
```

Para um projeto iniciante, a primeira estrutura é suficiente.

------------------------------------------------------------------------

# 18. Arquitetura sugerida

Mesmo sendo um projeto pequeno, é interessante separar as
responsabilidades.

Uma possível divisão:

``` text
CLI
 │
 ▼
Task Manager
 │
 ├── criar tarefa
 ├── atualizar tarefa
 ├── excluir tarefa
 ├── alterar status
 └── listar tarefas
 │
 ▼
Storage
 │
 └── tasks.json
```

## CLI

Responsável por:

-   receber os argumentos;
-   identificar o comando;
-   validar argumentos básicos;
-   chamar a operação correspondente;
-   mostrar mensagens ao usuário.

## Task Manager

Responsável pela lógica de negócio:

-   criação;
-   atualização;
-   exclusão;
-   alteração de status;
-   filtragem;
-   geração de ID.

## Storage

Responsável por:

-   criar o arquivo;
-   ler JSON;
-   salvar JSON;
-   tratar problemas relacionados ao arquivo.

------------------------------------------------------------------------

# 19. Fluxo de execução

Exemplo:

``` text
Usuário
   │
   │ task-cli add "Estudar Python"
   ▼
CLI
   │
   │ identifica comando "add"
   ▼
Task Manager
   │
   ├── gera ID
   ├── define status = todo
   ├── gera createdAt
   └── gera updatedAt
   │
   ▼
Storage
   │
   └── salva tasks.json
   │
   ▼
CLI
   │
   └── "Task added successfully (ID: 1)"
```

------------------------------------------------------------------------

# 20. Persistência

O arquivo JSON deve ser a fonte de persistência da aplicação.

Não é necessário:

-   banco de dados;
-   SQLite;
-   PostgreSQL;
-   MongoDB;
-   Redis;
-   API;
-   servidor web.

O objetivo do projeto é justamente praticar persistência simples
utilizando o sistema de arquivos.

------------------------------------------------------------------------

# 21. Formato recomendado para datas

O desafio exige data e hora para:

``` text
createdAt
updatedAt
```

Uma opção recomendada é utilizar ISO 8601.

Exemplo:

``` text
2026-09-28T21:30:00
```

Ou, preferencialmente, incluindo informação de timezone:

``` text
2026-09-28T21:30:00-03:00
```

A aplicação deve utilizar um formato consistente para todos os
registros.

------------------------------------------------------------------------

# 22. Sequência recomendada de implementação

Para evitar complexidade desnecessária, implemente em etapas.

## Etapa 1 --- Inicialização

Criar:

``` text
tasks.json
```

Caso não exista, criar automaticamente com:

``` json
[]
```

------------------------------------------------------------------------

## Etapa 2 --- Leitura e escrita

Implementar funções equivalentes a:

``` text
load_tasks()
save_tasks()
```

Testar:

1.  arquivo inexistente;
2.  arquivo vazio;
3.  JSON válido;
4.  JSON inválido.

------------------------------------------------------------------------

## Etapa 3 --- Criar tarefa

Implementar:

``` bash
task-cli add "Minha tarefa"
```

Validar:

-   descrição;
-   geração do ID;
-   status;
-   timestamps;
-   persistência.

------------------------------------------------------------------------

## Etapa 4 --- Listar

Implementar:

``` bash
task-cli list
```

Depois:

``` bash
task-cli list done
task-cli list todo
task-cli list in-progress
```

------------------------------------------------------------------------

## Etapa 5 --- Atualizar

Implementar:

``` bash
task-cli update 1 "Nova descrição"
```

Garantir que:

-   `createdAt` permaneça igual;
-   `updatedAt` seja alterado.

------------------------------------------------------------------------

## Etapa 6 --- Excluir

Implementar:

``` bash
task-cli delete 1
```

------------------------------------------------------------------------

## Etapa 7 --- Alterar status

Implementar:

``` bash
task-cli mark-in-progress 1
task-cli mark-done 1
```

------------------------------------------------------------------------

## Etapa 8 --- Tratamento de erros

Depois que os comandos principais funcionarem, tratar:

-   comandos inválidos;
-   argumentos ausentes;
-   IDs inválidos;
-   IDs inexistentes;
-   descrições vazias;
-   JSON inválido;
-   status inválidos.

------------------------------------------------------------------------

# 23. Checklist funcional

## Criar

-   [ ] `add` existe.
-   [ ] Recebe descrição.
-   [ ] Gera ID.
-   [ ] ID é único.
-   [ ] Status inicial é `todo`.
-   [ ] `createdAt` é criado.
-   [ ] `updatedAt` é criado.
-   [ ] Tarefa é salva no JSON.

## Atualizar

-   [ ] `update` existe.
-   [ ] Recebe ID.
-   [ ] Recebe nova descrição.
-   [ ] Atualiza descrição.
-   [ ] Mantém `createdAt`.
-   [ ] Atualiza `updatedAt`.
-   [ ] Salva no JSON.

## Excluir

-   [ ] `delete` existe.
-   [ ] Recebe ID.
-   [ ] Remove a tarefa.
-   [ ] Salva o JSON.
-   [ ] Trata ID inexistente.

## Status

-   [ ] `mark-in-progress` existe.
-   [ ] `mark-done` existe.
-   [ ] Status é persistido.
-   [ ] `updatedAt` é atualizado.

## Listagem

-   [ ] `list` mostra todas.
-   [ ] `list done` filtra `done`.
-   [ ] `list todo` filtra `todo`.
-   [ ] `list in-progress` filtra `in-progress`.

## Persistência

-   [ ] JSON é criado automaticamente.
-   [ ] JSON é lido corretamente.
-   [ ] JSON é salvo corretamente.
-   [ ] Não existe banco de dados externo.
-   [ ] Não existem bibliotecas externas.

## Erros

-   [ ] Comando desconhecido é tratado.
-   [ ] Argumentos ausentes são tratados.
-   [ ] ID inválido é tratado.
-   [ ] ID inexistente é tratado.
-   [ ] Descrição vazia é tratada.
-   [ ] Status inválido é tratado.
-   [ ] JSON inválido é tratado.

------------------------------------------------------------------------

# 24. Checklist dos critérios do desafio

Antes de considerar o projeto concluído, verificar:

### Requisito 1

A aplicação executa pelo terminal?

-   [ ] Sim

### Requisito 2

Recebe ações e entradas como argumentos posicionais?

-   [ ] Sim

### Requisito 3

Permite adicionar tarefas?

-   [ ] Sim

### Requisito 4

Permite atualizar tarefas?

-   [ ] Sim

### Requisito 5

Permite excluir tarefas?

-   [ ] Sim

### Requisito 6

Permite marcar como `in-progress`?

-   [ ] Sim

### Requisito 7

Permite marcar como `done`?

-   [ ] Sim

### Requisito 8

Permite listar todas?

-   [ ] Sim

### Requisito 9

Permite listar `done`?

-   [ ] Sim

### Requisito 10

Permite listar `todo`?

-   [ ] Sim

### Requisito 11

Permite listar `in-progress`?

-   [ ] Sim

### Requisito 12

Utiliza JSON no diretório atual?

-   [ ] Sim

### Requisito 13

Cria o JSON caso ele não exista?

-   [ ] Sim

### Requisito 14

Utiliza o sistema de arquivos nativo?

-   [ ] Sim

### Requisito 15

Não utiliza bibliotecas/frameworks externos?

-   [ ] Sim

### Requisito 16

Trata erros e casos extremos?

-   [ ] Sim

------------------------------------------------------------------------

# 25. Casos de teste manuais

Uma sequência mínima de testes pode ser:

## Teste 1 --- Estado inicial

Excluir `tasks.json` caso exista.

Executar:

``` bash
task-cli list
```

Resultado esperado:

``` text
Nenhuma tarefa encontrada.
```

E o arquivo deve existir após a execução.

------------------------------------------------------------------------

## Teste 2 --- Criar tarefa

``` bash
task-cli add "Estudar Python"
```

Esperado:

``` text
Task added successfully (ID: 1)
```

------------------------------------------------------------------------

## Teste 3 --- Criar segunda tarefa

``` bash
task-cli add "Fazer exercícios"
```

Esperado:

``` text
Task added successfully (ID: 2)
```

------------------------------------------------------------------------

## Teste 4 --- Listar

``` bash
task-cli list
```

Devem aparecer as duas tarefas.

------------------------------------------------------------------------

## Teste 5 --- Marcar em andamento

``` bash
task-cli mark-in-progress 1
```

Depois:

``` bash
task-cli list in-progress
```

A tarefa 1 deve aparecer.

------------------------------------------------------------------------

## Teste 6 --- Marcar como concluída

``` bash
task-cli mark-done 1
```

Depois:

``` bash
task-cli list done
```

A tarefa 1 deve aparecer.

------------------------------------------------------------------------

## Teste 7 --- Listar pendentes

``` bash
task-cli list todo
```

A tarefa 2 deve aparecer.

------------------------------------------------------------------------

## Teste 8 --- Atualizar

``` bash
task-cli update 2 "Fazer exercícios físicos"
```

Depois:

``` bash
task-cli list
```

A descrição deve estar atualizada.

------------------------------------------------------------------------

## Teste 9 --- Excluir

``` bash
task-cli delete 2
```

Depois:

``` bash
task-cli list
```

Somente a tarefa 1 deverá permanecer.

------------------------------------------------------------------------

## Teste 10 --- ID inexistente

``` bash
task-cli delete 999
```

A aplicação deve informar que a tarefa não existe e continuar
funcionando normalmente.

------------------------------------------------------------------------

## Teste 11 --- ID inválido

``` bash
task-cli delete abc
```

A aplicação deve informar que o ID é inválido.

------------------------------------------------------------------------

## Teste 12 --- Comando inválido

``` bash
task-cli qualquer-comando
```

A aplicação deve informar que o comando não existe.

------------------------------------------------------------------------

# 26. Critérios de aceite

O projeto pode ser considerado concluído quando:

1.  A aplicação inicia pelo terminal.
2.  Os comandos obrigatórios estão implementados.
3.  As tarefas são persistidas em JSON.
4.  O JSON é criado automaticamente.
5.  Cada tarefa possui `id`, `description`, `status`, `createdAt` e
    `updatedAt`.
6.  Os status válidos são exatamente `todo`, `in-progress` e `done`.
7.  IDs são únicos.
8.  Atualizações modificam `updatedAt`.
9.  Atualizações não alteram `createdAt`.
10. Os filtros de listagem funcionam.
11. Operações com IDs inexistentes são tratadas.
12. Entradas inválidas não causam encerramento inesperado.
13. Não é utilizado banco de dados.
14. Não são utilizadas bibliotecas ou frameworks externos.
15. O README explica como instalar/executar e utilizar a aplicação.
16. Todos os comandos foram testados manualmente.

------------------------------------------------------------------------

# 27. O que NÃO faz parte do requisito

Para manter o projeto alinhado ao desafio original, os itens abaixo não
são necessários:

-   interface gráfica;
-   aplicação web;
-   API REST;
-   banco de dados;
-   autenticação;
-   usuários;
-   login;
-   sincronização em nuvem;
-   notificações;
-   categorias;
-   prioridades;
-   datas de vencimento;
-   tags;
-   projetos;
-   subtarefas;
-   recorrência;
-   Docker;
-   CI/CD;
-   integração com serviços externos.

Esses recursos podem ser utilizados como evolução futura, mas não são
necessários para cumprir o desafio básico.

------------------------------------------------------------------------

# 28. Possíveis extensões futuras

Depois de concluir completamente o desafio original, o projeto pode
evoluir.

Exemplos:

``` text
prioridade
dueDate
tags
categorias
projetos
subtarefas
busca
ordenação
exportação CSV
exportação Markdown
interface web
API REST
banco de dados
testes automatizados
Docker
CI/CD
```

Essas funcionalidades devem ser consideradas **extensões**, e não
requisitos do desafio original.

------------------------------------------------------------------------

# 29. Exemplos de uso completo

``` bash
# Criar tarefas
task-cli add "Estudar Python"
task-cli add "Fazer exercícios"
task-cli add "Ler documentação"

# Listar todas
task-cli list

# Iniciar uma tarefa
task-cli mark-in-progress 1

# Listar tarefas em andamento
task-cli list in-progress

# Concluir uma tarefa
task-cli mark-done 1

# Listar concluídas
task-cli list done

# Listar pendentes
task-cli list todo

# Alterar descrição
task-cli update 2 "Fazer exercícios de programação"

# Excluir tarefa
task-cli delete 3

# Listar novamente
task-cli list
```

------------------------------------------------------------------------

# 30. Resumo da implementação

O fluxo principal da aplicação deve ser:

``` text
                  ┌──────────────────┐
                  │    task-cli      │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │  Interpretar CLI │
                  └────────┬─────────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
           ADD          UPDATE        DELETE
             │             │             │
             └─────────────┼─────────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │   Task Manager   │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │  tasks.json     │
                  └──────────────────┘
```

A ideia central é simples:

``` text
CLI → lógica da aplicação → JSON
```

Não é necessário servidor, banco de dados ou framework.

------------------------------------------------------------------------

# 31. Referência oficial

Projeto original:

https://roadmap.sh/projects/task-tracker

A especificação oficial define o Task Tracker como um projeto de CLI
para gerenciamento de tarefas, com persistência em JSON, comandos para
criação/alteração/exclusão/status/listagem e uso do sistema de arquivos
nativo, sem bibliotecas ou frameworks externos.

------------------------------------------------------------------------

## Status do projeto

Use este espaço para acompanhar a implementação:

``` text
[ ] Inicialização
[ ] Storage JSON
[ ] Add
[ ] List
[ ] Update
[ ] Delete
[ ] Mark in-progress
[ ] Mark done
[ ] Filtros
[ ] Validações
[ ] Tratamento de erros
[ ] Testes manuais
[ ] README
[ ] Projeto concluído
```

------------------------------------------------------------------------

## Licença

MIT
