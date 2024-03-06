## BigInteger в Java

Класс Java `BigInteger` используется как аналог целочисленных значений произвольной длины, у которого нет ограничения в 64 битов, как например у long. При этом он потомок класса `Number`, как и стандартные обертки для числовых простых типов — `Integer`, `Long`, `Byte`, `Double` и так далее — поэтому имеет реализации методов, приводящих к простым типам:

BigInteger value = new BigInteger("32145"); 
int intValue = value.intValue();//32145 
long longValue = value.longValue();//32145 
double doubleValue = value.doubleValue();//32145.0


Тут же мы видим создание такого объекта `BigInteger` с передачей в конструктор нашего значения, но в формате строки. Стоит отметить, что конструктор у него не один, а на все случаи жизни. Если простые типы не вмещают полный объем данных из `BigInteger`, данные будут урезаны до диапазона этого примитивного типа. Но при этом есть аналоги этих методов (`intValueExact()`, `longValueExact()` и т. д.), с той только разницей, что если простой тип, в который идет преобразование, не вмещает диапазон данных, выбрасывается [ArithmeticException](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/ArithmeticException.html).

### Константы BigInteger

Для внутреннего пользования, у класса есть константы:

`BigInteger.ZERO BigInteger.ONE BigInteger.TEN`

Это константные объекты `BigInteger` со значениями, соответственно, `0`, `1` и `10`.

### Методы BigInteger

Одна из главных особенностей данного класса — он полон методов, реализующих стандартные арифметические операции на Java. Например:

- операции суммирования:
    
    `BigInteger firstValue = new BigInteger("37995"); BigInteger secondValue = new BigInteger("35466"); BigInteger resultValue =  firstValue.add(secondValue);//73461`
    
- операции умножения:
    
    `BigInteger firstValue = new BigInteger("37995"); BigInteger secondValue = new BigInteger("35466"); BigInteger resultValue =  firstValue.multiply(secondValue);//1347530670`
    
- операции нахождения остатка при делении одного числа на другое:
    
    `BigInteger firstValue = new BigInteger("37995"); BigInteger secondValue = new BigInteger("35466"); BigInteger resultValue =  firstValue.remainder(secondValue);//2529`
    
- получение абсолютного значения числа (то есть по модулю, без знака):
    
    `BigInteger firstValue = new BigInteger("-37995"); BigInteger resultValue =  firstValue.abs();//37995`
    

Также присутствуют методы для более сложных (специфических) операций:

- операции с вычислением [mod](https://habr.com/ru/post/421071/):
    
    `BigInteger firstValue = new BigInteger("-34"); BigInteger secondValue = new BigInteger("5"); BigInteger resultValue = firstValue.mod(secondValue); //1`
    

Существуют несколько разных вариаций данной функции:

- получение рандомного числа с заданием количества битов, которое будет использовать полученное значение:
    
    `BigInteger firstValue = BigInteger.probablePrime(8, new Random());//211 BigInteger secondValue = BigInteger.probablePrime(16, new Random());//42571`
    
- операции побитовых сдвигов(this >> n)
    
    Сдвиг влево:
    
    `BigInteger firstValue = new BigInteger("5"); BigInteger firstResultValue = firstValue.shiftLeft(3);//40`
    
    Сдвиг вправо:
    
    `BigInteger secondValue = new BigInteger("34"); BigInteger secondResultValue = secondValue.shiftRight(2); //8`
    

Конечно же, полный перечень методов лучше посмотреть в [документации](https://docs.oracle.com/javase/8/docs/api/java/math/BigInteger.html).

