# Test Utilities

Collecting some helpful testing-related utilities.

## Gradle Usage

    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.trigonic:test-utils:0.2'
    }


## Enhanced JUnit Test Parameterization

JUnit's built-in test parameterization, accessed through the Parameterized runner, is helpful but doesn't produce
very usable JUnit test labels for integrated development environments (IDEs) and can be overly verbose for simple
single-value parameterization.  The Parameterized class in this package extends JUnit's and attempts to provide
an improved experience.

Using this on a class requires an annotation on the class, an annotated static method that returns the parameters
to use, and a constructor that matches the parameters:

    import com.trigonic.utils.test.junit.Parameterized;
    import com.trigonic.utils.test.junit.Parameters;

    @RunWith(Parameterized.class)
    public class SomeTest {
        ...
        @Parameters
        public static Object[][] parameterMethod() {
            return /* an array of arrays */;
        }
        ...
        public SomeTest( /* parameters that match the array returned above */ ) {{
            ...
        }
        ...
    }

If the constructor only takes a single value, then the @Parameters method can return a one-dimensional array.
This method can also return an array, Iterable, or Collection.

To aid in the presentation of the resulting tests, you can control the labels applied to the test through
a LabelMaker.  This can be specified through a static, annotated factory method:

    import com.trigonic.utils.test.junit.Parameterized;
    import com.trigonic.utils.test.junit.Parameters;
    import com.trigonic.utils.test.junit.LabelMakerFactory;

    @RunWith(Parameterized.class)
    public class SomeTest {
        ...
        @LabelMakerFactory
        public static LabelMaker labelMaker() {
            return /* an instance of LabelMaker */
        }
        ...
    }

You can also just specify the LabelMaker class to use, if it doesn't require any special construction:

    import com.trigonic.utils.test.junit.Parameterized;
    import com.trigonic.utils.test.junit.Parameters;
    import com.trigonic.utils.test.junit.LabelMakerClass;

    @RunWith(Parameterized.class)
    @LabelMakerClass(DefaultLabelMaker.class)
    public class SomeTest {
        ...
    }

