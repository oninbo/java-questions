# Вопросы для собеса по джаве
## Паттерны
### Adapter
Структурный паттерн проектирования, который позволяет объектам с несовместимыми интерфейсами работать вместе.
Это объект-переводчик, который трансформирует интерфейс или данные одного объекта в такой вид, чтобы он стал понятен другому объекту.
При этом адаптер оборачивает один из объектов, так что другой объект даже не знает о наличии первого.

Адаптер это обертка, которую мы используем если у нас есть какой-то класс, но он не имплементирует нужный нам интерфейс например и мы не хотим его менять мы можем создать для этого класс обертку, который будет наследовать нужный нам интерфейс и уже там работать с нужным нам классом.

➕ Отделяет и скрывает от клиента подробности преобразования различных интерфейсов.

➖ Усложняет код программы из-за введения дополнительных классов.

### Decorator
Структурный паттерн проектирования, который позволяет добавлять объектам новую функциональность, оборачивая их в полезные «обёртки».(Надстройка когда уже есть готовый функционал(класс) и мы хотим вызывать этот же функционал но с добавлением своей реализации).

Целевой объект помещается в другой объект-обёртку (Ассоциация), который запускает базовое поведение обёрнутого объекта, а затем добавляет к результату что-то своё.
Оба объекта имеют общий интерфейс, поэтому для пользователя нет никакой разницы, с каким объектом работать — чистым или обёрнутым. Вы можете использовать несколько разных обёрток одновременно — результат будет иметь объединённое поведение всех обёрток сразу.
Адаптер не менят состояния объекта, а декоратор может менять.

Декоратор мы используем когда у нас есть какой-то класс, поведение которого нас устраивает, но иногда нам необходимо это поведение расширять. То есть если нам нужно использовать какой-то существующий объект и как-то видоизменять его функциональность(даже заменять). Декоратор позволяет изменять функционал динамически, в отличии от наследования, оборачивая их в объект класса декоратора.

➕ Большая гибкость, чем у наследования.

➖ Труднее конфигурировать многократно обёрнутые объекты.

Декоратор много используется в Java IO классах, например, `FileReader`, `BufferedReader`

https://www.digitalocean.com/community/tutorials/decorator-design-pattern-in-java-example
## Core
### JIT
JIT (Just-in-time compilation) - компиляция на лету или динамическая компиляция - технология увеличения производительности программных систем, использующих байт-код, путем компиляции байт-кода в машинный код во время работы программы.
В основном отвечает за оптимизацию производительности приложений во время выполнения за счет использования ранее скомпилированного байт-кода, который не нужно интерпретировать.
### Сравнение примитивных типов и экзепляров классов-оберток
#### Cравнения между типами `boolean` и `Boolean`
```java
boolean a = true;
Boolean b = true; // будет равен Boolean.TRUE
Boolean c = true; // будет равен Boolean.TRUE

a == b; // true (сравниваются как примитивы по значению)
a == c; // true (сравниваются как примитивы по значению)
b == c; // true (сравниваются по ссылке, но указывают на один и тот же объект)
```
Однако, если создать экземпляр класса `Boolean` через конструктор как `new Boolean(true)`, то он не будет равен `Boolean.TRUE`, если сравнивать через оператор `==`, так как ссылки будут разные. Также стоит учитывать, что ссылка на `Boolean` может быть `null`.

Если вы сравниваете `int` и `Integer`, `Integer` преобразовывается в `int`:
```java
int a = 5;
Integer b = 5; // Integer b = Integer.valueOf(5);
if (a == b) // if (a == b.intValue())
{
   ...
}
```
Если сравнить между собой два объекта типа `Integer`, преобразовываться к типу `int` они не будут:
```java
Integer a = 500;
Integer b = 500;
int c = 500;

System.out.println(a == b); // сравнение ссылок, поэтому false
System.out.println(a == c); // true
System.out.println(b == c); // true
```
Но, вот если мы заменим 500 на 100, получим совсем другой результат: `a == b` будет `false`. Все дело в том, что при autoboxing не всегда создается действительно новый объект `Integer`. Для значений от -128 до 127 включительно объекты кэшируются. Такой кэш есть у всех типов-оберток. У типа `Boolean` оба его значения `TRUE` и `FALSE` являются константами: по сути тоже кэшируются.
https://javarush.com/quests/lectures/questsyntaxpro.level12.lecture01
### Модификаторы доступа
У классов верхнего уровня (не вложенных в другой класс):
- **public** — доступ к компоненту из экземпляра любого класса и любого пакета;
- **default (когда модификатор не указан)** — компонент доступен для любого другого класса в том же пакете.

У методов, полей и внутренних классов, кроме **public** и **default**, есть еще:
- **private** — доступ к компоненту только из этого класса, в котором объявлен;
- **protected** — поля доступны всем классам внутри пакета, а также всем классам-наследникам вне пакета.

https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html
### Методы класса Object
- `equals()` — проверка на равенство двух обьектов
- `hashCode()` — изначально случайно число int
- `toString()` — представления данного объекта в виде строки.
- `getClass()` — получение типа данного обьекта
- `clone()` —  клонирует объект методом.
- `finalize()` — deprecated, вызывается GC перед удалением. (нет гарантии что будет вызван)
#### Для многопоточки
- `notify()` — «размораживает» одну случайную нить
- `notifyAll()` — «размораживает» все нити данного монитора
- `wait()` — нить освобождает монитор и «становится на паузу»
- `wait(long timeOut)` — нить освобождает монитор и «становится на паузу»,принимает максимальное время ожидания в миллисекундах.
- `wait(long timeOut, int nanos)` — нить освобождает монитор и «становится на паузу»,принимает максимальное время ожидания в миллисекундах, дополнительное время, в диапазоне наносекунд 0-999999.
### Интерфейс
Интерфейс — это требование к реализации класса. 
