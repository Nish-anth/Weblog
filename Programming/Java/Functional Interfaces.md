# Functional Interfaces
refer [Java Functional Programming](https://www.youtube.com/watch?v=VRpHdSFWGPs&t=3151s&ab_channel=Amigoscode)

The support for functional programming in Java came with the release of Java 8 and the java.util.function exposes interface to aid functional programming in Java.

[java.util.function](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html#package.description) description:
>_Functional interfaces_ provide target types for lambda expressions and method references. Each functional interface has a single abstract method, called the _functional method_ for that functional interface, to which the lambda expression's parameter and return types are matched or adapted. Functional interfaces can provide a target type in multiple contexts, such as assignment context, method invocation, or cast context.


There are a lot of interfaces provided by java.util.function, including *Function, BiFunction, Consumer, Predicate* etc

#### Function
> **[Function](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html "interface in java.util.function")<T,R>**  
> Represents a function that accepts one argument and produces a result.

```
// imperative way of writing functions
static int increment(int number) {
	return number + 1;
} 

increment(2); // method invocation // result: 3
```

```
// writing function with the Function interface
static Function<Integer, Integer> increment = number -> number + 1;

increment.apply(2); // method invocation // result: 3
```

#### BiFunction
> **[BiFunction](https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html "interface in java.util.function")<T,U,R>**  
  Represents a function that accepts two arguments and produces a result.

```
// imperative way of writing functions
static int addTwoNumbers(int numberA, int numberB) {
	return numberA + numberB;
} 

addTwoNumbers(2, 1); // method invocation // result: 3
```

```
// writing function with the Function interface
static BiFunction<Integer, Integer, Integer> addTwoNumbers = (a, b) -> a + b;

addTwoNumbers.apply(2, 1); // method invocation // result: 3
```

#### Consumer  
> **[Consumer](https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html "interface in java.util.function")\<T>**
  Represents an operation that accepts a single input argument and returns no result.  

```
// imperative way of writing functions
static void sayHi(String name) {
	System.out.println("Hello " + name);
} 

sayHi("Nishanth"); // method invocation // result: Hello Nishanth
```

```
// writing function with the Function interface
static BiFunction<String> sayHi = name -> System.out.println("Hello " + name);

sayHi.apply("Nishanth"); // method invocation // result: Hello Nishanth
```

There is also a BiConsumer

#### Predicate
> [Predicate](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html "interface in java.util.function")\<T>  
> Represents a predicate (boolean-valued function) of one argument.


```
// imperative way of writing functions
static boolean isAValidEmail(String email) {
	email.contains('@')
} 

isAValidEmail("nishanth@email.com"); // method invocation // result: true
```

```
// writing function with the Function interface
static Predicate<String> isAValidEmail = email -> email.contains('@');

isAValidEmail.test("nishanth"); // method invocation // result: false
```

There is also a BiPredicate

#### Supplier
> [Supplier](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html "interface in java.util.function")\<T>  
> Represents a supplier of results.

```
// imperative way of writing functions
static String getSecret() {
	return "open sesame";
} 

getSecret(); // method invocation // result: open sesame
```

```
// writing function with the Function interface
static Predicate<String> getSecret = () -> "open sesame";

getSecret.get(); // method invocation // result: open sesame
```
