Полное имя класса `Calendar` — java.util.Calendar. Не забывайте добавлять его в import, если решите использовать в своем коде.

Создать объект `Calendar` можно командой:

```java
Calendar date = Calendar.getInstance();
```

Статический метод `getInstance()` класса `Calendar` создаст объект `Calendar`, инициализированный текущей датой. В зависимости от настроек компьютера, на котором выполняется программа, будет создан нужный календарь.

## Создание объекта календарь
Объект календарь с произвольной датой создается командой:

```java
Calendar date = new GregorianCalendar(год, месяц, день);
```

Да, каждый раз придется писать `GregorianCalendar`. Можно и вместо `Calendar` писать `GregorianCalendar`: так тоже будет работать. Но просто `Calendar` писать короче.

Год нужно писать полностью: никаких 19 вместо 2019. Месяцы по-прежнему нумеруются с нуля. А дни месяца по-прежнему нумеруются не с нуля (слабаки!).

Чтобы задать не только дату, но и время, нужно передать их дополнительными параметрами:

```java
... = new GregorianCalendar(год, месяц, день, часы, минуты, секунды);
```

Можно даже передать миллисекунды, если это необходимо: их указывают следующим параметром после секунд.
## Вывод объекта календарь на экран
Правильно будет отображать объект календарь с помощью класса `SimpleDateFormat`, но пока мы его не изучили, можно использовать лайфхак.

```java
Date date = calendar.getTime();
```

Дело в том, что объект типа `Calendar` можно легко преобразовать к объекту типа `Date`, а как выводить на экран объект типа `Date`, вы уже знаете. Примерно так можно преобразовать объект `Calendar` в `Date`:

Использование метода `getTime()`:

|Код|Вывод на экран|
|---|---|
|```java<br>Calendar calendar = new GregorianCalendar(2019, 03, 12);<br>System.out.println(calendar.getTime());<br>```|```<br> Fri Apr 12 00:00:00 EEST 2019<br>```|

## Работа с фрагментами даты

Чтобы получить фрагмент даты (год, месяц, ...), у класса `Calendar` есть специальный метод — `get()`. Метод один, зато с параметрами:

```java
int month = calendar.get(Calendar.MONTH);
```

Где `calendar` — это переменная типа `Calendar`, а `MONTH` — это переменная-константа класса `Calendar`.

В метод `get` в качестве параметра вы передаете специальную константу класса `Calendar`, и в результате получаете нужное значение.

Примеры

|Код|Описание|
|---|---|
|```java<br>Calendar calendar = Calendar.getInstance();<br><br>int era = calendar.get(Calendar.ERA);<br>int year = calendar.get(Calendar.YEAR);<br>int month = calendar.get(Calendar.MONTH);<br>int day = calendar.get(Calendar.DAY_OF_MONTH);<br><br>int dayOfWeek = calendar.get(Calendar.DAY_OF_WEEK);<br>int hour = calendar.get(Calendar.HOUR);<br>int minute = calendar.get(Calendar.MINUTE);<br>int second = calendar.get(Calendar.SECOND);<br>```|эра (до нашей эры или после)  <br>год  <br>месяц  <br>день месяца  <br>  <br>день недели  <br>часы  <br>минуты  <br>секунды|

Для изменения фрагмента даты используется метод `set`:

```java
calendar.set(Calendar.MONTH, значение);
```

Где `calendar` — это переменная типа `Calendar`, а `MONTH` — это переменная-константа класса `Calendar`.

В метод `set` в качестве первого параметра вы передаете специальную константу класса `Calendar`, а в качестве второго параметра — новое значение.

Примеры

