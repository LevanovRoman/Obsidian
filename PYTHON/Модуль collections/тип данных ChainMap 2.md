1. Тип данных `ChainMap`
2. Атрибуты `maps` и `parents`
3. Метод `new_child()`

**Аннотация.** Урок посвящен типу данных `ChainMap`.

## Атрибут maps

Объект `ChainMap` хранит все объединяемые словари во внутреннем списке. Этот список доступен через атрибут `maps` и может быть изменен. Порядок словарей в списке `maps` соответствует порядку, в котором словари были указаны при создании объекта `ChainMap`.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(pets)
print(pets.maps)
print(type(pets.maps))
```

выводит:

```no-highlight
ChainMap({'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3})
[{'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3}]
<class 'list'>
```

Атрибут `maps` является обычным списком, поэтому он поддерживает все основные операции со списками. Мы можем добавлять в него новые словари, удалять уже добавленные, а также изменять их порядок.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

pets.maps.reverse()
pets.maps[0]['lions'] = 10
del pets.maps[1]['cats']

print(pets)
print(pets.maps)
```

выводит:

```no-highlight
ChainMap({'dogs': 7, 'cats': 2, 'tigers': 3, 'lions': 10}, {'dogs': 15, 'pythons': 9})
[{'dogs': 7, 'cats': 2, 'tigers': 3, 'lions': 10}, {'dogs': 15, 'pythons': 9}]
```

Обратите внимание, что изменяя порядок словарей в списке атрибута `maps`, мы также меняем сами объединяемые словари, а также порядок поиска в объекте `ChainMap`.

Атрибут `maps` можно использовать для обработки абсолютно всех значений во всех словарях. С помощью этого атрибута мы можем обойти поведение по умолчанию, заключающееся в получении (изменении) первого значения из первого словаря.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for animals in pets.maps:
    for key, value in animals.items():
        print(key, '->', value)
```

выводит:

```no-highlight
dogs -> 15
cats -> 8
pythons -> 9
dogs -> 7
cats -> 2
tigers -> 3
```

## Метод new_child()

Метод `new_child(m=None)` возвращает **новый объект** `ChainMap()`, содержащий новый словарь `m`, за которым следуют все словари текущего объекта:

- если указан словарь `m`, то он вставляется первым в списке существующих словарей текущего объекта `ChainMap`
- если `m` не указан, то используется пустой словарь, который также вставляется первым

Приведенный ниже код:

```python
from collections import ChainMap

dad = {'name': 'Timur', 'age': 29}
mom = {'name': 'Rosaly', 'age': 28}

old_family = ChainMap(dad, mom)

son = {'name': 'Soslan', 'age': 0}

new_family = old_family.new_child(son)

print(old_family)
print(new_family)
```

выводит:

```no-highlight
ChainMap({'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
ChainMap({'name': 'Soslan', 'age': 0}, {'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
```

Приведенный ниже код:

```python
from collections import ChainMap

dad = {'name': 'Timur', 'age': 29}
mom = {'name': 'Rosaly', 'age': 28}

old_family = ChainMap(dad, mom)
new_family = old_family.new_child()

print(old_family)
print(new_family)
```

выводит:

```no-highlight
ChainMap({'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
ChainMap({}, {'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
```

   Обратите внимание, что метод `new_child()` не обновляет старый объект `ChainMap`, а создаёт и возвращает новый.

## Атрибут parents

Атрибут `parents` возвращает новый объект `ChainMap`, содержащий все словари, кроме первого. Это полезно для пропуска первого словаря при поиске ключей.

Приведенный ниже код:

```python
from collections import ChainMap

dad = {'name': 'Timur', 'age': 29}
mom = {'name': 'Rosaly', 'age': 28}
son = {'name': 'Soslan', 'age': 0}

family = ChainMap(son, dad, mom)

print(family)
print(family.parents)
print(type(family.parents))
```

выводит:

```no-highlight
ChainMap({'name': 'Soslan', 'age': 0}, {'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
ChainMap({'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
<class 'collections.ChainMap'>
```

В некотором смысле атрибут `parents` выполняет обратную функцию методу `new_child()`. Первый удаляет словарь, а второй добавляет новый словарь в начало списка. В обоих случаях вы получаете новый объект `ChainMap`.

## Примечания

**Примечание 1.** При создании пустого `ChainMap` объекта его атрибут `maps` будет содержать пустой словарь.

Приведенный ниже код:

```python
from collections import ChainMap

empty_chain = ChainMap()

print(empty_chain.maps)
```

выводит:

```no-highlight
[{}]
```

**Примечание 2.** Вызов метода `d.new_child()` эквивалентен вызову `ChainMap({}, *d.maps)`.

**Примечание 3.** Обращение к атрибуту `d.parents` эквивалентно вызову `ChainMap(*d.maps[1:])`.

**Примечание 4.** Два объекта типа `ChainMap` `chainmap1` и `chainmap2` считаются равными, если значение следующего выражения равно `True`:

```python
dict(chainmap1.items()) == dict(chainmap2.items())
```

Учитывая специфику работы метода `items()`, равенство двух объектов типа `ChainMap` не гарантирует того, что эти объекты в точности совпадают.

Приведенный ниже код:

```python
from collections import ChainMap

chainmap1 = ChainMap({'a': 10, 'b': 20})
chainmap2 = ChainMap({'a': 10, 'b': 20})

print(chainmap1 == chainmap2)

chainmap1 = ChainMap({'a': 10, 'b': 20}, {'a': 1, 'b': 2})
chainmap2 = ChainMap({'a': 10, 'b': 20})

print(chainmap1 == chainmap2)
```

выводит:

```no-highlight
True
True
```

**Примечание 5.** Тип данных `ChainMap` удобен в том случае, когда мы уже имеем некоторую коллекцию с большим количеством словарей и нам требуется производить поиск по всем словарям одновременно.

Приведенный ниже код:

```python
from collections import ChainMap

dicts = [{'math': 4, 'physics': 5, 'geometry': 4},
         {'biology': 3, 'chemistry': 3},
         {'literature': 4, 'languages': 4},
         {'history': 3, 'music': 3}]

lessons = ChainMap(*dicts)

print(lessons['literature'])
```

выводит:

```python
4
```

