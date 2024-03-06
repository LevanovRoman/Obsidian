Иногда программистам очень неудобно работать с ссылками на `null`. Например, вы сравниваете две строки. Если обе переменные не `null`, тогда можно просто вызвать `s1.equals(s2)`, и все будет работать. А вот если `s1` может быть `null`, придется писать код, который учитывает эту ситуацию, чтобы не возникло `NullPointerException`.

Поэтому программисты придумали служебный класс `Optional<T>`. Выглядит его код примерно так:

|Код|Примечание|
|---|---|
|```java<br>class Optional<Tип><br>{<br>   private final Tип value;<br>   private Optional() { this.value = null;}<br>   private Optional(value) { this.value = value;}<br>   public static <Tип> Optional<Tип> of(Tип value)<br>   {<br>      return new Optional<Tип>(value);<br>   }<br><br>   public boolean isPresent()<br>   {<br>      return value != null;<br>   }<br><br>   public boolean isEmpty()<br>   {<br>      return value == null;<br>   }<br><br>   public Tип get()<br>   {<br>      if (value == null)<br>      {<br>         throw new NoSuchElementException();<br>      }<br>      return value;<br>   }<br><br>   public Tип orElse(Tип other)<br>   {<br>      return value != null ? value : other;<br>   }<br><br>   public Tип orElseThrow()<br>   {<br>      if (value == null)<br>      {<br>         throw new NoSuchElementException();<br>      }<br>      return value;<br>   }<br>}<br>```|Проверяет, что внутри находится значение (ссылка не `null`)  <br>  <br>  <br>  <br>Проверяет, что объект хранит ссылку на `null`  <br>  <br>  <br>  <br>  <br>Возвращает значение, которое хранит. Кидает исключение, если значения нет.  <br>  <br>  <br>  <br>  <br>  <br>  <br>  <br>Возвращает значение, или если внутри хранится `null`, то переданное в метод второе значение  <br>  <br>  <br>  <br>Возвращает значение или кидает исключение, если значения нет.|

Цель этого класса – просто хранить в себе объект T (ссылку на объект типа T). Ссылка на объект внутри класса `Optional<T>` может быть `null`.

Этот класс позволяет писать программистам код немного красивее. Сравните:

|С использованием Optional|Без использования Optional|
|---|---|
|```java<br>public void printString(String s)<br>{<br>   Optional<String> str = Optional.ofNullable(s);<br>   System.out.println(str.orElse(""));<br>}<br>```|```java<br>public void printString(String s)<br>{<br>   String str = s != null ? s : "";<br>   System.out.println(str)<br>}<br>```|

Один объект `Optional` всегда можно сравнить с другим объектом `Optional` через метод `equals`, даже если они хранят в себе ссылки на `null`.

Грубо говоря, класс Optional позволяет «более красиво» записывать проверки на `null` и действия в случае, если внутри объект `Optional` хранится `null`.


///
public static void printList(List<String> list) {  
    String text = "Этот элемент равен null";  
    list.forEach(s -> System.out.println(Optional.ofNullable(s).orElse(text)));  
  
}

