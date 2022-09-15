
# ==EFFECTIVE JAVA==

&emsp;
### 01 - Static Factory

Use static factory methods to instantiate classes rather than using constructor of various signatures. It is easy to use and remember when defined with appropriate method names such as ***from, of, valueOf, getInstance, newInstance***, etc..

&emsp;
### 02 - Builder Pattern

Use builder pattern instead of constructors or static factory methods when the object creation has multiple optional parameters.

Recommended over #Telescopic constructor pattern and #JavaBeans pattern

&emsp;
### 03 - Singleton

Use singleton pattern in cases when only one instance of a class is ever needed. When using singleton pattern ensure a class only has one instance, and provide a global point of access to it.

***Keep in mind to make the implementation thread safe as multiple threads could create multiple instances***