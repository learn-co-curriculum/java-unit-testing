# Introduction to Unit Testing

## Learning Goals

- Introduce the concept of testing.
- Explain unit testing.
- Create unit tests in Java.

## Introduction to Testing

Testing is a fundamental step in software development. We want to test the code
we write before deploying it and letting users interact with it. Through
testing, we can aim to eliminate bugs (or issues within the code) and ensure
edge cases and exceptions are handled in an appropriate manner.

So far, we have been testing manually through the use of print statements. For
example, in our labs, we compile our code and then run it by providing the
program some input. We then wait to see what the program prints out and how the
input was handled. If the output was not what we were expecting, we go back and
try to address the issue. We also use the debugger tool to help us track down
any bugs due to unexpected behavior.

This presents two issues:

1. We needed to manually run our program in order to determine if it still
   worked as we intended.
2. We needed to manually check the program's output in order to make that
   determination.

But how else could we test our code then?

## Unit Testing in Java

**Unit testing** is a specific kind of testing that focuses on validating the
functionality of a single component of the system. It's also focused on making
sure that all possible permutations of the input (valid or invalid) can be
handled successfully, even though that might sometimes mean throwing an
exception or returning an error message.

Even though unit testing alone cannot guarantee that a system will work
without errors or work as expected by the users of the system (more on that
distinction later), it is the foundational work that ensures that each
individual, small, specific piece of functionality works properly.

Overall, unit testing focuses on:

- Testing a small, specific piece of functionality.
- Covering all permutations of input parameters (valid and invalid).
- Is deterministic, as in, calling the same method with the same input multiple
  times in a row should produce the same output.

Unit testing will also address the issues that are found in manual testing
that we described above with the print statements:

1. Unit tests can be run automatically as part of the code lifecycle. The exact
   mechanisms to do this are out of scope for this module, but the ability to
   integrate unit testing into the code's flow to production is critical.
2. The actual result of a unit test isn't subject to anyone looking at the code.
   The unit test framework itself can report pass/fail and the code pipeline
   can react accordingly.

To help us create unit tests, we will be making use of an open-source framework
called **JUnit**. JUnit is a unit testing framework for Java which is used for
writing and running tests. It comes with annotations to identity testing
methods, assertions to test expected results, and test runners to quickly run
individual tests.

Let's consider a method that performs division:

```java
public class Calculator {
    public static double divide(double dividend, double divisor) {
        return 0.0;
    }
}
```

This basic implementation doesn't actually perform the division between the
two parameters that are passed in, so it currently does not work. Let's go
ahead and write a test for it though - we will discuss the process of writing
unit tests before the corresponding functionality is actually implemented later
on when we talk about "Test Driven Development".

## Code Along: Creating Unit Tests in IntelliJ

Pull down this lesson to look at the `Calculator` class. You can find it under
`java-unit-testing/src/main/java/Calculator.java`. Open it up, and you will see
the method that we showed above. We will create a unit test for it.

In IntelliJ, right click anywhere inside the method and select the "Generate" -
we can also use the Cmd-N/Ctrl-N keyboard shortcut to bring up the
same menu:

![generate-test](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/intellij-generate-test.png)

From there, go ahead and select "Test". The first time you do this, IntelliJ
will give you a dialog window to let you set up unit testing in your project:

![create-test-popup](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/intellij-create-test-popup.png)

If IntelliJ tells you that it can't find the unit testing library, click on the
"Fix" button to fix it.

Select to create the `setUp()` and `tearDown()` methods and select the method
you would like IntelliJ to create a unit test for:

