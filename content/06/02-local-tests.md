---
layout: default
title: 6.2. Local tests
parent: 6. Debugging and testing
nav_order: 2
---

# 6.2. Local tests 

Local tests exercise components that have no dependence on the Android framework. That is, they are platform-independent class tests. As they are platform-independent, they are executed using the Java Virtual Machine of the development team and they do not run on the emulator.

Local tests are often implemented as **unit tests**, where a given class and a given method are tested in isolation. For this reason, local testing can be designed and applied early in the development process.

To prepare a unit test in Android Studio, we place the cursor on the name of the class or method to be tested and press `Control + Caps + T` (`Command + Caps + T in macOS`). Then, we indicate that we want to create a new test.

> ![Creating a test in Android Studio.](/images/06/create-test.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating a test in Android Studio.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then we will indicate that we will use the [JUnit library](https://junit.org/junit5/) and the class method we want to test:

> ![Setting up a JUnit test.](/images/06/configuration-junit.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Setting up a JUnit test.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Finally, we indicate that in this case the test file corresponds a local test and therefore must be created in the `test` folder:

> ![Preparing a local test.](/images/06/local-test.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Preparing a local test.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


 JUnit tests work by running the method with certain parameters and comparing the result to an expected output that is also provided in the test.

---

## JUnit 4


This is an example of a JUnit test:

```kotlin
public class UserTest {
   var user:User = User("Xavier",38)

   @Test
   fun calculateCarInsuranceCost() {
       assertEquals(30000,user.CalculateCarInsuranceCost(4));
   }
}
```

To run the test, click on the icon next to the method name:

> ![Running a JUnit test (step 1).](/images/06/running-tests.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Running a JUnit test (step 1).*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then we select `Run`:

> ![Running a JUnit test (step 2).](/images/06/running-tests2.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Running a JUnit test (step 2).*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

When running the method with parameter 4, we expect a result of 30000. To compare the expected result with the actual result we have a set of assertions. You can find a list of different assertions here:
[https://developer.android.com/reference/junit/framework/Assert](https://developer.android.com/reference/junit/framework/Assert)

Android Studio shows us on its interface if the test has passed successfully or not: 

> [Inspecting the result of a test.](/images/06/test-result.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Inspecting the result of a test.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

If an error occurs, Android Studio reports which type of error occurred. The error occurs when the actual result does not correspond to the expected result.

> [Inspecting a failed test.](/images/06/failed-test.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Inspecting a failed test.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

If we want to run **all the tests** in a test class, we must click on the icon next to the class name:

> [Running all tests.](/images/06/test-all.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Running all tests.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

From the Terminal window we can also run all tests using the command: `gradlew test`.

Moreover, we can create parameterized tests so that we can apply several different tests of a method.

```kotlin
@RunWith(Parameterized::class)
public class UserTest(val param: Int, val result: Int) {
   var user:User = User("Xavier",18)

   companion object {
       @JvmStatic
       @Parameterized.Parameters
       fun data() : Collection<Array<Any>> {
           return listOf(
               arrayOf(4, 24000),         // First test: (param = 4, result = 2400)
               arrayOf(7, 30000) // Second test: (param = 1999, result = 30000)
           )
       }
   }


   @Test
   fun calculateCarInsuranceCost() {
       assertEquals(result,user.CalculateCarInsuranceCost(param));
   }
}
```

JUnit 5 provides much more expressiveness than its predecessor. It makes it easy to perform multiple parameterized tests of different methods in the same test class thanks to its annotations.

To achieve this, you have to add the following in dependencies:

```gradle
testImplementation "org.junit:junit-bom:5.8.2"
testImplementation 'io.kotlintest:kotlintest-runner-junit5:3.3.2'
testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
testRuntimeOnly "org.junit.platform:junit-platform-launcher"
testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
testRuntimeOnly 'org.junit.vintage:junit-vintage-engine:5.8.1'
testImplementation 'org.junit.jupiter:junit-jupiter-params:5.8.2'
```

And, for example, we can create parameterized tests for two different methods with different parameters:

```kotlin
internal class UserTest {

   var user:User = User("Xavier",18)

   @ParameterizedTest
   @CsvSource("4,24000","7,30000")
   fun calculateCarInsuranceCost(param:Int,result:Int) {
       assertEquals(result,user.CalculateCarInsuranceCost(param));

   }

   @ParameterizedTest
   @CsvSource("4,30000","7,30000")
   fun CalculateCarInsuranceCostV2(param:Int,result:Int) {
       assertEquals(result,user.CalculateCarInsuranceCostV2(param));

   }
}
```


In the example above the methods are `calculateCarInsuranceCost` and `calculateCarInsuranceCostV2` and the parameters are entered using the powerful `@CsvSource` annotation. In this annotation, the parameters are indicated in the order in which they are declared in the test header. That is, in both cases the first parameter is `param` and the second is `result`.


