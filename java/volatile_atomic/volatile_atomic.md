## volatile

volatile 키워드는 변수명 앞에 붙일 수 있으며 해당 변수를 사용할 때 Main Memory에서 접근하겠다는 것을 명시한다.



왜 이러한 키워드를 사용할까?

변수를 읽어올 때 자바에서는 메인 메모리에 있는 값에 바로 접근해 사용하지 않고 CPU cache에 값을 복사한 후 사용한다.

이러한 처리 구조를 가질 때 일어날 수 있는 문제는 다음과 같다.

Thread 1은 첫 번째 CPU에서 동작하고 Thread 2가 두 번째 CPU에서 동작할 때 각각의 CPU cache에 저장된 변수 값이 일치하지 않는 문제가 발생하게 된다.

![cpu%20cache](https://github.com/gamzagamza/study-note/blob/master/java/volatile_atomic/images/cpu%20cache.png)



아래처럼 변수명 앞에 volatile 키워드를 붙여주면 메인 메모리에서 값을 읽어오고 저장함으로 위의 문제를 해결할 수 있다.

```java
public volatile int count = 0;
```



volatile은 동시성 문제를 해결할 수 있지만 모든 상황에서의 해결책이 될수는 없다.
하나의 Thread에서 쓰기연산을 수행하고 나머지 Thread에서 읽기 연산을 수행할때 유효하며 두 개 이상의 Thread에서 쓰기 연산을 수행하면 volatile 키워드를 사용해도 동시성 문제가 발생할 수 있다.



## atomic

atomic 변수는 CAS(Compared Ans Swap)알고리즘을 사용해 동시성 문제를 해결한다.

CAS 알고리즘은 현재 쓰레드에 저장된 변수의 값과 메인 메모리의 값을 비교해 일치하면 새로운 값으로 교체하고 일치하지 않으면 다시 재시도를 수행한다.



동시성 문제를 해결하기위해 Synchronized 키워드를 사용하게되면 해당 코드 블럭에 락이 걸리기 때문에 어플리케이션의 성능이 낮아질 수 있다.

하지만 Atomic 클래스는 Non-Blocking으로 동작하기 때문에 조금 더 나은 성능으로 동시성 문제를 해결할 수 있다.
