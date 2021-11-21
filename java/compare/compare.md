## ==과 equals()

==은 변수 혹은 객체가 동일한지 비교하기위한 연산자이다.

==연산자를 통해 Primitive 타입의 변수를 비교하면 변수가 담고있는 값을 비교하여 True 혹은 False값을 리턴해준다.

Reference 타입의 변수도 ==연산자로 비교가 가능하며 이때는 변수가 담고있는 객체의 주소값을 비교한다.

==연산자의 핵심은 변수가 담고있는 값을 비교한다는 점이다.



equals()는 객체의 동등성을 비교하기위한 메소드이다.

A.equals(B) 라는 형태로 비교를 수행할 때 A와 B가 동등한 객체이면 True를 동등하지 않다면 False를 리턴한다.

equals()는 모든 클래스의 최 상위 클래스인 Object 클래스에 다음과 같이 정의되어있다.

```java
public boolean equals(Object obj) {

	return (this == obj)

}
```

정의되어있는 코드를 보니 뭔가 이상한 부분이 있다.

equals를 사용하면 객체가 동등한지 비교를 해준다고 했는데 ==연산자를 사용해 객체가 같은 인스턴스인지 즉 같은 주소에있는 같은 객체인지를 비교하는 동일성 비교를 수행하고있다.

이부분은 어쩔수 없는 부분인게 만들어지는 객체마다 인스턴스 변수가 다르고 객체마다 무엇이 같아야 동등한 객체인지 미리 정의해놓을 수 없기 때문이다.

그래서 객체의 동등성을 비교하기위해선 필요한 객체마다 equals() 메소드를 오버라이딩해 재정의해주어야 한다.



지금까지 작성하며 동일성과 동등성이란 용어를 사용했는데 두 용어는 다른 의미이므로 이를 알아두어야 한다.

**동일성**은 두 객체가 완전히 동일한 즉, 같은 주소에 존재하는 같은 인스턴스라는 의미이다.

**동등성**은 두 객체의 내용이 같다는 의미로 쓰인다.



참고로 String 객체의 경우 equals() 메소드 사용 시 다음과 같이 이미 문자열의 내용을 비교하는 코드가 재정의 되어있기 때문에 재정의해서 사용할 필요가 없다.

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    
    return false;
}
```

