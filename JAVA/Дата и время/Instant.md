Также разработчики Java не забыли интересы и старой школы.

В Date Time API добавили класс Instant для работы со временем, нацеленным на процессы внутри компьютеров. Вместо часов, минут и секунд, он оперирует **секундами, миллисекундами и наносекундами.**

Этот класс хранит в себе два поля:

- **количество секунд**, прошедшее с 1 января 1970 года
- **количество наносекунд**

Класс создан для разработчиков? Да. Поэтому он опять считает время в Unix-time: от начала 1970 года.

Можно даже сказать, что класс `Instant` — это упрощенная версия класса `Date`: оставили только то, что нужно программистам.

Получить объект `Instant` можно точно так же, как объект `LocalTime`:

```java
Instant timestamp = Instant.now();
```

Где `timestamp` — это переменная класса `Instant`, а `Instant.now()` — вызов статического метода `now()` у класса `Instant`.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>Instant timestamp = Instant.now();<br>System.out.println(timestamp);<br>```|```<br><br>2019-02-22T08:42:42.234945300Z<br>```|

Также можно создать новый объект с помощью разновидностей метода `of()`, если передать в него время, прошедшее с 1 января 1970 года:

|   |   |
|---|---|
|```java<br>ofEpochMilli(long milliseconds)<br>```|Нужно передать количество миллисекунд|
|```java<br>ofEpochSecond(long seconds)<br>```|Нужно передать количество секунд|
|```java<br>ofEpochSecond(long seconds, long nanos)<br>```|Нужно передать секунды и наносекунды|

**Методы объектов `Instant`**

У класса Instant есть два метода, которые возвращают его значения:

|   |   |
|---|---|
|```java<br>long getEpochSecond()<br>```|Количество секунд, прошедшее с 1 января 1970 года|
|```java<br>int getNano()<br>```|Наносекунды.|
|```java<br>long toEpochMilli()<br>```|Количество миллисекунд, прошедшее с 1 января 1970 года|

Также есть методы, которые способны создавать новые объекты `Instant` на основе уже существующего:

|   |   |
|---|---|
|```java<br>Instant plusSeconds(long)<br>```|Добавляет секунды к текущему моменту времени|
|```java<br>Instant plusMillis(long)<br>```|Добавляет миллисекунды|
|```java<br>Instant plusNanos(long)<br>```|Добавляет наносекунды|
|```java<br>Instant minusSeconds(long)<br>```|Вычитает секунды|
|```java<br>Instant minusMillis(long)<br>```|Вычитает миллисекунды|
|```java<br>Instant minusNanos(long)<br>```|Вычитает наносекунды|

Примеры:

|Код|Вывод на экран|
|---|---|
|```java<br>Instant timestamp = Instant.now();<br>System.out.println(timestamp);<br><br>long n = timestamp.toEpochMilli();<br>Instant time = Instant.ofEpochMilli(n);<br>System.out.println(time);<br>```|```<br><br>2019-02-22T09:01:20.535344Z<br><br><br><br>2019-02-22T09:01:20.535Z<br>```|

