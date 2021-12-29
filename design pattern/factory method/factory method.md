## Factory Method Pattern

팩토리 메소드 패턴이란 서브 클래스에서 객체 생성을 담당하도록 하는 패턴을 말한다.

즉, 객체를 생성하는 공장을 만드는 패턴이다.

이러한 팩토리 메소드 패턴을 사용하는 이유는 컴파일 타임에 어떤 구체 타입을 사용해야할지 모르거나 이를 사용하는 클래스에서 구체 클래스의 이름을 감추기 위해서 사용된다.



```java
public class ShapeFactory {
    
    public Shape getInstance(String type) {
        switch (type) {
            case "circle":
                return new Circle();
            case "rectangle":
                return new Rectangle();
            default:
                return null;
        }
    }
}
```

```java
public class Circle implements Shape {

    @Override
    public void print() {
        System.out.println("Circle");
    }
}

public class Rectangle implements Shape {

    @Override
    public void print() {
        System.out.println("Rectangle");
    }
}
```