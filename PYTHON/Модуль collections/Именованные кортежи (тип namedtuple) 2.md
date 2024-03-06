## Атрибуты _fields, _field_defaults

Именованные кортежи имеют два дополнительных атрибута: `_fields` и `_field_defaults`. Первый атрибут содержит кортеж строк, в котором перечислены имена полей. Второй атрибут содержит словарь, который сопоставляет имена полей с соответствующими значениями по умолчанию, если таковые имеются.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

tim = Person('Тимур', 29, 170)

print(tim)
print(tim._fields)
print(Person._fields)
```

выводит:

```no-highlight
Person(name='Тимур', age=29, height=170)
('name', 'age', 'height')
('name', 'age', 'height')
```

Обратите внимание на то, что мы можем обращаться к атрибуту `_fields` как через переменную (`tim`), так и через сам тип именованного кортежа (`Person`).

С помощью атрибута `_fields` мы можем создавать новые именованные кортежи на основании уже существующих. В следующем примере мы создадим новый именованный кортеж с именем `ExtendedPerson`, который расширяет старый `Person` новым полем `weight`.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

ExtendedPerson = namedtuple('ExtendedPerson', [*Person._fields, 'weight'])  # распаковка полей старого кортежа

timur = ExtendedPerson('Тимур', 29, 170, 65)

print(timur)
print(ExtendedPerson._fields)
```

выводит:

```no-highlight
ExtendedPerson(name='Тимур', age=29, height=170, weight=65)
('name', 'age', 'height', 'weight')
```

Мы также можем использовать атрибут `_fields` для перебора полей и их значений с помощью встроенной функции `zip()`:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

timur = Person('Тимур', 29, 170)

for field, value in zip(Person._fields, timur):
    print(field, '->', value)
```

выводит:

```no-highlight
name -> Тимур
age -> 29
height -> 170
```

С помощью атрибута `_field_defaults` мы можем выяснить, какие поля именованного кортежа имеют значения по умолчанию. Значения по умолчанию делают поля необязательными. Например, предположим, что наш именованный кортеж `Person` должен включать дополнительное поле для хранения страны, в которой живет человек. Поскольку в основном мы работаем с людьми из России, то мы устанавливаем соответствующее значение по умолчанию для поля страны следующим образом:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'], defaults=['Russia'])

timur = Person('Тимур', 29, 170)

print(timur)
print(timur._field_defaults)
print(Person._field_defaults)
```

Приведенный выше код выводит:

```no-highlight
Person(name='Тимур', age=29, height=170, country='Russia')
{'country': 'Russia'}
{'country': 'Russia'}
```

Если именованный кортеж не предоставляет значений по умолчанию, тогда атрибут `_field_defaults` содержит пустой словарь.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'])

timur = Person('Тимур', 29, 170, 'Russia')

print(Person._field_defaults)
```

выводит:

```no-highlight
{}
```

## Методы _make(), _replace(), _asdict()

Напомним, что обычные кортежи (`tuple`) имеют два встроенных метода:

- `index()`: возвращает индекс первого элемента, значение которого равняется переданному значению
- `count()`: возвращает количество элементов в кортеже, значения которых равны переданному значению

Именованные кортежи (тип `namedtuple`) являются производными от обычных кортежей (тип `tuple`), поэтому наследует их методы, а также добавляют три новых: `_make(), _replace(), _asdict()`.

Имена новых методов (`_make(), _replace(), _asdict()`) и атрибутов (`_fields, _field_defaults`) начинаются с подчеркивания,  чтобы предотвратить конфликты имен с полями именованных кортежей.

### Метод _make()

Метод `_make()` используется для создания именованных кортежей из итерируемых объектов (список, кортеж, строка, словарь и т.д.).

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

timur = Person._make(['Timur', 29, 170])

print(timur)
```

выводит:

```no-highlight
Person(name='Timur', age=29, height=170)
```

Обратите внимание на то, что метод `_make()` – это метод типа, а не конкретного экземпляра, поэтому вызывать его нужно через название типа (`Person._make`). Метод `_make()` работает как альтернативный конструктор типа.

### Метод _asdict()

Мы можем преобразовывать именованные кортежи в словари с помощью метода `_asdict()`. Этот метод возвращает словарь, в котором имена полей используются в качестве ключей. Ключи результирующего словаря находятся в том же порядке, что и поля в исходном именованном кортеже.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

timur = Person._make(['Timur', 29, 170])

print(timur._asdict())
```

выводит:

```no-highlight
{'name': 'Timur', 'age': 29, 'height': 170}
```

Python 3.8 метод `_asdict()` возвращал тип данных `OrderedDict`. В настоящий момент метод возвращает обычный словарь (тип `dict`), так как сейчас словари запоминают порядок добавления в них ключей.

### Метод _replace()

Метод `_replace()` позволяет создавать новые именованные кортежи на основании уже существующих с заменой некоторых значений. Потребность в данном методе вызвана тем, что именованные кортежи являются неизменяемыми.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'])

timur1 = Person('Тимур', 29, 170, 'Russia')
timur2 = timur1._replace(age=30, country='Germany')

print(timur1)
print(timur2)
```

выводит:

```no-highlight
Person(name='Тимур', age=29, height=170, country='Russia')
Person(name='Тимур', age=30, height=170, country='Germany')
```

Обратите внимание на то, что метод `_replace()` не изменяет текущий именованный кортеж, а возвращает новый.
