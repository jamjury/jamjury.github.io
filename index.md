# Практичні завдання до іспиту

### 15
***Створіть тестовий приклад, що демонструє 2 способи створення потоків, – з
використанням спадкоємства від класу Thread та через реалізацію інтерфейсу Runnable.***
```java
class ThreadTask {
    public static void main(String args[]) {
        Thread runner = new Thread(new Escapee());
        Thread weaver = new CottonThread();
        runner.start();
        weaver.start();
    }
}

class Escapee implements Runnable {
    @Override
    public void run() {
        System.out.println("It runs");
    }
}

class CottonThread extends Thread {
    @Override
    public void run() {
        System.out.println("It weaves");
    }
}
```
```
It runs
It weaves
```

### 16
***Продемонструйте в тестовому прикладі вплив пріоритету потоку на розподіл обчислювальних ресурсів між потоками.***
```java
class PriorityTask {
    private static volatile boolean run = true;

    public static void main(String args[]) throws InterruptedException {
        final int[] counter = {0, 0, 0};
        Thread highPriorityThread = new Thread(() -> {
            System.out.println("High priority thread started");
            while (run) {
                counter[0]++;
            }
        });
        Thread normPriorityThread = new Thread(() -> {
            System.out.println("Norm priority thread started");
            while (run) {
                counter[1]++;
            }
        });
        Thread lowPriorityThread = new Thread(() -> {
            System.out.println("Low priority thread started");
            while (run) {
                counter[2]++;
            }
        });
        highPriorityThread.setPriority(Thread.MAX_PRIORITY);
        normPriorityThread.setPriority(Thread.NORM_PRIORITY);
        lowPriorityThread.setPriority(Thread.MIN_PRIORITY);
        lowPriorityThread.start();
        normPriorityThread.start();
        highPriorityThread.start();
        Thread.sleep(1000);
        run = false;
        System.out.println("High: " + counter[0]);
        System.out.println("Norm: " + counter[1]);
        System.out.println("Low : " + counter[2]);
    }
}
```
```
Low priority thread started
Norm priority thread started
High priority thread started
High: 94546044
Norm: 97516158
Low : 95815129
```

### 17
***Продемонструйте в тестовому прикладі головну особливість використання в потоках синхронізованих методів.***
```java
class SynchronizedTask {
    public static void main(String args[]) {
        new Thread(SynchronizedTask::work, "Thread 1").start();
        new Thread(SynchronizedTask::work, "Thread 2").start();
    }

    static synchronized void work() {
        System.out.println(Thread.currentThread().getName() + ": began working");
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            return;
        }
        System.out.println(Thread.currentThread().getName() + ": ended working");
    }
}
```
```
Thread 1: began working
Thread 1: ended working
Thread 2: began working
Thread 2: ended working
```

Якщо метод синхронізований, то потоки не можуть виконувати його одночасно
(наприклад, друкувати щось на екран, або у файл), тобто, не створюють колізій.

Якби метод не був синхронізований, то на виході було б

```
Thread 1: began working
Thread 2: began working
Thread 1: ended working
Thread 2: ended working
```

### 18
***Створіть тестовий приклад, що демонструє особливості застосування перерахувань у упраляючій конструкції switch.***
```java
enum Digit {
    ZERO, ONE, TWO, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT, NINE;

    public static void main(String args[]) {
        for (Digit digit: Digit.values())
            test(digit);
    }

    private static void test(Digit digit) {
        System.out.print(digit.toString() + " is ");
        switch (digit) {
            case ZERO: case ONE: case TWO:
                System.out.println("too little");
                break;
            case THREE:
                System.out.println("less than enough");
                break;
            case SIX: case SEVEN:
                System.out.println("more than enough");
                break;
            case EIGHT: case NINE:
                System.out.println("too much");
                break;
            default:
                System.out.println("enough");
        }
    }
}
```
```
ZERO is too little
ONE is too little
TWO is too little
THREE is less than enough
FOUR is enough
FIVE is enough
SIX is more than enough
SEVEN is more than enough
EIGHT is too much
NINE is too much
```

