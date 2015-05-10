# Java Primer

## Section 1: Java Basics

### Item 1: Define the scope of variables

- There are 4 types of variables:
  - **Instance Variables (Non-Static Fields)** - Variables declared without the *static* modifier, belong to the instance and are independent from the class.
  - **Class Variables (Static Fields)** - Declared with the *static* modifier, this tells the compiler there is exactly one copy of this variable, independent on how many instances use it. Add the *final* keyword to prevent the variable from ever changing.
  - **Local Variables** - Variables defined within the scope of a method, and therefore not visible from outside.
  - **Parameters** - Arguments of our methods.

- Naming considerations are as follow:
  - Casesensitive
  - $ and leading _ are allowed but generally frowned upon
  - Single-words variables are lowercase, multi-word are camel case and contant value vars are upper-case with an underscore between words.


### Item 2: Define the structure of Java class

- There are three types of comments:
  - /* multiline */
  - /** multiline that gets picked up by javadoc */
  - // singleline

Continues at http://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html

### Item 3: Create executable Java applications with a main method

``bash
javac ./HelloWorld.java
java HelloWorld
``

### Item 4: Import other Java packages to make them accessible in your code.
 
- Provide access protection and name space management
- Defined my *package pkg_name;* statement at the top of each .java file
- Each source file can only have one package
- In the absence of a **package** statement, types are set to an unamed package
- The "fully qualified name" of a type includes the packages (ie. java.awt.Rectangle)
- Package naming convention:
  - Lower case
  - Revers internet domain name
- You can access package types by any of the following:
  - Referring to the Package Member's qualified name
  - Import a Package Member (ie. import graphics.Rectangle;)
  - Import an Entire Member (ie. import graphics.*;)
- Packages are not hierarchical in the sense that importing all types in java.awt will not expose types in java.awt.colors
- Ambiguos imports are allowed, but must we called by qualified name (ie. graphics.Rectangle rect = new graphics.Rectangle();)
- static imports
  - When importing static attributesm you must refer to them using qualified names
  - You can overrride this behaviour by using the *import static* keyword
  - ie.
 
  ``java
  import static java.lang.Math.PI;
  import static java.lang.Math.cos;
  double r = cos(PI * 2);
  ``
 
- The compiler and JVM look for packages in the $CLASSPATH (ie. /home/fcox/classes/com/example/graphics)
- The source files don't need to be places hierarchically in packages since the package statement defined the replationship for the compiler
- each source file can only have one public type and it must have the same name as the file

----

## Section 2: Working with Java Data Types

### Item 1: Declare & Initialise Variables

- Java is *statically-typed*: all variables must be declared before being used.
- Eight primitive types:

  - **int**
  - **byte** - 8-bit ranging from -128 to 127. Default: 0.
  - **short** - 32-bit ranging from -2^31 to 2^31 -1. Default: 0.
  - **long** - 64-bit ranging from -2^64 to 2^63 -1. Default: 0L
  - **float** - 32-bit. Default: 0.0f
  - **double** - 64-bit. Default: 0.0d
  - **boolean** - true or false, but not necessarily 1-bit. Default: false
  - **char** - 16-bit rangin from 0 ('/u0000') to 65535 ('\uffff'). Default:'\u0000'

- Strings are not primitive (java.lang.String) and are enclosed by double quotes.

- *literals* are the source code representation of a fixed value that doesn't require computation and don't require the keyword **new** on assignment. i.e:

``java
boolean result = true;
char capitalC = 'C';
byte b = 100;
short s = 10000;
int i = 100000;
``

  - **Integer Literals** 
    - 0L / 0l for **long** otherwise int.
    - **int** literals can be used to create **bute**, **short**, **int** and **long**
    - Int literals can be represented as decimal, hexadecimal of binary as follows:
    - You can place underscores between digits to improve readability (floats as well).

