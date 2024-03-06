## Класс `URL`
**Получение странички из интернета**

|Код|Примечание|
|---|---|
|```java<br>URL url = new URL("https://javarush.com");<br>InputStream input = url.openStream();<br>byte[] buffer = input.readAllBytes();<br>String str = new String(buffer);<br>System.out.println(str);<br>```|Создает объект URL с путем к странице  <br>Получает `InputStream` у интернет-объекта  <br>Читает все байты и возвращает массив байт  <br>Преобразуем массив в строку  <br>Выводим строку на экран|

На экране отобразится содержимое HTML-файла:

|Вывод на экран|
|---|
|```<br><!DOCTYPE html><html lang="ru" class="light"><head><br>    <meta charset="utf-8″><br>    <meta http-equiv="X-UA-Compatible" content="IE=edge"><br>    <meta name="viewport" content="width=device-width, initial-scale=1″><br>    ...<br>```|

**Сравнение работы `File` и `URL`**

`URL` — это такой аналог `File` или `Path`, только `Path` хранит путь к ресурсу в файловой системе, а `URL` — путь к ресурсу в интернете.

Вся магия происходит, когда мы за один вызов метода `openStream()` получаем сразу объект типа `InputStream`. Это же стандартный объект, и мы его уже изучили вдоль и поперек. Все становится очевидно уже после получения объекта типа `InputStream`. Ведь как вычитать из него данные, мы уже знаем.

Сравните: отличаются только две первые строчки, и то немного. Вот оно, преимущество стандартизации и работы с цепочками потоков данных:

|Работа с интернетом|Работа с файлом|
|---|---|
|```java<br>URL url = new URL("https://javarush.com");<br>InputStream input = url.openStream();<br><br>byte[] buffer = input.readAllBytes();<br>String str = new String(buffer);<br>System.out.println(str);<br>```|```java<br>File file = new File("c:\\readme.txt");<br>InputStream input = new FileInputStream(file);<br><br>byte[] buffer = input.readAllBytes();<br>String str = new String(buffer);<br>System.out.println(str);<br>```|

## 2. Класс `URLConnection`

Кроме простого чтения данных из интернета, мы еще можем и загружать туда данные. Загрузка данных — это куда более сложный процесс, чем считывание. Вам понадобится на несколько методов больше. Пример:

|Код|Примечание|
|---|---|
|```java<br>URL url = new URL("https://javarush.com");<br>URLConnection connection = url.openConnection();<br><br>// получили поток для отправки данных<br>OutputStream output = connection.getOutputStream();<br>output.write(1); // отправляем данные<br><br>// получили поток для чтения данных<br>InputStream input = connection.getInputStream();<br>int data = input.read(); // читаем данные<br>```|Создаем объект URL с путем к странице  <br>Создаем двустороннее соединение  <br>  <br>  <br>Получаем поток вывода  <br>Выводим в него данные  <br>  <br>  <br>Получаем поток ввода  <br>Читаем из него данные|

Обратите внимание, что тут мы больше не вызываем метод `url.openStream()`. Вместо этого мы идем по более длинному пути:

- Сначала мы устанавливаем стабильное двустороннее соединение с помощью метода `URLConnection openConnection()`
- Затем получаем поток для отправки данных с помощью метода `connection.getOutputStream()` и отправляем данные серверу
- Затем получаем поток для чтения данных с помощью метода `connection.getInputStream()` и начинаем читать из него данные.

**Контроль ресурсов**

Строго говоря, мы должны все потоки обернуть в `try`-with-resources для безопасной работы. А еще не помешало бы обернуть голые `InputStream` и `OutputStream` во что-нибудь более удобное. Например, в `PrintStream` и `BufferedReader`.

Если мы все это сделаем, наш код будет выглядеть как-то так:

```java
URL url = new URL("https://javarush.com");
URLConnection connection = url.openConnection();

// отправляем данные
try (OutputStream output = connection.getOutputStream();
   PrintStream sender = new PrintStream(output))
{
   sender.println("Привет");
}

// читаем данные
try(InputStream input = connection.getInputStream();
   BufferedReader reader = new BufferedReader(new InputStreamReader(input)))
{
   while (reader.ready())
      System.out.println(reader.readLine());
}
```

---

## 3. Примеры работы с сетью

А давай-ка мы что-нибудь загрузим из интернета. И не просто загрузим, а сохраним на диск.

Например, давайте напишем программу, которая сохраняет на диск картинку с главной страницы Google.

В принципе ничего сложного. В самой простой интерпретации этот код будет выглядеть так:

|Сохранение файла на диск|
|---|
|```java<br>String image = "https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png";<br>URL url = new URL(image);<br>InputStream input = url.openStream();<br><br>Path path = Path.of("c:\\GoogleLogo.png");<br>Files.copy(input, path);<br>```|

С помощью трех первых строк мы получаем поток данных от интернет-ресурса — от картинки.

В четвертой строке мы создаем имя файла, в который будем сохранять картинку. Имя может быть любым, однако расширение файла должно совпадать с расширением картинки в интернете. Так локальные программы просмотра картинок будут правильно ее открывать.

Ну и наконец, последняя строка — это один из методов copy класса `Files`. У класса `Files` их несколько. Этот метод, который мы использовали, принимает в качестве первого параметра источник данных — байтовый поток (`InputStream`), а в качестве второго параметра — имя файла, куда нужно записывать данные.

Теоретически, если бы `URL` картинки был коротким, этот код вообще можно было бы записать в одну строку:

|Копирование данных из потока в файл|
|---|
|```java<br>Files.copy(<br>   new URL("https://www.google.com/logo.png").openStream(),<br>   Path.of("c:\\GoogleLogo.png")<br>);<br>```|

Писать так, конечно же, не нужно, однако этот пример демонстрирует, насколько удобные и мощные в Java потоки ввода-вывода.

