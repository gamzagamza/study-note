## Adapter Pattern

어댑터 패턴이란 기존의 코드를 수정하지 않고 호환되지 않는 객체의 기능을 사용하기 위해 사용되는 디자인 패턴이다.



아래의 코드를 보면 기존에 Main Object는 Printer 인터페이스를 확장하는 OldPrinter 객체를 사용해 로그를 출력하고 있다.

```java
public class Client {
    
    private Printer printer;
    
    public Client(Printer printer) {
        this.printer = printer;
    }
    
    public void run() {
        String message = "Run!";
        printer.print(message);
    }
}
```

```java
public interface Printer {
    public void print(String message);
}
```

```java
public class OldPrinter implements Printer {

    @Override
    public void print(String message) {
        System.out.println("OLD : " + message);
    }
}
```



개발 중 로그에 관한 변경사항이 생겨 새로운 로그 출력을 위한 클래스가 정의되었는데 이 클래스를 사용하기 위해서는 아래처럼 기존의 코드를 수정해야하는 문제가 생긴다.

```java
public class Client {
    
    private NewPrinter newPrinter;
    
    public Client(NewPrinter newPrinter) {
        this.newPrinter = newPrinter;
    }
    
    public void run() {
        String message = "Run!";
        newPrinter.print(message);
    }
}
```

```java
public class NewPrinter {

    public void print(String message) {
        System.out.println("NEW : " + message);
    }
}
```



어댑터 패턴을 사용하면 이러한 문제를 해결할 수 있는데 다음과 같이 Printer를 확장하고 NewPrinter에 의존하는 Adapter클래스를 정의하면 된다.

```java
public class PrinterAdapter implements Printer {

    private final NewPrinter printer = new NewPrinter();

    @Override
    public void print(String message) {
        printer.print(message);
    }
}
```

PrinterAdapter는 Printer를 확장하고 있기 때문에 Client는 해당 어댑터를 주입받기만 하면 기존의 코드를 수정하지 않고 새로운 로그의 기능을 사용할 수 있게 된다. 