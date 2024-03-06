Класс `LocalTime` создан для случаев, когда нужно работать только со временем без даты. Например, вы пишете приложение-будильник. Время для вас важно, а дата — нет.

Класс `LocalTime` очень похож на класс `LocalDate`: его объекты тоже нельзя изменять после создания.

**Получение текущего времени**

Чтобы создать новый объект класса `LocalTime`, нужно воспользоваться статическим методом `now()`. Пример:

```java
LocalTime time = LocalTime.now();
```

Где `time` — это переменная класса `LocalTime`, а `LocalTime.now()` — вызов статического метода `now()` у класса `LocalTime`.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalTime time = LocalTime.now();<br>System.out.println("Сейчас = " + time);<br>```|```<br><br>Сейчас = 09:13:13.642881600<br>```|

После точки указывается текущее значение наносекунд.

## 2. Получение заданного времени

Чтобы получить заданное время, нужно воспользоваться статическим методом `of()`. Пример:

```java
LocalTime time = LocalTime.of(часы, минуты, секунды, наносекунды);
```

В который можно передать соответственно часы, минуты, секунды и наносекунды.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalTime time = LocalTime.of(12, 15, 0, 100);<br>System.out.println("Сейчас = " + time);<br>```|```<br>Сейчас = 12:15:00.000000100<br>```|

Есть, кстати, еще две модификации этого метода:

```java
LocalTime time = LocalTime.of(часы, минуты, секунды);
```

И

```java
LocalTime time = LocalTime.of(часы, минуты);
```

Так что можете пользоваться каким вам удобнее.

**Получение времени по номеру секунды**

Также можно получить время по номеру секунды в сутках: для этого есть специальный статический метод `ofSecondOfDay()`:

```java
LocalTime time = LocalTime.ofSecondOfDay(секунды);
```

Где **секунды** — это количество секунд, прошедшее с начала суток.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalTime time = LocalTime.ofSecondOfDay(10000);<br>System.out.println(time);<br>```|```<br><br>02:46:40<br>```|

Да,10 тысяч секунд — это чуть меньше трех часов. Все верно.

## 3. Получение фрагментов времени

Чтобы из объекта `LocalTime` получить значение определенного элемента времени, используют специальные методы:

|Метод|Описание|
|---|---|
|```java<br>int getHour()<br>```|Возвращает часы|
|```java<br>int getMinute()<br>```|Возвращает минуты|
|```java<br>int getSecond()<br>```|Возвращает секунды|
|```java<br>int getNano()<br>```|Возвращает наносекунды|

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalTime now = LocalTime.now();<br>System.out.println(now.getHour());<br>System.out.println(now.getMinute());<br>System.out.println(now.getSecond());<br>System.out.println(now.getNano());<br>```|```<br><br>2<br>46<br>40<br>0<br>```|

## 4. Изменение времени в объекте класса `LocalTime`

Класс `LocalTime` также содержит методы, которые позволяют работать со временем. Эти методы реализованы по аналогии с методами класса `LocalDate`: каждый из них не меняет существующий объект `LocalTime`, а возвращает новый с нужными данными.

Вот какие методы есть у класса `LocalTime`:

|Метод|Описание|
|---|---|
|```java<br>plusHours(int hours)<br>```|Добавляет часы|
|```java<br>plusMinutes(int minutes)<br>```|Добавляет минуты|
|```java<br>plusSeconds(int seconds)<br>```|Добавляет секунды|
|```java<br>plusNanos(int nanos)<br>```|Добавляет наносекунды|
|```java<br>minusHours(int hours)<br>```|Вычитает часы|
|```java<br>minusMinutes(int minutes)<br>```|Вычитает минуты|
|```java<br>minusSeconds(int seconds)<br>```|Вычитает секунды|
|```java<br>minusNanos(int nanos)<br>```|Вычитает наносекунды|

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalTime time = LocalTime.now();<br>LocalTime time2 = time.plusHours(2);<br>LocalTime time3 = time.minusMinutes(40);<br>LocalTime time4 = time.plusSeconds(3600);<br><br>System.out.println(time);<br>System.out.println(time2);<br>System.out.println(time3);<br>System.out.println(time4);<br>```|```<br><br><br><br><br><br>10:33:55.978012200<br>12:33:55.978012200<br>09:53:55.978012200<br>11:33:55.978012200<br>```|

Обратите внимание, что каждый раз мы получаем новое время относительно первого объекта `time`. Если добавить ко времени `3600 секунд`, это будет ровно `1 час`.
