
Когда нам нужно вещественное число произвольной длины, используется класс Java — `BigDecimal`. Как правило его применяют для работы с финансами вместо `double`, так как он дает больше возможности настройки. Как и `BigInteger`, `BigDecimal` является потомком класса `Number` и имеет методы, возвращающие значение объекта в виде определенного примитивного типа:

`BigDecimal value = new BigDecimal(35563.3);  long longValue = value.longValue();//35563  double doubleValue = value.doubleValue();//35563.3`

Как мы можем видеть при приведении к `long`, остаётся только целая часть, а знаки после запятой отбрасываются.

### Конструкторы BigDecimal

С конструкторами `BigDecimal` мы ознакомимся подробнее, так как у класса есть их гораздо более широкий выбор. Есть конструкторы, позволяющие задать значение объекта разными способами (передачей `int`, `long`, `double`, `String` и даже `BigInteger`), а есть такие, которые позволяю. задавать настройки создаваемого объекта (способы округления, количество знаков после запятой):

`BigDecimal firstValue = new BigDecimal("455656.545");//455656.545`

Тут всё понятно, мы непосредственно задали значение и количество знаков после запятой, которые хотим видеть.

`BigDecimal secondValue = new BigDecimal(3445.54);//3445.5399999999999636202119290828704833984375`

Результаты этого конструктора могут быть весьма непредсказуемыми, ведь мы задаем double, который по своей природе весьма неоднозначный тип. Поэтому обычно рекомендуется использовать в конструкторе `String`.

`BigDecimal thirdValue = new BigDecimal(3445.554645675444, MathContext.DECIMAL32);//3445.555`

Мы задаем `double`, но при этом и задаём параметр, описывающий правило округления (который содержит количество знаков после запятой и алгоритм для округления).

`char[] arr = new String("455656.545").toCharArray();  BigDecimal fourthValue = new BigDecimal(arr, 2, 6);//5656.5`

Задаем массив знаков, с которого элемента берём значения для объекта и сколько этих элементов берём.

`BigDecimal fifthValue = new BigDecimal(new BigInteger("44554"), 3);//44.554`

Берем уже существующий объект `BigInteger`, задаём какое количество знаков после запятой.

### Методы BigDecimal

Класс `BigDecimal` также содержит в себе методы для разнообразных арифметических операций, но методов работы с битами, как у `BigInteger`, у него нет. Но тем не менее, главная фишка `BigDecimal` — гибкость в работе с числами с плавающей запятой. Давайте рассмотрим некоторые методы, которые дают нам возможность властвовать над вещественными числами:

- получаем точность (количество чисел):
    
    `BigDecimal value = new BigDecimal("454334.34334"); int result = value.precision();//11`
    
- задаем количество знаков после запятой и правило округления:
    
    `BigDecimal firstValue = new BigDecimal(3445.544445);  BigDecimal secondValue = firstValue.setScale(3,BigDecimal.ROUND_CEILING);//3445.545`
    
    Немного ниже мы рассмотрим подробнее константы для задания правил округления.
    
- делим `BigDecimal` на другой `BigDecimal`, при этом указывая необходимое количество знаков после запятой и правило округления:
    
    `BigDecimal firstValue = new BigDecimal("455656.545");  BigDecimal secondValue = new BigDecimal(3445.544445);  BigDecimal result = firstValue.divide(secondValue, 2,RoundingMode.DOWN);//132.24`
    
- перемещение запятой вправо/влево на определенное количество знаков:
    
    `BigDecimal value = new BigDecimal("455656.545"); BigDecimal firstResult = value.movePointRight (2);//45565654.5 BigDecimal secondResult = value.movePointLeft (2);//4556.56545`
    
- обрезаем конечные нули:
    
    `BigDecimal value = new BigDecimal("45056.5000"); BigDecimal result = value.stripTrailingZeros();//45056.5`
    
    Если же у нас в вещественной части все нули и в целой тоже есть (или у нас и вовсе нет знаков после запятой), тогда:
    
    `BigDecimal value = new BigDecimal("450000.000"); BigDecimal result = value.stripTrailingZeros();//4.5E+5`
    

### Правила округления BigDecimal

Для задания правил округления, внутри `BigDecimal` мы можем увидеть специальные константы, описывающие алгоритмы округления: `ROUND_UP` — округления от нуля, округление в сторону вещественной части:

`BigDecimal firstValue = new BigDecimal("2.578"); BigDecimal firstResult =  firstValue.setScale(1, BigDecimal.ROUND_UP );//2.6 BigDecimal secondValue = new BigDecimal("-2.578"); BigDecimal secondResult = secondValue.setScale(1, BigDecimal.ROUND_UP );//-2.5`

`ROUND_DOWN` — округления до нуля, то есть усечение вещественной части:

`BigDecimal firstValue = new BigDecimal("2.578"); BigDecimal firstResult =  firstValue.setScale(1, BigDecimal.ROUND_DOWN  );//2.5 BigDecimal secondValue = new BigDecimal("-2.578"); BigDecimal secondResult = secondValue.setScale(1, BigDecimal.ROUND_DOWN  );//-2.6`

`ROUND_CEILING` — округления до положительной бесконечности. То есть, если число у нас положительное, то -> `ROUND_UP`, если отрицательное, то -> `ROUND_DOWN`

