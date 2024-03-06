Собственно, с двумя методами класса `PrintStream` ты уже знаком. Это методы `print()` и `println()`, которыми ты пользуешься, наверное, каждый день :) Поскольку переменная `System.out` является объектом `PrintStream`, вызывая метод `System.out.println()`, ты вызываешь метод именно этого класса. Общее назначение класса `PrintStream` — вывод информации в какой-то поток. У данного класса есть несколько конструкторов. Вот несколько наиболее распространенных:

- `PrintStream(OutputStream outputStream)`
- `PrintStream(File outputFile) throws FileNotFoundException`
- `PrintStream(String outputFileName) throws FileNotFoundException`

Как видишь, мы можем передать в конструктор объекта `PrintStream`, например, название файла, в который необходимо вывести данные. Или же, как альтернативу, сам объект `File`. Давай рассмотрим, как это работает на примерах:

import java.io.File; 
import java.io.FileNotFoundException;
import java.io.PrintStream;  
public class Main {  
public static void main(String arr[]) throws FileNotFoundException 
{   
PrintStream filePrintStream = new PrintStream(new File("C:\\Users\\Username\\Desktop\\test.txt"));         filePrintStream.println(222); 
filePrintStream.println("Hello world"); 
filePrintStream.println(false);    } } 


Этот код создаст на рабочем столе файл `test.txt` (если он там еще не существует) и запишет туда последовательно наши число, строку и `boolean`-переменную.

Вот содержимое нашего файла после работы программы:

```

222
Hello world!
false
```

Как мы и сказали выше, не обязательно передавать сам объект файла `File`. Достаточно просто указать в конструкторе путь к нему:

Еще один интересный метод, на который стоит обратить внимание, — `printf()`, или вывод форматированной строки. Что значит «форматированная строка»? Чтобы объяснить, приведу пример:

import java.io.IOException;
import java.io.PrintStream;
public class Main {    
public static void main(String[] args) throws IOException {
PrintStream printStream = new PrintStream("C:\\Users\\Евгений\\Desktop\\test.txt");         printStream.println("Hello!");
printStream.println("I'm robot!");  
printStream.printf("My name is %s, my age is %d!", "Amigo", 18);  
printStream.close();     } }

Здесь вместо того, чтобы явно записывать в строке имя и возраст нашего робота, мы как бы оставляем для этой информации «свободное место» с помощью указателей `%s` и `%d`. А те данные, которые должны оказаться в этих местах, мы передаем в качестве параметров. В нашем случае это строка «_Amigo_» и число 18. Мы могли бы, например, создать еще одно пространство: скажем, `%b`, и передать еще один параметр. Для чего это нужно? Прежде всего, для повышения гибкости. Если в твоей программе необходимо будет часто выводить приветственное сообщение, для каждого нового робота тебе придется вручную набирать нужный текст. Ты не сможешь даже вынести этот текст в константу: имена и возраст-то у всех разные! Но используя новый метод, ты можешь вывести строку с приветствием в константу, а при необходимости просто менять параметры в методе `printf()`.
  
import java.io.IOException; 
import java.io.PrintStream;
public class Main { 
private static final String GREETINGS_MESSAGE = "My name is %s, my age is %d!"; 
public static void main(String[] args) throws IOException {
PrintStream printStream = new PrintStream("C:\\Users\\Евгений\\Desktop\\test.txt"); printStream.println("Hello!");
printStream.println("We are robots!");
printStream.printf(GREETINGS_MESSAGE, "Amigo", 18); 
printStream.printf(GREETINGS_MESSAGE, "R2-D2", 35);
printStream.printf(GREETINGS_MESSAGE, "C-3PO", 35); printStream.close(); 
} }

## Подмена System.in

В этой лекции мы будем «бороться с системой» и научимся подменять переменную `System.in` и перенаправлять системный вывод в нужное нам место.
Ты мог подзабыть что такое `System.in`, но эту конструкцию ни один студент JavaRush не забудет никогда:

`BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));`

`System.in` (как и `System.out`) — это статическая переменная класса `System`. Но, в отличие от `System.out`, она относится к другому классу, а именно — к `InputStream`. По умолчанию `System.in` — это поток, считывающий данные с системного устройства — клавиатуры. Однако, как и в случае с `System.out`, мы можем подменить источник данных, и чтение будет происходить не с клавиатуры, а из нужного нам места! Давай рассмотрим пример:

import java.io.\*;
public class Main {
public static void main(String[] args) throws IOException { 
String greetings = "Привет! Меня зовут Амиго!\nЯ изучаю Java на сайте JavaRush.\nОднажды я стану крутым программистом!\n";  
byte[] bytes = greetings.getBytes();        
InputStream inputStream = new ByteArrayInputStream(bytes);        
System.setIn(inputStream);        
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));         
String str;        
while ((str = reader.readLine())!= null) {             
System.out.println(str);        }    
} }  `

Итак, что же мы сделали? Обычно `System.in` «привязана» к клавиатуре. Но мы не хотим, чтобы данные читались с клавиатуры: пусть читаются из обычной строки с текстом!
Мы создали строку, и получили ее в виде массива байт. Зачем нам байты? Дело в том, что `InputStream` — абстрактный класс, и мы не можем создать его экземпляр. Придется выбрать какой-то класс из числа его наследников. Для примера можем взять `ByteArrayInputStream`. Он простой, и по одному названию понятно, как он работает: источником данных для него является массив байт. Так что мы создаем этот самый массив байт и передаем в конструктор нашему `stream`’у, который будет читать данные. По сути, все уже готово! Теперь нам достаточно использовать метод `System.setIn()`, чтобы явно установить значение переменной `in`. В случае с `out`, как ты помнишь, установить значение переменной явно тоже было нельзя: приходилось использовать специальный метод `setOut()`. После того, как мы присвоили созданный нами `InputStream` переменной `System.in`, надо проверить, сработала ли наша задумка. В этом нам поможет старый знакомый — `BufferedReader`. В обычной ситуации этот код привел бы к тому, что у тебя в Intellij IDEa открылась бы консоль, и оттуда считывались данные, которые ты вводил с клавиатуры.

BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));         String str; 
while ((str = reader.readLine())!= null) { 
System.out.println(str);       
}

Но запустив его теперь ты увидишь, что в консоль просто выведется наш текст из программы, никакого считывания с клавиатуры не будет. Мы подменили источник данных, теперь им является не клавиатура, а наша строка! Вот так легко и просто :)
