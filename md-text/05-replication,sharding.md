# Generics

Інформація в презентації.

# Java Collection Framework

**Java Collection Framework** - ієрархія інтерфейсів і їх реалізацій, яка є частиною JDK і дозволяє розробнику користуватися великою кількість структур даних з «коробки».

## Ієрархія класів і інтерфейсів

**Вся ієрархія класмів і інтерфейсів показана нижче**:

![](../resources/img/4/1.png)

**Розгляньмо детальніше:**

На вершині ієрархії в Java Collection Framework розташовуються 2 інтерфейсу: Collection і Map. Ці інтерфейси поділяють всі колекції, що входять у фреймворк на дві частини за типом зберігання даних: прості послідовні набори елементів і набори пар «ключ - значення» (словники).

![](../resources/img/4/2.png)

**Collection** - цей інтерфейс знаходиться в складі JDK c версії 1.2 і визначає основні методи роботи з простими наборами елементів, які будуть спільними для всіх його реалізацій (наприклад size (), isEmpty (), add (E e) і ін.).

```java
public interface Collection < E > extends Iterable < E > {
 /**
  * Add an element to this collection.
  *
  */
 boolean add(E o);

 /**
  * Add the contents of a given collection to this collection.
  *
  */
 boolean addAll(Collection < ? extends E > c);

 /**
  * Clear the collection, such that a subsequent call to isEmpty() 
  */
 void clear();

 /**
  * Test whether this collection contains a given object as one of its
  * elements.
  *
  */
 boolean contains(Object o);

 /**
  * Test whether this collection contains every element in a given collection.
  */
 boolean containsAll(Collection < ? > c);

 /**
  * Test whether this collection is equal to some object. The Collection
  */
 boolean equals(Object o);

 /**
  * Obtain a hash code for this collection.
  */
 int hashCode();

 /**
  * Test whether this collection is empty, that is, if size() == 0.
  */
 boolean isEmpty();

 /**
  * Obtain an Iterator over this collection.
  */
 Iterator < E > iterator();

 /**
  * Remove a single occurrence of an object from this collection. That is
  */
 boolean remove(Object o);

 /**
  * Remove all elements of a given collection from this collection. That is,
  * remove every element e such that c.contains(e).
  */
 boolean removeAll(Collection < ? > c);

 /**
  * Remove all elements of this collection that are not contained in a given
  * collection.
  */
 boolean retainAll(Collection < ? > c);

 /**
  * Get the number of elements in this collection.
  */
 int size();

 /**
  * Copy the current contents of this collection into an array.
  */
 Object[] toArray();
}
```

**Map** - Даний інтерфейс також знаходиться в складі JDK c версії 1.2 і надає розробнику базові методи для роботи з даними виду «ключ - значення».

```java
public interface Map < K, V > {
 /**
  * Remove all entries from this Map (optional operation).
  */
 void clear();

 /**
  * Returns true if this contains a mapping for the given key.
  */
 boolean containsKey(Object key);

 /**
  * Returns true if this contains at least one mapping with the given value.
  */
 boolean containsValue(Object value);

 /**
  * Returns a set view of the mappings in this Map.  Each element in the
  * set is a Map.Entry.
  */
 Set < Map.Entry < K,
 V >> entrySet();

 /**
  * Compares the specified object with this map for equality. Returns
  * <code>true</code> if the other object is a Map with the same mappings
  */
 boolean equals(Object o);

 /**
  * Returns the value mapped by the given key. Returns <code>null</code> if
  * there is no mapping.
  */
 V get(Object key);

 /**
  * Associates the given key to the given value (optional operation). If the
  * map already contains the key, its value is replaced.
  */
 V put(K key, V value);

 /**
  * Returns the hash code for this map.
  */
 int hashCode();

 /**
  * Returns true if the map contains no mappings.
  */
 boolean isEmpty();

 /**
  * Returns a set view of the keys in this Map.
  */
 Set < K > keySet();

 /**
  * Copies all entries of the given map to this one
  */
 void putAll(Map < ? extends K, ? extends V > m);

 /**
  * Removes the mapping for this key if present (optional operation).
  */
 V remove(Object o);

 /**
  * Returns the number of key-value mappings in the map.
  */
 int size();

 /**
  * Returns a collection (or bag) view of the values in this Map.
  */
 Collection < V > values();
}
```

### Ітнерфейс List

#### ArrayList

**ArrayList** - автоматично розширюваний масив. Ви можете працювати з масивом, але при цьому не використовуються квадратні дужки.

Масиви мають фіксовану довжину, і після того як масив створений, він не може зростати або зменшуватися. ArrayList може змінювати свій розмір під час виконання програми, при цьому не обов'язково вказувати розмірність при створенні об'єкта. Крім того, ви без проблем можете вставити новий елемент в середину колекції. А також спокійно видалити елемент з будь-якого місця. Елементи ArrayList можуть бути абсолютно будь-яких типів в тому числі і null. Це зручно, коли ви не знаєте точного розміру масиву.

