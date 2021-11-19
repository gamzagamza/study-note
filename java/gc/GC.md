## Garbage Collector

Garbage Collector 줄여서 GC란 JVM의 데이터 영역 중 Heap영역에 생성된 객체들에 대해 참조되고있지 않은 객체를 영역에서 제거해주는 역할을 하는 JVM의 구성요소 중 하나이다.

Java를 사용해 개발하는 개발자는 이러한 GC덕에 다른 언어(C/C++)에 비해 메모리관리에 신경을 덜 쓰면서 개발할 수 있다.



## Heap Area

GC의 대상이 되는 Heap영역은 다음과 같이 구성되어있다.

![heap](/Users/ywj/Documents/study-note/java/gc/images/heap.png)

새롭게 생성되는 객체는 Eden 영역에 생성되어진다.

이후 계속해서 새로운 객체가 생겨나 Eden 영역이 가득차게되면 Eden 영역의 객체를 Servivor1 영역으로 이동시킨다.

Eden 영역과 Servivor1 영역이 차게되면 객체들중 참조되고있는 객체를 Survivor2 영역으로 이동시킨 후 나머지 영역을 초기화 한다. 그리고 다시 Eden 영역과 Servivor2 영역이 가득차게되면 참조되고있는 객체를 Survivor1 영역으로 이동시킨 후 나머지 영역을 초기화 시킨다.

위 과정을 반복하며 일정 횟 수 이상 살아남은 객체들은 Old 영역으로 이동하게된다.

이러한 과정을 Minor GC라고 부른다.



Old 영역에 대해 일어나는 GC에 대해선 Full GC / Major GC라고 부른다.

Young 영역의 객체들은 비교적 수명이 짧고 많은 객체를 검사하지 않기 때문에 빠르게 GC가 일어나지만 Old 영역은 수명이 긴 객체들을 모두 검사해야하기 때문에 GC에 많은 시간이 소요된다.



추가적으로 Permanent 영역은 Java 1.8 이전 버전에서 사용되던 영역으로 클래스와 메소드의 메타정보와 static object, 문자열 상수등이 저장되던 공간이다.

공간의 크기가 고정이기때문에 OutOfMemory 등의 오류가 발생할 가능성이 있어 Java 1.8 부터는 Metaspace라는 영역으로 대체되었다. 또한, static object와 문자열 상수가 Heap 영역에 할당되도록 변경되었다.

Metaspace 영역은 JVM내에 있지않고 OS 영역에 존재해 OS가 메모리를 관리하며 Default 크기를 가지고 있지 않다.



## GC의 종류

가장 간단한 GC는 Serial GC 이며 Mark-Sweep-Compaction 알고리즘을 사용한다.

Mark 단계에서 살아있는 객체를 확인하고 Sweep 단계에서 표시된 객체를 제거한다. 이후 Compaction 단계에서 메모리 단편화를 방지하기 위해 메모리의 앞 부분부터 살아있는 객체들을 채운다.



Serial GC는 단순하지만 실제로 사용하기에는 너무 느리다는 단점이 있다. 이를 조금 보완해서 나온 GC로 Parallel GC가 있다.

Parallel GC의 처리과정은 Serial GC와 동일하지만 Minor GC 수행 시 병렬로 GC를 수행함으로써 수행시간을 줄였다.



Old 영역은 병렬로 처리하지 못한다는 Parallel GC의 단점이 존재하는데 이를 보완하기 위해 Parallel Old GC가 있다.

Parallel Old GC는 Old 영역에대해 Mark-Summary-Compaction알고리즘을 사용해 GC를 수행한다.



CMS(Concurrent Mark Sweep)

CMS GC는 Parallel GC와 마찬가지로 여러개의 쓰레드를 이용하지만 기존의 GC와는 다르게 Mark Sweep알고리즘을 Concurrent하게 수행한다.

1. Initial : 클래스로더에서 가장 가까운 객체들 중 살아있는 객체를 찾는다.
2. Concurrent Mark : initial 객체에서 살아있다고 표시한 객체가 참조하는 객체를 따라가며 확인한다.
3. Remark : Concurrent Mark단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다.
4. Concurrent Sweep : 참조가 끊긴 객체들을 제거한다.



G1

G1 GC는 다른 GC처럼 각 영역들이 고정된 위치에 존재하는 것이 아니라 전체 힙 메모리를 Region이라는 특정한 크기로 나눠 각 역할을 동적으로 부여한다.

G1 GC에서 수행되는 Minor GC는 GC대상이 많은 Region에서 수행되며 살아남은 객체를 다른 Region으로 옮긴 후 원래 Region을 사용가능 상태로 변경한다.

Major GC는 다음과 같이 수행된다

1. Initial Mark : Old 영역의 객체를 참조하고있는 객체가 존재하는 Region을 식별한다.
2. Root Region Scanning : Old 영역의 객체를 참조하고있는 Young영역의 객체를 식별한다.
3. Concurrent Mark : 전체 Heap영역을 스캔하며 참조되고있는 객체를 식별한다.
4. Remark : 이전 단계의 결과를 검증한다.
5. Clean Up / Copy : 살아있는 객체들을 비어있는 영역으로 옮긴 후 참조되지 않는 객체들을 제거한다.