Технически `Path` — это не класс, а интерфейс. Так сделано для того, чтобы можно было под каждую операционную (и файловую) систему писать свой класс-наследник `Path`.
везде в методах для работы с файлами указан интерфейс `Path`, а реально работа идет с его классами-наследниками: `WindowsPath`, `UnixPath`, ...
**Класс `Path`**

Технически `Path` — это не класс, а интерфейс. Так сделано для того, чтобы можно было под каждую операционную (и файловую) систему писать свой класс-наследник `Path`.

У Windows свои стандарты написания пути файлов, у Linux — свои. А ведь в мире еще много операционных систем, и у каждой — свои стандарты.

Поэтому везде в методах для работы с файлами указан интерфейс `Path`, а реально работа идет с его классами-наследниками: `WindowsPath`, `UnixPath`, ...

**Создание объекта `Path`**

Чтобы создать объект `Path` (на самом деле это будет объект класса-наследника — `WindowsPath`), нужно воспользоваться командой вида:

```java
Path имя = Path.of(путь);
```

Где `имя` — это имя переменной типа `Path`. `путь` — это путь к файлу (или директории) вместе с именем файла (или директории). А `of` — статический метод класса `Path`.

Метод `of()` используется для того, чтобы создать объекты типа `WindowsPath` если программа запускается под Windows, а если программа запускается под Linux — объекты `UnixPath`. Вы не можете создать объект типа `Path` с помощью кода вида `new Path()`.

Примеры:

|Код|Примечание|
|---|---|
|```java<br>Path file = Path.of("c:\\projects\\note.txt");<br>```|Путь к файлу|
|```java<br>Path directory = Path.of("c:\\projects\\");<br>```|Путь к директории|

Файл (или директория) не обязаны существовать, чтобы мог существовать валидный объект типа `Path`. Может вы только хотите создать файл... Объект типа `Path` — это как продвинутая версия типа `String` — он не привязан к конкретному файлу на диске: он просто хранит некий путь на диске и все.
## 2. Методы типа `Path`

У интерфейса `Path` есть довольно много интересных методов. Самые интересные представлены в таблице ниже.

|Метод|Описание|
|---|---|
|```<br>Path getParent()<br>```|Возвращает родительскую директорию|
|```<br>Path getFileName()<br>```|Возвращает имя файла без директории|
|```<br>Path getRoot()<br>```|Возвращает корневую директорию из пути|
|```<br>boolean isAbsolute()<br>```|Проверяет, что текущий путь — абсолютный|
|```<br>Path toAbsolutePath()<br>```|Преобразует путь в абсолютный|
|```<br>Path normalize()<br>```|Убирает шаблоны в имени директории.|
|```<br>Path resolve(Path other)<br>```|Строит новый абсолютный путь из абсолютного и относительного.|
|```<br>Path relativize(Path other)<br>```|Получает относительный путь из двух абсолютных путей.|
|```<br>boolean startsWith(Path other)<br>```|Проверяет, что текущий путь начинается с пути|
|```<br>boolean endsWith(Path other)<br>```|Проверяет, что текущий путь заканчивается на путь|
|```<br>int getNameCount()<br>```|Дробит путь на части с помощью разделителя `/`.  <br>Возвращает количество частей.|
|```<br>Path getName(int index)<br>```|Дробит путь на части с помощью разделителя `/`.  <br>Возвращает часть по ее номеру.|
|```<br>Path subpath(int beginIndex, int endIndex)<br>```|Дробит путь на части с помощью разделителя `/`.  <br>Возвращает часть пути, заданную интервалом.|
|```<br>File toFile()<br>```|Преобразует объект `Path` в устаревший объект `File`|
|```<br>URI toUri()<br>```|Преобразует объект `Path` в объект типа `URI`|

Ниже идет краткое описание существующих методов.

---

## 3. Разделение пути на части

Метод `getParent()` возвращает путь, который указывает на родительскую директорию для текущего пути. Независимо от того, был этот путь директорией или файлом:

|Код|Значение|
|---|---|
|```java<br>String str = "c:\\windows\\projects\\note.txt";<br>Path path = Path.of(str).getParent();<br>```|```<br>"c:\\windows\\projects\\"<br>```|
|```java<br>String str = "c:\\windows\\projects\\";<br>Path path = Path.of(str).getParent();<br>```|```<br>"c:\\windows\\"<br>```|
|```java<br>String str = "c:\\";<br>Path path = Path.of(str).getParent();<br>```|```<br>null<br>```|

Метод `getFileName()` возвращает одно имя файла (или директории) — то, что идет после последнего разделителя:

