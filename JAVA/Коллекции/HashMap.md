Создать объект типа `HashMap` можно с помощью команды вида:

```java
HashMap<TКлюч, TЗначение> имя = new HashMap<TКлюч, TЗначение>();
```

Где `TКлюч` — это тип ключей из пары элементов, `TЗначение` — тип значений в паре элементов, которые будут храниться в коллекции `HashMap`.

У класса `HashMap` есть такие методы:

|Метод|Описание|
|---|---|
|```java<br>void put(ТКлюч key, ТЗначение value)<br>```|Добавляет в коллекцию пару (`key`, `value`)|
|```java<br>ТЗначение get(ТКлюч key)<br>```|Возвращает значение по ключу.|
|```java<br>boolean containsKey(ТКлюч key)<br>```|Проверяет наличие ключа в коллекции|
|```java<br>boolean containsValue(ТЗначение value)<br>```|Проверяет наличие значения в коллекции|
|```java<br>ТЗначение remove(ТКлюч key)<br>```|Удаляет элемент из коллекции|
|```java<br>void clear()<br>```|Очищает коллекцию: удаляет все элементы|
|```java<br>int size()<br>```|Возвращает количество пар элементов в коллекции|
|```java<br>Set<ТКлюч> keySet()<br>```|Возвращает множество ключей коллекции|
|```java<br>Collection<ТЗначение> values()<br>```|Возвращает множество элементов коллекции|
|```java<br>Set<Map.Entry<TКлюч, TЗначение>> entrySet()<br>```|Возвращает все значения коллекции в виде множества (`Set`) пар (`Map.Entry`).|

**Добавление элементов в `HashMap`**

Элементы добавляются в карту сразу парами: для этого используется метод `put()`. Первым в него передается ключ, вторым — значение.

```java
HashMap<String, Integer> map = new HashMap<String, Integer>();
map.put("Серега", 21);
map.put("Николай", 22);
map.put("Иван Петрович", 48);
map.put("Анюта", null);
```

Если при добавлении элемента выяснится, что элемент с таким ключом уже есть, старое значение ключа заменится на новое.

Такое поведение делает `HashMap` похожим на массив или список, если бы у них в качестве индексов выступали слова (`String`), а не числа.

## 3. Подмножества `HashMap`: множество ключей

Допустим мы хотим просто вывести все элементы `HashMap` на экран, как нам это сделать? Для этого нужно понять, как пройтись по всем элементам `HashMap`. Это можно сделать разными способами.

**Самый простой способ – использовать цикл по ключам**

У элементов класса `HashMap` нет порядкового номера, поэтому цикл со счетчиком тут не подойдет. Зато мы можем получить множество ключей с помощью метода `keySet()`, а как пройтись по множеству вы уже знаете:

|Код|Описание|
|---|---|
|```java<br>HashMap<String, Integer> map = new HashMap<String, Integer>();<br>map.put("Серега", 21);<br>map.put("Николай", 22);<br>map.put("Иван Петрович", 48);<br>map.put("Анюта", null);<br><br>for (String key: map.keySet())<br>{<br>   Integer value = map.get(key);<br>   System.out.println(key + " --> " + value);<br>}<br>```|Цикл по всем ключам `map`  <br>  <br>Получаем значение по ключу|

## 4. Использование цикла по парам

Есть и более сложный способ: можно преобразовать `Map` в множество пар элементов, а потом использовать цикл по элементам множества, как мы уже раньше учили.

В коллекции `HashMap` есть вспомогательный класс для хранения пары элементов. Выглядит он примерно так:

```java
class Entry<KeyType, ValueType>
{
   private KeyType key;
   private ValueType value;

   public KeyType getKey()
   {
      return this.key;
   }

   public ValueType getValue()
   {
      return this.value;
   }
}
```

Результат вызова метода `entrySet()` у объекта типа `HashMap<ТКлюч, ТЗначение>` будет иметь тип `Set<Entry<ТКлюч, ТЗначение>>`:

```java
Set<Entry<Ключ, Значение>> имя = map.entrySet();
```

Тут мы видим сложный тип `Set` с параметром-значением, а в качестве параметра-значение выступает еще один сложный тип (`Entry`), так еще и с двумя параметрами.

Новичку очень легко в этом запутаться. Хотя, если разберетесь, сможете писать код вида:

```java
HashMap<String, Integer> map = new HashMap<String, Integer>();
map.put("Серега", 21);
map.put("Николай", 22);
map.put("Иван Петрович", 48);
map.put("Анюта", null);

Set<Map.Entry<String, Integer>> entries = map.entrySet();
for(Map.Entry<String, Integer> pair: entries)
{
   String key = pair.getKey();
   Integer value = pair.getValue();
   System.out.println(key + " --> " + value);
}
```

Хотя этот код можно и немножко упростить:

Во-первых, можно не создавать отдельную переменную для `entries`, а сразу вызвать метод `entrySet()` внутри цикла `for`:

```java
for(Map.Entry<String, Integer> pair: map.entrySet())
{
   String key = pair.getKey();
   Integer value = pair.getValue();
   System.out.println(key + " --> " + value);
}
```

Во-вторых, можно воспользоваться недавно появившимся оператором `var` для автоматического выведения типа пары элементов:

```java
for(var pair: map.entrySet())
{
   String key = pair.getKey();
   Integer value = pair.getValue();
   System.out.println(key + " --> " + value);
}
```

