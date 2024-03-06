1. Тип данных `datetime`
2. Методы `combine(), date(), time()`
3. Методы `now(), utcnow(), today()`
4. Метод `timestamp()`
5. Форматирование даты-времени
6. Преобразование строки в дату-время

**Аннотация.** Урок посвящен типу данных `datetime` из модуля `datetime`.

## Тип данных datetime

Типы данных `date` и `time` позволяют работать по отдельности с датами и временами. Однако на практике чаще требуется работать одновременно и с датой, и со временем. Для таких целей используется тип данных `datetime` из одноименного модуля `datetime`.

![](https://ucarecdn.com/2158ef20-52ba-49cd-94df-1a5d17763b33/)

Чтобы иметь возможность использовать этот тип данных, необходимо предварительно его импортировать из модуля `datetime`:

```
from datetime import datetime
```

При создании новой даты-времени (тип `datetime`) нужно указать год, месяц, день, часы, минуты, секунды и микросекунды. При этом год, месяц и день являются **обязательными**, а часы, минуты, секунды и микросекунды **необязательными**.

Приведенный ниже код:

```python
from datetime import datetime

my_datetime = datetime(1992, 10, 6, 9, 40, 23, 51204)    # создаем полную дату-время
only_date = datetime(2021, 12, 31)                       # создаем дату-время с нулевой временной информацией

print(my_datetime)
print(only_date)
print(type(my_datetime))
```

выводит:

```no-highlight
1992-10-06 09:40:23.051204
2021-12-31 00:00:00
<class 'datetime.datetime'>
```

Конструктор типа `datetime` сначала принимает год, месяц, день, часы, минуты, секунды, а уже потом микросекунды. Мы также можем использовать именованные аргументы, нарушая указанный порядок `datetime(day=6, month=10, year=1992, second=23, minute=40, microsecond=51204, hour=9)`.

Так же, как и при работе с типами `date` и `time`, пользуясь типом `datetime`, можно получать доступ к отдельным значениям созданной даты-времени: годам, месяцам, дням, часам, минутам, секундам и микросекундам. Получить доступ к ним можно с помощью атрибутов:

- `year` — год
- `month` — месяц
- `day` — день
- `hour` — час
- `minute` — минуты
- `second` — секунды
- `microsecond` — микросекунды

Приведенный ниже код:

```python
from datetime import datetime

my_datetime = datetime(1992, 10, 6, 9, 40, 23, 51204)

print('Год =', my_datetime.year)
print('Месяц =', my_datetime.month)
print('День =', my_datetime.day)
print('Часы =', my_datetime.hour)
print('Минуты =', my_datetime.minute)
print('Секунды =', my_datetime.second)
print('Микросекунды =', my_datetime.microsecond)
```

выводит:

```no-highlight
Год = 1992
Месяц = 10
День = 6
Часы = 9
Минуты = 40
Секунды = 23
Микросекунды = 51204
```

### Методы combine(), date(), time()

Сформировать новый объект типа `datetime` можно с помощью двух разных объектов, представляющих дату и время (`date` и `time`). Для этого используется метод `combine()`.

Приведенный ниже код:

```python
from datetime import date, time, datetime

my_date = date(1992, 10, 6)
my_time = time(10, 45, 17)
my_datetime = datetime.combine(my_date, my_time)

print(my_datetime)
```

выводит:

```no-highlight
1992-10-06 10:45:17
```

Если, наоборот, нужно получить из даты-времени (тип `datetime`) по отдельности дату (тип `date`) и время (тип `time`), то используются методы `date()` и `time()` соответственно.

Приведенный ниже код:

```python
from datetime import datetime

my_datetime = datetime(2022, 10, 7, 14, 15, 45)
my_date = my_datetime.date()                     # получаем только дату (тип date)
my_time = my_datetime.time()                     # получаем только время (тип time)

print(my_datetime, type(my_datetime))
print(my_date, type(my_date))
print(my_time, type(my_time))
```

выводит:

```no-highlight
2022-10-07 14:15:45 <class 'datetime.datetime'>
2022-10-07 <class 'datetime.date'>
14:15:45 <class 'datetime.time'>
```

### Методы now(), utcnow(), today()

Для того, что получить текущее время на момент исполнения программы, используются методы `now()` и `utcnow()` для локального и UTC времени соответственно.

Приведенный ниже код:

```python
from datetime import datetime

datetime_now = datetime.now()
datetime_utcnow = datetime.utcnow()

print(datetime_now)           # текущее локальное время (московское) на момент запуска программы
print(datetime_utcnow)        # текущее UTC время на момент запуска программы
```

выводит:

```no-highlight
2021-08-13 08:03:43.224568
2021-08-13 05:03:43.224568
```

**Всемирное координированное время** (Coordinated Universal Time, UTC) — [стандарт](https://ru.wikipedia.org/wiki/%D0%92%D1%81%D0%B5%D0%BC%D0%B8%D1%80%D0%BD%D0%BE%D0%B5_%D0%BA%D0%BE%D0%BE%D1%80%D0%B4%D0%B8%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%B2%D1%80%D0%B5%D0%BC%D1%8F), по которому общество регулирует часы и время. Московское время соответствует [UTC+3](https://ru.wikipedia.org/wiki/UTC%2B03:00 "Часовой пояс").

Метод `today()` аналогичен методу `now()`. Для получения локальной даты-времени рекомендуется использовать именно метод `now()`.

### Метод timestamp()

Метод `timestamp()` позволяет преобразовать объект типа `datetime` в количество секунд, прошедших с момента начала эпохи. Данный метод возвращает значение типа `float`.
Начало эпохи — это полночь 11 января 19701970 года (00:00:00 UTC).

Приведенный ниже код:

```python
from datetime import datetime

my_datetime = datetime(2021, 10, 13, 8, 10, 23)

print(my_datetime.timestamp())
```

выводит:

```no-highlight
1634101823.0
```

### Метод fromtimestamp()

Метод `fromtimestamp()` позволяет преобразовать количество секунд, прошедших с момента начала эпохи, в объект типа `datetime`. Данный метод является обратным по отношению к методу `timestamp()`.

Приведенный ниже код:

```python
from datetime import datetime

my_datetime = datetime.fromtimestamp(1634101823.0)

print(my_datetime)
```

выводит:

```no-highlight
2021-10-13 08:10:23
```

Обратите внимание на то, что метод `fromtimestamp()` по умолчанию возвращает объект `datetime` в вашем часовом поясе.

### Форматирование даты-времени

По умолчанию объекты типа `datetime` (как и объекты типа `date` и `time`) выводятся в специальном формате, который называется ISO 8601. Данное представление не всегда удовлетворяет нашим запросам.

Чтобы преобразовать дату-время в строку нужного формата, следует воспользоваться методом `strftime()`, указав ему в качестве аргумента параметры форматирования.

```python
from datetime import datetime

my_datetime = datetime(2021, 8, 10, 18, 20, 34)

print(my_datetime)                                            # вывод в ISO формате
print(my_datetime.strftime('%d.%m.%y --- %H::%M::%S'))
print(my_datetime.strftime('%d/%m/%y'))
print(my_datetime.strftime('%A %d, %B %Y'))
print(my_datetime.strftime('%H:%M:%S'))
```

выводит

```no-highlight
2021-08-10 18:20:34
10.08.21 --- 18::20::34
10/08/21
Tuesday 10, August 2021
18:20:34
```

Для форматированного вывода объектов типов `date, time, datetime` используется один и тот же метод `strftime()`.

Для форматирования даты-времени (`datetime`) нам с вами потребуется уже знакомая таблица.

|Формат|Значение|Пример|
|---|---|---|
|`%a`|Сокращенное название дня недели|Sun, Mon, …, Sat (en_US)  <br>Пн, Вт, ..., Вс (ru_RU)|
|`%A`|Полное название дня недели|Sunday, Monday, …, Saturday (en_US)  <br>понедельник, ..., воскресенье (ru_RU)|
|`%w`|Номер дня недели [0, …, 6]|0, 1, …, 6 (0=воскресенье, 6=суббота)|
|`%d`|День месяца [01, …, 31]|01, 02, …, 31|
|`%b`|Сокращенное название месяца|Jan, Feb, …, Dec (en_US);  <br>янв, ..., дек (ru_RU)|
|`%B`|Полное название месяца|January, February, …, December (en_US);  <br>Январь, ..., Декабрь (ru_RU)|
|`%m`|Номер месяца [01, …,12]|01, 02, …, 12|
|`%y`|Год без века [00, …, 99]|00, 01, …, 99|
|`%Y`|Год с веком|0001, 0002, …, 2013, 2014, …, 9999  <br>В Linux год выводится без ведущих нулей:  <br>1, 2, …, 2013, 2014, …, 9999|
|`%H`|Час (24-часовой формат) [00, …, 23]|00, 01, …, 23|
|`%I`|Час (12-часовой формат) [01, …, 12]|01, 02, …, 12|
|`%p`|До полудня или после (при 12-часовом формате)|AM, PM (en_US)|
|`%M`|Число минут [00, …, 59]|00, 01, …, 59|
|`%S`|Число секунд [00, …, 59]|00, 01, …, 59|
|`%f`|Число микросекунд|000000, 000001, …, 999999|
|`%z`|Разница с UTC в формате ±HHMM[SS[.ffffff]]|+0000, -0400, +1030, +063415, ...|
|`%Z`|Временная зона|UTC, EST, CST|
|`%j`|День года [001,366]|001, 002, …, 366|
|`%U`|Номер недели в году  <br>(нулевая неделя начинается с воскр.) [00, …, 53]|00, 01, …, 53|
|`%W`|Номер недели в году  <br>(нулевая неделя начинается с пон.) [00, …, 53]|00, 01, …, 53|
|`%c`|Дата и время|Tue Aug 16 21:30:00 1988 (en_US);  <br>03.01.2019 23:18:32 (ru_RU)|
|`%x`|Дата|08/16/88 (None); 08/16/1988 (en_US);  <br>03.01.2019 (ru_RU)|
|`%X`|Время|21:30:00|

## Преобразование строки в дату-время

Как уже было сказано в прошлом уроке, преобразовать данные из строки в объект типа `datetime` можно двумя способами:

- ручным преобразованием
- с помощью метода `strptime()`

Ручной подход основан на использовании метода `split()`:

```python
from datetime import datetime

datetime_str = input('Введите дату/время в формате ДД.ММ.ГГГГ ЧЧ:ММ:СС')

date_str, time_str = datetime_str.split(' ')

date_info = [int(i) for i in date_str.split('.')]
time_info = [int(i) for i in time_str.split(':')]

my_datetime = datetime(date_info[2], date_info[1], date_info[0], time_info[0], time_info[1], time_info[2])

print(my_datetime)
```

На практике для преобразования строки в объект `datetime` редко используется ручной подход. Вместо него используется метод `strptime()`, который преобразует строку (первый аргумент) в объект `datetime` согласно переданному формату (второй аргумент).

Приведенный ниже код работает аналогично коду выше:

```python
from datetime import datetime

datetime_str = input('Введите дату/время в формате ДД.ММ.ГГГГ ЧЧ:ММ:СС')

my_datetime = datetime.strptime(datetime_str, '%d.%m.%Y %H:%M:%S')

print(my_datetime)
```

Рассмотрим примеры работы данного метода.

Приведенный ниже код:

```python
from datetime import datetime

datetime0 = datetime.strptime('10.08.2034 13:55:59', '%d.%m.%Y %H:%M:%S')
datetime1 = datetime.strptime('10/08/21', '%d/%m/%y')
datetime2 = datetime.strptime('Tuesday 10, August 2021', '%A %d, %B %Y')
datetime3 = datetime.strptime('18.20.34', '%H.%M.%S')
datetime4 = datetime.strptime('2021/08/10', '%Y/%m/%d')
datetime5 = datetime.strptime('10.08.2021 (Tuesday, August)', '%d.%m.%Y (%A, %B)')
datetime6 = datetime.strptime('Year: 2021, Month: 08, Day: 10, Hour: 18.', 'Year: %Y, Month: %m, Day: %d, Hour: %H.')

print(datetime0, datetime1, datetime2, datetime3, datetime4, datetime5, datetime6, sep='\n')
```

выводит:

```no-highlight
2034-08-10 13:55:59
2021-08-10 00:00:00
2021-08-10 00:00:00
1900-01-01 18:20:34
2021-08-10 00:00:00
2021-08-10 00:00:00
2021-08-10 18:00:00
```

Обратите внимание на то, что первый аргумент должен соответствовать формату второго аргумента. Если он не соответствует, то возникает исключение `ValueError`.

В результате выполнения кода:

```python
from datetime import datetime

my_datetime = datetime.strptime('10/08/2034 13:55:59', '%d.%m.%Y %H:%M:%S')

print(my_datetime)
```

мы получим:

```no-highlight
ValueError: time data '10/08/2034 13:55:59' does not match format '%d.%m.%Y %H:%M:%S'
```

## Примечания

**Примечание 1.** Тип данных `datetime` наследует весь функционал (атрибуты и методы) от типа `date`.

**Примечание 2.** Тип данных `datetime` является неизменяемым. Мы можем создать множества, содержащие объекты данного типа, а также они могут выступать в качестве ключей словаря.

**Примечание 3.** Мы можем использовать встроенные функции `min(), max(), sorted()` и т.д. при работе с объектами типа `datetime`.

**Примечание 4.** Объекты типа `datetime` можно сравнивать (`==, !=, <, >, <=, >=`), как и большинство других типов данных.

**Примечание 5.** При создании объекта `datetime` из строки с помощью метода `strptime()` необязательно, чтобы строка содержала год, месяц и день, в отличие от ручного создания объекта `datetime`.

Приведённый ниже код:

```python
from datetime import datetime

d = datetime.strptime('9:00', '%H:%M')
print(d)
```

Не приведёт к возбуждению исключения, а выведет:

```no-highlight
1900-01-01 09:00:00
```

**Примечание 6.** Для создания новой даты-времени на основании уже существующей можно использовать метод `replace()`. Он возвращает новый объект `datetime` с переданными измененными значениями атрибутов.

Приведенный ниже код:

```python
from datetime import datetime

datetime1 = datetime(1992, 10, 6, 10, 12, 45)
datetime2 = datetime1.replace(year=1995, month=12)         
datetime3 = datetime1.replace(day=17, hour=14, minute=37)

print(datetime1)
print(datetime2)
print(datetime3)
```

выводит:

```no-highlight
1992-10-06 10:12:45
1995-12-06 10:12:45
1992-10-17 14:37:45
```

**Примечание 7.** Помните про ограничения на `year, month, day, hour, minute, second, microsecond`, которые используете для создания объекта типа `datetime`. В случае использования неверного значения возникнет ошибка (исключение) `ValueError`.

Ограничения:

```no-highlight
1 <= year <= 9999
1 <= month <= 12
1 <= day <= количество дней в заданом месяце и году
0 <= hour < 24
0 <= minute < 60
0 <= second < 60
0 <= microsecond < 1000000
```

**Примечание 8.** Для того, чтобы использовать конкретную локализацию (перевод на язык), нужно использовать модуль `locale`. Приведенный ниже код устанавливает русскую локализацию:

```python
from datetime import datetime
import locale

locale.setlocale(locale.LC_ALL, 'ru_RU.UTF-8')

my_datetime = datetime(2021, 8, 10, 12, 34, 15)

print(my_datetime.strftime('%A %d, %B %Y, %H:%M:%S'))
```

и выводит:

```no-highlight
вторник 10, Август 2021, 12:34:15
```

Для установки английской локализации используется код:

```python
import locale

locale.setlocale(locale.LC_ALL, 'en_EN.UTF-8')
```

Почитать подробнее о модуле `locale` можно в официальной документации [тут](https://docs.python.org/3/library/locale.html) (английский язык) или [тут](https://docs-python.ru/standart-library/modul-locale-python/) (русский язык).

**Примечание 9.** Документация по модулю `datetime` доступна по [ссылке](https://docs.python.org/3/library/datetime.html).

**Примечание 10.** По умолчанию объекты типа `datetime` выводятся в ISO 8601 формате: `YYYY-MM-DD HH:MM:SS.ffffff`.

**Примечание 11.** Для того, чтобы получить строковое представление объекта `datetime` в ISO формате, можно воспользоваться методом `isoformat()` или встроенной функцией `str()`.

Приведенный ниже код:

```python
from datetime import datetime

my_datetime = datetime(1992, 10, 6, 10, 12, 45)

print(my_datetime.isoformat())
print(str(my_datetime))
```

выводит:

```no-highlight
1992-10-06T10:12:45
1992-10-06 10:12:45
```

Обратите внимание на символ `T`, который вставляется между датой и временем при использовании метода `isoformat()` для `datetime` объектов.
