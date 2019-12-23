# Зміст

${toc}

# Exceptions

Виняток - це проблема (помилка), яка виникає під час виконання програми. Винятки можуть виникати у багатьох випадках, наприклад:

- Користувач ввів некоректні дані.
- Файл, до якого звертається програма, не знайдений.
- Мережеве з'єднання з сервером було втрачено під час передачі даних.

Якщо ми, наприклад запустимо наступний код:

```java
public class Main {
	
	public static int div() {
		return 1 / 0;
	}

	public static void main(String[] args) {
		div();
	}

}
```

Як результат ми отримаємо виключення java.lang.ArithmeticExceptions

```
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at demo.Main.div(Main.java:6)
	at demo.Main.main(Main.java:10)
```

Для того, щоб прграма не завершалася аварійно, виключення потрібно перехопити та обробити.

Для цього використовуються 3 ключових слова:

1. **try** - це ключове слово використовується для позначки початку блоку коду, який потенційно може привести до помилки.
2. **catch** - ключове слово для позначки початку блоку коду, призначеного для перехоплення і обробки винятків.
3. **finally** - ключове слово для позначки початку блоку коду, яке є додатковим. Цей блок поміщається після останнього блоку 'catch'. Управління зазвичай передається в блок 'finally' в будь-якому випадку.

Приклад обробки виключення:

```java
public class Main {
	
	public static int div() {
		return 1 / 0;
	}

	public static void main(String[] args) {
		try {
			div();
		}
		catch(Exception ex) {
			System.out.println("Exception " + ex.getMessage());
		}
		
	}
}
```

Блок коду ``` catch(Exception ex) ``` перехоплює всі можливі вийнятки, які трапляються. Перехоплювати загальне виключення, зазвичай, не дуже хороша ідея, тому ми можемо переписати код, який буде перехоплювати лише ArithmeticException:

```java
public class Main {
	
	public static int div() {
		return 1 / 0;
	}

	public static void main(String[] args) {
		try {
			div();
		}
		catch(ArithmeticException ex) {
			System.out.println("Exception " + ex.getMessage());
		}
		
	}
}
```

Також ми можемо перехоплювати різні винятки використовуючи множинні блоки catch(віж вужчого до загального):

```java
public class Main {
	
	public static int div() {
		return 1 / 0;
	}

	public static void main(String[] args) {
		try {
			div();
		}
		catch(ArithmeticException ex) {
			System.out.println("Exception " + ex.getMessage());
		}
		catch(Exception ex) {
			System.out.println("Somethong happens");
		}
		
	}
}
```

В Java 7 стала доступна нова конструкція, за допомогою якої можна перехоплювати кілька винятків одним блоком catch:

```java
try {  
 ... 
} catch( IOException | SQLException ex ) {  
  logger.log(ex); 
  throw ex; 
}
```

Тепер поговорімо детальніше про блок finally. Коли виняток передано, виконання методу направляється по нелінійному шляху. Це може стати джерелом проблем. Наприклад, при вході метод відкриває файл і закриває при виході. Щоб закриття файлу не було пропущено через обробку виключення, був запропонований механізм finally.

Ключове слово finally створює блок коду, який буде виконаний після завершення блоку try / catch, але перед кодом, наступним за ним. Блок буде виконаний, незалежно від того, передано виняток чи ні. Оператор finally не обов'язковий, проте кожен оператор try вимагає наявності або catch, або finally. Код в блоці finally буде виконаний завжди.

**Розгляньмо детальніше ієрархія вбудованих виключень в java**:

![](../resources/img/5/1.png)

Винятки діляться на кілька класів, але всі вони мають спільного предка - клас Throwable. Його нащадками є підкласи Exception і Error.

Винятки (Exceptions) є результатом проблем в програмі, які в принципі можуть бути вирішені і передбачувані. Наприклад, відбулося ділення на нуль в цілих числах.

Помилки (Errors) представляють собою більш серйозні проблеми, які, відповідно до специфікації Java, не слід намагатися обробляти у власній програмі, оскільки вони пов'язані з проблемами рівня JVM. Наприклад, виключення такого роду виникають, якщо закінчилася пам'ять, доступна віртуальній машині. Програма додаткову пам'ять все одно не зможе забезпечити для JVM.

У Java всі виключення діляться на два типи: контрольовані виключення (checked) і неконтрольовані виключення (unchecked), до яких відносяться помилки (Errors) і виключення часу виконання (RuntimeExceptions, нащадок класу Exception).

Контрольовані виключення є помилки, які можна і потрібно обробляти в програмі, до цього типу належать усі нащадки класу Exception (але не RuntimeException).

Давайте ще раз поглянемо на код:

