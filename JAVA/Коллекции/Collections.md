## 1. Список методов

Помните, что разработчики Java для удобства работы с массивами написали целый класс-помощник — `Arrays`?

Для коллекций они сделали то же самое. В Java есть класс `java.util.Collections`, у которого очень много методов, полезных при работе с коллекциями. Ниже приведем только самые интересные из них:

|Методы|Описание|
|---|---|
|```java<br>addAll(colls, e1, e2, e3, ..)<br>```|Добавляет в коллекцию `colls` элементы `e1`, `e2`, `e3`,...|
|```java<br>fill(list, obj)<br>```|Заменяет в переданном списке все элементы на `obj`|
|```java<br>nCopies(n, obj)<br>```|Возвращает список, состоящий из `n` копий объекта `obj`|
|```java<br>replaceAll(list, oldVal, newVal)<br>```|Заменяет в списке `list` все значения `oldVal` на `newVal`|
|```java<br>copy(dest, src)<br>```|Копирует все элементы из списка `src` в список `dest`|
|```java<br>reverse(list)<br>```|Разворачивает список задом наперед|
|```java<br>sort(list)<br>```|Сортирует список в порядке возрастания|
|```java<br>rotate(list, n)<br>```|Циклично сдвигает элементы списка `list` на `n` элементов|
|```java<br>shuffle(list)<br>```|Случайно перемешивает элементы списка|
|```java<br>min(colls)<br>```|Находит минимальный элемент коллекции `colls`|
|```java<br>max(colls)<br>```|Находит максимальный элемент коллекции `colls`|
|```java<br>frequency(colls, obj)<br>```|Определяет, сколько раз элемент `obj` встречается в коллекции `colls`|
|```java<br>binarySearch(list, key)<br>```|Ищет элемент `key` в отсортированном списке, возвращает индекс.|
|```java<br>disjoint(colls1, colls2)<br>```|Возвращает `true`, если у коллекций нет общих элементов|

Важно:

Многие из этих методов работают не с классами `ArrayList`, `HashSet` и `HashMap`, а с их интерфейсами: `Collection<T>`, `List<T>`, `Map<K, V>`.

Это не проблема: если метод принимает `List<T>`, в него всегда можно передать `ArrayList<Integer>`, но вот в обратную сторону присваивание не работает.

---

## 2. Создание и изменение коллекций

**Метод `Collections.addAll(Collection<T> colls, T e1, T e2, T e3, ...)`**

Метод `addAll()` добавляет в коллекцию `colls` элементы `e1`, `e2`, `e3`, ... Количество переданных элементов может быть любым.

|Код|Вывод на экран|
|---|---|
|```java<br>ArrayList<Integer> list = new ArrayList<Integer>();<br>Collections.addAll(list, 1, 2, 3, 4, 5);<br><br>for (int i: list)<br>   System.out.println(i);<br>```|```<br>1<br>2<br>3<br>4<br>5<br>```|

**Метод `Collections.fill(List<T> list, T obj)`**

Метод `fill()` заменяет все элементы коллекции `list` на элемент `obj`.

|Код|Вывод на экран|
|---|---|
|```java<br>ArrayList<Integer> list = new ArrayList<Integer>();<br>list.add(1);<br>list.add(2);<br>list.add(3);<br><br>Collections.fill(list, 10);<br><br>for (int i: list)<br>   System.out.println(i);<br>```|```<br>10<br>10<br>10<br>```|

**Метод `Collections.nCopies (int n, T obj)`**

Метод `nCopies()` возвращает список из `n` копий элементов `obj`. Список можно назвать фиктивным (реального массива внутри нет), поэтому изменять его нельзя! Можно использовать только для чтения.

|Код|Описание|
|---|---|
|```java<br>List<String> fake = Collections.nCopies(5, "Привет");<br><br>ArrayList<String> list = new ArrayList<String>(fake);<br><br>for(String s: list)<br>   System.out.println(s);<br>```|Создаем неизменяемый список из 5 элементов `Привет`  <br>Создаем реальный список `list`, заполняем его значениями из списка `fake`.  <br>  <br>Выводим на экран:  <br><br>```<br>Привет<br>Привет<br>Привет<br>Привет<br>Привет<br>```|

**Метод `Collections.replaceAll (List<T> list, T oldValue, T newValue)`**

Метод `replaceAll()` заменяет все элементы коллекции `list`, равные `oldValue`, на элемент `newValue`.

|Код|Вывод на экран|
|---|---|
|```java<br>ArrayList<Integer> list = new ArrayList<Integer>();<br>list.add(1);<br>list.add(2);<br>list.add(3);<br><br>Collections.replaceAll(list, 2, 20);<br><br>for (int i: list)<br>   System.out.println(i);<br>```|```<br>1<br>20<br>3<br>```|

**Метод `Collections.copy (List<T> dest, List<T> src)`**

Метод `copy()` копирует все элементы коллекции `src` в коллекцию `dest`.

Если изначально коллекция `dest` длиннее чем коллекция `src`, то оставшиеся элементы в коллекции `dest` останутся нетронутыми.

Важно:

Коллекция `dest` должна иметь длину не меньше, чем длина коллекции `src` (иначе кинется исключение `IndexOutOfBoundsException`).

|Код|Вывод на экран|
|---|---|
|```java<br>ArrayList<Integer> srcList = new ArrayList<Integer>();<br>Collections.addAll(srcList, 99, 98, 97);<br><br>ArrayList<Integer> destList = new ArrayList<Integer>();<br>Collections.addAll(destList, 1, 2, 3, 4, 5, 6, 7);<br><br>Collections.copy(destList, srcList);<br><br>for (int i: destList)<br>   System.out.println(i);<br>```|```<br>99<br>98<br>97<br>4<br>5<br>6<br>7<br>```|

