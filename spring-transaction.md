## Unchecked Exceptions

+ ***CheckedException***

  are the exceptions that are checked at compile time. If some code within a method throws a checked exception, then the method must either handle the exception or it must specify the exception using throws keyword.

+ ***UncheckedException(a.k.a RuntimeException, Error, and their subclasses)***

  are the exceptions that are not checked at compiled time. Java programming language does not require methods to catch or to specify unchecked exceptions.

+ **guideline**

If a client can reasonably be expected to recover from an exception, make it a checked exception. If a client cannot do anything to recover from the exception, make it an unchecked exception. see [Exceptions](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html).



## [Transaction](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)