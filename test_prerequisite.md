## JUnit

[JUnit Annotations](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)


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