### 19
***Покажіть в тестовому прикладі, що за умовчанням дочірні потоки
створюються з таким же значенням пріоритету, що і головний потік.***
```java
class ChildPriorityTask {
    public static void main(String args[]) {
        Thread.currentThread().setName("Grandparent");
        printPriority();
        new Thread(() -> {
            printPriority();
            Thread.currentThread().setPriority(6);
            printPriority();
            new Thread(ChildPriorityTask::printPriority, "Child").start();
        }, "Parent").start();
    }

    private static void printPriority() {
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName() + " thread priority: " + thread.getPriority());
    }
}
```
```
Grandparent thread priority: 5
Parent thread priority: 5
Parent thread priority: 6
Child thread priority: 6
```

### 20
***У тестовому прикладі створіть свій тип анотацій, покажіть яким чином
можна аналізувати значення анотацій при виконанні програми.***
```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

class AnnotationTask {
    public static void main(String args[]) {
        Class<?>[] labors = new Class<?>[]{Mining.class, WoodCutting.class, Hunting.class};
        for (Class<?> labor : labors) {
            System.out.println(labor.getSimpleName() + " requires " +
                    labor.getAnnotation(ToolRequired.class).value());
        }
    }
}

enum Tool {
    BOW, AXE, PICK
}

@Retention(RetentionPolicy.RUNTIME)
@interface ToolRequired {
    Tool value();
}

@ToolRequired(Tool.PICK)
class Mining {}

@ToolRequired(Tool.AXE)
class WoodCutting {}

@ToolRequired(Tool.BOW)
class Hunting {}
```
```
Mining requires PICK
WoodCutting requires AXE
Hunting requires BOW
```

### 21
***Продемонструйте в коді головну особливість колекцій, що реалізовують інтерфейс Set.***
```java
import java.util.*;

class SetTask {
    public static void main(String args[]) {
        String words[] = "you are not you when you are hungry".split(" ");
        Set<String> uniqueWords = new HashSet<>(Arrays.asList(words));
        uniqueWords.addAll(Arrays.asList("not", "commercial"));
        System.out.println(uniqueWords);
    }
}
```
```
[hungry, not, commercial, are, when, you]
```

### 22
***Продемонструйте на працюючому коді відмінність інтерфейсу Iterator від інтерфейсу Listiterator.***
```java
import java.util.*;

class IteratorTask {
    public static void main(String args[]) {
        List<String> words = new ArrayList<>(Arrays.asList("you are good".split(" ")));
        Iterator<String> iter = words.iterator();
        System.out.println(iter.hasNext());           // true
        System.out.println(iter.next());              // "you"
        System.out.println(iter.next());              // "are"
        System.out.println(iter.next());              // "good"
        System.out.println(iter.hasNext());           // false
        iter.remove();
        System.out.println(words);                    // ["you", "are"]
    }
}
```
```java
import java.util.*;

class ListIteratorTask {
    public static void main(String args[]) {
        List<String> words = new ArrayList<>(Arrays.asList("you are good".split(" ")));
        ListIterator<String> listIter = words.listIterator();
        System.out.println(listIter.hasNext());       // true
        System.out.println(listIter.hasPrevious());   // false
        System.out.println(listIter.next());          // "you"
        System.out.println(listIter.next());          // "are"
        System.out.println(listIter.next());          // "good"
        System.out.println(listIter.hasNext());       // false
        System.out.println(listIter.hasPrevious());   // true
        System.out.println(listIter.previous());      // "good"
        System.out.println(listIter.previous());      // "are"
        System.out.println(listIter.nextIndex());     // 1
        System.out.println(listIter.previousIndex()); // 0
        listIter.remove();
        System.out.println(words);                    // ["you", "good"]
        System.out.println(listIter.next());          // "good"
        listIter.set("are");
        System.out.println(words);                    // ["you", "are"]
        listIter.add("bad");
        System.out.println(words);                    // ["you", "are", "bad"]
    }
}
```

### 23
***Продемонструйте роботу 5 будь-яких статичних методів класу Collections.***
```java
import java.util.*;

class CollectionsTask {
    public static void main(String args[]) {
        List<Integer> list = Arrays.asList(1, 3, 0, 2, 4, 5, 1, 0);
        Collections.sort(list);
        System.out.println(list);                            // [0, 0, 1, 1, 2, 3, 4, 5]
        System.out.println(Collections.frequency(list, 1));  // 2
        System.out.println(Collections.max(list));           // 5
        Collections.fill(list, 1);
        System.out.println(list);                            // [1, 1, 1, 1, 1, 1, 1, 1]
        Collections.replaceAll(list, 1, 0);
        System.out.println(list);                            // [0, 0, 0, 0, 0, 0, 0, 0]
    }
}
```
