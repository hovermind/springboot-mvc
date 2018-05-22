## JUnit
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

Details:    
[JUnit Annotations](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)    
[Unit Testing with JUnit](http://www.vogella.com/tutorials/JUnit/article.html)

## Java assertion frameworks - AssertJ vs Hamcrest
**Note:**    
AssertJ uses fluent api, therefore recommended. Details - [Fluent assertions for java](http://joel-costigliola.github.io/assertj/)

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
[Details - Hamcrest vs. AssertJ](https://dzone.com/articles/hamcrest-vs-assertj-assertion-frameworks-which-one)
