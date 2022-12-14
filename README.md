# JVM
## **Код для исследования**
```java
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```
## **Описание кода с точки зрения происходящего в JVM**
Загрузка классов начинается с ClassLoader’ов (Application, Platform, Bootstrap). Данные о найденных классах помещаются в Metaspace.

1. В момент вызова метода `main()` создаётся фрейм (кадр) в стеке (Stack Memory). Далее в этот фрейм записывается и хранится там примитив `int i = 1`.
1. Во фрейм записывается переменная «о», которая содержит в себе ссылку на объект `Object`, который хранится в heap (куча).
1. Во фрейм записывается переменная `ii = 2`, которая содержит ссылку на объект `Integer`, хранящийся в куче.
1. В момент вызова метода `printAll()` создаётся новый фрейм в стеке. Туда добавляется примитив `int i`, и переменные «о» и «ii», 
которые хранят в себе ссылки на объекты `Object` и `Integer`, находящиеся в куче.
1. В новый фрейм `printAll()` записывается переменная `uselessVar = 700`, которая ссылается на объект `Integer` из кучи.
1. Создаётся новый фрейм в стеке с методом `println()`. Создается еще один фрейм с методом `toString()`, результат которого помещается в кучу. 
Ссылка на результат добавляется в фрейм `println()` вместе с переменной «i», а также «ii», которая содержит ссылку на объект `Integer` из кучи. 
Метод `println()` выполняется и значения складываются. Результат выводится на экран.
1. Создаётся новый фрейм в стеке с методом `println()`, куда передаётся ссылка на объект типа `String` в куче со значением "finished", которое выводится на экран.

Garbage Collection периодически собирает объекты из памяти (heap), которые больше не используются.  Они удаляются из кучи и стеков.
