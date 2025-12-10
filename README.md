Step1 anagram.feature (in src/test/resources/features/anagram.feature)
Feature: Check if two strings are anagrams

  Scenario Outline: Verify two strings are anagrams
    Given I have two strings "<first>" and "<second>"
    When I check if they are anagrams
    Then the result should be "<expected>"

    Examples:
      | first     | second    | expected |
      | listen    | silent    | true     |
      | triangle  | integral  | true     |
      | apple     | paple     | true     |
      | hello     | world     | false    |
      | a gentleman | elegant man | true |

Step2 Step Definitions — AnagramSteps.java (in src/test/java/steps/AnagramSteps.java)
import java.util.Arrays;

import io.cucumber.java.en.*;

public class AnagramSteps {

    private String str1;
    private String str2;
    private boolean result;

    @Given("I have two strings {string} and {string}")
    public void i_have_two_strings(String first, String second) {
        this.str1 = first;
        this.str2 = second;
    }

    @When("I check if they are anagrams")
    public void i_check_if_they_are_anagrams() {
        result = areAnagrams(str1, str2);
    }

    @Then("the result should be {string}")
    public void the_result_should_be(String expected) {
        boolean expectedBool = Boolean.parseBoolean(expected);
        assertEquals(expectedBool, result);
    }

    /**
     * Checks if two strings are anagrams.
     * Ignores case and spaces.
     */
    private boolean areAnagrams(String s1, String s2) {
        if (s1 == null || s2 == null) return false;

        // Remove spaces and convert to lowercase
        String clean1 = s1.replaceAll("\\s+", "").toLowerCase();
        String clean2 = s2.replaceAll("\\s+", "").toLowerCase();

        // Quick length check
        if (clean1.length() != clean2.length()) return false;

        // Sort and compare
        char[] arr1 = clean1.toCharArray();
        char[] arr2 = clean2.toCharArray();
        Arrays.sort(arr1);
        Arrays.sort(arr2);

        return Arrays.equals(arr1, arr2);
    }
}

Step3  Test Runner — RunCucumberTest.java (in src/test/java/runner/RunCucumberTest.java)
package runner;

import org.junit.platform.suite.api.ConfigurationParameter;
import org.junit.platform.suite.api.IncludeEngines;
import org.junit.platform.suite.api.SelectClasspathResource;
import org.junit.platform.suite.api.Suite;

import static io.cucumber.junit.platform.engine.Constants.*;

@Suite
@IncludeEngines("cucumber")
@SelectClasspathResource("features")
@ConfigurationParameter(key = GLUE_PROPERTY_NAME, value = "steps")
@ConfigurationParameter(key = PLUGIN_PROPERTY_NAME, value = "pretty")
public class RunCucumberTest {
}

Maven Dependencies (pom.xml)
<dependencies>
    <!-- Cucumber Java -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-java</artifactId>
        <version>7.14.0</version>
        <scope>test</scope>
    </dependency>

    <!-- Cucumber JUnit Platform Engine -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-junit-platform-engine</artifactId>
        <version>7.14.0</version>
        <scope>test</scope>
    </dependency>

    <!-- JUnit 5 -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>


https://www.bing.com/search?pglt=2083&q=check+two+strings+are+anagram+using+cocumber+framework&cvid=a2b4e2586236405ab2a959e72865185f&gs_lcrp=EgRlZGdlKgYIABBFGDkyBggAEEUYOTIICAEQ6QcY_FXSAQkzNzQ1M2owajGoAgCwAgA&FORM=ANNAB1&PC=U531
