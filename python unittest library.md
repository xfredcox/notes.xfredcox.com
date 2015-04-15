# unittest

### Definitions
- test fixture 
  - A test fixture represents the preparation needed to perform one or more tests, and any associate cleanup actions. This may involve, for example, creating temporary or proxy databases, directories, or starting a server process.
- test case
  - A test case is the smallest unit of testing. It checks for a specific response to a particular set of inputs. unittest provides a base class, TestCase, which may be used to create new test cases.
- test suite
  - A test suite is a collection of test cases, test suites, or both. It is used to aggregate tests that should be executed together.
  - Test suites are implemented by the TestSuite class. This class allows individual tests and test suites to be aggregated; when the suite is executed, all tests added directly to the suite and in child test suites are run.
- test runner
  - A test runner is a component which orchestrates the execution of tests and provides the outcome to the user. The runner may use a graphical interface, a textual interface, or return a special value to indicate the results of executing the tests.
  - A test runner is an object that provides a single method, run(), which accepts a TestCase or TestSuite object as a parameter, and returns a result object. The class TestResult is provided for use as the result object.

### Basic Examples

``python
import random
import unittest
class TestSequenceFunctions(unittest.TestCase):
    def setUp(self):
        self.seq = range(10)
    def test_shuffle(self):
        # make sure the shuffled sequence does not lose any elements
        random.shuffle(self.seq)
        self.seq.sort()
        self.assertEqual(self.seq, range(10))
        # should raise an exception for an immutable sequence
        self.assertRaises(TypeError, random.shuffle, (1,2,3))
    def test_choice(self):
        element = random.choice(self.seq)
        self.assertTrue(element in self.seq)
    def test_sample(self):
        with self.assertRaises(ValueError):
            random.sample(self.seq, 20)
        for element in random.sample(self.seq, 5):
            self.assertTrue(element in self.seq)
if __name__ == '__main__':
    unittest.main()
``

### Test Fixtures

> When a setUp() method is defined, the _test runner will run that method prior to each test_. Likewise, if a tearDown() method is defined, the test runner will invoke that method after each test. In the example, setUp() was used to create a fresh sequence for each test.

However, you can can setup costly fixtures with this pattern:

``python
import unittest
class SimpleWidgetTestCase(unittest.TestCase):
    def setUp(self):
        self.widget = Widget('The widget')
class DefaultWidgetSizeTestCase(SimpleWidgetTestCase):
    def runTest(self):
        self.assertEqual(self.widget.size(), (50,50),
                         'incorrect default size')
class WidgetResizeTestCase(SimpleWidgetTestCase):
    def runTest(self):
        self.widget.resize(100,150)
        self.assertEqual(self.widget.size(), (100,150),
                         'wrong size after resize')
``

### CLI

The unittest module can be used from the command line to run tests from modules, classes or even individual test methods:

``bash
python -m unittest test_module1 test_module2
python -m unittest test_module.TestClass
python -m unittest test_module.TestClass.test_method
``

-b, --buffer

The standard output and standard error streams are buffered during the test run. Output during a passing test is discarded. Output is echoed normally on test fail or error and is added to the failure messages.

-c, --catch

Control-C during the test run waits for the current test to end and then reports all the results so far. A second control-C raises the normal KeyboardInterrupt exception.

See Signal Handling for the functions that provide this functionality.

-f, --failfast

Stop the test run on the first error or failure.

### Test Discoverability

Test discovery is implemented in TestLoader.discover(), but can also be used from the command line. The basic command-line usage is:

``bash
cd project_directory
python -m unittest discover
``

This probably has a very interesting implementation...

### Decorators

``python
class MyTestCase(unittest.TestCase):
    @unittest.skip("demonstrating skipping")
    def test_nothing(self):
        self.fail("shouldn't happen")
    @unittest.skipIf(mylib.__version__ < (1, 3),
                     "not supported in this library version")
    def test_format(self):
        # Tests that work for only a certain version of the library.
        pass
    @unittest.skipUnless(sys.platform.startswith("win"), "requires Windows")
    def test_windows_support(self):
        # windows specific testing code
        pass
``

unittest.skip(reason)
Unconditionally skip the decorated test. reason should describe why the test is being skipped.

unittest.skipIf(condition, reason)
Skip the decorated test if condition is true.

unittest.skipUnless(condition, reason)
Skip the decorated test unless condition is true.

unittest.expectedFailure()
Mark the test as an expected failure. If the test fails when run, the test is not counted as a failure.

exception unittest.SkipTest(reason)
This exception is raised to skip a test.

### Assertion API

- assertEqual(a, b) :: a == b	 
- assertNotEqual(a, b) :: a != b	 
- assertTrue(x) :: bool(x) is True	 
- assertFalse(x) :: bool(x) is False	 
- assertIs(a, b) :: a is b
- assertIsNot(a, b) ::	a is not b
- assertIsNone(x) :: x is None
- assertIsNotNone(x) :: x is not None
- assertIn(a, b) :: a in b
- assertNotIn(a, b) :: a not in b
- assertIsInstance(a, b) :: isinstance(a, b)
- assertNotIsInstance(a, b) :: not isinstance(a, b)
- assertAlmostEqual(a, b)	:: round(a-b, 7) == 0	 
- assertNotAlmostEqual(a, b) :: round(a-b, 7) != 0	 
- assertGreater(a, b) :: a > b
- assertGreaterEqual(a, b) :: a >= b	
- assertLess(a, b) :: a < b
- assertLessEqual(a, b) :: a <= b
- assertRegexpMatches(s, r) :: r.search(s)	
- assertNotRegexpMatches(s, r) :: not r.search(s)
- assertItemsEqual(a, b) :: sorted(a) == sorted(b) and works with unhashable objs
- assertDictContainsSubset(a, b) :: all the key/value pairs in a exist in b
- assertMultiLineEqual(a, b) :: strings
- assertSequenceEqual(a, b) :: sequences
- assertListEqual(a, b) :: lists
- assertTupleEqual(a, b) :: tuples
- assertSetEqual(a, b) :: sets or frozensets
- assertDictEqual(a, b) :: dicts

# Bring on the TDD!