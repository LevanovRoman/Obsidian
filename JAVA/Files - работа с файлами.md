## Класс `Files`
Чтобы работать с файлами, есть шикарный утилитный класс — `java.nio.file.Files`. У него есть методы просто на все случаи жизни. Все методы этого класса статические и работают с объектами типа Path. Методов очень много, поэтому мы рассмотрим только основные:

|Метод|Описание|
|---|---|
|```<br>Path createFile(Path path)<br>```|Создает новый файл с путем `path`|
|```<br>Path createDirectory(Path path)<br>```|Создает новую директорию|
|```<br>Path createDirectories(Path path)<br>```|Создает несколько директорий|
|```<br>Path createTempFile(prefix, suffix)<br>```|Создает временный файл|
|```<br>Path createTempDirectory(prefix)<br>```|Создает временную директорию|
|```<br>void delete(Path path)<br>```|Удаляет файл или директорию, если она пуста|
|```<br>Path copy(Path src, Path dest)<br>```|Копирует файл|
|```<br>Path move(Path src, Path dest)<br>```|Перемещает файл|
|```<br>boolean isDirectory(Path path)<br>```|Проверяет, что путь — это директория, а не файл|
|```<br>boolean isRegularFile(Path path)<br>```|Проверяет, что путь — это файл, а не директория|
|```<br>boolean exists(Path path)<br>```|Проверяет, что объект по заданному пути существует|
|```<br>long size(Path path)<br>```|Возвращает размер файла|
|```<br>byte[] readAllBytes(Path path)<br>```|Возвращает все содержимое файла в виде массива байт|
|```<br>String readString(Path path)<br>```|Возвращает все содержимое файла в виде строки|
|```<br>List<String> readAllLines(Path path)<br>```|Возвращает все содержимое файла в виде списка строк|
|```<br>Path write(Path path, byte[])<br>```|Записывает в файл массив байт|
|```<br>Path writeString(Path path, String str)<br>```|Записывает в файл строку|
|```<br>DirectoryStream<Path> newDirectoryStream(Path dir)<br>```|Возвращает коллекцию файлов (и поддиректорий) из заданной директории|

---

## 2. Создание файлов и директорий

Файлы и директории создавать очень просто. Убедимся на примерах:

|Код|Примечание|
|---|---|
|```java<br>Files.createFile(Path.of("c:\\readme.txt"));<br>```|Создает файл|
|```java<br>Files.createDirectory(Path.of("c:\\test"));<br>```|Создает директорию|
|```java<br>Files.createDirectories(Path.of("c:\\test\\1\\2\\3"));<br>```|Создает директорию и все нужные поддиректории, если их не существует.|

---

## 3. Копирование, перемещение и удаление

Копировать, перемещать и удалять файлы так же легко. На директории это тоже распространяется, но они должны быть пустые.

|Код|Примечание|
|---|---|
|```java<br>Path path1 = Path.of("c:\\readme.txt");<br>Path path2 = Path.of("c:\\readme-copy.txt");<br>Files.copy(path1, path2);<br>```|Копирует файл|
|```java<br>Path path1 = Path.of("c:\\readme.txt");<br>Path path2 = Path.of("d:\\readme-new.txt");<br>Files.move(path1, path2);<br>```|Перемещает и переименовывает файл|
|```java<br>Path path = Path.of("d:\\readme-new.txt");<br>Files.delete(path);<br>```|Удаляет файл|

---

## 4. Проверка типа файла и факта существования

Когда у вас есть какой-то путь, полученный извне, вы бы хотели знать, это файл или директория. Ну и вообще: существует такой файл/директория или нет?

Для этого тоже есть специальные методы. Так же можно легко узнать длину файла:

|Код|Примечание|
|---|---|
|```java<br>Files.isRegularFile(Path.of("c:\\readme.txt"));<br>```|```<br>true<br>```|
|```java<br>Files.isDirectory(Path.of("c:\\test"));<br>```|```<br>true<br>```|
|```java<br>Files.exists(Path.of("c:\\test\\1\\2\\3"));<br>```|```<br>false<br>```|
|```java<br>Files.size(Path.of("c:\\readme.txt"));<br>```|```<br>10112<br>```|

---

## 5. Работа с содержимым файла

И наконец, есть целая серия методов, которые позволяют легко прочитать или записать содержимое файла. Пример:

|Код|Описание|
|---|---|
|```java<br>Path path = Path.of("c:\\readme.txt");<br>List<String> list = Files.readAllLines(path);<br><br>for (String str : list)<br>   System.out.println(str);<br>```|Читаем содержимое файла в виде списка строк.  <br>  <br>Выводим строки на экран|

## Получение содержимого директории

Остался еще самый интересный метод — получение файлов и поддиректорий в заданной директории.

Для этого есть специальный метод — `newDirectoryStream()`, который возвращает специальный объект типа `DirectoryStream<Path>`. У него есть итератор(!), и с помощью этого итератора можно получить все файлы и поддиректории заданной директории.

Выглядит проще, чем кажется:

|Код|Описание|
|---|---|
|```java<br>Path path = Path.of("c:\\windows");<br><br>try (DirectoryStream<Path> files = Files.newDirectoryStream(path)) {<br>   for (Path path : files)<br>      System.out.println(path);<br>}<br>```|Получаем объект со списком файлов  <br>Цикл по списку файлов|

Объект `DirectoryStream<Path>` обладает двумя свойствами. Во-первых, у него есть итератор, который возвращает пути к файлам, и мы можем этот объект использовать внутри цикла `for-each`.

А во-вторых, этот объект является потоком данных, и его нужно закрывать с помощью метода `close()`, ну или использовать внутри `try`-with-resources.
## Метод `Files.newInputStream`

Начиная с Java 5 классы `FileInputStream` и `FileOutputStream` стали считаться устаревшими. Одним из их минусов было то, что при создании объекта этих классов сразу происходит создание файлов на диске. И потенциально выбрасываются все ошибки, связанные с созданием файлов.

Впоследствии это было признано не самым хорошим решением. Поэтому для создания объектов-файлов рекомендуется использовать методы утилитного класса – `java.nio.Files`.

Вот сравнение старого подхода к созданию файлов и нового:

|Было|
|---|
|```java<br>String src = "c:\\projects\\log.txt";<br>InputStream input = new FileInputStream( src );<br>```|
|Стало|
|```java<br>String src = "c:\\projects\\log.txt";<br>InputStream input = Files.newInputStream( Path.of( src ) );<br>```|

Аналогично замена и для `FileOutputStream`:

|Было|
|---|
|```java<br>String src = "c:\\projects\\log.txt";<br>OutputStream  output = new FileOutputStream( src );<br>```|
|Стало|
|```java<br>String src = "c:\\projects\\log.txt";<br>OutputStream  output = Files.newOutputStream( Path.of( src ) );<br>```|