``java
int decVal = 26; // no prefix
int hexVal = 0x1a; // prefix by 0x
int binVal = 0b11010; // prefix by 0b
``

  - **Floating-Point Literals**
    - **float** literals end with F or f otherwise **double** (optionally D/d)
    - **float** can be coerced from F/f, D/d or E/e (sceintific notation). i.e.:

``java
double d1 = 123.4;
double d2 = 1.234e2;
float f1 = 123.4f;
``

  - **Character & String Literals**
    - May contain any Unicode (UTF-16) either directly or through unicode escaping;
    - Single quotes for **Char** literals, double quotes for **String**
    - Special escape sequences: \b (backspace), \t (tab), \n (line feed), \f (form feed), \r (carriage return), \" (double quote, \' (single quote), \\ (backslash)

  - **Class Literals**
    - Represented by the class name followed by *.class*
    - Type Class.


Continues at http://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

**Arrays**

- Arrays are zero-indexed containers of fixed type and immutable length.
- You instantiate an array by declaring the type + [ ] and the variable name. The length is defined on assignment.
- These are alternative syntax for initialising an array:

``java
anArray = new int[10];
anArray[0] = 100 // etc.
// vs
int[] anArray = { 
    100, 200, 300,
    400, 500, 600, 
    700, 800, 900, 1000
};
``

- Multidimensional arrays are defined with two square brackets and allow for varying element lengths.
- All arrays have a length attribute.
- The *System* class has an arraycopy method which allows you to copy slices of an array into another (iitialized) array.
- The javautils.Arrays class offers several arrya manipulation methods.

http://docs.oracle.com/javase/tutorial/java/nutsandbolts/QandE/questions_variables.html

**Initializing Fields**
 
example:
 
``java
public class Test {
    static{
        System.out.println("Static");
    }
    {
        System.out.println("Non-static block");
    }
    public static void main(String[] args) {
        Test t = new Test();
        Test t2 = new Test();
    }
}
``
 
- Static blocks:
  - Gets called once when the class is defined
 
``java
static {
    // whatever code is needed for class initialization goes here
}
``
 
- Private static method initialization (alternative to static blocks)
 
``java
class Whatever {
                public static varType myVar = initializeClassVariable();
                private static varType initializeClassVariable() {
                                // initialization code goes here
                }
}
``
 
- Initialization Blocks
  - Get called every time an instance is constructed
 
``java
{
    // whatever code is needed for instance initialization goes here
}
``

### Item 2: Differentiate between object reference variables and primitive variables.
 
**Numbers Classes**
 
- Numbers classes build on the number primitives functionality by wrapping an object around them.
- This happens at the compiler level (autoboxing) if you use a primitive where an object was expected.
- The Number (base) class exposes methods for converting and comparing number types:
 
``java
double doubleValue();
int compareTo(Byte anotherByte);
boolean equals(Object obj);
``
 
**Integer Class**
 
see methods: http://docs.oracle.com/javase/tutorial/java/data/numberclasses.html
 
### Item 3: Read or Write to Object Fields
 
**Access Modifiers**
 
- *public* - accessible from all classes
- *protected* - accessible from subclasses, but not instances
- *package-private* (default) - accessible from the package, but not subsclasses
- *private* - accessible only from its own class
 
**Creating Objects**
 
- Declaration - associates a variable name with an object type
- Instantiation - the *new* keyword creates the object
- Initialization - the *new* operator followed by a contructor
 
- Java support method overloading - having functions with the same name but which take different parameter types, be different function. You can therefore have different constructor methods depending on what parameters are passed.
 
- The default constructor is a no-argument constructor which calls the parent class' no-argument constructor.
 
- Java does automatic garbage collection when references to an object are out of scope or non-existant

 
### Item 4: Explain an Object's Lifecycle
 
 
### Item 5: Call methods on objects
 
- If you try to return a value for a method declared void, a compiler error will be thrown.
 
**this Keyword**
 
``java
public class Rectangle {
    private int x, y;
    private int width, height;
       
