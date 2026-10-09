# Связанные списки

Структура данных, в которой элементы расположены в линейном порядке.
Порядок определяется указателями (ссылками) на каждый элемент.
В псевдокоде процедур используются следующие атрибуты списка:
```
L.first: указатель на первый элемент списка
```
Каждый элемент списка имеет следующие атрибуты:
```
e.obj: указатель на объект, хранящий данные
e.next: указатель на следующий элемент
e.prev: указатель на предыдущий элемент (для двусвязного списка)
```
Если `L.first` имеет значение `NIL`, то список пуст.
Если `e.next` имеет значение `NIL`, то такой элемент является хвостовым.
Если `e.prev` (для двусвязного списка) имеет значение `NIL`, то такой элемент
является головным.
Атрибут `e.obj` либо представляет собой непосредственные данные (например,
числовое значение), либо указатель на структуру, содержащей данные.
Для сравнения на равенство значений применяется процедура `compare`,
которая возвращает нулевое значение при равенстве своих аргументов.

## Операции

- Проверка на наличие элементов (empty)
- Поиск объекта (search)
- Вставка объекта:
    - в начало списка (insert-first)
    - в конец списка (insert-last)
- Удаление объекта:
    - в начале списка (delete-first)
    - в конце списка (delete-last)
    - в середине списка (delete)

### Проверка на наличие элементов (empty)

Процедура `empty` одинакова для одно- и двусвязных списков:
```
empty(L)
    Вход:
        L: список
    Выход:
        TRUE: если список не содержит элементов
        FALSE: в противном случае

    if L.first == NIL
        return TRUE
    else
        return FALSE
```

### Поиск объекта (search)

Процедура `search` одинакова для одно- и двусвязных списков:
```
search(L, x)
    Вход:
        L: список
        x: объект, содержащий искомые данные (ключ)
    Выход:
        указатель на объект с искомым ключом или NIL

    e := L.first
    while e != NIL и compare(e.obj, x) != 0
        e := e.next
    if e == NIL             // объект не найден
        return NIL
    return e.obj
```

### Вставка объекта в начало списка (insert-first)

#### Односвязный список

```
insert-first(L, x)
    Вход:
        L: список
        x: объект, добавляемый в начало списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := L.first
    L.first := e
```

#### Двусвязный список

```
insert-first(L, x)
    Вход:
        L: список
        x: объект, добавляемый в начало списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := L.first
    e.prev := NIL
    if L.first != NIL
        L.first.prev := e
    L.first := e
```

### Вставка объекта в конец списка (insert-last)

#### Односвязный список

```
insert-last(L, x)
    Вход:
        L: список
        x: объект, добавляемый в конец списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := NIL
    if empty(L)
        L.first := e
    else
        // поиск последнего элемента списка
        last := L.first
        while last.next != NIL
            last := last.next
        last.next := e
```

#### Двусвязный список

```
insert-last(L, x)
    Вход:
        L: список
        x: объект, добавляемый в конец списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := NIL
    e.prev := NIL
    if empty(L)
        L.first := e
    else
        // поиск последнего элемента списка
        last := L.first
        while last.next != NIL
            last := last.next
        last.next := e
        e.prev := last
```

### Удаление объекта в начале списка (delete-first)

#### Односвязный список

```
delete-first(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    x := L.first.obj
    L.first := L.first.next
    return x
```

#### Двусвязный список

```
delete-first(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    x := L.first.obj
    L.first := L.first.next
    L.first.prev := NIL
    return x
```

### Удаление объекта в конце списка (delete-last)

#### Односвязный список

```
delete-last(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    // поиск последнего элемента и его предшественника
    p := NIL
    last := L.first
    while last.next != NIL
        p := last
        last := last.next
    x := last.obj
    if last == L.first      // единственный элемент в списке
        L.first := NIL
    else
        p.next := NIL
    return x
```

#### Двусвязный список

```
delete-last(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    // поиск последнего элемента
    last := L.first
    while last.next != NIL
        last := last.next
    x := last.obj
    if last == L.first      // единственный элемент в списке
        L.first := NIL
    else
        last.prev.next := NIL
    return x
```

### Удаление объекта в середине списка (delete)

#### Односвязный список

```
delete(L, x)
    Вход:
        L: список
        x: удаляемый объект
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    // поиск удаляемого элемента и его предшественника
    p := NIL
    e := L.first
    while e != NIL и compare(e.obj, x) != 0
        p := e
        e := e.next
    if e == NIL             // объект не найден
        return NIL
    x := e.obj
    if e == L.first         // первый элемент списка
        L.first := L.first.next
    else
        p.next := e.next
    return x
```

#### Двусвязный список

```
delete(L, x)
    Вход:
        L: список
        x: удаляемый объект
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    // поиск удаляемого элемента
    e := L.first
    while e != NIL и compare(e.obj, x) != 0
        e := e.next
    if e == NIL             // объект не найден
        return NIL
    x := e.obj
    if e == L.first         // первый элемент списка
        L.first := L.first.next
        L.first.prev := NIL
    else
        e.prev.next := e.next
        if e.next != NIL
            e.next.prev := e.prev
    return x
```

