Java класс **BufferedReader** читает текст из потока ввода символов, буферизуя прочитанные символы, чтобы обеспечить эффективное считывание символов, массивов и строк. Можно указать в конструкторе вторым параметром размер буфера.

![BufferedReader и BufferedWriter - 1](https://cdn.javarush.com/images/article/86bb6413-b982-4f25-992d-abc72eba7c11/1080.webp)

**Конструкторы:**

`BufferedReader(Reader in) // Создает буферный поток ввода символов, который использует размер буфера по умолчанию. 
BufferedReader(Reader in, int sz) // Создает буферный поток ввода символов, который использует указанный размер.  

**Методы:**

close() // закрыть поток 
mark(int readAheadLimit) // отметить позицию в потоке 
markSupported() // поддерживает ли отметку потока 
int read() // прочитать буфер
int read(char[] cbuf, int off, int len) // прочитать буфер
String readLine() // следующая строка 
boolean ready() // может ли поток читать 
reset() // сбросить поток 
skip(long n) // пропустить символы`

**Пример использования классов BufferedReader и BufferedWriter:** Чтения файла:

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException; 
public class FileReaderExample {  	
public static void main(String[] args) { 
String inputFileName = "file.txt";  		
try (BufferedReader reader = new BufferedReader(new FileReader(inputFileName))) 
{
String line; 
while ((line = reader.readLine()) != null)
{ 				
System.out.println(line + "\n"); 			
} 	
}    
catch (IOException e) { 
e.printStackTrace(); 		
} 	}  }

Java класс **BufferedWriter** записывает текст в поток вывода символов, буферизуя записанные символы, чтобы обеспечить эффективную запись символов, массивов и строк. Можно указать в конструкторе вторым параметром размер буфера. **Конструкторы:**

BufferedWriter(Writer out) // Создает буферный поток вывода символов, который использует размер буфера по умолчанию. 
BufferedWriter(Writer out, int sz) // Создает буферный поток вывода символов, который использует указанный размер.  

**Методы:**

`close() // закрыть поток
flush() // передать данные из буфера во Writer
newLine() // перенос на новую строку 
write(char[] cbuf, int off, int len) // запись в буфер 
write(int c) // запись в буфер
write(String s, int off, int len) // запись в буфер`

**Пример использования классов Java BufferedReader и BufferedWriter:**

Запись в файл

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException; 
public class FileWritterExample {  
public static void main(String[] args) { 	
String outputFileName = "file.txt"; 	
String[] array = { "one", "two", "three", "four" };  
try (BufferedWriter writter = new BufferedWriter(new FileWriter(outputFileName)))
{ 			for (String value : array) { 	
writter.write(value + "\n"); 	
} 		
}    
catch (IOException e) { 	
e.printStackTrace(); 	
} 	}  }

`FileWriter` сразу записывает данные на диск и каждый раз к нему обращается, буфер работает как обертка и ускоряет работу приложения. Буфер будет записывать данные в себя, а потом большим куском файлы на диск.
**Считываем данные с консоли и записываем в файл:**

import java.io.BufferedReader; 
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader; 
public class ConsoleReaderExample {  
public static void main(String[] args) { 
String outputFileName = "file.txt";  	
try (BufferedReader reader = new BufferedReader(new InputStreamReader(System.in))) { 
try (BufferedWriter writter = new BufferedWriter(new FileWriter(outputFileName))) { 	
String line; 	
while (!(line = reader.readLine()).equals("exit")) { 
// Прерывание цикла при написании строки exit 				
writter.write(line); 
writter.newLine();  //с новой строки
} 	
} 	
}    
catch (IOException e) { 	
e.printStackTrace(); 		
} 	}  }  