```java
public class Main {
	
	public static int div() {
		return 1 / 0;
	}

	public static void main(String[] args) {
		div();
	}

}
```

Ми можемо не використовувати try...catch оскільки AriphmeticExcpetion є uncheked. А тепер, погляньмо на інший код:

```java
public class Main {
	
	public static void read() {
		InputStream is = new FileInputStream("manifest.mf");
		BufferedReader buf = new BufferedReader(new InputStreamReader(is));
		        
		String line = buf.readLine();
		StringBuilder sb = new StringBuilder();
		        
		while(line != null){
		   sb.append(line).append("\n");
		   line = buf.readLine();
		}
		        
		String fileAsString = sb.toString();
		System.out.println("Contents : " + fileAsString);

	}

	public static void main(String[] args) {
		read();
	}
}
```

Цей код не скомпілюється, оскільки він містить checked exceptions, і нам потрібно їх обробляти. Або в самому методі:

```java
public static void read() {
		InputStream is = null;
		try {
			is = new FileInputStream("manifest.mf");
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		BufferedReader buf = new BufferedReader(new InputStreamReader(is));
		        
		String line = null;
		try {
			line = buf.readLine();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		StringBuilder sb = new StringBuilder();
		        
		while(line != null){
		   sb.append(line).append("\n");
		   try {
			line = buf.readLine();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		}
		        
		String fileAsString = sb.toString();
		System.out.println("Contents : " + fileAsString);

	}
```

або у місті виклику цього методу (в цьому випадку метод описує всі виключення, які він може кинути):

```java
public class Main {
	
	public static void read() throws IOException {
		InputStream is = new FileInputStream("manifest.mf");
		BufferedReader buf = new BufferedReader(new InputStreamReader(is));
		        
		String line = buf.readLine();
		StringBuilder sb = new StringBuilder();
		        
		while(line != null){
		   sb.append(line).append("\n");
		   line = buf.readLine();
		}
		        
		String fileAsString = sb.toString();
		System.out.println("Contents : " + fileAsString);

	}

	public static void main(String[] args) {
		try {
			read();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

Насправді обробку виключення ще можна передати самій JVM:

```java
public class Main {
	
	public static void read() throws IOException {
		InputStream is = new FileInputStream("manifest.mf");
		BufferedReader buf = new BufferedReader(new InputStreamReader(is));
		        
		String line = buf.readLine();
		StringBuilder sb = new StringBuilder();
		        
		while(line != null){
		   sb.append(line).append("\n");
		   line = buf.readLine();
		}
		        
		String fileAsString = sb.toString();
		System.out.println("Contents : " + fileAsString);

	}

	public static void main(String[] args) throws IOException {
			read();
	}
}
```

## Створення власного exception

Система не може передбачити всі винятки, іноді вам доведеться створити власний тип виключення для вашої програми. Вам потрібно успадковуватися від Exception (нагадаю, що цей клас успадковується від Trowable) і перевизначити потрібні методи класу Throwable. Або ви можете успадковуватися від вже існуючого типу, який найбільш близький за логікою з вашим винятком.

Наприклад:

```java
class ZeroAmountException extends Exception {
	
	public ZeroAmountException(String errorMessage) {
		super(errorMessage);
	}
}

public class Main {
	
	public static void evalAmount (int amount) throws ZeroAmountException {
		if (amount == 0)
			throw new ZeroAmountException("Amount cant be zero");
		System.out.println("Everything is ok!!!");
	}

	public static void main(String[] args) {
			try {
				evalAmount(0);
			}
			catch(ZeroAmountException ex) {
				System.out.println(ex.getMessage());
			}
	}
}
```


# Lambda - expressions

Java замислювалася як об'єктно-орієнтована мова в 90-і роки, коли об'єктно-орієнтоване програмування було головною парадигмою в розробці додатків. Задовго до цього були функціональні мови програмування, такі, як Lisp і Scheme, але їх переваги не були оцінені за межами академічного середовища. Останнім часом функціональне програмування сильно виросло в значущості, тому що воно добре підходить для паралельного програмування та програмування, заснованого на події ( «reactive»). Це не означає, що об'єктна орієнтованість - погано. Навпаки, замість цього, виграшна стратегія - змішувати об'єктно-орієнтоване програмування та функціональне. Це має сенс, навіть якщо вам не потрібна паралельність. Наприклад, бібліотеки колекцій можуть отримати потужний API, якщо мова має зручний синтаксис для функціональних виразів.

У функціональних мовах програмування на перший план виходять функції. Вони існують самі по собі. Можна привласнювати їх змінним і передавати через аргументи іншим функціям. JavaScript один з кращих прикладів функціональних мов програмування. На просторах Інтернету можна знайти хороші статті, в яких детально описані переваги JavaScript як функціональної мови. Функціональні мови мають в своєму арсеналі такі потужні інструменти як замикання (Closure), які забезпечують ряд переваг на традиційними способами створення програмного забезпечення. Замикання - це функція з прив'язаною до неї середовищем - таблицею, що зберігає посилання на всі нелокальних змінні функції. В Java замикання можна імітувати через Lambda-вирази. Безумовно між замиканнями і Lambda-виразами є відмінності і не малі, але Lambda-вирази є хорошою альтернативою замикань.

Lambda-вирази - це анонімні функції (може і не 100% вірне визначення для Java, але зате привносить деяку ясність). Простіше кажучи, це метод без оголошення, тобто без модифікаторів доступу, поверненого значення і ім'я.

Коротше кажучи, вони дозволяють написати метод і відразу ж використовувати його. Особливо корисно в разі одноразового виклику методу, тому що скорочує час на оголошення і написання методу без необхідності створювати клас.

Lambda-вирази в Java зазвичай мають наступний синтаксис (аргументи) -> (тіло). наприклад:

```
(арг1, арг2...) -> { тіло }

