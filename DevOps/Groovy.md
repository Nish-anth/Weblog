# ==Groovy==

&nbsp
### What is Groovy?
Apache Groovy is a java environment compatible object oriented scripting language. Groovy DSL is popularly used to write build scripts for Gradle

&nbsp
### Groovy - overview

&nbsp
###### Hello World

```
println "Hello World"
println("Hello World")
```

Groovy DSL supports both the above syntaxes (with and without paranthesis)

&nbsp
##### Functions
```
def sayHello() {
	def y = "Hello!"
}
```

```
def sayHello() {
	"Hello!"
}
```

Both the above functions when called will return the same value. In Groovy DSL the parameters of the last line of the function will be return implicitly

&nbsp
- Function definition requires paranthesis; function invoking does not
```
sayHello "Nishanth","Have a nice day"
  
def sayHello(def name, def greeting) {  
    def y = "Hello $name! $greeting"  
}
```

&nbsp
- Although when nesting function calls, the outer function requires paranthesis
```
println(sayHello "Nishanth","Have a nice day")
  
def sayHello(def name, def greeting) {  
    def y = "Hello $name! $greeting"  
}
```


&nbsp
##### Properties
```
Map map = new HashMap()

map.put("name", "Nishanth") // java
map.name = "Nishanth"       // groovy

```

Object properties can be accessed in Groovy DSL with the dot notation and does not require the use of getters or setters

&nbsp
###### Closures
A closure in Groovy is **an open, anonymous, block of code that can take arguments, return a value and be assigned to a variable**. A closure may reference variables declared in its surrounding scope

[source] https://www.baeldung.com/groovy-closures


&nbsp
### Related Tags
#groovy #gradle #dsl