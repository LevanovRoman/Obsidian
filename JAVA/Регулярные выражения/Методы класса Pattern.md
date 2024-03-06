В классе `Pattern` есть и другие методы для работы с регулярными выражениями: `**String pattern()**` – возвращает исходное строковое представление регулярного выражения, из которого был создан объект `Pattern`:

`Pattern pattern = Pattern.compile("abc"); System.out.println(Pattern.pattern())//"abc"`

`**static boolean matches(String regex, CharSequence input)**` – позволяет проверить регулярное выражение, переданное в параметре regex на соответствие тексту, переданному в параметре `input`. Возвращает: _true_ – если текст соответствует шаблону; _false_ – в противном случае; Пример:

`System.out.println(Pattern.matches("А.+а","Алла"));//true System.out.println(Pattern.matches("А.+а","Егор Алла Александр"));//false`

`**int flags()**` – возвращает значения параметра `flags` шаблона, которые были установлены при его создании, или 0, если этот параметр не был установлен. Пример:

`Pattern pattern = Pattern.compile("abc"); System.out.println(pattern.flags());// 0 Pattern pattern = Pattern.compile("abc",Pattern.CASE_INSENSITIVE); System.out.println(pattern.flags());// 2`

`**String[] split(CharSequence text, int limit)**` – разбивает текст, переданный в качестве параметра на массив элементов `String`. Параметр `limit` определяет предельное количество совпадений, которое ищется в тексте:

- при `limit>0` – выполняется поиск `limit-1` совпадений;
- при `limit<0` – выполняется поиск всех совпадений в тексте
- при `limit=0` – выполняется поиск всех совпадений в тексте, при этом пустые строки в конце массива отбрасываются;

Пример:

`public static void main(String[] args) {     String text = "Егор Алла Анна";     Pattern pattern = Pattern.compile("\\s");     String[] strings = pattern.split(text,2);     for (String s : strings) {         System.out.println(s);     }     System.out.println("---------");     String[] strings1 = pattern.split(text);     for (String s : strings1) {         System.out.println(s);     } }`

Вывод на консоль: _Егор Алла Анна -------- Егор Алла Анна_