![select-method-to-test.png](https://curriculum-content.s3.amazonaws.com/java-mod-3/unit-testing/select-method-to-test.png)

We now have a test class that looks like this:

![create-unit-test](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/intellij-create-unit-test.png)

```java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

   @BeforeEach
   void setUp() {
   }

   @AfterEach
   void tearDown() {
   }

   @Test
   void divide() {
   }
}
```

## Writing the Unit Test

Sometimes when we are writing multiple tests, we find that we have to do
a little prep work before we can even test the method or functionality. An
example could be defining the variables we are working with. This is where
we could condense that set-up work by creating a `setUp()` method. To force the
`setUp()` method to run before each test, we have to add the annotation,
`@BeforeEach`, right before we define the method. Now the `setUp()` method will
be called every time before each test runs.

Similarly, we may also have some clean-up work to do after testing. If we find
we have to perform the same clean-up steps after every test, then we can also
take those lines of code and move it to a `tearDown()` method. To force the
`tearDown()` method to run after each test, we have to add the annotation,
`@AfterEach`, right before we define the method. Now the `tearDown()` method
will be called after every time a test runs.

The last annotation we see in the `CalculatorTest` file is the `@Test`
annotation. This annotation signals that the following method is a _test_
method. This will tell the JUnit framework to treat this as a test case and run
it as a unit test.

Let's go ahead and add logic to our test method:

```java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

   @BeforeEach
   void setUp() {
   }

   @AfterEach
   void tearDown() {
   }

   @Test
   void divide() {
      double dividend = 4;
      double divisor = 2;
      double quotient = Calculator.divide(dividend, divisor);
      assertEquals(2.0, quotient);
   }
}
```

Note: To call the `divide()` method from the `Calculator` class here, we need to
call it using the syntax `Calculator.divide(dividend, divisor)`. This is because
of the keyword `static`. We'll learn more about this later, but for now, know
that this is how we would access the `divide()` method from another class, like
the `CalculatorTest` class.

In the test `divide()` method, we create two `double` variables that we will use
for testing. We can check to see if the `Calculator`'s `divide()` method returns
the appropriate `double` value that we expect by using an **assertion**.

```java
assertEquals(2.0, quotient);
```

`assertEquals()` is an assertion method, which is a method that unit test
frameworks, like JUnit, provide in order for us to be able to test expected
values against actual values. When we need such a test, we should check the
[JUnit documentation](https://junit.org/junit5/docs/5.7.2/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html)
to see which assertion methods it supports for the
particular use case.

In this case, we want to see if the `divide()` method actually performed the
division between the two operands.

## Running the Unit Tests

Now that our unit test is set up properly, we can choose to run a single test by
pressing the play button next to the test `divide()` method:

![run-single-test.png](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/unit-tests-run-single-test.png)

Or we can choose to run all the tests in a specific test class by clicking the
double play button next to the line with `CalculatorTest`. Note that in our
current example, we only have one test defined. If we had multiple tests defined
in this class though, it would run all of them by running the
`CalculatorTest`:

![run-all-tests.png](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/unit-tests-run-all-tests.png)

Either way, we will see the following error for our unit test, since we have
not actually implemented the reverse functionality:

![unit-test-error.png](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/unit-test-error.png)

This indicates that we expected to see the value `2.0`, but instead saw the
value `0.0`.

## Capturing More Test Cases

So now let's implement a silly version of the `divide()` method to show why
unit testing needs to be thorough:

```java
    public static double divide(double dividend, double divisor) {
        return 2.0;
    }
```

This is obviously not a good implementation since it always returns the same
value, but since it returns exactly the value our unit test expects, our unit
test will now pass if we run it again.

![tests-passed](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/unit-tests-test-passed.png)

An easy fix to this is to make sure we add another test to our existing method:

```java
    @Test
    void divide() {
            double dividend = 4;
            double divisor = 2;
            double quotient = Calculator.divide(dividend, divisor);
            assertEquals(2.0, quotient);

            // Add another test
            dividend = 6;
            quotient = Calculator.divide(dividend, divisor);
            assertEquals(3.0, quotient);
            }
```

Here, we only change the dividend value. We should get back a different result
from before. If we run the test again, it will fail on the second
`assertEquals()`.

Let's now come up with a better implementation of our `divide()` method:

```java
    public static double divide(double dividend, double divisor) {
        return dividend / divisor;
    }
```

Run the unit test now. It should all pass!

## Edge Cases

It may seem like we're done, but we're not - what if we tried to divide by 0?
We know that anything divided by 0 results in not a number, so we want to guard
against this case.

In Java, when we perform division by 0 with `double` or `float` data types, an
exception **will not be thrown**. This is because Java handles real division
differently from integer division. When division by 0 is performed with a
`double`, we could end up with any of the following:

- `1.0 / 0.0` results in Infinity (or `Double.POSITIVE_INFINITY`).
- `-1.0 / 0.0` results in -Infinity (or `Double.NEGATIVE_INFINITY`)
- `0.0 / 0.0` results in NaN (or `Double.NaN`).

The "why" Java throws an exception with integer division but not real division
is out of scope for this lesson; however, if you wish to learn more, see
[Division by Zero in Java](https://www.baeldung.com/java-division-by-zero).

Back to unit testing, let's write a new unit test to guard against a division by
zero when the dividend is positive. We'll create a new test method called
`divisionByZero`:

```java
    @Test
    void divisionByZero() {
        double dividend = 8.5;
        double divisor = 0;
        double quotient = Calculator.divide(dividend, divisor);
    }
```

Before we even add an assertion, set a breakpoint on the line:
`double quotient = Calculator.divide(dividend, divisor);` Then run the debugger
on the `divisionByZero()` test method. We can run the debugger by clicking the
play button next to the method name and choosing "Debug divisionByZero()".

![debug-unit-test](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/debug-division-by-zero-test.png)

Once we get to the breakpoint, we can open up the Java Visualizer plugin in
IntelliJ and see that both the `dividend` and `divisor` variables have been
initialized to `8.5` and `0.0` respectively. We can now click the "step-into"
action to take us into the `Calculator.divide(dividend, divisor)` method:

![unit-test-step-into](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/intellij-debugger-unit-test-step-into.png)

When we step-into the `divide()` method, notice that the `dividend` and
`divisor` variables are also set in that method.

![step-into-divide](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/intellij-debugger-unit-test-calculator-divide-method.png)

If we step-over now twice, we'll see the `quotient` variable back in the test
`divisionByZero()` method is now set to `Infinity`. This happens when a positive
`double` value is divided by 0:

![quotient-infinity](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/quotient-inifinity.png)

Consider the following statement to add to the test:

```java
assertEquals(Double.POSITIVE_INFINITY, quotient);
```

To ensure that when a `double` value is divided by 0 is set to `Infinity`, we
can do an `assertEquals(Double.POSITIVE_INFINITY, quotient);` The
`Double.POSITIVE_INFINITY` is a `double` that contains the value of "Infinity".

There are two more cases to account for when we perform division by zero with a
`double`: when the dividend is negative and when the dividend is also 0. Let's
add to the `divisionByZero()` test to account for these two additional cases:

```java
    @Test
    void divisionByZero() {
        double dividend = 8.5;
        double divisor = 0;
        double quotient = Calculator.divide(dividend, divisor);
        assertEquals(Double.POSITIVE_INFINITY, quotient);

        dividend = -8.5;
        quotient = Calculator.divide(dividend, divisor);
        assertEquals(Double.NEGATIVE_INFINITY, quotient);

        dividend = 0;
        quotient = Calculator.divide(dividend, divisor);
        assertEquals(Double.NaN, quotient);
    }
```

If we were to run the debugger again and step through the code, we'd see the
`quotient` be set to `-Infinity` when the `dividend` is negative:

![quotient-negative](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/quotient-negative-inifinity.png)

And we'd also see the `quotient` be set to `NaN` when the `dividend` is set to
0:

![quotient-nan](https://curriculum-content.s3.amazonaws.com/java-mod-1/unit-tests/quotient-nan.png)

This example highlights a very important aspect of unit testing: when
discovering new test cases to test, like we did with the case of division by
zero, we must _add_ to the existing test, not just
replace them. That's because our unit tests play a very significant role in
another form of testing called **regression testing** as well.

Regression testing is the idea that as we make changes to the code, we must
continue to ensure that things that used to work are still going to work. In
order to ensure that it works for _all_ known cases, we have to make sure
that our **test suite**, or collection of tests, continues to test all the
cases, even the ones that we think we already have covered.

## Code Check

### Calculator.java

```java
public class Calculator {
    public static double divide(double dividend, double divisor) {
        return dividend / divisor;
    }

    public static void main(String[] args) {

    }
}
```

### CalculatorTest.java

```java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    @BeforeEach
    void setUp() {
    }

    @AfterEach
    void tearDown() {
    }

    @Test
    void divide() {
        double dividend = 4;
        double divisor = 2;
        double quotient = Calculator.divide(dividend, divisor);
        assertEquals(2.0, quotient);

        // Add another test
        dividend = 6;
        quotient = Calculator.divide(dividend, divisor);
        assertEquals(3.0, quotient);
    }

    @Test
    void divisionByZero() {
        double dividend = 8.5;
        double divisor = 0;
        double quotient = Calculator.divide(dividend, divisor);
        assertEquals(Double.POSITIVE_INFINITY, quotient);

        dividend = -8.5;
        quotient = Calculator.divide(dividend, divisor);
        assertEquals(Double.NEGATIVE_INFINITY, quotient);

        dividend = 0;
        quotient = Calculator.divide(dividend, divisor);
        assertEquals(Double.NaN, quotient);
    }
}
```

## References

[JUnit documentation](https://junit.org/junit5/docs/5.7.2/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html)