    public Rectangle() {
        this(0, 0, 1, 1);
    }
    public Rectangle(int width, int height) {
        this(0, 0, width, height);
    }
    public Rectangle(int x, int y, int width, int height) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
    }
    ...
}
``
 
### Item 6: Manipulate data using the *StringBuilder* class and its methods
 
- *StringBuilder* objects are like strings but mutable. (variable-length arrays of chars).
- In addition to *length()*, StringBuilders have a *capacity()* value which auto expands if necessary. Defaults to 16.
- StringBuilder are especially usefull for concatenating strings.
- Most methods are overloaded and *in place* operations.
 
**Constructors**
 
- StringBuilder()
- StringBuilder(CharSequence cs)
- StringBuilder(int initCapacity)
- StringBuilder(String s)
 
**Methods**
 
- void setLength(int newLength) - Truncates or appends nulls accordingly.
- void ensureCapacity(int minCapacity) - Ensures capacity is at least minCapacity.
- StringBuilder append(String s) - Overloaded method, converts type and appends to the end of StringBuilder.
- StringBuilder delete(int start, int end)
- StringBuilder insert(int offset, String s
- StringBuilder replace(int start, int end, String s)
- void setCharAt(int index, char c)
- StringBuilder reverse()
- String toString()
 
### Item 7: Create and Manipulate Strings
 
- 13 Contructors to choose from:
 
``java
String greeting = "Hello world!";
//
char[] helloArray = { 'h', 'e', 'l', 'l', 'o', '.' };
String helloString = new String(helloArray);
// etc.
``
 
**Methods**
 
- String length()
- String concat(String s) - Same as "+"
- char charAt(int i)
- String substring(int beginIndex, int endIndex)
- CharSequence subSequence(int beginIndex, int endIndex)
- String[] split(String regex, int limit)
- String trim() - returns a copy w/ trimmed leading and trailing whitespaces removed.
- String toLowerCase()
- String toUpperCase()
- int indexOf(String str)
- int lastIndexOf(String str)         
- boolean contains(CharSequence s)
- String replace(CharSequence target, CharSequence replacement)
- String replaceAll(String regex, String replacement)
- String replaceFirst(String regex, String replacement)
- boolean endsWith(String suffix)
- boolean startsWith(String prefix)
- boolean matches(String regex)
- etc.
 
**Format Strings**
 
``java
String fs;
fs = String.format("The value of the float " +
                   "variable is %f, while " +
                   "the value of the " +
                   "integer variable is %d, " +
                   " and the string is %s",
                   floatVar, intVar, stringVar);
System.out.println(fs);
``
 
**Converting Between Numbers & String**
 
``java
float a = (Float.valueOf(args[0])).floatValue();
float b = (Float.valueOf(args[1])).floatValue();
// or
float a = Float.parseFloat(args[0]);
float b = Float.parseFloat(args[1]);
``
 
``java
String s1 = "" + i;
// or
String s2 = String.valueOf(i);
// or
String s3 = Integer.toString(i);
String s4 = Double.toString(d);
``

# Section 3: Using Operators & Decision Constructs
 
### Item 1: Use Java Operators
 
**Operators & Precedence**
 
postfix  expr++ expr--
unary    ++expr --expr +expr -expr ~ !
multiplicative     * / %
additive                + -
shift       << >> >>>
relational             < > <= >= instanceof
equality                == !=
bitwise AND       &
bitwise exclusive OR       ^
bitwise inclusive OR        |
logical AND         &&
logical OR             ||
ternary ? :
assignment         = += -= *= /= %= &= ^= |= <<= >>= >>>=
 
**Prefix vs Postfix Increment Operator**
 
``java
        System.out.println(i);
        // prints 6
        System.out.println(++i);
        // prints 6
        System.out.println(i++);
        // prints 7
``
 
**instanceOf Comparison Operator**
 
``java
obj1 instanceof Parent
``
 
**Bitwise & Bitshift Operators**
 
...
 
### Item 2: Use parentheses to override operator precendence
 
- Statements can be *Expression Statements*, *Declaration Statements*, or *Control Flow Statements*
- Expression and declaration statements end with ";
- Blocks are groups of zero or more statements between balanced braces
 
### Item 3: Test equality between strings and other objects using == and equals()
 
**Object Class Methods**
 
- protected Object clone() throws CloneNotSupportedException
- public boolean equals(Object obj)
- protected void finalize() throws Throwable - Called by the garbage collector on an object when garbage collection determines that there are no more references to the object
- public final Class getClass()
- public int hashCode() - Returns a hash code value for the object.
- public String toString()
- Threading-related Methods
  - public final void notify()
  - public final void notifyAll()
  - public final void wait()
  - public final void wait(long timeout)
  - public final void wait(long timeout, int nanos)
 
**Notes**
 
- The default clone method needs to be overriden if the subclass of Object contains references to external objects.
- The equals method uses "==" for primitives, but tests reference equality for Objects and so will most likely require overriding.
 
### Item 4: Create and use if-else constructs.
 
``java
if (testscore >= 90) {
            grade = 'A';
        } else if (testscore >= 80) {
            grade = 'B';
        } else if (testscore >= 70) {
            grade = 'C';
        } else if (testscore >= 60) {
            grade = 'D';
        } else {
            grade = 'F';
        }
``
 
### Item 5: Use a switch statement.
 
``java
switch (month) {
            case 1:  monthString = "January";
                     break;
            case 2:  monthString = "February";
                     break;
            case 3:  monthString = "March";
                     break;
            case 4:  monthString = "April";
                     break;
            case 5:  monthString = "May";
                     break;
            case 6:  monthString = "June";
                     break;
            case 7:  monthString = "July";
                     break;
            case 8:  monthString = "August";
                     break;
            case 9:  monthString = "September";
                     break;
            case 10: monthString = "October";
                     break;
            case 11: monthString = "November";
                     break;
            case 12: monthString = "December";
                     break;
            default: monthString = "Invalid month";
                     break;
        }
``
 
``java
        switch (month) {
            case 1:  futureMonths.add("January");
            case 2:  futureMonths.add("February");
            case 3:  futureMonths.add("March");
            case 4:  futureMonths.add("April");
            case 5:  futureMonths.add("May");
            case 6:  futureMonths.add("June");
            case 7:  futureMonths.add("July");
            case 8:  futureMonths.add("August");
            case 9:  futureMonths.add("September");
            case 10: futureMonths.add("October");
            case 11: futureMonths.add("November");
            case 12: futureMonths.add("December");
                     break;
            default: break;
        }
``
 
``java
switch (month) {
            case 1: case 3: case 5:
            case 7: case 8: case 10:
            case 12:
                numDays = 31;
                break;
            case 4: case 6:
            case 9: case 11:
                numDays = 30;
                break;
            case 2:
                if (((year % 4 == 0) &&
                     !(year % 100 == 0))
                     || (year % 400 == 0))
                    numDays = 29;
                else
                    numDays = 28;
                break;
            default:
                System.out.println("Invalid month.");
                break;
        }
``
 
# Section 4: Creating and Using Arrays
 
### Item 1: Declare, instantiate, initialize and use a one-dimensional array.
 
``java
// declares an array of integers
int[] anArray;
// allocates memory for 10 integers
anArray = new int[10];    
// initialize first element
anArray[0] = 100;
``
 
**Copying Arrays**
 
- public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length) - System.arraycopy(...)
- char[] copyTo = java.util.Arrays.copyOfRange(copyFrom, 2, 9);
 
