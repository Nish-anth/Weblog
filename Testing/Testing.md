[Test Definition](https://blog.cleancoder.com/uncle-bob/2017/05/05/TestDefinitions.html) - By Uncle Bob  
[Udemy Course](https://www.udemy.com/course/testing-java-code-with-junit-5-and-mockito/)

&nbsp
### Unit Test
FIRST Principle

	F - Unit tests run fast  
	I - Unit tests are independent (order or output of other tests shouldn't affect a unit test)  
	R - Unit tests are repeatable (results should be same irrespective of how many times it is tested)  
	S - Self Validating (validate the results within the tests )  
	T - Thorough & Timely (cover edge cases as well; unit tests should be written when the code is written)  

The methods must be isolated while testing i.e if the code that is being tested has any dependent objects then those objects should be injected with mocks.  

&nbsp
### JUnit

JUnit is a Java and JVM unit testing framework. 

JUnit = JUnit Platform + JUnit Jupiter + JUnit Vintage

| Component | Purpose |
|-----------|---------|
| Platform | serves as a foundation for running JUnit tests on the JVM |
| Jupiter | collection of JUnit5 API's that help write unit tests |
| Vintage | provides test engine to run tests written with newer and older JUnit versions |


JUnit Params dependency helps write [parameterized tests](https://www.baeldung.com/parameterized-tests-junit-5)

assertEquals(expected, result, message?)
Writing messages in assertions are a good practice

	assertEquals, assertNotEquals, assertTrue, assertFalse, assertNull, 
	asssertNotNull, assertThrows, assertDoesNotThrows, fail are few of the 
    commonly used assertions in JUnit5

Use **@DisplayName** to provide alternative short description for test methods

Structure test code with AAA (Arrange, Act, Assert) method. 

BeforeAll, AfterAll, BeforeEach, AfterEach are all lifecycle method annotations

&nbsp
##### [Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)

Replace @Test annotation with @ParmeterizedTest annotation to test a method with different set of parameters.

**@ValueSource** annotation is used to pass in single value arguments to the test method and does not support complex parameters

```
@ParameterizedTest 
@ValueSource(ints = {1, 3, 5, -3, 15, Integer.MAX_VALUE}) // six numbers 
void isOdd_ShouldReturnTrueForOddNumbers(int number) { 
   assertTrue(Numbers.isOdd(number)); 
}
```

**@MethodSource** annotation should be used if there are complex parameters

```
@ParameterizedTest 
@MethodSource("provideStringsForIsBlank") 
void isBlank_ShouldReturnTrueForNullOrBlankStrings(String input, boolean expected) { 
   assertEquals(expected, Strings.isBlank(input)); 
}

private static Stream<Arguments> provideStringsForIsBlank() { 
   return Stream.of( 
       Arguments.of(null, true), 
       Arguments.of("", true), 
       Arguments.of(" ", true), 
       Arguments.of("not blank", false) 
    ); 
}
```

The static method name should be the same as the value passed in the @MethodSource annotation. Alternatively, it is possible to name the static method exactly same as the test method and pass in an empty @MethodSource() annotation

**@RepeatedTest** repeats a test method for the specified number of times

By default the execution order of tests is deterministic but intentionally non-obvious (to make sure that unit tests are isolated and not inter-dependent)

##### Test Instance Lifecycle
By default JUnit creates a new test class instance for each method in the test class. This promotes isolation between unit test methods. But when running integration tests in which the methods might depend on each other, it is possible to change the configuration of the test instance using the **@TestInstance** annotation

```
@TestInstance(TestInstance.Lifecycle.PER_METHOD)
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
```


&nbsp
### TDD
##### Test Driven Development Lifecycle

> Red --> Green --> Refactor --> Repeat

&nbsp

| Lifecycle | Purpose |
|-----------|---------|
| Red       | Write unit tests that fails|
| Green     | Write application code to make unit test pass |
| Refactor  | Improve both unit test and application code |
| Repeat    | Repeath this process until all the functionality is completed |

### Mockito
Mockito is a testing framework which is used to mock or create *test doubles* objects in unit tests

Mock, Fake, Spy, Stub are types of test doubles used to replace real objects.

[Mocks aren't stubs](https://martinfowler.com/articles/mocksArentStubs.html)

Test in isolation is a core principle of unit testing. So if an object-under-test relies on another object or class (a collaborator) for it's execution then the collaborator object must be mocked. If the collaborator is an application code then it must be unit tested in isolation.

To use Mockito in a class annotate the class with 
```
@ExtendWith(MockitoExtension.class)
```

To mock an object, the object has to be an injected into the class in the first place. Mocking is not feasible if the object is instantiated concretely with the *new* keyword.

The object to be mocked (collaborator) should be annotated with @Mock or @MockBean annotations and the the *object-under-test* should be annotated with @InjectMocks. InjectMocks annotation can be avoided if the mock is manually injected (through constructor?).

##### Mocks and Stubs
- A mock is a substitution object which expects for methods to be called against it. 
- A stub provides doctored data for test execution. 

```
Mockito.when(dbRepository.existsOneByEntity(Mockito.any(Entity,class)))
       .thenReturn(true)
```

In the above example the first line is an expected behavior on the mocked object (dbRepository) and the second line is the data stub.

When testing with mocks, it is always a best practice to assert the number of invocations on the mocked object. 

```
Mockito.verify(dbRepository, Mockito.times(1))
	   .existsOneByEntity(Mockito.any(Entity.class))
```

**Mocking void methods**
```
Mockito.when(mailerService.sendMail()) 
       .thenThrow(MailerServiceException.class)

// this does not work
```

```
Mockito.doThrow(MailerServiceException.class)
       .when(mailerService)
       .sendMail()
       
// this works
```

```
Mockito.doNothing()
       .when(mailerService)
       .sendMail()
       
//this also works
```

**Call real methods of a mocked object**
```
Mockito.doCallRealMethod()
       .when(mailerService)
       .sendMail()
```

### Springboot Testing
Adding **@WebMvcTest** on a test class notifies SpringBoot to create only web layer beans and adds them to the application context. By doing so we refrain from creating other beans thereby speeding up the test execution.
To narrow the scope of the web layer beans (controllers etc), we can specify the controller name in the annotation to add only the specific controller to the application context.
```
@WebMvcTest(controllers = OrderController.class)
```

**[@WebMvcTest vs @SpringBootTest](https://stackoverflow.com/questions/39865596/difference-between-using-mockmvc-with-springboottest-and-using-webmvctest)**

@WebMvcTest only scans the controllers or a specified controller for the test. It uses *[slicing](https://docs.spring.io/spring-boot/docs/current/reference/html/test-auto-configuration.html#appendix.test-auto-configuration)* to reduce the dependencies.  

@SpringBootTest loads the entire application context to run tests and is preferably used for integration tests.

By default, @SpringBootTest runs on a mock Spring environment.
```
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
```