(тип1 арг1, тип2 арг2...) -> { тіло }
```

Далі йде кілька прикладів справжніх Lambda-виразів:

```java
(int a, int b) -> {  return a + b; }

() -> System.out.println("Hello World");

(String s) -> { System.out.println(s); }

() -> 42

() -> { return 3.1415 };
```

Структура Lambda-виразів:

- Lambda-вирази можуть мати від 0 і більше вхідних параметрів.
- Тип параметрів можна вказувати явно або може бути отриманий з контексту. Наприклад (int a) можна записати і так (a)
- Параметри моміщаються в круглі дужки і розділяються комами. Наприклад (a, b) або (int a, int b) або (String a, int b, float c)
- Якщо параметрів немає, то потрібно використовувати порожні круглі дужки. Наприклад () -> 42
- Коли параметр один, якщо тип не вказується явно, дужки можна опустити. Приклад: a -> return a * a
- Тіло Lambda-вирази може містити від 0 і більше виразів.
- Якщо тіло складається з одного оператора, його можна не укладати в фігурні дужки, а повертається значення можна вказувати без ключового слова return.
- В іншому випадку фігурні дужки обов'язкові (блок коду), а в кінці треба вказувати значення, що повертається з використанням ключового слова return (в іншому випадку типом значення, що повертається буде void).

## Функціональний інтерфейс

Функціональні інтерфейси в Java 8 - це інтерфейси, які містять в собі тільки один абстрактний метод. Функціональні інтерфейси мають тісний зв'язок з лямбда виразами і служать як основа для застосування лямбда виразів у функціональному програмуванні на Java.

Давайте розглянемо приклад функціонального інтерфейсу:

```java
@FunctionalInterface
interface functionalInterface{
    abstract public void abstractMethod(); 
}
```

functionalInterface, в нашому прикладі є типовим функціональним інтерфейсом, який містить в собі один абстрактний метод - abstractMethod (). Анотація @FunctionalInterface не обов'язкова, але я б рекомендував її використовувати, хоча б для самоконтролю:

```java
@FunctionalInterface // помилка компіляції
interface functionalInterface{
    abstract public void abstractMethod(); 
    abstract public void abstractMethod1();
}
```

Реалізація функціонального інтерфейсу, нічим не відрізняється від реалізації звичайного інтерфейсу:

```java
@FunctionalInterface 
interface functionalInterface{
    abstract public void abstractMethod(); 
}
 
class Test implements functionalInterface{
    @Override
    public void abstractMethod(){
    }
}
```

Крім звичайної реалізації класом, ми можемо реалізувати функціональні інтерфейси за допомогою лямбда виразів. Для початку створимо клас:

```java
class Car{
    private String name;
    private boolean isFullDrive;
    private boolean isGasEngine;
    
    public Car(String name, boolean isFullDrive, boolean isGasEngine){
        this.name = name;
        this.isFullDrive = isFullDrive;
        this.isGasEngine = isGasEngine;
    }
    
    public boolean isFullDrive(){
        return isFullDrive;
    }
    
    public boolean isGasEngine(){
        return isGasEngine;
    }
    
    @Override
    public String toString(){
        return name;
    }
}
```

Черга за функціональним інтерфейсом:

```java	
@FunctionalInterface
interface CheckCar{
    public boolean test(Car car);
}
```

```java
public class Example{
    private static void printTest(Car car, CheckCar check){
        if(check.test(car)){
            System.out.println(car);
        }
    }
    
