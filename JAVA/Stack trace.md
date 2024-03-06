в любой момент работы программы можно узнать класс и имя метода, который сейчас выполняется. И даже не одного метода, а получить информацию о всей цепочке вызовов методов от текущего метода до метода `main()`.

Список, состоящий из текущего метода, метода, который его вызвал, его вызвавшего метода и т.д., называется stack trace. Получить его можно с помощью команды:

```java
StackTraceElement[] methods = Thread.currentThread().getStackTrace();
```

Можно записать ее и в две строки:

```java
Thread current = Thread.currentThread();
StackTraceElement[] methods = current.getStackTrace();
```

Статический метод `currentThread()` класса `Thread` возвращает ссылку на объект типа `Thread`, который содержит информацию о текущей нити (о текущем потоке выполнения). Подробнее о нитях вы узнаете в квесте Java Core.

У этого объекта `Thread` есть метод `getStackTrace()`, который возвращает массив элементов `StackTraceElement`, каждый из которых содержит информацию об одном методе. Все элементы вместе и образуют stack trace.
Пример:

public class Main
	{    
	public static void main(String[] args)
	    {       test();
	        }     public static void test()
	            { 
	                  Thread current = Thread.currentThread();
	                  StackTraceElement[] methods = current.getStackTrace();
	                   for(var info: methods)          System.out.println(info);
	                       }
	                        }`|

Вывод на экран
java.base/java.lang.Thread.getStackTrace(Thread.java:1606)
Main.test(Main.java:11)<br>Main.main(Main.java:5)

Как мы видим по выводу на экран, в приведенном примере метод `getStackTrace()` вернул массив из трех элементов:

- Метод `getStackTrace()` класса `Thread`
- Метод `test()` класса `Main`
- Метод `main()` класса `Main`

Из этого стек-трейса можно сделать вывод, что:

- Метод `Thread.getStackTrace()` был вызван методом `Main.test()` в строке 11 файла Main.java
- Метод `Main.test()` был вызван методом `Main.main()` в строке 5 файла Main.java
- Метод `Main.main()` никто не вызывал — это первый метод в цепочке вызовов.

Кстати, на экране отобразилась только часть всей имеющийся информации. Все остальное можно получить прямо из объекта `StackTraceElement`
## `StackTraceElement`

Класс `StackTraceElement`, как следует из его названия, создан для того, чтобы хранить информацию по одному элементу stack trace — т.е. по одному методу из `StackTrace`.

У объектов этого класса есть такие методы:

|Метод|Описание|
|---|---|
|```java<br>String getClassName()<br>```|Возвращает имя класса|
|```java<br>String getMethodName()<br>```|Возвращает имя метода|
|```java<br>String getFileName()<br>```|Возвращает имя файла (в одном файле может быть много классов)|
|```java<br>int getLineNumber()<br>```|Возвращает номер строки в файле, в которой был вызов метода|
|```java<br>String getModuleName()<br>```|Возвращает имя модуля (может быть `null`)|
|```java<br>String getModuleVersion()<br>```|Возвращает версию модуля (может быть `null`)|

С их помощью можно получить более полную информацию о текущем стеке вызовов:

|Код|Вывод на экран|Примечание|
|---|---|---|
|```java<br>public class Main<br>{<br>   public static void main(String[] args)<br>   {<br>      test();<br>   }<br><br>   public static void test()<br>   {<br>      Thread current = Thread.currentThread();<br>      StackTraceElement[] methods = current.getStackTrace();<br><br>      for(StackTraceElement info: methods)<br>      {<br>         System.out.println(info.getClassName());<br>         System.out.println(info.getMethodName());<br><br>         System.out.println(info.getFileName());<br>         System.out.println(info.getLineNumber());<br><br>         System.out.println(info.getModuleName());<br>         System.out.println(info.getModuleVersion());<br>         System.out.println();<br>      }<br>   }<br>}<br>```|```<br>java.lang.Thread<br>getStackTrace<br>Thread.java<br>1606<br>java.base<br>11.0.2<br><br>Main<br>test<br>Main.java<br>11<br>null<br>null<br><br>Main<br>main<br>Main.java<br>5<br>null<br>null<br>```|имя класса  <br>имя метода  <br>имя файла  <br>номер строки  <br>имя модуля  <br>версия модуля  <br>  <br>имя класса  <br>имя метода  <br>имя файла  <br>номер строки  <br>имя модуля  <br>версия модуля  <br>  <br>имя класса  <br>имя метода  <br>имя файла  <br>номер строки  <br>имя модуля  <br>версия модуля|

---
## Вывод стек-трейса при обработке ошибок

Почему же список вызовов методов назвали StackTrace? Да потому, что если представить список методов в виде стопки листов с именами методов, при вызове очередного метода на эту стопку кладется лист с именем метода, на него — следующий, и т.д.

Когда метод завершается, лист с верха стопки удаляется. Нельзя удалить лист из середины стопки, не удалив все листы, лежащие в нем — нельзя прекратить работу метода в цепочке вызовов, не завершив все методы, вызванные им.

**Исключения**

Еще одно интересное применение стека — обработка исключений.

Когда в программе происходит ошибка и создается исключение, в него записывается текущий **stack trace**: массив, состоящий из списка методов начиная с метода main и заканчивая методом, где произошла ошибка. Там даже есть строка, в которой было создано исключение!

Этот stack trace ошибки хранится внутри исключения и может быть легко извлечен из нее с помощью метода: `StackTraceElement[] getStackTrace()`

Пример:

|Код|Примечание|
|---|---|
|```java<br>try<br>{<br>   // тут может возникнуть исключение<br>}<br>catch(Exception e)<br>{<br>   StackTraceElement[] methods = e.getStackTrace()<br>}<br>```|Захватываем исключение  <br>  <br>Получаем из него стек-трейс в момент возникновения ошибки.|

Это метод класса `Throwable`, а значит, все его классы-наследники (т.е. вообще все исключения), имеют метод `getStackTrace()`. Очень удобно, не так ли?

**Печать стек-трейса ошибки**

Кстати, у класса `Throwable` есть еще один метод для работы со stack-trace: он выводит в консоль всю информацию по stack trace, который хранится внутри исключения. Он так и называется `printStackTrace()`.

Вызвать его можно у любого исключения, что очень удобно.

Пример:

|Код|
|---|
|```java<br>try<br>{<br>   // тут может возникнуть исключение<br>}<br>catch(Exception e)<br>{<br>   e.printStackTrace();<br>}<br>```|
|Вывод на экран|
|```<br>java.base/java.lang.Thread.getStackTrace(Thread.java:1606)<br>Main.test(Main.java:11)<br>Main.main(Main.java:5)<br>```|



