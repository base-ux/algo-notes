# Разное

## Поиск максимального подмассива

### Итеративный вариант

```
find-max-subarray(A, low, high)
    Вход:
        A: массив
        low: нижний индекс
        high: верхний индекс
    Выход:
        кортеж, содержащий нижний и верхний индексы максимального подмассива,
        а также сумму значений

    // инициализация: максимальный и текущий подмассивы состоят
    // из одного первого элемента A[low..low]
    max-low := low
    max-high := max-low
    max-sum := A[max-low]
    cur-low := low
    cur-high := cur-low
    cur-sum := A[cur-low]

    for j := low to high - 1    // до предпоследнего элемента
        next := A[j + 1]
        // выбор максимального подмассива:
        //   либо максимальный подмассив A[j + 1..j + 1]
        //   либо максимальный подмассив A[cur-low..j + 1]
        if next > cur-sum + next
            // обновление значений текущего подмассива
            cur-low := j + 1
            cur-high := j + 1
            cur-sum := next
        else
            // обновление значений максимального подмассива
            max-low := cur-low
            max-high := cur-high
            max-sum := cur-sum

    return (max-low, max-high, max-sum)
```

### Рекурсивный вариант

```
find-max-crossing-subarray(A, low, mid, high)
    Вход:
        A: массив
        low: нижний индекс
        mid: средний индекс
        high: верхний индекс
    Выход:
        кортеж, содержащий нижний и верхний индексы максимального подмассива,
        а также сумму значений

    // поиск максимального подмассива в левой половине A[low..mid]
    left-sum := -INF        // минимально возможное значение (минус бесконечность)
    sum := 0
    for i := mid downto low
        sum := sum + A[i]
        if sum > left-sum
            left-sum := sum
            max-left := i

    // поиск максимального подмассива в правой половине A[mid + 1..high]
    right-sum := -INF       // минимально возможное значение (минус бесконечность)
    sum := 0
    for j := mid + 1 to high
        sum := sum + A[j]
        if sum > right-sum
            right-sum := sum
            max-right := j

    return (max-left, max-right, left-sum + right-sum)
```
```
find-max-subarray(A, low, high)
    Вход:
        A: массив
        low: нижний индекс
        high: верхний индекс
    Выход:
        кортеж, содержащий нижний и верхний индексы максимального подмассива,
        а также сумму значений

    if high == low
        // базовый случай: только один элемент
        return (low, high, A[low])
    else
        mid := (low + high) / 2     // средний индекс (целочисленное деление)
        (left-low, left-high, left-sum) := find-max-subarray(A, low, mid)
        (right-low, right-high, right-sum) := find-max-subarray(A, mid+1, high)
        (cross-low, cross-high, cross-sum) := find-max-crossing-subarray(A, low, mid, high)
        if left-sum >= right-sum и left-sum >= cross-sum
            return (left-low, left-high, left-sum)
        elseif right-sum >= left-sum и right-sum >= cross-sum
            return (right-low, right-high, right-sum)
        else
            return (cross-low, cross-high, cross-sum)
```

## Случайная перестановка с равномерным распределением

```
randomize-in-place(A, p, r)
    Вход:
        A: массив
        p: начальный индекс подмассива A[p..r]
        r: конечный индекс подмассива A[p..r]
    Выход:
        подмассив A[p..r], элементы которого переставлены случайным образом

    for i := p to r
        // random(a, b) возвращает случайное целое число в диапазоне a <= X <= b
        обменять A[i] и A[random(i, r)]
```
