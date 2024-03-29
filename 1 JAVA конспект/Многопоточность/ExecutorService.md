В многопоточный пакет concurrent для управления потоками включено средство, называемое сервисом исполнения _ExecutorService_. Данное средство служит альтернативой классу [Thread](https://java-online.ru/java-thread.xhtml), предназначенному для управления потоками. В основу сервиса исполнения положен интерфейс **Executor**, в котором определен один метод : 
	void execute(Runnable thread);

При вызове метода execute исполняется поток thread. То есть, метод execute запускает указанный поток на исполнение. Следующий код показывает, как вместо обычного старта потока Thread.start() можно запустить поток с использованием сервиса исполнения :

// Вместо следующего кода
new Thread(new RunnableTask()).start();

// можно использовать
ExecutorService executor;
. . .
executor.execute(new CallableSample1());
Future<String> f1 = executor.submit(new CallableSample2());

При запуске задач с помощью Executor пакета java.util.concurrent не требуется прибегать к низкоуровневой поточной функциональности класса Thread, достаточно создать объект типа ExecutorService с нужными свойствами и передать ему на исполнение задачу типа Callable. Впоследствии можно легко просмотреть результат выполнения этой задачи с помощью объекта Future.

Интерфейс **ExecutorService** расширяет свойства _Executor_, дополняя его методами управления исполнением и контроля. Так в интерфейс ExecutorService включен метод shutdown(), позволяющий останавливать все потоки исполнения, находящиеся под управлением экземпляра ExecutorService. Также в интерфейсе ExecutorService определяются методы, которые запускают потоки исполнения FutureTask, возвращающие результаты и позволяющие определять статус остановки.

boolean **awaitTermination**(long timeout, TimeUnit unit)
