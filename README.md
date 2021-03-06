# Об'єктно-орієнтоване програмування 2 курс (java)

## Питання по теорії
[Скорочена версія](https://drive.google.com/open?id=1fZO9gzYMt9wHUD-VkbnfEyPG09cuuJ00)  
[Повна версія](https://drive.google.com/open?id=1bTnmY_W6dApQOWG5OaOT6HlunLaSFhMi)

## Практичні завдання до іспиту

### 1
***Організувати заповнення будь-якого двовимірного масиву значеннями, що обчислюють по формулі (індекс масиву\*індекс масиву).***
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        final int ROW = 5;
        final int COLUMN = 6;
        Integer[][] arr = new Integer[ROW][COLUMN];

        for(Integer[] tmpArr : fillArray(arr)) {
            for(Integer value : tmpArr) {
                System.out.print(value + " ");
            }
            System.out.println();
        }
    }

    public static <T extends Number> T[][] fillArray(T[][] arr) {
        Number nmb;
        for(int i = 0;i < arr.length;i++) {
            for(int j = 0;j < arr[i].length; j++) {
                nmb = i * j;
                arr[i][j] = (T)nmb;
            }
        }
        return arr;
    }
}
```
> *Бабін Олександр. [repl.it](https://repl.it/@entrancescoder/IndexMultiplCode)*

### 2
***Тестовий приклад , що демонструє особливості використання модифікаторів доступу.***

Модификатор доступа по умолчанию — означает, что мы явно не объявляем модификатор доступа в Java для класса, поля, метода и т.д.

Модификатор `private` — методы, переменные и конструкторы, которые объявлены как `private` в Java могут быть доступны только в пределах самого объявленного класса.

```java
public class Logger {
   private String format;
   public String getFormat() {
      return this.format;
   }
   public void setFormat(String format) {
      this.format = format;
   }
}
```

Модификатор `public` — класс, метод, конструктор, интерфейс и т.д. объявленные как `public` могут быть доступны из любого другого класса. Поэтому поля, методы, блоки, объявленные внутри public класса могут быть доступны из любого класса, принадлежащего к "вселенной" Java.

```java
public static void main(String[] arguments) {
   // ...
}
```

Модификатор `protected` — переменные, методы и конструкторы, которые объявляются как `protected` в суперклассе, могут быть доступны только для подклассов в другом пакете или для любого класса в пакете класса `protected`.

```java
class AudioPlayer {
   protected boolean openSpeaker(Speaker sp) {
      // детали реализации
   }
}

class StreamingAudioPlayer {
   boolean openSpeaker(Speaker sp) {
      // детали реализации
   }
}
```

Правила контроля доступа и наследования

Следующие правила в Java применяются для унаследованных методов:

1. Методы, объявленные как `public` в суперклассе, также должны быть `public` во всех подклассах.
2. Методы, объявленные как `protected` в суперклассе, должны либо быть либо `protected`, либо `public` в подклассах; они не могут быть private.
3. Методы, объявленные как `private` для всех не наследуются, так что нет никакого правила для них.

Модификаторы класса, метода, переменной и потока, используемые не для доступа
Java предоставляет ряд модификаторов не для доступа, а для реализации многих других функциональных возможностей:

1. модификатор `static` применяется для создания методов и переменных класса;
2. модификатор `final` используется для завершения реализации классов, методов и переменных;
3. модификатор `abstract` необходим для создания абстрактных классов и методов;
4. модификаторы `synchronized` и `volatile` используются в Java для потоков.


Модификатор `final` — используется для завершения реализации классов, методов и переменных.
```java
public class Test{
  final int value = 10;
  // Ниже приведены примеры объявления констант:
  public static final int BOXWIDTH = 6;
  static final String TITLE = "Менеджер";
  
  public void changeValue(){
     value = 12; //будет получена ошибка
  }
}
```
Основная цель в Java использования класса объявленного в качестве `final` заключается в предотвращении класс от быть подклассом. Если класс помечается как `final`, то ни один класс не может наследовать любую функцию из класса `final`.
```java
public final class Test {
   // тело класса
}
```

Модификатор `static` — применяется для создания методов и переменных класса.
```java
public class InstanceCounter {

   private static int numInstances = 0;

   protected static int getCount() {
      return numInstances;
   }

   private static void addInstance() {
      numInstances++;
   }

   InstanceCounter() {
      InstanceCounter.addInstance(); 
   }

   public static void main(String[] arguments) {
      System.out.println("Начиная с " +
      InstanceCounter.getCount() + " экземпляра");
      for (int i = 0; i < 500; ++i){
         new InstanceCounter();
      }
      System.out.println("Создано " +
      InstanceCounter.getCount() + " экземпляров");
   }
}
```

Модификатор `abstract` — используется для создания абстрактных классов и методов.

```
public abstract class SuperClass{
    abstract void m(); //абстрактный метод
}