### Item 2: Declare, instantiate, initialize and use a multi-dimensional array
 
``java
int[] anArray = {
    100, 200, 300,
    400, 500, 600,
    700, 800, 900, 1000
};
// or
String[][] names = {
            {"Mr. ", "Mrs. ", "Ms. "},
            {"Smith", "Jones"}
        };
``
 
### Item 3: Declare and use an ArrayList.
 
- Two list implementations in Java:
  - ArrayList
  - LinkedList
 
... TBC ...
 
http://docs.oracle.com/javase/tutorial/collections/interfaces/list.html
 
# Section 5: Using Loop Constructs
 
### Item 1: Create and use while loops.
### Item 3: Create and use do-while loops.
 
 
``java
int count = 1;
while (count < 11) {
  System.out.println("Count is: " + count);
  count++;
}
//
int count = 1;
do {
  System.out.println("Count is: " + count);
  count++;
} while (count < 11);
``
 
### Item 2: Create and use for loops including the enhanced for loop.
 
``java
for (initialization; termination;
     increment) {
    statement(s)
}
``
 
- If the initialization variable doesn't need to be used outside the scope of the loop it is best to define it in the loop definition.
 
``java
// Inifinite Loop
for ( ; ; ) {
   
    // your code goes here
}
``
 
``java
// Enhanced For Loop
int[] numbers = {1,2,3,4,5,6,7,8,9,10};
for (int item : numbers) {
  System.out.println("Count is: " + item);
}
``
 
