1. Методы `most_common(), elements()`
2. Методы `total(), subtract()`
3. Операторы `+, -, &, |`

**Аннотация.** Урок посвящен типу данных `Counter`.

##  Методы Counter

Помимо доступных для всех словарей методов, словарь `Counter` поддерживает еще четыре дополнительных:

- `most_common()`
- `elements()`
- `total()` (начиная с Python 3.10)
- `subtract()`

### Метод most_common()

Метод `most_common()` возвращает список наиболее повторяемых элементов и количество каждого из них. Метод возвращает список кортежей вида `(ключ, число повторений)`.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common())
print(numbers.most_common())
```

выводит:

```no-highlight
[('i', 4), ('s', 4), ('p', 2), ('m', 1)]
[(5, 3), (7, 3), (9, 3), (1, 2), (6, 1), (3, 1), (2, 1)]
```

Если методу `most_common()` передать целочисленный аргумент `n`, то он вернет `n` самых часто повторяющихся элементов.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common(3))
print(numbers.most_common(4))
```

выводит:

```no-highlight
[('i', 4), ('s', 4), ('p', 2)]
[(5, 3), (7, 3), (9, 3), (1, 2)]
```

Для поиска самых редких элементов, можно использовать срезы с отрицательным шагом.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common()[-1])
print(letters.most_common()[::-1])
print(numbers.most_common()[-3:-1])
```

выводит:

```no-highlight
('m', 1)
[('m', 1), ('p', 2), ('s', 4), ('i', 4)]
[(6, 1), (3, 1)]
```

Для корректной работы метода `most_common()` нужно, чтобы значения в `Counter` словаре поддерживали сортировку.

### Метод elements()

Метод `elements()` возвращает итератор по элементам, в котором каждый элемент повторяется столько раз, во сколько установлено его значение. Элементы возвращаются в порядке их появления.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(list(letters.elements()))
print(list(numbers.elements()))
```

выводит:

```no-highlight
['m', 'i', 'i', 'i', 'i', 's', 's', 's', 's', 'p', 'p']
[5, 5, 5, 6, 7, 7, 7, 1, 1, 3, 9, 9, 9, 2]
```

Метод `elements()` возвращает итератор. Для того чтобы увидеть его элементы, требуется преобразовать его в список с помощью функции `list()`.

Если количество элементов по некоторому ключу меньше единицы, то метод `elements()` просто проигнорирует его.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(list(letters.elements()))
```

выводит:

```no-highlight
['i', 'i', 'i', 'i', 's', 's', 's', 's', 'p', 'p', 'm']
```

### Метод total()

В Python 3.10 появился метод `total()`, который вычисляет сумму всех значений `Counter` словаря, включая отрицательные.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(letters.total())
```

выводит:

```no-highlight
-87
```

В более ранних версиях Python приведенный выше код можно заменить кодом:

```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(sum(letters.values()))
```

### Метод subtract()

Метод `subtract()` вычитает из значений элементов одного словаря `Counter` значения элементов другого словаря. Метод `subtract()` подобен методу `update()`, но вычитает количества, а не складывает их. При этом у результирующего словаря значения ключей могут быть нулевыми или отрицательными.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=4, s=40, a=1, p=20, b=98, z=69)
counter2 = Counter(i=2, s=20, a=6, p=12, m=1, z=69)

counter1.subtract(counter2)       # обновляем значения в counter1

print(counter1)
```

выводит:

```no-highlight
Counter({'b': 98, 's': 20, 'p': 8, 'i': 2, 'z': 0, 'm': -1, 'a': -5})
```

Мы можем использовать метод `subtract()` с именованными аргументами. Вызов метода `counter1.subtract(i=2, s=20, a=6, p=12, m=1, z=69)` равнозначен вызову метода `counter1.subtract(counter2)`.

Помимо словарей, метод `subtract()` может принимать любой итерируемый объект: список, строку, кортеж и т.д.

Приведенный ниже код:

```python
from collections import Counter

counter = Counter(i=4, s=40, a=1, p=20, b=98, z=69)
letters = 'iisssssapppz'

counter.subtract(letters)       # обновляем значения в counter

