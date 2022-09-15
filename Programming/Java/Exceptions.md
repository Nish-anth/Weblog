[Exceptions are expensive](https://stackoverflow.com/questions/36343209/which-part-of-throwing-an-exception-is-expensive)



**Creating** an exception object is not _necessarily_ more expensive than creating other regular objects. The main cost is hidden in native [`fillInStackTrace`](http://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html#fillInStackTrace--) method which walks through the call stack and collects all required information to build a stack trace: classes, method names, line numbers etc.

Most of `Throwable` constructors implicitly call `fillInStackTrace`. This is where the idea that creating exceptions is slow comes from. However, there is one [constructor](http://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html#Throwable-java.lang.String-java.lang.Throwable-boolean-boolean-) to create a `Throwable` without a stack trace. It allows you to make throwables that are very fast to instantiate. Another way to create lightweight exceptions is to override `fillInStackTrace`.