### Item 4: Compare loop constructs.
### Item 5: Use break and continue.
 
- **Unlabeled** *break* statement exits a for, while or do-while statement or a switch statement.
- **Labeled** *break* exits the named outer loop:
 
``java
search:
  for (i = 0; i < arrayOfInts.length; i++) {
      for (j = 0; j < arrayOfInts[i].length;
            j++) {
            if (arrayOfInts[i][j] == searchfor) {
               foundIt = true;
               break search;
            }
      }
  }
``
 
- The unlabeled *continue* statement skips the current iteration/
- The labeled *continue* statement skips the named loop's current iteration.
 
``java
test:
        for (int i = 0; i <= max; i++) {
            int n = substring.length();
            int j = i;
            int k = 0;
            while (n-- != 0) {
                if (searchMe.charAt(j++) != substring.charAt(k++)) {
                    continue test;
                }
            }
            foundIt = true;
                break test;
        }
``
 
- Return statements are also a branching technique (returning control to the function the current function)

## Section 6: Working with Methods and Encapsulation

## Section 7: Working with Inheritance

### Item 1: Implement Inheritance

**Inheritance**
 
- derived / extended / child class vs. superclass / base / parent class
- The subclass inherents everything (methods, variables, etc.). The constructor is not inherited, by can be called from the child class.
 
``java
public class MountainBike extends Bicycle { // SUBCLASSES Bicycle
    // the MountainBike subclass adds one field
    public int seatHeight;
    // the MountainBike subclass has one constructor
    public MountainBike(int startHeight,
                        int startCadence,
                        int startSpeed,
                        int startGear) {
        super(startCadence, startSpeed, startGear); // INVOKES PARENT CONTRUCTOR
        seatHeight = startHeight;
    }  
        
    // the MountainBike subclass adds one method
    public void setHeight(int newValue) {
        seatHeight = newValue;
   }  
}
``
 
- You can "hide" parent variables by declaring fields with the same name in the child class (though not recommended)
- You can override methods.
- If the child class is in the same module, the parent class' package-private members are visible.
- All objects are subclasses of the Object class.
 
**Casting**
 
- Casting is the process of trasnforming one variable into aother variable type (class).
- Downcasting = casting to a more "specific" object
 
``java
Object aSentenceObject = "This is just a regular sentence";
String aSentenceString = (String)aSentenceObject;
``
 
- Upcasting = casting to a more generic object
 
``java
String aSentenceString = "This is just another regular sentence";
Object aSentenceObject = (Object)aSentenceString;
``
 
- Upcasting is always permitted.
- Downcasting may throw a run-time exception if the type can't be coerced. (ClassCastException)
 
**Multiple Inheritance**
 
