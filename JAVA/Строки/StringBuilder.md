## 1. Изменения строк

Строки в Java — это неизменяемые объекты (immutable). Так было сделано для того, чтобы класс-строку можно было сильно оптимизировать и использовать повсеместно. Например, [в качестве ключей у коллекции HashMap](https://javarush.com/groups/posts/1940-klass-hashmap-) рекомендуется использовать только immutable-типы.

Однако часто возникают ситуации, когда программисту все же было бы удобнее иметь `String`-класс, который можно менять. Который не создает новую подстроку при каждом вызове его метода.

Например, у нас есть очень большая строка и мы часто дописываем что-то в ее конец. В этом случае даже коллекция символов (`ArrayList<Character>`) может быть эффективнее, чем постоянное пересоздание строк и конкатенации объектов типа `String`.

Именно поэтому в язык Java все же добавили тип String, который можно менять. Называется он `StringBuilder`.

**Создание объекта**

Чтобы создать объект `StringBuilder` на основе существующей строки, нужно выполнить команду вида:

```java
StringBuilder имя = new StringBuilder(строка);
```

Чтобы создать пустую изменяемую строку, нужно воспользоваться командой вида:

```java
StringBuilder имя = new StringBuilder();
```

Список методов

Класс `StringBuilder` имеет два десятка полезных методов, вот самые важные из них:

|Метод|Описание|
|---|---|
|```java<br>StringBuilder append(obj)<br>```|Преобразовывает переданный объект в строку и добавляет к текущей строке|
|```java<br>StringBuilder insert(int index, obj)<br>```|Преобразовывает переданный объект в строку и вставляет в текущую строку|
|```java<br>StringBuilder replace(int start, int end, String str)<br>```|Заменяет часть строки, заданную интервалом start..end на переданную строку|
|```java<br>StringBuilder deleteCharAt(int index)<br>```|Удаляет из строки символ под номером index|
|```java<br>StringBuilder delete(int start, int end)<br>```|Удаляет из строки символы, заданные интервалом|
|```java<br>int indexOf(String str, int index)<br>```|Ищет подстроку в текущей строке|
|```java<br>int lastIndexOf(String str, int index)<br>```|Ищет подстроку в текущей строке с конца|
|```java<br>char charAt(int index)<br>```|Возвращает символ строки по его индексу|
|```java<br>String substring(int start, int end)<br>```|Возвращает подстроку, заданную интервалом|
|```java<br>StringBuilder reverse()<br>```|Разворачивает строку задом наперед.|
|```java<br>void setCharAt(int index, char)<br>```|Изменяет символ строки, заданный индексом на переданный|
|```java<br>int length()<br>```|Возвращает длину строки в символах|

Вот краткое описание каждого метода
## 2. Краткое описание методов

Добавление к строке

Чтобы что-то добавить к изменяемой строке (`StringBuilder`), нужно воспользоваться методом `append()`. Пример:

|Код|Описание|
|---|---|
|```java<br>StringBuilder builder = new StringBuilder("Привет");<br>builder.append("Пока");<br>builder.append(123);<br>```|```<br>Привет<br>ПриветПока<br>ПриветПока123<br>```|

**Преобразование к стандартной строке**

Чтобы преобразовать объект `StringBuilder` к строке типа String, нужно просто вызвать у него метод `toString()`. Пример

|Код|Вывод на экран|
|---|---|
|```java<br>StringBuilder builder = new StringBuilder("Привет");<br>builder.append(123);<br>String result = builder.toString();<br>System.out.println(result);<br>```|```<br>Привет123<br>```|

**Как удалить символ?**

Чтобы удалить символ в изменяемой строке, вам нужно воспользоваться методом `deleteCharAt()`. Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>StringBuilder builder = new StringBuilder("Привет");<br>builder.deleteCharAt(2);<br>String result = builder.toString();<br>System.out.println(result);<br>```|```<br>Првет<br>```|

**Как заменить часть строки на другую?**

Для этого есть метод `replace(int begin, int end, String str)`. Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>StringBuilder builder = new StringBuilder("Привет");<br>builder.replace(2, 5, "Hello!");<br>String result = builder.toString();<br>System.out.println(result);<br>```|```<br>ПрHello!т<br>```|

## 3. Полезные примеры работы со строками

**Как развернуть строку задом наперед?**

Для этой операции есть специальный метод — `reverse()`; Пример:

|Код|Вывод на экран|
|---|---|
|```java<br>String str = "Привет";<br>StringBuilder builder = new StringBuilder(str);<br>builder.reverse();<br>String result = builder.toString();<br>System.out.println(result);<br>```|```<br>тевирП<br>```|

**Класс `StringBuffer`**

Есть еще один класс — `StringBuffer` — это аналог класса `StringBuilder`, только его методы имеют модификатор `synchronized`. А это значит, что к объекту `StringBuffer` можно одновременно обращаться из нескольких потоков.

Зато он работает гораздо медленнее, чем `StringBuilder`. Такой класс может вам понадобиться, когда вы начнете активно изучать многопоточность в квесте Java Multithreading.