class SubClass extends SuperClass{
     // реализует абстрактный метод
      void m(){
      .........
      }
}
```

Модификатор `synchronized` — используются в Java для потоков.
Доступен только одним потоком одновременно

```java
public synchronized void showDetails(){
.......
}
```

Модификатор `volatile` — используются в Java для потоков.
используется, чтобы позволить знать JVM, что поток доступа к переменной всегда должен объединять свою собственную копию переменной с главной копией в памяти

```java
public class MyRunnable implements Runnable{
    private volatile boolean active;
 
    public void run(){
        active = true;
        while (active){ // линия 1
            // здесь какой-нибудь код
        }
    }
    
    public void stop(){
        active = false; // линия 2
    }
}
```
> *Бабін Олександр*

### 3
***Тестовий клас, що демонструє не менш 5 методів класу Arrays.***
```java
int arr[] = {3,0,4,5};
Arrays.asList(arr);                                     // arr as List
Arrays.binarySearch(arr,0);                             // binary search
int arr1[] = Arrays.copyOf(arr, 5);                     // copy 5 values from arr to arr1
Arrays.fill(arr1, 1);                                   // fill arr1 with 1
Arrays.sort(arr1);                                      // sort
Arrays.stream(arr1).forEach(System.out::println);       // return array as stream and print
```
> *Волков Андрій*

### 4
***Тестовий клас, що демонструє не менш 10 методів класу String.***
```java
String str = "hello";
str.charAt(0);                 //returns 'h'
str.compareTo("abc");          //compares strings
str.equalsIgnoreCase("HELLO"); // 
str.concat(" world");          //concating string
str.indexOf("el");             //return index of substring
str.length();                  // length
str.replace('h', 'a');         //replaces all 'h' with 'a'
str.substring(1, 3);           // substring that begins at 1 and extends to character at pos 3
str.toCharArray();             //string to char[]
str.toLowerCase();             //string to lower case
```
> *Волков Андрій*

### 5
***Створити статичний метод, що сортує масив, який передається як параметр, у такий спосіб, що перший елемент стає останнім, другий-передостаннім і т.д.***
```java
static int[] sort(int arr[]){
    int z = arr.length-1;
    int temp;
    for (int i = 0; i < z; i++) {
        temp = arr[z];
        arr[z] = arr[i];
        arr[i] = temp;
        z--;
    }
    return arr;
}
```
> *Гапішко Дмитро*

### 6
***Покажіть на тестовому  прикладі особливості використання ключового слова `final` для класів, змінних і методів.***
###### Змінна.

При оголошені змінних з ключовим словом `final` ми забороняємо її зміну. Ці змінні застосовуються головним чином в двух ситуаціях: для запобігання випадкової зміни параметра методу, а також змінних, до яких звертаються анонімні класи.
```java
final int count = 4;
if (count != 0) {
  count = 2;  
}
```
В такому випадку буде помилка, так як змінна `count` оголошена як `final` тому не може бути змінена.

###### Клас.

При оголошені класу з ключовим словом `final` він не може наслідуватись іншими класами.
Якщо клас `final`, то всі його методи неявно `final`.
```java
final class My {
    int k;

    public My() {}

    public My(int k) {
        this.k = k;
    }  
}

class My1 extends My {}
```
В такому випадку буде помилка, так як клас `My` оголошений з ключовим словом `final`, а клас `My1` намагається його наслідувати.

###### Метод.

Якщо при створенні класу `final` не все задовольняє нашим потребам, і ми дійсно
хочемо захистити тільки деякі з методів нашого класу від того, щоб бути
наслідуваними, ми можемо використовувати ключове слово `final` в оголошенні
методу, щоб вказати компілятору, що метод не може бути наслідуваний підкласами.
Метод `final` реалізується тільки одого разу, в оголошенні класу. Такий метод
не може бути наслідуваний: підкласи не можуть замінити метод новим визначенням.
Будь-які приватні методи в класі неявно `final`. 
```java
class My {        
    final int[] sort(int arr[]){
        int z = 0;
        int sarr[] = new int[arr.length];
        for (int i = arr.length-1; i >= 0; i--) {
            sarr[z] = arr[i];
            z++;
        }
        return sarr;
    }
}
```
В такому випадку клас, що наслідує клас `My` не може перевизначити метод `sort`.

> *Гапішко Дмитро*

### 7
***Покажіть на тестовому прикладі особливості використання атрибутів интерфейсів
змінних. Реалізуйте в коді лічильник створених об'єктів певного типу.***

```java
interface ICount {
   public void Show();
}

static class Counter implements ICount {
   public static int i;

   public Counter(){
       i++;
   }

   @Override
   public void Show() {
       System.out.println("wow");
   }
}
public static void main(String[] args) {
    Counter i = new Counter(), j = new Counter(), z = new Counter();
    System.out.println(i.i);
}
```
> *[Косолап Ілля](https://t.me/kasper_1291)*

### 8
***Створіть клас із декількома різними типами атрибутів. У класі реалізуйте
метод, що дозволяє одержати копію об'єкта даного типу й інший метод, що дозволяє
зрівняти 2 об'єкти, даного типу.***

```java
class Comp_n_copy {
    public Integer i = 5;
    public Double j = 10.2;
    public String z = new String("qwerty");

