# Базовые алгоритмы для работы с графами

### Сброс атрибутов вершин графа (вспомогательная процедура)

```
graph-reset(G)
    Вход:
        G: структура данных графа

    for v in G.V                // для каждой вершины в графе
        v.visited := FALSE
```

### Обход в ширину (BFS)

```
BFS(G)
    Вход:
        G: структура данных графа

    graph-reset(G)              // сброс атрибутов вершин графа
    for v in G.V                // для каждой вершины в графе
        if v.visited == FALSE
            BFS-visit(G, v)
```
```
BFS-visit(G, s)
    Вход:
        G: структура данных графа
        s: посещаемая вершина

    Пусть Q - новая очередь
    s.visited := TRUE
    выполнение какой-либо операции с вершиной s
    queue-enqueue(Q, s)
    while queue-empty(Q) == FALSE
        u := queue-dequeue(Q)
        for v in G.Adj[u]       // для каждой смежной с u вершины
            if v.visited == FALSE
                v.visited := TRUE
                выполнение какой-либо операции с вершиной v
                queue-enqueue(Q, v)
```

### Обход в глубину (DFS)

```
DFS(G)
    Вход:
        G: структура данных графа

    graph-reset(G)              // сброс атрибутов вершин графа
    for v in G.V                // для каждой вершины в графе
        if v.visited == FALSE
            DFS-visit(G, v)
```

#### Рекурсивный вариант

```
DFS-visit(G, s)
    Вход:
        G: структура данных графа
        s: посещаемая вершина

    s.visited := TRUE
    выполнение какой-либо операции с вершиной s
    for v in G.Adj[s]           // для каждой смежной с s вершины
        if v.visited == FALSE
            DFS-visit(G, v)
```

#### Итеративный вариант

```
DFS-visit(G, s)
    Вход:
        G: структура данных графа
        s: посещаемая вершина

    Пусть S - новый стек
    s.visited := TRUE
    выполнение какой-либо операции с вершиной s
    stack-push(S, s)
    while stack-empty(S) == FALSE
        u := stack-peek(S)
        v := следующая смежная с u непосещённая вершина
        if v == NIL             // все смежные с u вершины посещены
            stack-pop(S)
        else
            v.visited := TRUE
            выполнение какой-либо операции с вершиной v
            stack-push(S, v)
```