## Список с указателем на последний элемент

Если помимо указателя на головной элемент списка использовать дополнительно указатель
на хвостовой элемент, т.е.
```
L.first: указатель на первый элемент списка
L.last: указатель на последний элемент списка
```
то для операций вставки-удаления в конец списка для двусвязных списков время выполнения
становится константным, т.е. O(1) вместо O(n).
Алгоритмическая реализация процедур с учётом дополнительного указателя приведена ниже.

### Проверка на наличие элементов (empty)

Процедура `empty` одинакова для одно- и двусвязных списков:
```
empty(L)
    Вход:
        L: список
    Выход:
        TRUE: если список не содержит элементов
        FALSE: в противном случае

    if L.first == NIL
        return TRUE
    else
        return FALSE
```

### Поиск объекта (search)

Операция `search` одинакова для одно- и двусвязных списков:
```
search(L, x)
    Вход:
        L: список
        x: объект, содержащий искомые данные (ключ)
    Выход:
        указатель на объект с искомым ключом или NIL

    e := L.first
    while e != NIL и compare(e.obj, x) != 0
        e := e.next
    if e == NIL             // объект не найден
        return NIL
    return e.obj
```

### Вставка объекта в начало списка (insert-first)

#### Односвязный список

```
insert-first(L, x)
    Вход:
        L: список
        x: объект, добавляемый в начало списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := L.first
    L.first := e
    if L.last == NIL
        L.last := e
```

#### Двусвязный список

```
insert-first(L, x)
    Вход:
        L: список
        x: объект, добавляемый в начало списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := L.first
    e.prev := NIL
    if L.first != NIL
        L.first.prev := e
    L.first := e
    if L.last == NIL
        L.last := e
```

### Вставка объекта в конец списка (insert-last)

#### Односвязный список

```
insert-last(L, x)
    Вход:
        L: список
        x: объект, добавляемый в конец списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := NIL
    if L.last != NIL
        L.last.next := e
    L.last := e
    if L.first == NIL
        L.first := e
```

#### Двусвязный список

```
insert-last(L, x)
    Вход:
        L: список
        x: объект, добавляемый в конец списка

    Пусть e - новый элемент списка
    e.obj := x
    e.next := NIL
    e.prev := L.last
    if L.last != NIL
        L.last.next := e
    L.last := e
    if L.first == NIL
        L.first := e
```

### Удаление объекта в начале списка (delete-first)

#### Односвязный список

```
delete-first(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    x := L.first.obj
    if L.last == L.first    // единственный элемент в списке
        L.last := NIL
    L.first := L.first.next
    return x
```

#### Двусвязный список

```
delete-first(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    x := L.first.obj
    if L.last == L.first    // единственный элемент в списке
        L.last := NIL
    L.first := L.first.next
    L.first.prev := NIL
    return x
```

### Удаление объекта в конце списка (delete-last)

#### Односвязный список

```
delete-last(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    x := L.last.obj
    if L.last == L.first    // единственный элемент в списке
        L.first := L.last := NIL
    else
        // поиск предшественника последнего элемента
        p := L.first
        while p.next != L.last
            p := p.next
        L.last := p
        L.last.next := NIL
    return x
```

#### Двусвязный список

```
delete-last(L)
    Вход:
        L: список
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    x := L.last.obj
    if L.last == L.first    // единственный элемент в списке
        L.first := L.last := NIL
    else
        L.last := L.last.prev
        L.last.next := NIL
    return x
```

### Удаление объекта в середине списка (delete)

#### Односвязный список

```
delete(L, x)
    Вход:
        L: список
        x: удаляемый объект
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    // поиск удаляемого элемента и его предшественника
    p := NIL
    e := L.first
    while e != NIL и compare(e.obj, x) != 0
        p := e
        e := e.next
    if e == NIL                 // объект не найден
        return NIL
    x := e.obj
    if L.last == L.first        // единственный элемент в списке
        L.first := L.last := NIL
    elseif e == L.first         // первый элемент списка
        L.first := L.first.next
    elseif e == L.last          // последний элемент списка
        L.last := p
        L.last.next := NIL
    else
        p.next := e.next
    return x
```

#### Двусвязный список

```
delete(L, x)
    Вход:
        L: список
        x: удаляемый объект
    Выход:
        указатель на удалённый объект или NIL

    if empty(L)
        return NIL
    // поиск удаляемого элемента
    e := L.first
    while e != NIL и compare(e.obj, x) != 0
        e := e.next
    if e == NIL                 // объект не найден
        return NIL
    x := e.obj
    if L.last == L.first        // единственный элемент в списке
        L.first := L.last := NIL
    elseif e == L.first         // первый элемент списка
        L.first := L.first.next
        L.first.prev := NIL
    elseif e == L.last          // последний элемент списка
        L.last := L.last.prev
        L.last.next := NIL
    else
        e.prev.next := e.next
        e.next.prev := e.prev
    return x
```