    public void Copy(Comp_n_copy obj) {
        this.i = obj.i;
        this.j = obj.j;
        this.z = obj.z;
    }

    public int Copmare(Comp_n_copy obj) {
        return this.i - obj.i;
    }
}
```
> *[Косолап Ілля](https://t.me/kasper_1291)*

### 9
***Створіть статичні методи, що дозволяють піднести до степеня, скласти, відняти
або розділити 2 змінні, типу int. Організуйте обробку виключень, пов'язаних з діленням на 0.***

```java
public class Exam {
    static int sum_2_vars(int one, int two) {
        return one+two;
    }

    static int minus_2_vars(int one, int two) {
        return one-two;
    }

    static int power_2_vars(int one, int two) {
        return (int) Math.pow(one, two); 
    }

    static int divide_2_vars(int one, int two) {
        int rez=0;
        try {
            rez = one / two;
        }
        catch (ArithmeticException e) {
            System.out.println("Нельзя делить на ноль!");
        }
        return rez;
    }

    public static void main(String[] args) {
        System.out.println(sum_2_vars(5, 6));
        System.out.println(minus_2_vars(5, 6));
        System.out.println(power_2_vars(5, 6));
        System.out.println(divide_2_vars(5, 0));
    }
}
```
> *[Кір'якіді Анна](https://t.me/kiryakidi)*

### 10
***Створіть статичний метод, що дозволяє знайти суму всіх елементів діапазону чисел типу int.***

```java
public class Exam {
    static int sum_all_elements(int arr[]) {
        int sum=0;
        for (int m: arr) {
            sum+=m;
        }
        return sum;
    }

    public static void main(String[] args) {
        int arr[]={1, 2, 3, 4, 5, 6, 7, 8, 9};
        System.out.println(sum_all_elements(arr));
    }
}
```
> *[Кір'якіді Анна](https://t.me/kiryakidi)*

### 11
***Реалізуйте тестовий клас, що показує різний характер операції присвоювання (а1=а2), для посилальних і простих типів даних.***

```java
public class Main {
    static class Eq {
        public String str = "";
        public int dec = 0;
    }

    public static void main(String[] args) {
        Eq one = new Eq(), two=new Eq();
        one.str = new String("abv");
        two.str = "abv";
        one.dec = 1;
        two.dec = 1;

        System.out.println(two.str.equals(one.str));
        System.out.println(two.dec == one.dec);
    }
}
```
> *[Медвінський Сергій](https://t.me/Ya_ne_umeyu_pridumyvat_niki)*

### 12
***Створіть код, що відображає взаємозв'язки між наступними сутностями - Ссавці,
Птахи, сокіл, соловей, Хижаки, Котячі, сіамські коти, бенгальські тигри. При
цьому врахувати, що деякі живі організми можуть комбінувати в собі ознаки
декількох груп (сокіл - птах і хижак одночасно, сіамські коти - ссавці, хижаки
й крім того належать до загону Котячі)***

```java
interface Carnivore {}
interface Bird {}
interface Mammal {}
interface Cats {}
class Hawk implements Bird, Carnivore {}
class Songbird implements Bird {}
class Siam implements Mammal, Cats, Carnivore {}
class Tiger implements Mammal, Cats, Carnivore {}
```
> *[Медвінський Сергій](https://t.me/Ya_ne_umeyu_pridumyvat_niki)*

### 13
***Створіть код, що відображає взаємини між наступними обьектами реального світу - Людина, Учень, Пенсіонер***

```java
class Human implements studying, Pensiya {
    int age;
    String name;
    String studPlace;
    String studHistory;
    int studYears;
    double money = 0;

    public Human(int age, String name, double money, String studPlace, String studHistory){
        this.age = age;
        this.name = name;
        this.studPlace = studPlace;
        this.studHistory = studHistory;
        this.money = money;
    }

    @Override
    public void startStudy(String place, int years) {
        studPlace = place;
        studYears = years;
    }

    @Override
    public void StopStudy() {
        if (studYears==0) {
            studHistory += studPlace;
            studPlace = null;
        }               
    }

    @Override
    public void getPensiya() {
        if (age>65)
            money+=3500;
    }
}

interface studying {
    void startStudy(String place,int years);
    void StopStudy();
}

interface Pensiya {
    void getPensiya();
}
```
> *Паленко Роман*

### 14
***Реалізуйте тестовий клас, що демонструє особливості передачі параметрів у метод класу (наприклад масиву типу int)***

```java
class Test {
    static void Testarr(int arr[]) {
        for (int a : arr) {
            System.out.println(a);
        }
    }
}

public class Main {
    public static void main(String args[]) {
        int arr[] = {1, 2, 3, 4, 5};
        Test.Testarr(arr);
    }
}
```
> *Паленко Роман*

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