Працювати з ArrayList просто: створіть потрібний об'єкт, вставте об'єкт методом add (), звертайтеся до нього методом get (), використовуйте індексування так само, як для масивів, але без квадратних дужок. ArrayList також містить метод size (), який повертає поточну кількість елементів в масиві (нагадаю, що в звичайному масиві використовується властивість length).

```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(12);
numbers.add(500);
System.out.println("Element at index 0: " + numbers.get(0));
System.out.println("Is numbers contain 500 " + numbers.contains(500));
numbers.remove(Integer.valueOf(500));
System.out.println("Is numbers contain 500 " + numbers.contains(500));

for (Integer number : numbers) {
    System.out.println("elem " + number);
}
```

#### Vector

**Vector** - реалізація динамічного масиву об'єктів. Дозволяє зберігати будь-які дані, включаючи null як елемент. Vector з'явився в JDK версії Java 1.0, але цю колекцію не рекомендується використовувати, якщо не потрібно досягнення потокобезпечна. Тому як в Vector, на відміну від інших реалізацій List, всі операції з даними є синхронізованими. В якості альтернативи часто застосовується аналог - ArrayList.

```java
Vector<Integer> numbers = new Vector<>();
numbers.add(12);
numbers.add(500);
System.out.println("Element at index 0: " + numbers.get(0));
System.out.println("Is numbers contain 20 " + numbers.contains(500));
numbers.remove(Integer.valueOf(500));
System.out.println("Is numbers contain 20 " + numbers.contains(500));

for (Integer number : numbers) {
    System.out.println("elem " + number);
}
```

#### Stack

**Stack** -  колекція є розширенням колекції Vector. Була додана в Java 1.0 як реалізація стека LIFO (last-in-first-out). Є частково синхронізованою колекцією (крім методу додавання push ()). Після додавання в Java 1.6 інтерфейсу Deque, рекомендується використовувати саме реалізації цього інтерфейсу, наприклад ArrayDeque.

Метод push () поміщає об'єкт в стек, а метод pop (), навпаки, витягує об'єкт з стека. Метод peek() може бути використаний для того щою бізнатися, який елемент знаходиться на вершині стека не видаляючи його.

```java
Stack<Integer> s = new Stack<Integer>();
s.push(12);
s.push(33);
for (Integer integer : s) {
    System.out.println(integer);
}
s.pop();
s.peek();
for (Integer integer : s) {
    System.out.println(integer);
}
```

#### LinkedList

**LinkedList** - ще одина реалізація List. Дозволяє зберігати будь-які дані, включаючи null. Особливістю реалізації даної колекції є те, що в її основі лежить двонаправлений зв'язаний список (кожен елемент має посилання на попередній і наступний).

```java
LinkedList<Integer> integers = new LinkedList<Integer>();
integers.add(12);
integers.addFirst(33);
integers.addLast(44);
for (Integer integer : integers) {
    System.out.println(integer);
}
```

### Ітнерфейс Map

#### Hashtable

**Hashtable** - реалізація такої структури даних, як хеш-таблиця. Вона не дозволяє використовувати null як значення або ключа. Ця колекція була реалізована раніше, ніж Java Collection Framework, але надалі була включена в його склад. Як і інші колекції з Java 1.0, Hashtable є синхронізованою (майже всі методи позначені як synchronized). Через цю особливість у неї є істотні проблеми з продуктивністю і, починаючи з Java 1.2, в більшості випадків рекомендується використовувати інші реалізації інтерфейсу Map через відсутність у них синхронізації. Дана колекція не зберігає порядок елементів.

```java
Hashtable<String, String> hashtable = new Hashtable<String, String>();
hashtable.put("key1", "value1");
hashtable.put("key2", "value2");

for(String key: hashtable.keySet()) {
    System.out.println("Value is " + hashtable.get(key) + " for key " + key );
}
```

#### HashMap

**HashMap** - колекція є альтернативою Hashtable. Двома основними відмінностями від Hashtable є те, що HashMap не синхронізована і HashMap дозволяє використовувати null як в якості ключа, так і значення. Так само як і Hashtable, дана колекція не є упорядкованою: порядок зберігання елементів залежить від хеш-функції.

```java
HashMap<String, String> hashtable = new HashMap<String, String>();
hashtable.put("key1", "value1");
hashtable.put("key2", "value2");

for(String key: hashtable.keySet()) {
    System.out.println("Value is " + hashtable.get(key) + " for key " + key );
}
```

#### LinkedHashMap

