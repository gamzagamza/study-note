## 부동소수점 오차

```java
double d = 0.0;
double b = 0.1;

for(int i = 1; i <= 100; i++) {
    d += b;
}

System.out.println(d);
```

위 자바코드를 실행하면 어떤 결과가 나올까 0.1을 100번 더하는 연산을 수행하기 때문에 10.0이라는 결과가 출력되면 좋겠지만 아쉽게도 9.99999999999998라는 결과가 출력된다. 왜 이러한 결과가 나올까?<br/><br/>

컴퓨터에서 실수를 표현하는 방식에는 **고정 소수점 방식**과 **부동 소수점 방식**이 있다. <br/>
고정 소수점 방식은 실수를 표현하기위한 정수부와 소수부로 공간을 나눈 방식이다. 이 방식은 정수부와 소수부의 자릿수가 크지 않아 표현할 수 있는 범위가 적다는 단점이 있다.<br/>
부동 소수점 방식은 가수부와 지수부로 나누어 실수를 표현한다. 부동 소수점 방식은 매우 큰 실수까지도 표현이 가능하기 때문에 현재 대부분의 시스템에서 이 방식을 사용하고 있으며 자바도 부동 소수점 방식을 이용해 실수를 표현한다.<br/>
하지만 부동소수점 방식에는 항상 오차가 존재한다는 단점이 존재하는데 그 이유는 다음과 같다.<br/><br/>

컴퓨터는 2진수로 값을 표현하는데 소수들은 유한한 공간에 2진수로 정확한 값의 표현이 불가능하다.
그래서 그 값과 가장 근사한 값을 반환해주는데 이때 부동소수점의 부정확성이 나타나게 되는것이다.
자바에서도 부동소수점 방식을 이용해 실수를 처리하기때문에 위의 부정확성이 나타나게 된다.<br/><br/>

이러한 오차를 해결하기위해서 자바에서는 BigDecimal이라는 클래스를 제공한다.
하지만 BigDecimal같은 경우 많은 공간을 차지하기 때문에 효율이 좋지 못하다.
만약 실수를 처리해야하고 정확한 값을 반환해야 하는경우 정수형을 가지고 계산한 결과를 반환해주는 것이 좋다.
