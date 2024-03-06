Объекты этого класса не изменяются после создания (класс `LocalDate` immutable). Зато это добавило классу простоты и надежности. Особенно если с объектом класса одновременно взаимодействуют несколько нитей (потоков исполнения).

Чтобы создать новый объект класса `LocalDate`, нужно использовать один из статических методов. Вот список основных.

**Получение текущей даты**

Чтобы получить текущую дату, нужно воспользоваться статическим методом `now()`. Это гораздо проще, чем кажется:

```java
LocalDate today = LocalDate.now();
```

Где `today` — это переменная класса `LocalDate`, а `LocalDate.now()` — вызов статического метода `now()` у класса `LocalDate`.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalDate today = LocalDate.now();<br>System.out.println("Сегодня = " + today);<br>```|```<br><br>Сегодня = 2019-02-22<br>```|

**Получение даты в определенном часовом поясе**

Также у класса `LocalDate` есть разновидность метода `now(ZoneId)`, который позволяет получить текущую дату в определенном часовом поясе.

Для этого нам понадобится еще один класс — `ZoneId` (java.time.ZoneId). У него есть метод `of()`, который возвращает объект `ZoneId` по имени часового пояса.

Чтобы определить текущую дату в Шанхае, нужно написать код:

|Код|Вывод на экран|
|---|---|
|```java<br>ZoneId  timezone = ZoneId.of("Asia/Shanghai");<br>LocalDate today = LocalDate.now(timezone);<br>System.out.println("Сейчас в Шанхае = " + today);<br>```|```<br><br><br>Сейчас в Шанхае = 2019-02-22<br>```|

Список имен всех часовых поясов (time zone) можно найти в интернете.
## Получение конкретной даты

Чтобы получить объект `LocalDate`, содержащий определенную дату, нужно воспользоваться статическим методом `of()`. Выглядит все тоже очень просто и понятно:

```java
LocalDate date = LocalDate.of(2019, Month.FEBRUARY, 22);
```

Где `date` — это переменная класса `LocalDate`, а `LocalDate.of()` — вызов статического метода `of()` у класса `LocalDate`.

Также тут мы видим использование специальной константы `FEBRUARY` класса `Month` (java.time.Month) для задания месяца Февраль.

Можно задать месяц и по старинке — с помощью числа:

```java
LocalDate date = LocalDate.of(2019, 2, 22);
```

Двойка? Вместо месяца Февраль? Это что, месяцы теперь опять нумеруются с единицы?

Да, наконец-то спустя почти 20 лет после создания Java месяцы перестали нумеровать с нуля.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalDate today = LocalDate.of(2019, 2, 22);<br>System.out.println("Сегодня = " + today);<br>```|```<br><br>Сегодня = 2019-02-22<br>```|

**Получение даты по номеру дня**

Есть еще один любопытный метод создания даты: с помощью метода `ofYearDay` можно получить дату, имея только номер года и номер дня года. Общий вид такой:

```java
LocalDate date = LocalDate.ofYearDay(год, день);
```

Где год — это номер года, а день — номер дня в году.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalDate today = LocalDate.ofYearDay(2019, 100);<br>System.out.println("Сегодня = " + today);<br>```|```<br><br>Сегодня = 2019-04-10<br>```|

Сотый день в 2019 году — это 10 апреля.

**Получение даты Unix**

Помните, что объекты класса `Date` всегда хранили время в миллисекундах с 1 января 1970 года? Чтобы программисты не скучали по старым добрым временам, в класс `LocalDate` добавили метод `ofEpochDay()`, который возвращает дату, отсчитанную от 1 января 1970 года. Общий вид такой:

```java
LocalDate date = LocalDate.ofEpochDay(день);
```

Где `день` — это количество дней, прошедшее с 1 января 1970 года.

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalDate today = LocalDate.ofEpochDay(1);<br>System.out.println("Сегодня = " + today);<br>```|```<br><br>Сегодня = 1970-01-02<br>```|

## Получение фрагментов даты

Изменять объекты класса `LocalDate` нельзя, а вот получать отдельные фрагменты даты еще как можно. Для этого у объектов класса `LocalDate` есть несколько методов:

|Метод|Описание|
|---|---|
|```java<br>int getYear()<br>```|Возвращает год из конкретной даты|
|```java<br>Month getMonth()<br>```|Возвращает месяц даты — одну из специальных констант  <br>`JANUARY, FEBRUARY, ...;`|
|```java<br>int getMonthValue()<br>```|Возвращает номер месяца из даты. Январь == 1.|
|```java<br>int getDayOfMonth()<br>```|Возвращает номер дня в месяце|
|```java<br>int getDayOfYear()<br>```|Возвращает номер дня с начала года|
|```java<br>DayOfWeek getDayOfWeek()<br>```|Возвращает день недели: одну из специальных констант  <br>`MONDAY, TUESDAY, ...;`|
|```java<br>IsoEra getEra()<br>```|Возвращает эру: константа `BC` (Before Current Era) и `CE`(Current Era)|

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalDate today = LocalDate.now();<br>System.out.println(today.getYear());<br>System.out.println(today.getMonth());<br>System.out.println(today.getMonthValue());<br>System.out.println(today.getDayOfMonth());<br>System.out.println(today.getDayOfWeek());<br>```|```<br><br>2019<br>FEBRUARY<br>2<br>22<br>FRIDAY<br>```|
## Изменение даты в объекте `LocalDate`

Класс `LocalDate` содержит несколько методов, которые позволяют работать с датой. Эти методы реализованы по аналогии с методами класса `String`: каждый из этих методов не меняет существующий объект LocalDate, а возвращает новый с нужными данными.

Вот какие методы есть у класса `LocalDate`:

|Метод|Описание|
|---|---|
|```java<br>plusDays(int days)<br>```|Добавляет определенное количество дней к дате|
|```java<br>plusWeeks(int weeks)<br>```|Добавляет недели к дате|
|```java<br>plusMonths(int months)<br>```|Добавляет месяцы к дате|
|```java<br>plusYears(int years)<br>```|Добавляет годы к дате|
|```java<br>minusDays(int days)<br>```|Отнимает дни от даты|
|```java<br>minusWeeks(int weeks)<br>```|Отнимает недели от даты|
|```java<br>minusMonths(int months)<br>```|Отнимает месяцы от даты|
|```java<br>minusYears(int years)<br>```|Отнимает годы от даты|

Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>LocalDate birthday = LocalDate.of(2019, 2, 28);<br>LocalDate nextBirthday = birthday.plusYears(1);<br>LocalDate firstBirthday = birthday.minusYears(30);<br><br>System.out.println(birthday);<br>System.out.println(nextBirthday);<br>System.out.println(firstBirthday);<br>```|```<br><br><br><br><br>2019-02-28<br>2020-02-28<br>1989-02-28<br>```|

Объект `birthday`, чьи методы мы вызываем, не меняется. Вместо этого его методы возвращают новые объекты, которые и содержат нужные данные.