**LinkedHashMap** - це впорядкована реалізація хеш-таблиці. Тут, на відміну від HashMap, порядок перебору рівний порядку додавання елементів. Дана особливість досягається завдяки двонаправленим зв'язків між елементами (аналогічно LinkedList). Але ця перевага має також і недолік - збільшення пам'яті, яке занімає колекція.

```java
LinkedHashMap<String, String> hashtable = new LinkedHashMap<String, String>();
hashtable.put("key1", "value1");
hashtable.put("key2", "value2");

for(String key: hashtable.keySet()) {
    System.out.println("Value is " + hashtable.get(key) + " for key " + key );
}
```

#### TreeMap

**TreeMap** - реалізація Map заснована на червоно-чорних деревах. Як і LinkedHashMap є впорядкованою. За замовчуванням, колекція сортується по ключам.

### Ітнерфейс Set

#### HashSet

**HashSet** - реалізація інтерфейсу Set, що базується на HashMap. Усередині використовує об'єкт HashMap для зберігання даних. Як ключ використовується для додавання, а в якості значення - об'єкт-заглушка (new Object ()). Через особливості реалізації порядок елементів не гарантується при додаванні.

```java
HashSet<String> s = new HashSet<String>();
s.add("some");

for (String elem: s) {
    System.out.println(elem);
}
```

#### LinkedHashSet

**LinkedHashSet** - відрізняється від HashSet тільки тим, що в основі лежить LinkedHashMap замість HashSet. Завдяки цій відмінності порядок елементів при обході колекції є ідентичним порядку додавання елементів.

```java
LinkedHashSet<String> s = new LinkedHashSet<String>();
s.add("some");
s.add("aa");
s.add("bbb");

for (String elem: s) {
    System.out.println(elem);
}
```

#### TreeSet

**TreeSet** - аналогічно іншим класам-реалізацій інтерфейсу Set містить в собі об'єкт NavigableMap, що й обумовлює його поведінку. Надає можливість управляти порядком елементів в колекції за допомогою об'єкта Comparator, або зберігає елементи з використанням "natural ordering".

```java
TreeSet<String> s = new TreeSet<String>();
s.add("some");
s.add("aa");
s.add("bbb");

for (String elem: s) {
    System.out.println(elem);
}
```

### Ітнерфейс Queue

#### PriorityQueue

Особливістю даної черги є можливість управління порядком елементів. За замовчуванням, елементи сортуються з використанням «natural ordering», але це поведінка може бути перевизначити за допомогою об'єкта Comparator, який задається при створенні черги. Ця колекція не підтримує null як елементи.

```java
PriorityQueue<Integer> q = new PriorityQueue<>();
q.add(20);
q.add(1);

for (Integer elem: q) {
    System.out.println(elem);
}

q.peek();
q.poll();
```

#### ArrayDeque

**ArrayDeque** - реалізація інтерфейсу Deque, який розширює інтерфейс Queue методами, що дозволяють реалізувати конструкцію виду LIFO (last-in-first-out). Інтерфейс Deque і реалізація ArrayDeque були додані в Java 1.6. Ця колекція представляє собою реалізацію з використанням масивів, подібно ArrayList, але не дозволяє звертатися до елементів за індексом і зберігання null. Як заявлено в документації, колекція працює швидше ніж Stack, якщо використовується як LIFO колекція, а також швидше ніж LinkedList, якщо використовується як FIFO.

```java
// ArrayDEqueue as Queue
ArrayDeque<Integer> q = new ArrayDeque<>();
q.offer(Integer.valueOf(12));
q.offer(Integer.valueOf(22));
q.poll();
q.poll();

// ArrayDEqueue as Stack
ArrayDeque<Integer> s = new ArrayDeque<>();
s.push(Integer.valueOf(12));
s.push(Integer.valueOf(22));
s.pop();
s.pop();
```

## Як вибрати структуру даних?

Ось алгоритм при воборі структури даних із Java Collection Framework:

![](../resources/img/4/1.gif)


І не забуваємо про часові складності:

![](../resources/img/4/3.png)

# Домашнє завдання

Запропонуйте власну реалізацію динамічного масиву, який заснований на звичайномк масивові. Інтерфейс - add(elem), remove(elem), remove(index), get(index), isExists(elem).

# Контрольні запитання

1. Що таке Generics і навіщо вони потрібні?
2. Що таке Java Collection Framework?
3. Перелічіть інтерфейси Java Collection Map.
4. Поясніть колекції - Hashtable, HashMap, LinkedHashMap, TreeMap.
4. Поясніть колекції - Vector, Stack, ArrayList, LinkedList.
4. Поясніть колекції - HashSet, LinkedHashSet, TreeSet.
4. Поясніть колекції - PriorityQueue, ArrayDeque.
4. Як вибрати, яку колекцію використовувати?