# ==Gradle==

### What is Gradle?
Gradle is a build automation tool. Build automation helps in standardizing project builds, thereby minimizing human mistakes. Build scripts also encompass *'tasks'* that are repetitive in nature.

Gradle is a flexible build tool and can be configured to build any type of software. It runs on JVM and commonly uses [[Groovy]] or Kotlin DSL for its build scripts. Unlike other build tools the build scripts in Gradle are also *'code'*.

&nbsp
### What is DSL?
Domain-specific languages, or DSLs, are languages specialized for a specific part of an app. It is used to extract a part of the code to make it reusable and more understandable. As opposed to a function or a method, DSLs also change the way you use and write code. Usually, DSLs make the code more readable, almost like a spoken language. This means that even people who don’t understand the architecture behind the code will be able to grasp the meaning of it.

[source] https://www.raywenderlich.com/2780058-domain-specific-languages-in-kotlin-getting-started

&nbsp
### Gradle Lifecycle Phases
	1 Initialization
	2 Configuration
	3 Execution

[refer] https://docs.gradle.org/current/userguide/build_lifecycle.html

Each phase of the Gradle Lifecycle is mapped to a Gradle build script.

##### Initialization
Gradle supports both single and multi-project builds. During the initialization phase Gradle determines which projects are to be built.

init.gradle, settings.gradle, \*.gradle are examples of files which contain scripts for the initialization phase.

Initialization scripts should be place under /.gradle/init.d directory.
Gradle files in the initialization phase are evaluated in the alphebetical order. 
eg: a.gradle is evaluated first and then b.gradle is evaluated.

##### Configuration
Projects that are to be built will have their configurations created in this phase of the Gradle lifecycle. 

build.gradle is a common example where the configuration scripts for the projects are written.

##### Execution 
This is the phase where the actual project build takes place.

Both the configuration and the execution phase depends on the build.gradle file.

Each project in a multi project setup should have its own build.gradle file.


&nbsp
### Gradle Object Model
##### Interface Script
	public interface Script

This interface is implemented by all Gradle Groovy DSL scripts to add in some Gradle-specific methods. As your compiled script class will implement this interface, you can use the methods and properties declared by this interface directly in your script.