    public static void main(String[] args) {
        Car audiA3 = new Car("AudiA3", true, true);
        Car audiA6 = new Car("AudiA6", true, false);
        printTest(audiA3, c -> c.isFullDrive());
        printTest(audiA3, c -> c.isGasEngine());
        printTest(audiA6, c -> c.isFullDrive());
        printTest(audiA6, c -> c.isGasEngine());
    }
}
```

Можна визначити функціональний інтерфейс, який є загальним для типу X і має функціональний метод, який приймає два аргументи типу X і повертає значення типу X.

```java
@FunctionalInterface
interface ArgumentsProcessor<X>
{
    X process(X arg1, X arg2);
}
```

Приклад використання:

```java
@FunctionalInterface
interface ArgumentsProcessor<X>
{
    X process(X arg1, X arg2);
}

class IntArgSystem {
	private ArgumentsProcessor<Integer> processor;
	
	public IntArgSystem(ArgumentsProcessor<Integer> processor) {
		this.processor = processor;
	}
	
	public Integer process(Integer a, Integer b) {
		return this.processor.process(a, b);
	}
}

public class Main {

	public static void main(String[] args) {
		IntArgSystem argSystem = new IntArgSystem((a, b) -> a + b);
		System.out.println(argSystem.process(2, 3));
	}
}
```

Можна визначити функціональний інтерфейс, який обмежений певними типами, використовуючи ключове слово розширення, тобто X extends Number:

```java
@FunctionalInterface
public interface ArgumentsProcessor<X extends Number>
{
    X process(X arg1, X arg2);
}
```

### Вбудовані функціональні інтерфейси

**Predicate**

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

Predicate - це твердження, яке може бути істинним або хибним залежно від значень його змінних. Це можна розглядати як функцію, яка повертає значення, яке є істинним, або хибним.

```java
Predicate<String> isALongWord = new Predicate<String>() {
    @Override
    public boolean test(String t) {
        return t.length() > 10;
    }
};
String s = "successfully"
boolean result = isALongWord.test(s);
```

**Consumer**

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

Цей функціональний інтерфейс являє собою операцію, яка приймає єдиний вхідний аргумент і не повертає результату. Справжній результат - це побічні ефекти, які він викликає.

```java
class Product {
  private double price = 0.0;

  public void setPrice(double price) {
    this.price = price;
  }

  public void printPrice() {
    System.out.println(price);
  }
}

public class Test {
  public static void main(String[] args) {
    Consumer<Product> updatePrice = p -> p.setPrice(5.9);
    Product p = new Product();
    updatePrice.accept(p);
    p.printPrice();
  }
}
```

**Function**

Цей функціональний інтерфейс являє собою функцію, яка приймає один аргумент і видає результат. Одне із використання, наприклад, це перетворення з одного об'єкта на інший.

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

```java
public class Test {
  public static void main(String[] args) {
    int n = 5;
    modifyTheValue(n, val-> val + 10);
    modifyTheValue(n, val-> val * 100);
  }

  static void modifyValue(int v, Function<Integer, Integer> function){
    int result = function.apply(v);
    System.out.println(newValue);
  }

}
```

**Supplier**

Цей функціональний інтерфейс робить протилежне до Consumer, він не бере аргументів, але повертає певне значення.

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

```java
public class Program {
    public static void main(String[] args) {
        int n = 3;
        display(() -> n + 10);
        display(() -> n + 100);
    }

    static void display(Supplier<Integer> arg) {
        System.out.println(arg.get());
    }
}
```

**BiPredicate**

BiPredicate <T, U> схожий на Predicate, але тут ви можете створити умову на основі 2 заданих параметрів.

```java
@FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
}
```

**BiConsumer**

BiConsumer <T, U> схожий на Consumer <T>, але він приймає 2 параметри, щоб зробити щось.

**BiFunction**

BiFunction <T, U, R> те саме, що Function <T, R>, але для виконання чогось може прийняти 2 параметри.

# Домашнє завдання

Доробіть до структури даних, яка була розроблена на минулій лабораторній роботі:

1. Користувацький Exception, який може бути викинутий, якщо індекс переданий в метод get() від'ємний або знаходиться за межами діапазону. Розробние виключення unchecked.
2. Зробіть метод transform в структурі даних, який приймає першим параметром індекс елемента, другим(функціональний інтерфейс, який вибраний Вами). Метод повинен передати знайдений елемент, ззовні його можуть модифікувати і повернути модифікований елемент.

# Контрольні запитання

1. Що таке виключення?
2. Як в Java обробляти виключення?
3. Назвіть ієрархію класів виключень в Java.
4. Яка різниця між checked і unchecked виключеннями?
5. Як в Java створити власне виключення?
6. Що таке lambda - вирази?
7. Що таке функціональний інтерфейс?
8. Перелічіть вбудовані в Java функціональні інтерфейси.