TBC: http://docs.oracle.com/javase/tutorial/java/IandI/multipleinheritance.html

## Section 8: Handling Exceptions

### Item 1: Differentiate among checked exceptions, RuntimeException, and Error.

- *Exception* == *Exceptional Event*
- The call stack throws throws the exception until someone catches it (*exception handler*). If none is available, the runtime system (and therefore the program) terminates.
- Types of Exception:
  - Checked Exception 
  - Unchecked Exceptions:
    - Error
    - Runtime Exception

``java
try {
    // code...
} catch (ExceptionType name) {
    // code...
} catch (IOException|SQLException ex) {
    logger.log(ex);
    throw ex;
}
``

- *catch* statements with multiple exception types have implicitly *final* parameters

``java
// clean-up code to prevent resource leaks (without try-with-resource statement)
finally {
    if (out != null) { 
        System.out.println("Closing PrintWriter");
        out.close(); 
    } else { 
        System.out.println("PrintWriter not open");
    } 
} 
``

- try-with-resource can be used with any object that implements the java.lang.AutoCloseable / java.io.Closeable 

``java
try (
        java.util.zip.ZipFile zf =
             new java.util.zip.ZipFile(zipFileName);
        java.io.BufferedWriter writer = 
            java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
    ) { 
// code...
}
``

- **Suppressed Exceptions** can occur when exception handlers introduce new exceptions (ie. erro closing a file that raised an exception). The suppressed exceptions are available in the Throwable.getSuppressed method of the exception thrown.

**Checked vs Unchecked Excpetions**

 

- Unchecked exceptions are "defects in the program" - often invalid arguments to non-private methods. They subclass *RuntimeException* and "reflect errors in your program's logic and cannot be reasonably recovered from at run time".

- Checked exceptions are "invalid conditions in areas outside the immediate control of the program", such as invalid user input, database problems, network outages, absent files, etc. These are subclasses of exception. Methods are obliged to establish a policy for all checked exceptions (handle or throw).

- Any exception that can be thrown from a method is part of its API (Checked Exceptions)

 

**Exception Throwing**

 

- If you can't assume the use cases for a method, it is sometimes better to throw exceptions up the stack instead of handling them

- To allow methods to throw checked exceptions, you must specify a *throw* clause.

- *Throwable* API exposes methods to inspect the exception chain: *getCause()*, *initCause(Throwable)*, Throwable(String, Throwable), Throwable(Throwable), *getStackTrace()*, etc.

 

``java

// Chain of Exception

try {

  // code

} catch (IOException e) {

    throw new SampleException("Other IOException", e);

}

``

 

**Stack Trace**

 

``java

// Stack Trace Inpection

catch (Exception cause) {

    StackTraceElement elements[] = cause.getStackTrace();

    for (int i = 0, n = elements.length; i < n; i++) {      

        System.err.println(elements[i].getFileName()

            + ":" + elements[i].getLineNumber()

            + ">> "

            + elements[i].getMethodName() + "()");

    }

}

``

 

``java

// Stack Tracing with Logging

try {

    Handler handler = new FileHandler("OutFile.log");

    Logger.getLogger("").addHandler(handler); 

} catch (IOException e) {

    Logger logger = Logger.getLogger("package.name");

    StackTraceElement elements[] = e.getStackTrace();

    for (int i = 0, n = elements.length; i < n; i++) {

        logger.log(Level.WARNING, elements[i].getMethodName());

    }

}

``

 

**Custom Exceptions**

 

- One strong motivation to create a higher level exception is when you want to be able to catch all exceptions uncer one class (ie. a *LinedListException* could be thrown off an *InvalidIndexException* or a *ObjectNotFoundException*)

- Include the word *Excpetion* in the class name for readbility.

- Most often subclassed from *Exception*.

 

**Advantages of Exceptions**

 

- Separating Error-Handling Code from "Regular" Code

- Propagating Errors Up the Call Stack

- Grouping and Differentiating Error Types

 

 