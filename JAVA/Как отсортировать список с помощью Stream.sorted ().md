### Вступление

Поток представляет собой _последовательность элементов_ и поддерживает различные виды операций, которые приводят к желаемому результату.

Источником этих элементов обычно является _Коллекция_ или _Массив_ , из которых данные передаются в поток.

Потоки отличаются от коллекций несколькими способами; прежде всего в том, что потоки не являются **структурой данных, в** которой хранятся элементы. Они функциональны по своей природе, и стоит отметить, что операции с потоком производят результат, но не изменяют его источник.
### Сортировка списка целых чисел с помощью Stream.sorted ()

Найденный в интерфейсе `Stream` `sorted()` имеет два перегруженных варианта, которые мы рассмотрим.

Оба этих варианта являются методами экземпляра, которые требуют, чтобы объект его класса был создан, прежде чем его можно будет использовать:

```
 public final Stream<T> sorted() {} 
```

Эти методы возвращают поток, состоящий из элементов потока, отсортированных в соответствии с _естественным порядком_ - порядком, предусмотренным JVM. Если элементы потока `Comparable` , при выполнении может возникнуть `java.lang.ClassCastException`

Использовать этот метод довольно просто, поэтому давайте рассмотрим пару примеров:

```
 Arrays.asList(10, 23, -4, 0, 18).stream().sorted().forEach(System.out::println); 
```

Здесь мы создаем экземпляр `List` `asList()` , предоставляя несколько целых чисел и `stream()` их. После потоковой передачи мы можем запустить метод `sorted()` , который естественным образом сортирует эти целые числа. После сортировки мы просто распечатали их, каждое в строке:

```
 -4 
 0 
 10 
 18 
 23 
```

