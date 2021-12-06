## Runnable과 Callable

Runnable 과 Callable 두 키워드 모두 쓰레드를 만들고 실행하고자 할때 해당 쓰레드에서 어떠한 작업을 수행할지를 정의할때 사용하는 인터페이스 이다.



두 인터페이스의 차이점은 선언된 메서드의 리턴 여부와 예외 발생 가능성이다.



Runnable의 내부를 보면

```jsx
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```

인터페이스 내에 메서드를 한 개만 선언할 수 있는 FuntionalInterface 어노테이션이 선언되어있으며

run 메서드의 return type은 void로 선언되어 있다.



Callable은 다음과 같이 구성되어있다.

```jsx
@FunctionalInterface
public interface Callable<V> {
    /**
     * Computes a result, or throws an exception if unable to do so.
     *
     * @return computed result
     * @throws Exception if unable to compute a result
     */
    V call() throws Exception;
}
```

Runnable과 마찬가지로 함수형 인터페이스이지만 리턴값이 존재하며 리턴타입을 정의하기위한 Generic을 사용중이고 예외가 발생할 수 있다.



사용하는 방법에서도 차이가 존재하는데 Runnable의 경우 해당 Runnable 인터페이스를 구현하는 객체를 Thread객체의 생성자로 주입해 사용하면 된다.



Callable의 경우 FutureTask를 사용하거나 ExecutorService를 사용해 쓰레드를 실행시킬 수 있으며, FutureTask의 .get() 메서드 혹은 Future<> 객체의 .get() 메서드를 이용해 리턴값을 받을 수 있다.



*Future : 비동기적인 요청을 하고 응답을 기다릴때 사용하는 인터페이스