print(counter)
```

выводит:

```no-highlight
Counter({'b': 98, 'z': 68, 's': 35, 'p': 17, 'i': 2, 'a': 0})
```

## Операторы +, -, &, |

Как мы уже знаем, методы `update()` и `subtract()` объединяют `Counter` словари путем сложения и вычитания количества соответствующих элементов. Python предоставляет удобные операторы сложения (`+`) и вычитания (`-`), которые могут заменить вызовы данных методов.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=10, s=40, p=10, m=1)
counter2 = Counter(i=2, s=8, p=10, m=3)

print(counter1 + counter2)
print(counter1 - counter2)
print(counter2 - counter1)
```

выводит:

```no-highlight
Counter({'s': 48, 'p': 20, 'i': 12, 'm': 4})
Counter({'s': 32, 'i': 8})
Counter({'m': 2})
```

Обратите внимание на то, что при использовании операторов `+` и `-` из результирующего словаря исключаются элементы с нулевыми и отрицательными значениями.

Помимо указанных выше операторов, Python также предоставляет операторы пересечения (`&`) и объединения (`|`), которые возвращают минимум и максимум из соответствующих значений.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=10, s=40, p=10, m=1)
counter2 = Counter(i=2, s=8, p=10, m=3)

print(counter1 & counter2)
print(counter1 | counter2)
```

выводит:

```no-highlight
Counter({'p': 10, 's': 8, 'i': 2, 'm': 1})
Counter({'s': 40, 'i': 10, 'p': 10, 'm': 3})
```

Операторы сложения (`+`) и вычитания (`-`) работают только с `Counter` словарями, в то время как методы `update()` и `subtract()` с любым итерируем объектом: списком, строкой, кортежем и т.д.

## Примечания

**Примечание 1.** В математике мультимножество — модификация понятия множества, допускающая включение одного и того же элемента в совокупность по нескольку раз. Особенностью мультимножества является понятие **кратности** вхождения элемента. К примеру, мультимножеством является совокупность чисел `{1, 1, 2, 3, 3, 3, 4, 4}`, при этом обычное множество будет иметь вид `{1, 2, 3, 4}`.

Тип `Counter` позволяет реализовать концепцию мультимножеств.

Приведенный ниже код:

```python
from collections import Counter

multiset = Counter([1, 1, 2, 3, 3, 3, 4, 4])

print(multiset)                   # мультимножество
print(set(multiset.keys()))       # обычное множество
```

выводит:

```no-highlight
Counter({1: 2, 2: 1, 3: 3, 4: 2})
{1, 2, 3, 4}
```

**Примечание 2.** Тип `Counter` позволяет также использовать унарные операторы сложения (`+`) и вычитания (`-`).

Приведенный ниже код:

```python
from collections import Counter

counter = Counter(a=5, b=-9, c=0)

print(+counter)
print(-counter)
```

выводит:

```no-highlight
Counter({'a': 5})
Counter({'b': 9})
```

Когда мы используем оператор сложения (`+`) в качестве унарного, мы получаем новый `Counter` словарь, содержащий элементы, значения которых больше нуля. Аналогично, при использовании унарного оператора (`-`), мы получаем новый `Counter` словарь, содержащий элементы, значения которых меньше нуля.

Другими словами, операторы унарного сложения и вычитания прибавляют пустой `Counter` словарь или вычитают исходный из пустого.

Приведенный выше код, равнозначен коду:

```python
from collections import Counter

counter = Counter(a=5, b=-9, c=0)

print(Counter() + counter)
print(Counter() - counter)
```

**Примечание 3.** С версии Python 3.10 тип `Counter` поддерживают расширенные операторы сравнения для отношений **подмножества и надмножества**: `<`, `<=`, `>`, `>=`. Все эти сравнения рассматривают отсутствующие элементы как имеющие нулевое количество.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter('aabc')
counter2 = Counter('abc')

print(counter1 > counter2)

counter1 = Counter('abcde')
counter2 = Counter('abc')

print(counter1 > counter2)

counter1 = Counter('abcde')
counter2 = Counter('abcdf')

print(counter1 > counter2)
print(counter1 < counter2)
```

выводит:

```no-highlight
True
True
False
False
```

**Примечание 4.** Объекты типа `Counter`, аналогично объектам типа `OrderedDict`, содержат дополнительный атрибут `__dict__`, который используется для динамического наделения объектов дополнительным функционалом.

Приведенный ниже код:

```python
from collections import Counter

counter = Counter(green=10, red=25, blue=5)

print(counter.__dict__)

counter.__dict__['min_value'] = lambda: min(counter.values())
counter.max_value = lambda: max(counter.values())

print(counter.min_value())
print(counter.max_value())
```

выводит:

```no-highlight
{}
5
25
```