`BigDecimal firstValue = new BigDecimal("2.578"); BigDecimal firstResult =  firstValue.setScale(1, BigDecimal.ROUND_CEILING);//2.6 BigDecimal secondValue = new BigDecimal("-2.578"); BigDecimal secondResult = secondValue.setScale(1, BigDecimal.ROUND_CEILING);//-2.5`

`ROUND_FLOOR` — округления до отрицательной бесконечности, то есть если число у нас положительное, то -> `ROUND_DOWN`, если отрицательное, то -> `ROUND_UP`

`BigDecimal firstValue = new BigDecimal("2.578"); BigDecimal firstResult =  firstValue.setScale(1, BigDecimal.ROUND_FLOOR);//2.5 BigDecimal secondValue = new BigDecimal("-2.578"); BigDecimal secondResult = secondValue.setScale(1, BigDecimal.ROUND_FLOOR);//-2.6`

Для рассматриваемого значения это ближайшее число с урезанным знаком после запятой будем рассматривать как ближайшего соседа рассматриваемого числа. Например, 2.43 будет ближе к 2.4, чем к 2.5, но 2.48 будет уже ближе к 2.5. `ROUND_HALF_DOWN` — округления до «ближайшего соседа». Если оба соседа равноудалены от конкретного значения, тогда производится округление к нулю. Равноудалены — это, например, когда округляемое число — 5, и оно находится на одинаковом расстоянии от 0 и 10):

`BigDecimal firstValue = new BigDecimal("2.58"); BigDecimal firstResult =  firstValue.setScale(1, BigDecimal.ROUND_HALF_DOWN );//2.6 BigDecimal secondValue = new BigDecimal("2.55"); BigDecimal secondResult = secondValue.setScale(1, BigDecimal.ROUND_HALF_DOWN );//2.5`

`ROUND_HALF_UP` — режим для округления в сторону «ближайшего соседа». Если оба соседа равноудалены, округление выполняется к большему числу (это то самое округление, которому нас обучали в школе):

`BigDecimal firstValue = new BigDecimal("2.53"); BigDecimal firstResult =  firstValue.setScale(1, BigDecimal.ROUND_HALF_UP  );//2.5 BigDecimal secondValue = new BigDecimal("2.55"); BigDecimal secondResult = secondValue.setScale(1, BigDecimal.ROUND_HALF_UP  );//2.6`

`ROUND_HALF_EVEN` — округления до «ближайшего соседа», если оба соседа не равноудалены. В этом случае если перед округляемым числом стоит нечетное, выполняется округление в большую сторону, если чётное — в меньшую:

`BigDecimal firstValue = new BigDecimal("2222.2225"); BigDecimal secondValue = firstValue.setScale(3,BigDecimal.ROUND_HALF_EVEN );//2222.222`

Такой результат мы получаем, потому что при округлении 5 смотрит на предыдущее число 2, и видя, что оно чётное, округление идет в меньшую сторону. Но если:

`BigDecimal firstValue = new BigDecimal("2222.22255"); BigDecimal secondValue = firstValue.setScale(3,BigDecimal.ROUND_HALF_EVEN );//2222.223`

То округление идёт в большую сторону, так как последняя 5 смотрит на предыдущее значение и видит нечётное число. Как следствие, число округляется в большую сторону до 6, после чего следующая 6 тоже идёт на округление. Но шестерка уже не смотрит на число слева, так как число явно ближе в большую сторону, и в итоге последняя 2 увеличивается на 1. `ROUND_UNNECESSARY` — используется для проверки, что число в округлении не нуждается. То есть, мы проверяем, что число имеет нужное нам количество знаков после запятой:

`BigDecimal firstValue = new BigDecimal("2.55"); BigDecimal firstResult =  firstValue.setScale(2, BigDecimal.ROUND_UNNECESSARY);//2.55`

Тут всё хорошо, значение имеет два знака и мы проверяем, что после запятой только два знака. Но если:

`BigDecimal secondValue = new BigDecimal("2.55"); BigDecimal secondResult = secondValue.setScale(1, BigDecimal.ROUND_UNNECESSARY);`

То мы получим — `ArithmeticException`, так как проверяемое значение превышает заданное количество знаков после запятой. Но если мы будем проверять два знака после запятой, а по факту там есть один, то исключение не выпадет, а недостающие знаки просто дополняются нулями:

`BigDecimal thirdValue = new BigDecimal("2.5"); BigDecimal thirdResult = thirdValue.setScale(3, BigDecimal.ROUND_UNNECESSARY   );//2.500`

Еще хотелось бы отметить что у `BigDecimal` есть константы, аналогичные константам `BigInteger ZERO`, `ONE` и `TEN`. Вот ссылка на [документацию](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html#ROUND_CEILING). И напоследок: как вы, наверное, заметили, выполняя операции с объектами `BigInteger` и `BigDecimal`, мы не изменяем старые, а всегда получаем новые. Это нам говорит о том, что они — `immutable`, то есть неизменные после их создания, как и `String`. Иными словами, все их методы не могут изменить внутреннее состояние объекта, максимум — вернуть новый объект с параметрами, заданными используемой нами функцией.