|Код|Значение|
|---|---|
|```java<br>String str = "c:\\windows\\projects\\note.txt";<br>Path path = Path.of(str).getFileName();<br>```|```<br>"note.txt"<br>```|
|```java<br>String str = "c:\\windows\\projects\\";<br>Path path = Path.of(str).getFileName();<br>```|```<br>"projects"<br>```|
|```java<br>String str = "c:\\";<br>Path path = Path.of(str).getFileName();<br>```|```<br>null<br>```|

Метод `getRoot()` возвращает путь к корневой директории:

|Код|Значение|
|---|---|
|```java<br>String str = "c:\\windows\\projects\\";<br>Path path = Path.of(str).getRoot();<br>```|```<br>"c:\\"<br>```|

## 4. Абсолютный и относительный пути

Пути бывают двух типов: абсолютные и относительные. Абсолютный путь начинается с корневой директории. Для Windows это может быть папка `c:\`, для Linux — директория `/`

Относительный путь считается относительно какой-то директории. Т.е. это как бы конец пути, но только без начала. Относительный путь можно превратить в абсолютный и наоборот

**Метод `boolean isAbsolute()`**

Метод проверяет, является ли текущий путь абсолютным

|Код|Значение|
|---|---|
|```java<br>String str = "c:\\windows\\projects\\note.txt";<br>boolean abs = Path.of(str).isAbsolute();<br>```|```<br>true<br>```|
|```java<br>String str = "src\\com\\javarush\\Main.java";<br>boolean abs = Path.of(str).isAbsolute();<br>```|```<br>false<br>```|

**Метод `Path toAbsolutePath()`**

Превращает путь в абсолютный, если нужно — добавляет к нему текущую рабочую директорию:

|Код|Значение|
|---|---|
|```java<br>String str = "c:\\windows\\projects\\note.txt";<br>Path path = Path.of(str).toAbsolutePath();<br>```|```<br>"c:\\windows\\projects\\note.txt"<br>```|
|```java<br>String str = "src\\com\\javarush\\Main.java";<br>Path path = Path.of(str).toAbsolutePath();<br>```|```<br>"d:\\work\\src\\com\\javarush\\Main.java"<br>```|

**Метод `Path normalize()`**

В пути вместо имени директории можно писать «..», и это будет означать вернуться на одну директорию назад. Нормализация устраняет эти вещи. Примеры:

|Код|Значение|
|---|---|
|```java<br>String str = "c:\\windows\\..\\projects\\note.txt";<br>Path path = Path.of(str).normalize();<br>```|```<br>"c:\\projects\\note.txt"<br>```|
|```java<br>String str = "src\\com\\javarush\\..\\Main.java";<br>Path path = Path.of(str).normalize();<br>```|```<br>"src\\com\\Main.java"<br>```|

**Метод `Path relativize(Path other)`**

Метод `relativize()` позволяет вычислить «разницу путей»: один путь относительно другого

|Код|Значение|
|---|---|
|```java<br>Path path1 = Path.of("c:\\windows\\projects\\note.txt");<br>Path path2 = Path.of("c:\\windows\\");<br>Path result = path2.relativize(path1);<br>```|```<br>"projects\\note.txt"<br>```|
|```java<br>Path path1 = Path.of("c:\\windows\\projects\\note.txt");<br>Path path2 = Path.of("c:\\windows\\");<br>Path result = path1.relativize(path2);<br>```|```<br>"..\\.."<br>```|
|```java<br>Path path1 = Path.of("c:\\aaa\\bbb\\1.txt");<br>Path path2 = Path.of("d:\\zzz\\y.jpg");<br>Path result = path1.relativize(path2);<br>```|Ошибка IllegalArgumentException:<br>два пути имеют разный "корень" (разные диски)|

**Метод `Path resolve(Path other)`**

Метод `resolve()` выполняет операцию, обратную `relativize()`: из абсолютного и относительного пути он строит новый абсолютный путь.

|Код|Значение|
|---|---|
|```java<br>Path path1 = Path.of("projects\\note.txt");<br>Path path2 = Path.of("c:\\windows\\");<br>Path result = path1.resolve(path2);<br>```|```<br>"c:\\windows"<br>```|
|```java<br>Path path1 = Path.of("projects\\note.txt");<br>Path path2 = Path.of("c:\\windows\\");<br>Path result = path2.resolve(path1);<br>```|```<br>"c:\\windows\\projects\\note.txt"<br>```|

**Метод `toFile()`**

Метод возвращает устаревший объект `File`, который хранит тот же путь к файлу, что и объект `Path`.

**Метод `toURI()`**

Метод преобразует путь к стандарту URI, возвращает объект, который содержит путь к файлу:

|Путь к файлу|URI к файлу|
|---|---|
|```<br>c:\windows\projects\note.txt<br>```|```<br>file:///c:/windows/projects/note.txt<br>```|

