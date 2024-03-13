### СТРАТЕГИЯ (STRATEGY)

Стратегия (Strategy) — это шаблон проектирования, который позволяет выбирать алгоритм во время выполнения программы. С помощью Java Enum, можно реализовать стратегию следующим образом:

public enum Strategy {

STRATEGY1 {

@Override

public void execute() {

// execute strategy 1

}

},

STRATEGY2 {

@Override

public void execute() {

// execute strategy 2

}

};

public abstract void execute();

}

В этом примере `Strategy` представляет собой перечисление, которое содержит две стратегии (`STRATEGY1` и `STRATEGY2`). Каждая стратегия имеет свою реализацию метода `execute()`, которая может быть вызвана по имени.

### ФАБРИКА (FACTORY)

Фабрика (Factory) — это шаблон проектирования, который позволяет создавать объекты без явного указания их класса. С использованием Java Enum, можно создать фабрику следующим образом:

public enum Factory {

PRODUCT1 {

@Override

public Product create() {

return new Product1();

}

},

PRODUCT2 {

@Override

public Product create() {

return new Product2();

}

};

public abstract Product create();

}

В этом примере `Factory` представляет собой перечисление, которое содержит два продукта (`PRODUCT1` и `PRODUCT2`). Каждый продукт имеет свою реализацию метода `create()`, который возвращает новый объект продукта.

### ИТЕРАТОР (ITERATOR)

Итератор (Iterator) — это шаблон проектирования, который позволяет последовательно обходить элементы коллекции без знания ее внутреннего устройства. С использованием Java Enum, можно реализовать итератор следующим образом:

public enum Iterator {

FIRST {

@Override

public Iterator next() {

return SECOND;

}

},

SECOND {

@Override

public Iterator next() {

return THIRD;

}

},

THIRD {

@Override

public Iterator next() {

return null;

}

};

public abstract Iterator next();

}

В этом примере `Iterator` представляет собой перечисление, которое содержит три элемента (`FIRST`, `SECOND` и `THIRD`). Каждый элемент имеет свою реализацию метода `next()`, который возвращает следующий элемент в последовательности, либо `null`, если достигнут конец последовательности.

### СОСТОЯНИЕ (STATE)

Состояние (State) — это шаблон проектирования, который позволяет объекту изменять свое поведение в зависимости от внутреннего состояния. С использованием Java Enum, можно реализовать состояние следующим образом:

public enum State {

STATE1 {

@Override

public void handle() {

// handle state 1

setState(STATE2);

}

},

STATE2 {

@Override

public void handle() {

// handle state 2

setState(STATE1);

}

};

private static State currentState = STATE1;

public static void setState(State newState) {

currentState = newState;

}

public static State getState() {

return currentState;

}

public abstract void handle();

}

В этом примере `State` представляет собой перечисление, которое содержит два состояния (`STATE1` и `STATE2`). Каждое состояние имеет свою реализацию метода `handle()`, который изменяет текущее состояние и выполняет определенное поведение. Методы `setState()` и `getState()` позволяют установить и получить текущее состояние.