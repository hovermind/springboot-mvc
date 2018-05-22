## JUnit
A unit test should test functionality in isolation. Side effects from other classes or the system should be eliminated for a unit test, if possible.

A JUnit test is a method contained in a class which is only used for testing. This is called a Test class. To define that a certain method is a test method, annotate it with the @Test annotation. You use an assert method, provided by JUnit or another assert framework, to check an expected result versus the actual result.
```
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;

public class MyTests {

    @Test
    public void multiplicationOfZeroIntegersShouldReturnZero() {
        MyClass tester = new MyClass(); // MyClass is tested

        // assert statements
        assertEquals(0, tester.multiply(10, 0), "10 x 0 must be 0");
        assertEquals(0, tester.multiply(0, 10), "0 x 10 must be 0");
        assertEquals(0, tester.multiply(0, 0), "0 x 0 must be 0");
    }
```
**JUnit naming conventions**
* a test name should explain what the test does.
* use the "Test" suffix at the end of test classes names.
* One possible convention is to use the "should" in the test method name. For example, "ordersShouldBeCreated" or "menuShouldGetActive".
* Another approach is to use "Given[ExplainYourInput]When[WhatIsDone]Then[ExpectedResult]" for the display name of the test method

See:    
[JUnit Annotations](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)    
[Unit Testing with JUnit](http://www.vogella.com/tutorials/JUnit/article.html)

## Java assertion frameworks - AssertJ vs Hamcrest
**Note:** AssertJ uses fluent api, therefore recommended.      
Details: [Fluent assertions for java](http://joel-costigliola.github.io/assertj/) & [Introduction to AssertJ](http://www.baeldung.com/introduction-to-assertj)

#### Simple Assertions
```
assertThat(a, equalTo(b)); //Hamcrest
assertThat(a).isEqualTo(b); //AssertJ
```

#### Dates Assertions
```
assertThat(tomorrow, isAfter(today)); //Hamcrest
assertThat(tomorrow).isAfter(today); //AssertJ
```

#### List Assertions
```
assertThat(list, Matchers.<Collection<String>> allOf(CoreMatchers.hasItem("a"),
       CoreMatchers.not(CoreMatchers.hasItem("b"))
)); //Hamcrest 
assertThat(list).        
       contains("a").
       doesNotContain("b"); //AssertJ
```

#### Null Assertions
```
assertThat(a, nullValue()); //Hamcrest
assertThat(actual).isNull();  //AssertJ
```

#### Assertions With a Custom Message
```
assertThat("Error", a, equalTo(b)); //Hamcrest
assertThat(a).isEqualTo(b).overridingErrorMessage("Error"); //AssertJ
```
See: [Hamcrest vs. AssertJ](https://dzone.com/articles/hamcrest-vs-assertj-assertion-frameworks-which-one)

## Mockito
A mock object is a dummy implementation for an interface or a class in which you define the output of certain method calls. Mock objects are configured to perform a certain behavior during a test. They typically record the interaction with the system and tests can validate that.

Mockito is a popular mock framework which can be used in conjunction with JUnit. Mockito allows you to create and configure mock objects. Using Mockito simplifies the development of tests for classes with external dependencies significantly.

If you use Mockito in tests you typically:
* Mock away external dependencies and insert the mocks into the code under test
* Execute the code under test
* Validate that the code executed correctly

See:     
[Mockito Doc](http://static.javadoc.io/org.mockito/mockito-core/2.18.3/org/mockito/Mockito.html#verification)    
[Creating mock objects with Mockito](http://www.vogella.com/tutorials/Mockito/article.html#creating-mock-objects-with-mockito)
