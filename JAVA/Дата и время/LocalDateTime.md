Класс `LocalDateTime` объединяет в себе возможности классов `LocalDate` и `LocalTime`: он хранит и дату, и время. Его объекты тоже неизменяемые, и его методы также аналогичны методам классов `LocalDate` и `LocalTime`.

**Получение текущего момента: дата и время**

Тут все в пределах ожиданий: тоже используется метод `now()`. Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalDateTime time = LocalDateTime.now();<br>System.out.println("Сейчас = " + time);<br>```|```<br><br>Сейчас = 2019-02-22T09:49:19.275039200<br>```|

При выводе на экран дата и время разделяются буквой `T`.

**Получение определенного момента: дата и время**

Все ожидаемо аналогично классам `LocalDate` и `LocalTime` — используется метод `of()`:

```java
... = LocalDateTime.of(год, месяц, день, часы, минуты, секунды);
```

Сначала идут параметры, которые задают дату в тех же форматах, что и в классе `LocalDate`. Затем идут параметры, задающие время в тех же форматах, что и в классе `LocalTime`. Список всех вариаций метода `of()` приведен ниже:

|Методы|
|---|
|```java<br>of (int year, int month, int day, int hour, int minute)<br>```|
|```java<br>of (int year, int month, int day, int hour, int minute, int second)<br>```|
|```java<br>of (int year, int month, int day, int hour, int minute, int second, int nano)<br>```|
|```java<br>of (int year, Month month, int day, int hour, int minute)<br>```|
|```java<br>of (int year, Month month, int day, int hour, int minute, int second)<br>```|
|```java<br>of (int year, Month month, int day, int hour, int minute, int second, int nano)<br>```|
|```java<br>of (LocalDate date, LocalTime time)<br>```|

Можно задать дату явно или через объекты `LocalDate` и `LocalTime`:

|Код|
|---|
|```java<br>LocalDate date = LocalDate.now();<br>LocalTime time = LocalTime.now();<br>LocalDateTime current = LocalDateTime.of(date, time);<br>System.out.println("Сейчас = " + current);<br><br>LocalDateTime date = LocalDateTime.of(2019, Month.MAY, 15, 12, 15, 00);<br>System.out.println("Сейчас = " + date);<br>```|
|Вывод на экран|
|```<br>Сейчас = 2019-02-22T10:05:38.465675100<br>Сейчас = 2019-05-15T12:15<br>```|

У класса `LocalDateTime` есть методы получения фрагмента даты и/или времени. Они абсолютно аналогичны методам классов `LocalDate` и `LocalTime`. Дублировать их тут мы не будем.
