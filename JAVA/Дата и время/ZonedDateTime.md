Всего в настоящий момент официально известно 599 временных зон. Вдумайтесь: 599. Это совсем не 24. Добро пожаловать в глобальный мир.

Для хранения временной зоны в Java используется класс `ZoneId` из пакета `java.time`.

Кстати, у него есть статический метод `getAvailableZoneIds()`, который возвращает множество всех известных на текущий момент временных зон. Чтобы получить список всех зон, нужно написать такой код:

|Код|Вывод на экран (часть)|
|---|---|
|```java<br>for (String s: ZoneId.getAvailableZoneIds())<br>   System.out.println(s);<br>```|```<br>Asia/Aden<br>America/Cuiaba<br>Etc/GMT+9<br>Etc/GMT+8<br>```|

Чтобы получить объект `ZoneId` по его имени, нужно воспользоваться статическим методом `of()`;

|Код|Примечание|
|---|---|
|```java<br>ZoneId zone = ZoneId.of("Africa/Cairo");<br>```|```<br>Каир<br>```|

## Создание объекта `ZonedDateTime`

При создании объекта `ZonedDateTime` нужно вызвать у него статический метод `now()` и передать в него объект `ZoneId`.

|Код|Вывод на экран|
|---|---|
|```java<br>ZoneId zone = ZoneId.of("Africa/Cairo");<br>ZonedDateTime time = ZonedDateTime.now(zone);<br>System.out.println(time);<br>```|```<br><br><br>2019-02-22T11:37:58.074816+02:00[Africa/Cairo]<br>```|

Если в метод `now()` не передать объект `ZoneId`, а так можно, временная зона будет определена автоматически: на основе настроек компьютера, на котором выполняется программа.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>ZonedDateTime time = ZonedDateTime.now();<br>System.out.println(time);<br>```|```<br><br>2019-02-22T13:39:05.70842+02:00[Europe/Helsinki]<br>```|

**Преобразование глобальной даты в локальную**

Одной из интересных особенностей `ZonedDateTime` является возможность его преобразования в локальную дату и время. Пример.

```java
ZoneId zone = ZoneId.of("Africa/Cairo");
ZonedDateTime cairoTime = ZonedDateTime.now(zone);

LocalDate localDate = cairoTime.toLocalDate();
LocalTime localTime = cairoTime.toLocalTime();
LocalDateTime localDateTime = cairoTime.toLocalDateTime();
```

## Работа со временем

Как и у класса `LocalDateTime`, у класса `ZonedDateTime` есть много способов получить отдельные фрагменты даты и времени. Вот список этих методов:

|   |   |
|---|---|
|```java<br>int getYear()<br>```|Возвращает год из конкретной даты|
|```java<br>Month getMonth()<br>```|Возвращает месяц даты: одну из специальных констант `JANUARY, FEBRUARY, ...;`|
|```java<br>int getMonthValue()<br>```|Возвращает номер месяца из даты. Январь == 1|
|```java<br>int getDayOfMonth()<br>```|Возвращает номер дня в месяце|
|```java<br>DayOfWeek getDayOfWeek()<br>```|Возвращает день недели: одну из специальных констант `MONDAY, TUESDAY, ...;`|
|```java<br>int getDayOfYear()<br>```|Возвращает номер дня в году|
|```java<br>int getHour()<br>```|Возвращает часы|
|```java<br>int getMinute()<br>```|Возвращает минуты|
|```java<br>int getSecond()<br>```|Возвращает секунды|
|```java<br>int getNano()<br>```|Возвращает наносекунды|

Все методы полностью аналогичны методам `LocalDateTime`. И, конечно, у класса `ZonedDateTime` есть методы, которые позволяют работать с датой и временем. При этом объект, у которого вызываются методы, не меняется: вместо этого методы возвращают новый объект `ZonedDateTime`:

|Методы|Описание|
|---|---|
|```java<br>plusYears(int)<br>```|Добавляет годы к дате|
|```java<br>plusMonths(int)<br>```|Добавляет месяцы к дате|
|```java<br>plusDays(int)<br>```|Добавляет дни к дате|
|```java<br>plusHours(int)<br>```|Добавляет часы|
|```java<br>plusMinutes(int)<br>```|Добавляет минуты|
|```java<br>plusSeconds(int)<br>```|Добавляет секунды|
|```java<br>plusNanos(int)<br>```|Добавляет наносекунды|
|```java<br>minusYears(int)<br>```|Отнимает годы от даты|
|```java<br>minusMonths(int)<br>```|Отнимает месяцы от даты|
|```java<br>minusDays(int)<br>```|Отнимает дни от даты|
|```java<br>minusHours(int)<br>```|Вычитает часы|
|```java<br>minusMinutes(int)<br>```|Вычитает минуты|
|```java<br>minusSeconds(int)<br>```|Вычитает секунды|
|```java<br>minusNanos(int)<br>```|Вычитает наносекунды|



Можно ли, зная время в одном часовом поясе, определить время в другом? Реши эту задачу в методе changeZone. Его параметры:
fromTime — известное время;
fromZone — временная зона, в которой известно время;
toZone — временная зона, в которой нужно определить время.

package com.javarush.task.pro.task16.task1618;  
  
import java.time.LocalDateTime;  
import java.time.ZoneId;  
import java.time.ZonedDateTime;  
  
/* Лишь бы не запутаться  
*/  
  
public class Solution {  
  
    public static void main(String[] args) {  
        ZoneId zone1 = ZoneId.of("Zulu");  
        ZoneId zone2 = ZoneId.of("Etc/GMT+8");  
        System.out.println(ZonedDateTime.now(zone1));  
        System.out.println(ZonedDateTime.now(zone2));  
  
        LocalDateTime time = changeZone(LocalDateTime.of(2020, 3, 19, 1, 40), zone1, zone2);  
        System.out.println(time);  
    }  
  
    static LocalDateTime changeZone(LocalDateTime fromDateTime, ZoneId fromZone, ZoneId toZone) {  
        ZonedDateTime fromZonedDateTime = fromDateTime.atZone(fromZone);  
        ZonedDateTime toZonedDateTime = fromZonedDateTime.withZoneSameInstant(toZone);  
        return toZonedDateTime.toLocalDateTime();  
    }  
    }


В методе main присвой значение переменной globalTime, используя переменные localDateTime и zoneId.
/* Временная глобализация  
*/  
  
public class Solution {  
  
    static LocalDateTime localDateTime = LocalDateTime.of(2020, 3, 19, 9, 17);  
    static ZoneId zoneId = ZoneId.of("Zulu");  
    static ZonedDateTime globalTime;  
  
    public static void main(String[] args) {  
        globalTime = localDateTime.atZone(zoneId);  
        System.out.println(globalTime);  
    }  
}