Generally, a `Script` object will have a ==delegate object== attached to it. For example, a build script will have a [`Project`](https://docs.gradle.org/current/javadoc/org/gradle/api/Project.html "interface in org.gradle.api") instance attached to it, and an initialization script will have a [`Gradle`](https://docs.gradle.org/current/javadoc/org/gradle/api/invocation/Gradle.html "interface in org.gradle.api.invocation") instance attached to it. Any property reference or method call which is not found on this `Script` object is forwarded to the delegate object.

- init.gradle *has* `Script` interface which *delegates* to `Gradle` interface
- settings.gradle *has* `Script` interface which *delegates* to `Settings` interface
- build.gradle *has* `Script` interface which *delegates* to `Project` interface

Both the Project and Settings interface has access to the Gradle interface.

Example to access the gradle object inside these three files:

| File          | Gradle explict access        | Gradle implicit access   | 
| --------------| -----------------------------|--------------------------|
|init.gradle    | gradle.gradleVersion         | gradleVersion            |
|settings.gradle| settings.gradle.gradleVersion| gradle.gradleVersion     |
|build.gradle   | project.gradle.gradleVersion | gradle.gradleVersion     |

The above example shows that the delegate object is being implicitly taken into consideration by the build scripts

&nbsp
### Gradle Properties
Few ways to access properties in Gradle

- Built-in properties of domain objects such as Script, Project, Gradle etc can be accessed in the scripts through the dot notation. 
- It is also possible to set key-value pairs as properties in `gradle.properties` file. Properties defined in this file can be accessed from build.gradle and settings.gradle files.
- We can create and add user define properties to any of the domain objects like Script, Project, Gradle etc
- Pass in key-value pairs as command line arguments with -p flag

##### gradle.properties
gradle.properties file should be placed in the root directory of the project.

##### Command Line
-p name=Nishanth

Properties defined in gradle.properties and command line can be accessed by project.propertyName or simply propertyName. By default the properties will be loaded into memory in the Gradle environment.

##### ExtraPropertiesExtension
To add any user defined properties to a domain object use the ext property on those objects to dynamically add new properties.

```
project.ext.username = "Nishanth"
println project.username // this works
println username         // this also works
```

These properties can also be set during the initialization phase (init.gradle) and then accessed during the configuration phase

### Tasks & Actions
Gradle is built around two core concepts: projects and tasks

build.gradle *delegates* to Project interface which *has a collection of* tasks which in turn *has a collection of* actions

A task is a piece of work to be done by the build. Task creation and configuration happens in the configuration phase and is executed during the execution phase

Task definition:
```
task sayHello {

	doFirst {  
	    println "Hello World"  
	}

	doLast {  
	    println "Shutting Down!"  
	}
}
```

Task invocation:
```
gradle -q sayHello
```

or alternatively, select the task from the Gradle tasks panel from IntelliJ and run it manually.

To run any task upon building the build.gradle file without specifying it explicitly in command line or in the Gradle task bar, we can define tasks as default task.
```
defaultTasks "sayHello"
```

*defaultTasks* function takes in strings as arguments and so we can define more than one task as default tasks. 
```
defaultTasks "sayHello", "sayBye"
```

Actions are added to a task through the doFirst and doLast methods of the Task object as a closure.

doFirst - Appends the latest task to the beginning of the task action list
doLast - Appends the latest task to the end of the task action list

Example:
```
task sayHello {

	doFirst {  
	    println "doFirst : Executes Second"  
	}

	doLast {  
	    println "doLast : Executes First"  
	}
}

project.sayHello.doFirst {
	println "doFirst : Executes First"
}

sayHello.doLast {
	println "doLast : Executes Second"
}
```

Output:
```
doFirst : Executes First
doFirst : Executes Second
doLast : Executes First
doLast : Executes Second
```

&nbsp
### Task Dependencies
It is possible to write inter-dependent tasks in Gradle

Example:
```
defaultTasks "sayGreeting", "welcomePerson"

task sayHello {

	doLast {  
	    println "Hello!"  
	}
}

task sayGreeting (dependsOn: "sayHello") {

	doLast {
		println "Have a nice day!"
	}
}

task welcomePerson (dependsOn: "sayHello") {

	doLast {
		println "Welcome, Nishanth"
	}
}
```

Output:
```
Task: sayHello
Hello!

Task: sayGreeting
Have a nice day!

Task: welcomePerson
Welcome, Nishanth
```

One thing to notice in the above example is that, although both the default tasks depend on the sayHello task it gets executed only once.

The above example can also be written like,
```
defaultTasks "welcomePerson"

task sayHello {

	doLast {  
	    println "Hello!"  
	}
}

task sayGreeting {

	doLast {
		println "Have a nice day!"
	}
}

task welcomePerson (dependsOn: ["sayHello", "sayGreeting"]) {

	doLast {
		println "Welcome, Nishanth"
	}
}
```

Tasks can be dynamically added to depend on other tasks
```
task manufactureCar {
	doLast {
		println "Manufacture car"
	}
}

task designCarAerodynamics {

	doLast {
		println "Designing Car Aerodynamics"
	}
}

task buildCarEngine {
	
	doLast {
		println "Build a diesel engine"
	}
}

task buildCarTyre {
	
	doLast {
		println "Build a tubeless tyre"
	}
}

manufactureCar.dependsOn buildCarEngine, buildCarTyre
```

In the above example, if we need to conditionally add only the build modules and exclude the design module, we can dynamically do so in the below manner.

```
manufactureCar.dependsOn tasks.findAll { task -> task.name.startsWith("build") }
```

This only adds the tasks buildCarEngine and buildCarTyre as dependencies to the task manufactureCar

** Beware of circular dependencies when adding dependencies dynamically **

Gradle uses a Direct Acyclic Graph to map the task dependencies and therefore throws a runtime exception if there is a cyclic dependency

Gradle provides interface to interact with the TaskGraph. Trying to access the taskGraph during the configuration phase might throw an error as the graph wouldn't be constructed yet.

```
project.gradle.taskGraph.getAllTasks() // throws error
```

`whenReady()` method of the taskGraph is a callback to when the graph is constructed.

```
manufactureCar.dependsOn tasks.findAll { task -> task.name.startsWith("build") }  
project.gradle.taskGraph.whenReady { taskGraph -> println taskGraph.getAllTasks() }
```

`beforeTask()` and `afterTask()` are also methods which can be utilized to perform some operations at different stages of the task lifecycle. 

### Plugins
Plugins in Gradle are nothing but a collection of useful tasks and conventions serving a common purpose.

Eg: jacoco is a plugin which contains tasks that serve to track the test coverage in a Java project.

'java' plugin adds some useful tasks like 'clean', 'testClasses', 'jar' etc.

### Gradle Build Blocks
Gradle build blocks include dependencies, repositories etc.

The context inside the Gradle Build Blocks differ from the context within the build.gradle file (Project object is the delegate).

	dependencies { } - DependencyHandler
	repositories { } - RepositoryHandler


**repositories { }**

This build block encapuslates the repositories from which the dependencies are to be sourced

```
repositories {
	mavenCentral()
}
```

**dependencies { }**

This build block encapsulates the dependencies which are to be used in the project for development
```
dependencies {
	implementation group: 'org,apache.commons', name: 'commons-math3', version: '3.6.1'
}
```

Dependencies can also be represented in the below format
```
dependencies {
	implementation 'org,apache.commons:commons-math3:3.6.1'
}
```

> **implementation** — The dependency is available in **all** source sets, including the test source sets.

> **testImplementation** — The dependency is only available in the test source set.

&nbsp
### Related Tags
#gradle #dsl #groovy #build-tools
