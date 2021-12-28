## Template Method Pattern

템플릿 메소드 패턴은 특정 작업을 처리하기 위한 공통적인 부분을 슈퍼 클래스에 구현하고 변경되는 부분은 서브 클래스에서 정의해서 유연하게 기능을 확장할 수 있도록 한다.



아래의 코드처럼 변경되는 부분은 abstract 키워드를 붙여 하위 타입에서 반드시 구현하도록 하고 변경되지 않는 부분을 미리 정의하면 된다.

```java
public abstract class SuperType {

    public void run() {
        template();
        method();
    }

    private void template() {
        System.out.println("RUN TEMPLATE");
    }

    protected abstract void method();
}
```

```java
public class SubType extends SuperType {

    @Override
    protected void method() {
        System.out.println("RUN METHOD");
    }
}
```

위 처럼 템플릿 메소드 패턴을 적용해 개발함으로 써 자주 변경되지 않는 부분을 한곳에서만 관리할 수 있게 된다.