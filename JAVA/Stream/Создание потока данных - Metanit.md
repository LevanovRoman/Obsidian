Для создания потока данных можно применять различные методы. В качестве источника потока мы можем использовать коллекции. В частности, в JDK 8 в интерфейс Collection, который реализуется всеми классами коллекций, были добавлены два метода для работы с потоками:

- `default Stream<E> stream`: возвращается поток данных из коллекции
    
- `default Stream<E> parallelStream`: возвращается параллельный поток данных из коллекции
    

Так, рассмотрим пример с ArrayList:

|                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13 | `import` `java.util.stream.Stream;`<br><br>`import` `java.util.*;`<br><br>`public` `class` `Program {`<br><br>    `public` `static` `void` `main(String[] args) {`<br><br>        `ArrayList<String> cities =` `new` `ArrayList<String>();`<br><br>        `Collections.addAll(cities,` `"Париж"``,` `"Лондон"``,` `"Мадрид"``);`<br><br>        `cities.stream()` `// получаем поток`<br><br>            `.filter(s->s.length()==``6``)` `// применяем фильтрацию по длине строки`<br><br>            `.forEach(s->System.out.println(s));` `// выводим отфильтрованные строки на консоль`<br><br>    `}`<br><br>`}` |

Здесь с помощью вызова `cities.stream()` получаем поток, который использует данные из списка cities. С помощью каждой промежуточной операции, которая применяется к потоку, мы также можем получить поток с учетом модификаций. Например, мы можем изменить предыдущий пример следующим образом:

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6|`ArrayList<String> cities =` `new` `ArrayList<String>();`<br><br>`Collections.addAll(cities,` `"Париж"``,` `"Лондон"``,` `"Мадрид"``);`<br><br>`Stream<String> citiesStream = cities.stream();` `// получаем поток`<br><br>`citiesStream = citiesStream.filter(s->s.length()==``6``);` `// применяем фильтрацию по длине строки`<br><br>`citiesStream.forEach(s->System.out.println(s));` `// выводим отфильтрованные строки на консоль`|

Важно, что после использования терминальных операций другие терминальные или промежуточные операции к этому же потоку не могут быть применены, поток уже употреблен. Например, в следующем случае мы получим ошибку:

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4|`citiesStream.forEach(s->System.out.println(s));` `// терминальная операция употребляет поток`<br><br>`long` `number = citiesStream.count();` `// здесь ошибка, так как поток уже употреблен`<br><br>`System.out.println(number);`<br><br>`citiesStream = citiesStream.filter(s->s.length()>``5``);` `// тоже нельзя, так как поток уже употреблен`|

Фактически жизненный цикл потока проходит следующие три стадии:

1. Создание потока
    
2. Применение к потоку ряда промежуточных операций
    
3. Применение к потоку терминальной операции и получение результата
    

Кроме вышерассмотренных методов мы можем использовать еще ряд способов для создания потока данных. Один из таких способов представляет метод Arrays.stream(T[] array), который создает поток данных из массива:

|   |   |
|---|---|
|1<br><br>2|`Stream<String> citiesStream = Arrays.stream(``new` `String[]{``"Париж"``,` `"Лондон"``,` `"Мадрид"``}) ;`<br><br>`citiesStream.forEach(s->System.out.println(s));` `// выводим все элементы массива`|

Для создания потоков IntStream, DoubleStream, LongStream можно использовать соответствующие перегруженные версии этого метода:

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8|`IntStream intStream = Arrays.stream(``new` `int``[]{``1``,``2``,``4``,``5``,``7``});`<br><br>`intStream.forEach(i->System.out.println(i));`<br><br>`LongStream longStream = Arrays.stream(``new` `long``[]{``100``,``250``,``400``,``5843787``,``237``});`<br><br>`longStream.forEach(l->System.out.println(l));`<br><br>`DoubleStream doubleStream = Arrays.stream(``new` `double``[] {``3.4``,` `6.7``,` `9.5``,` `8.2345``,` `121``});`<br><br>`doubleStream.forEach(d->System.out.println(d));`|

И еще один способ создания потока представляет статический метод of(T..values) класса Stream:

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15|`Stream<String> citiesStream =Stream.of(``"Париж"``,` `"Лондон"``,` `"Мадрид"``);`<br><br>`citiesStream.forEach(s->System.out.println(s));`<br><br>`// можно передать массив`<br><br>`String[] cities = {``"Париж"``,` `"Лондон"``,` `"Мадрид"``};`<br><br>`Stream<String> citiesStream2 =Stream.of(cities);`<br><br>`IntStream intStream = IntStream.of(``1``,``2``,``4``,``5``,``7``);`<br><br>`intStream.forEach(i->System.out.println(i));`<br><br>`LongStream longStream = LongStream.of(``100``,``250``,``400``,``5843787``,``237``);`<br><br>`longStream.forEach(l->System.out.println(l));`<br><br>`DoubleStream doubleStream = DoubleStream.of(``3.4``,` `6.7``,` `9.5``,` `8.2345``,` `121``);`<br><br>`doubleStream.forEach(d->System.out.println(d));`|



