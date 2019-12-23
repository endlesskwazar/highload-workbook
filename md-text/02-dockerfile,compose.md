# Spring Core

${toc}

# Інверсія керування(IoC)

**Інверсія управління** (англ. Inversion of Control, IoC) - важливий принцип об'єктно-орієнтованого програмування, що використовується для зменшення зачеплення в комп'ютерних програмах. Також архітектурне рішення інтеграції, що спрощує розширення можливостей системи, при якому потік управління програми контролюється фреймворком.

методи реалізації:

- Шаблон «Фабрика» (англ. Factory pattern)
- Локатор служб (Service Locator)
- Впровадження залежності (англ. Dependency injection)

# Що таке Dependency injection?

Впровадження залежності (англ. Dependency injection, DI) — шаблон проектування програмного забезпечення, що передбачає надання зовнішньої залежності програмному компоненту, використовуючи «інверсію управління» (англ. Inversion of control, IoC) для розв'язання (отримання) залежностей.

Впровадження — це передача залежності (тобто, сервісу) залежному об'єкту (тобто, клієнту). Передавати залежності клієнту замість дозволити клієнту створити сервіс є фундаментальною вимогою до цього шаблону проектування.

## Приклад

Уявіть собі, що ми проектуємо систему в, якій кожного місяця потрібно виводити статистику на екран(статистику продаж тощо). Нехай цей об'єкт буди мати інтерфейс:

```java
public interface StatisticReporter {
	void report();
}
```

Все доволі просто, об'єкт буде мати один метод в інтерфейсі report(), який і виводить статистику на екран.

Користовуч може забажати мати статистику в різних форматах - HTML, XML, DOC. Для цих цілей ми введимо в системі інтерфейс:

```java
public interface Formatter {
	String format(String text);
}
```

І Зробимо декілька реалізації:

```java
public class HtmlFormatter implements Formatter {

	public String format(String text) {
		return "HTml format";
	}

}
```

```java
public class XMLFormatter implements Formatter {

	public String format(String text) {
		return "Xml format";
	}

}
```

І нерешті зробимо реалізацію StatisticReporter:

```java
public class StatisticReporterImpl implements StatisticReporter {
	
	HtmlFormatter formatter = new HtmlFormatter();
	
	public void report() {
		String report = "This is report";
		String outRep = formatter.format(report);
		System.out.print(outRep);
	}

}
```

Тестуємо систему:

```java
public class Main {
	
	public static void main(String[] args) {
		StatisticReporterImpl statisticReporterImpl = new StatisticReporterImpl();
		statisticReporterImpl.report();
	}
}
```

Якщо ми пішли таки шляхом, для того щоб виводити статистику в іншому форматы нам, або доведеться змінювати реалізацію statisticReporterImpl, або створювати ще один клас, який реалізує StatisticReporter.

Можна підти і іншим шляхом, замість того щоб створювати об'єкт Formatter всередині StatisticReporterImpl можна передати його ззовні:

```java
package test;

public class StatisticReporterImpl implements StatisticReporter {
	
	private Formatter formatter;
	
	public StatisticReporterImpl(Formatter formatter) {
		this.formatter = formatter;
	}
	
	public void report() {
		String report = "This is report";
		String outRep = formatter.format(report);
		System.out.print(outRep);
	}

}
```

```java
public class Main {
	
	public static void main(String[] args) {
		Formatter formatter = new HtmlFormatter();
		StatisticReporterImpl statisticReporterImpl = new StatisticReporterImpl(formatter);
		statisticReporterImpl.report();
	}
}
```

# Ще раз про IoC і DI

# Spring Boot

Через громіздку конфігурацію залежностей налаштування Spring для корпоративних додатків перетворилася в досить втомлива і схильне до помилок заняття. Особливо це відноситься до додатків, які використовують також кілька сторонніх бібліотек.

Автори Spring вирішили надати розробникам деякі утиліти, які автоматизують процедуру настройки і прискорюють процес створення і розгортання Spring-додатків, під загальною назвою **Spring Boot**.

**Spring Boot** - це корисний проект, метою якого є спрощення створення додатків на основі Spring. Він дозволяє найбільш простим способом створити web-додаток, вимагаючи від розробників мінімум зусиль по його настройці.

## Особливості Spring Boot

### Керування залежностями

Щоб прискорити процес управління залежностями, Spring Boot неявно упаковує необхідні сторонні залежності для кожного типу додатки на основі Spring і надає їх розробнику за допомогою так званих starter-пакетів (spring-boot-starter-web, spring-boot-starter-data-jpa і т . Д.).

**Starter-пакети** становлять собою набір зручних дескрипторів залежностей, які можна включити в свій додаток. Це дозволить отримати універсальне рішення для всіх, пов'язаних зі Spring технологій, позбавляючи програміста від зайвого пошуку прикладів коду та завантаження з них необхідних дескрипторів залежностей (приклад таких дескрипторів і стартових пакетів буде показаний нижче).

Іншими словами, Spring Boot збирає всі загальні залежності і визначає їх в одному місці, що дозволяє розробникам просто використовувати їх, замість того, щоб винаходити колесо кожен раз, коли вони створюють новий додаток.

Отже, при використанні Spring Boot, файл pom.xml містить набагато менше рядків, ніж при використанні його в Spring-додатках.

Подивитися start - пакети можна [в документації](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-starter).

### Автоматична конфігурація

Після вибору підходящого starter-пакета, Spring Boot спробує автоматично налаштувати Spring-додаток на основі доданих вами jar-залежностей.

Наприклад, якщо ви додасте Spring-boot-starter-web, Spring Boot автоматично сконфигурирует такі зареєстровані біни, як DispatcherServlet, ResourceHandlers, MessageSource.

Якщо ви використовуєте spring-boot-starter-jdbc, Spring Boot автоматично реєструє біни DataSource, EntityManagerFactory, TransactionManager і зчитує інформацію для підключення до бази даних з файлу application.properties.

Якщо ви не збираєтеся використовувати базу даних, і не надаєте ніяких докладних відомостей про підключення в ручному режимі, Spring Boot автоматично налаштує базу в пам'яті, без будь-якої додаткової конфігурації з вашого боку (при наявності H2 або HSQL бібліотек).

Автоматична конфігурація може бути повністю перевизначена в будь-який момент за допомогою налаштувань.

### Вбудований сервлет - контейнер

Кожний Spring Boot web-додаток включає вбудований web-сервер. Подивіться на список контейнерів сервлетів, які підтримуються "з коробки".

Розробникам тепер не треба турбуватися про налаштування контейнера сервлетів і розгортанні додатки на ньому. Тепер додаток може запускатися саме, як виконуваний jar-файл з використанням вбудованого сервера.

Створення автономних web-додатків з вбудованими серверами не тільки зручно для розробки, а й є допустимим рішенням для додатків корпоративного рівня і стає все більш корисно в світі мікросервісов.

# Eclipse і Spring Boot

# Spring Core

# Створюємо проект і додаємо Spring Core

# Конфігурація впровадження, використовуючи XML

# Конфігурація впровадження, використовуючи Java

# Домашня робота

# Контрольні запитання

1. ваів
2. іваів
3. аіва
4. іва
5. іваі