Если бы мы хотели сохранить результаты сортировки после выполнения программы, нам пришлось бы `collect()` данные обратно в `Collection` (в этом примере `List` `sorted()` не изменяет источник.

Сохраним этот результат в `sortedList` :

```
 List<Integer> list = Arrays.asList(10, 23, -4, 0, 18); 
 List<Integer> sortedList = list.stream().sorted().collect(Collectors.toList()); 
 
 System.out.println(list); 
 System.out.println(sortedList); 
```

Запуск этого кода приведет к:

```
 [10, 23, -4, 0, 18] 
 [-4, 0, 10, 18, 23] 
```

Здесь мы видим, что исходный список остался неизменным, но мы сохранили результаты сортировки в новом списке, что позволяет нам использовать оба, если нам это понадобится позже.

### Сортировка списка целых чисел в порядке убывания с помощью Stream.sorted ()

`Stream.sorted()` по умолчанию сортирует в естественном порядке. В случае с целыми числами это означает, что они отсортированы по возрастанию.

Иногда вам может потребоваться переключить это и отсортировать в порядке убывания. Есть два простых способа сделать это - предоставить `Comparator` и изменить порядок, который мы рассмотрим в следующем разделе, или просто использовать `Collections.reverseOrder()` в вызове `sorted()`

```
 List<Integer> list = Arrays.asList(10, 23, -4, 0, 18); 
 List<Integer> sortedList = list.stream() 
 .sorted(Collections.reverseOrder()) 
 .collect(Collectors.toList()); 
 
 System.out.println(sortedList); 
```

Это приводит к:

```
 [23, 18, 10, 0, -4] 
```

### Сортировка списка строк с помощью Stream.sorted ()

Однако мы не всегда просто сортируем целые числа. Сортировка строк немного отличается, поскольку она менее интуитивно понятна для их сравнения.

Здесь метод `sorted()` также следует естественному порядку, установленному JVM. В случае строк они лексикографически отсортированы:

```
 Arrays.asList("John", "Mark", "Robert", "Lucas", "Brandon").stream().sorted().forEach(System.out::println); 
```

Запуск этого дает:

```
 Brandon 
 John 
 Lucas 
 Mark 
 Robert 
```

Если мы хотим сохранить только что отсортированный список, здесь применяется та же процедура, что и с целыми числами:

```
 List<String> list = Arrays.asList("John", "Mark", "Robert", "Lucas", "Brandon"); 
 List<String> sortedList = list.stream().sorted().collect(Collectors.toList()); 
 
 System.out.println(sortedList); 
```

Это приводит к:

```
 [Brandon, John, Lucas, Mark, Robert] 
```

Сортировка строк в обратном порядке так же проста, как сортировка целых чисел в обратном порядке:

```
 List<String> list = Arrays.asList("John", "Mark", "Robert", "Lucas", "Brandon"); 
 List<String> sortedList = list.stream() 
 .sorted(Collections.reverseOrder()) 
 .collect(Collectors.toList()); 
 
 System.out.println(sortedList); 
```

Это приводит к:

```
 [Robert, Mark, Lucas, John, Brandon] 
```

### Сортировка настраиваемых объектов с помощью Stream.sorted (Comparator <? Super T> компаратор)

Во всех предыдущих примерах мы работали с типами `Comparable` Однако, если мы работаем с некоторыми настраиваемыми объектами, которые могут быть `Comparable` по дизайну, и все же хотели бы отсортировать их с помощью этого метода, нам необходимо предоставить `Comparator` для вызова `sorted()`

Давайте определим `User` , который не является `Comparable` и посмотрим, как мы можем отсортировать их в `List` с помощью `Stream.sorted()` :

```
 public class User { 
 
 private String name; 
 private int age; 
 
 // Constructor, getters, setters and toString() 
 } 
```

В первой итерации этого примера предположим, что мы хотим отсортировать наших пользователей по их возрасту. Если возраст пользователей такой же, то первый из добавленных в список будет первым в отсортированном порядке. Допустим, у нас есть следующий код:

Давайте сначала отсортируем их по возрасту. Если их возраст такой же, порядок вставки в список определяет их позицию в отсортированном списке:

```
 List<User> userList = new ArrayList<>(Arrays.asList( 
 new User("John", 33), 
 new User("Robert", 26), 
 new User("Mark", 26), 
 new User("Brandon", 42))); 
 
 List<User> sortedList = userList.stream() 
 .sorted(Comparator.comparingInt(User::getAge)) 
 .collect(Collectors.toList()); 
 
 sortedList.forEach(System.out::println); 
```

Когда мы запускаем это, мы получаем следующий результат:

```
 User:[name: Robert, age: 26] 
 User:[name: Mark, age: 26] 
 User:[name: John, age: 33] 
 User:[name: Brandon, age: 42] 
```

Здесь мы составили список объектов `User` Мы транслируем этот список и используем метод `sorted()` с `Comparator` . В частности, мы используем `comparingInt()` метод, и поставку возраста пользователя, с помощью `User::getAge` метод эталонных.

Есть несколько из этих встроенных компараторов, которые работают с числами ( `int` , `double` и `long` ) - `comparingInt()` , `comparingDouble()` и `comparingLong()` . В конечном итоге вы также можете просто использовать метод `comparing()` , который принимает ключевую функцию сортировки, как и другие.

Все они просто возвращают компаратор с переданной функцией в качестве ключа сортировки. В нашем случае мы используем метод `getAge()` в качестве ключа сортировки.

Мы также можем легко изменить этот порядок, просто связав метод `reversed()` после вызова `comparingInt()`

```
 List<User> sortedList = userList.stream() 
 .sorted(Comparator.comparingInt(User::getAge).reversed()) 
 .collect(Collectors.toList()); 
 
 sortedList.forEach(System.out::println); 
```

Это приводит к:

```
 User:[name: Brandon, age: 42] 
 User:[name: John, age: 33] 
 User:[name: Robert, age: 26] 
 User:[name: Mark, age: 26] 
```

### Определение настраиваемого компаратора с помощью Stream.sorted ()

Хотя `Comparator` создаются такими методами, как `comparing()` и `comparingInt()` , работать с ними очень просто, и для них требуется только ключ сортировки - иногда автоматическое поведение - это не то, что мы ищем.

Если мы отсортируем `User` s, и двое из них имеют одинаковый возраст, теперь они будут отсортированы по порядку вставки, а не по их естественному порядку, на основе их имен. `Mark` должен стоять перед `Robert` в списке, отсортированном по имени, но в списке, который мы отсортировали ранее, все наоборот.

Для таких случаев мы хотим написать собственный `Comparator` :

```
 List<User> userList = new ArrayList<>(Arrays.asList( 
 new User("John", 33), 
 new User("Robert", 26), 
 new User("Mark", 26), 
 new User("Brandon", 42))); 
 
 List<User> sortedList = userList.stream() 
 .sorted((o1, o2) -> { 
 if(o1.getAge() == o2.getAge()) 
 return o1.getName().compareTo(o2.getName()); 
 else if(o1.getAge() > o2.getAge()) 
 return 1; 
 else return -1; 
 }) 
 .collect(Collectors.toList()); 
 
 sortedList.forEach(System.out::println); 
```

И теперь, когда мы выполняем этот код, у нас есть естественный порядок имен, а также возраст, отсортированный:

```
 User:[name: Mark, age: 26] 
 User:[name: Robert, age: 26] 
 User:[name: John, age: 33] 
 User:[name: Brandon, age: 42] 
```

Здесь мы использовали лямбда-выражение для `Comparator` и определили логику сортировки / сравнения. Возврат положительного числа указывает на то, что один элемент больше другого. Возврат отрицательного числа указывает на то, что один элемент меньше другого.

Мы использовали соответствующие подходы к сравнению имен и возрастов - сравнение имен лексикографически с помощью `compareTo()` , если `age` одинаковы, и регулярное сравнение возрастов с помощью оператора `>`

Если вы не привыкли к лямбда-выражениям, вы можете `Comparator` , хотя для удобства чтения рекомендуется сократить его до лямбда:

```
 Comparator<User> customComparator = new Comparator<User>() { 
 @Override 
 public int compare(User o1, User o2) { 
 if(o1.getAge() == o2.getAge()) 
 return o1.getName().compareTo(o2.getName()); 
 else if(o1.getAge() > o2.getAge()) 
 return 1; 
 else return -1; 
 } 
 }; 
 
 List<User> sortedList = userList.stream() 
 .sorted(customComparator) 
 .collect(Collectors.toList()); 
```

Вы также можете технически создать анонимный экземпляр компаратора в вызове `sorted()`

```
 List<User> sortedList = userList.stream() 
 .sorted(new Comparator<User>() { 
 @Override 
 public int compare(User o1, User o2) { 
 if(o1.getAge() == o2.getAge()) 
 return o1.getName().compareTo(o2.getName()); 
 else if(o1.getAge() > o2.getAge()) 
 return 1; 
 else return -1; 
 } 
 }) 
 .collect(Collectors.toList()); 
```

И именно этот анонимный вызов сокращается до лямбда-выражения при первом подходе.