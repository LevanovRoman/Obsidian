![[2024-03-05_10-10-20.png]]

public class CallableFactorial {  
    static int factorialResult;  
    public static void main(String[] args) throws InterruptedException {  
        ExecutorService executorService = Executors.newSingleThreadExecutor();  
        Factorial2 factorial2 = new Factorial2(5);  
        Future<Integer> future = executorService.submit(factorial2);  
        try {  
            factorialResult = future.get(); // get заблокирует поток пока не                                              будет future
        } catch (ExecutionException e) {  
            System.out.println(e.getCause());  
        }  
        finally {  
            executorService.shutdown();  
        }  
        System.out.println(factorialResult);  
    }  
}
---------------------------------------------------------
После использования метода submit возвращается объект Future. -- результата еще нет, будет после выполнения get.

future.isDone() - можно проверить завершен ли наш task

Callable можно использовать только с ExecutorService.