|Код|Описание|
|---|---|
|```java<br>Calendar calendar = new GregorianCalendar();<br><br>calendar.set(Calendar.YEAR, 2019);<br>calendar.set(Calendar.MONTH, 6);<br>calendar.set(Calendar.DAY_OF_MONTH, 4);<br>calendar.set(Calendar.HOUR_OF_DAY, 12);<br>calendar.set(Calendar.MINUTE, 15);<br>calendar.set(Calendar.SECOND, 0);<br><br>System.out.println(calendar.getTime());<br>```|год = 2019  <br>месяц = Июль (нумерация с 0)  <br>4 число  <br>часы  <br>минуты  <br>секунды|

## 5. Константы класса `Calendar`

В классе `Calendar` есть константы не только для названия фрагментов даты, а кажется, на все случаи жизни.

```java
Calendar date = new GregorianCalendar(2019, Calendar.JANUARY, 31);
```

Например, есть константы для обозначения месяцев:

Или, например, для дней недели:

```java
Calendar calendar = new GregorianCalendar(2019, Calendar.JANUARY, 31);
if (calendar.get(Calendar.DAY_OF_WEEK) == Calendar.FRIDAY)
{
   System.out.println("Это пятница");
}
```

Всего перечислять не будем. Просто хотим, чтобы вы не удивлялись, если встретите такие записи в коде.

Использование констант позволяет сделать код более читабельным, поэтому программисты их и добавили. Ну а месяцы нумеруются с нуля тоже ради читабельности. Или нет.

## 6. Изменение даты в объекте `Calendar`

У класса `Calendar` есть метод, который позволяет проводить с датой более умные операции. Например, добавить к дате год, месяц или несколько дней. Или отнять. Называется этот метод `add()`. Выглядит работа с ним примерно так:

```java
calendar.add(Calendar.MONTH, значение);
```

Где `calendar` — это переменная типа `Calendar`, а `MONTH` — это переменная-константа класса `Calendar`.

В метод `add` в качестве первого параметра вы передаете специальную константу класса `Calendar`, и в качестве второго параметра — значение, которое нужно добавить.

Особенность этого метода в том, что он умный. Давайте сами посмотрим, насколько:

|Код|
|---|
|```java<br>Calendar calendar = new GregorianCalendar(2019, Calendar.FEBRUARY, 27);<br>calendar.add(Calendar.DAY_OF_MONTH, 2);<br>System.out.println(calendar.getTime());<br>```|
|Вывод на экран|
|```<br>Fri Mar 01 00:00:00 EET 2019<br>```|

Этот метод понимает, что в феврале 2019 года всего 28 дней, и итоговая дата — 1 марта.

А теперь давайте отнимем 2 месяца! Что должно получиться? 27 декабря 2018 года! Сейчас проверим.

Чтобы выполнить действие, уменьшающее дату, нужно в метод `add()` передать значение с отрицательным знаком:

|Код|
|---|
|```java<br>Calendar calendar = new GregorianCalendar(2019, Calendar.FEBRUARY, 27);<br>calendar.add(Calendar.MONTH, -2);<br>System.out.println(calendar.getTime());<br>```|
|Вывод на экран|
|```<br>Thu Dec 27 00:00:00 EET 2018<br>```|

Работает!

Этот метод учитывает длины месяцев и високосные годы. В общем, отличный метод. Именно то, что нужно большинству программистов, которые плотно работают с датами.

## 7. Прокручивание фрагментов даты

Но иногда бывают ситуации, когда такое умное поведение излишне: хочется что-то сделать с одной частью даты, не меняя все остальное.

Для этого у класса `Calendar` есть специальный метод — `roll()`. По своей сигнатуре он полностью аналогичен методу `add()`, но любые изменения с его помощью затрагивают один параметр, остальные остаются неизменными.

Пример:

|Код|
|---|
|```java<br>Calendar calendar = new GregorianCalendar(2019, Calendar.FEBRUARY, 27);<br>calendar.roll(Calendar.MONTH, -2);<br>System.out.println(calendar.getTime());<br>```|
|Вывод на экран|
|```<br>Fri Dec 27 00:00:00 EET 2019<br>```|

Месяц мы поменяли, а год и число остались неизменными.

---

