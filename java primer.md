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

# Section 6: Working with Methods and Encapsulation
 
### Item 1: Create methods with arguments and return values.
 
### Item 2: Apply the static keyword to methods and fields.
 
- Class methods (ie. static) can be acced through the object name. They can also be called from the instance, but this is discouraged as it makes things unclear.
 
### Item 3: Create an overloaded method.
 
- Java can distinguish between methods with different method signatures, allowing you to create overloaded methods.
 
 
### Item 4: Differentiate between default and user-defined constructors.
 
- Constructors have no return type.
- Constructors are called by the *new* operator to instanciate an object.
- Defaults to no-argument contructor of superclass if necessary.
 
### Item 5: Apply access modifiers.
### Item 6: Apply encapsulation principles to a class.
 
- Inner classes are those non-static declared inside the scope of another class.
- Inner classes can be regular, *local classes* (defined within a block / method) and *anonymous classes* (defined within a method and unamed).
- You cannot declare a static field inside a local class. Similarly, you can't define an interface, since those are inherently static. (Except for compile-time constant expressions.)
 
``java
public class HelloWorldAnonymousClasses {
    interface HelloWorld {
        public void greet();
        public void greetSomeone(String someone);
    }
      public void sayHello() {
         class EnglishGreeting implements HelloWorld {
            String name = "world";
            public void greet() {
                greetSomeone("world");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Hello " + name);
            }
        }
        HelloWorld englishGreeting = new EnglishGreeting();
        // Anonymous class
        HelloWorld frenchGreeting = new HelloWorld() {
            String name = "tout le monde";
            public void greet() {
                greetSomeone("tout le monde");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Salut " + name);
            }
        };
}
``
 
- The anonymous class requires:
  - The *new* operator
  - The interface or class that it implements / extends
  - Parenthesis containing the constructor arguments
  - Body of the class declaration
 
  - Non-static nested classes have access to other members of the enclosing class (private or not). Static classes do not.
 
  ``java
  // Static Nested Class
  OuterClass.StaticNestedClass nestedObject = new OuterClass.StaticNestedClass();
  // Non-static Inner Class
  OuterClass.InnerClass innerObject = outerObject.new InnerClass();
  ``
 
- **Shadowing** refers to hiding a variable within a nested class by defining a new value in the current scope which masks the higher level scope.
 
- The following method shadows x and y. To access the class x/y values, use the qualified name (*this*)
 
``java
public class Circle {
    private int x, y, radius;
    public void setOrigin(int x, int y) {
        ...
    }
}
``
 
### Item 7: Determine the effect upon object references and primitive values when they are passed into methods that change the values.
 
**varargs**
 
``java
public Polygon polygonFrom(Point... corners) {}
public PrintStream printf(String format, Object... args) {}
``
**Value vs Reference**
 
- Primitives are always passed by value
- Object references are also passed by value
- Objects themselves are reference, so changing a field will also change the object.
 
``java
public void moveCircle(Circle circle, int deltaX, int deltaY) {
    // code to move origin of circle to x+deltaX, y+deltaY
    circle.setX(circle.getX() + deltaX);
    circle.setY(circle.getY() + deltaY);
       
    // code to assign a new reference to circle
    circle = new Circle(0, 0);
}
moveCircle(myCircle, 23, 56)
``
> Inside the method, circle initially refers to myCircle. The method changes the x and y coordinates of the object that circle references (i.e., myCircle) by 23 and 56, respectively. These changes will persist when the method returns. Then circle is assigned a reference to a new Circle object with x = y = 0. This reassignment has no permanence, however, because the reference was passed in by value and cannot change. Within the method, the object pointed to by circle has changed, but, when the method returns, myCircle still references the same Circle object as before the method was called.
 
# Section 7: Working with Inheritance
 
### Item 1: Implement inheritance.
 
- The *@Override* annotation tells the compiler you intend to overrride a method, so the compiler will inforce the superclass to contain said method or throw an error.
 
- Overriden static methods can still be invoked from the superclass object.
 
TBC: http://docs.oracle.com/javase/tutorial/java/IandI/override.html
 
### Item 2: Develop code that demonstrates the use of polymorphism.
 
### Item 3: Differentiate between the type of a reference and the type of an object.
 
### Item 4: Determine when casting is necessary.
 
``java
Object obj = new MountainBike();
MountainBike myBike = (MountainBike)obj;
// or
if (obj instanceof MountainBike) {
    MountainBike myBike = (MountainBike)obj;
}
``
 
### Item 5: Use super and this to access objects and constructors.
 
**super**
 
``java
public MountainBike(int startHeight,
                    int startCadence,
                    int startSpeed,
                    int startGear) {
    super(startCadence, startSpeed, startGear);
    seatHeight = startHeight;
}  
``
 
``java
public class Subclass extends Superclass {
    // overrides printMethod in Superclass
    public void printMethod() {
        super.printMethod();
        System.out.println("Printed in Subclass");
    }
    public static void main(String[] args) {
        Subclass s = new Subclass();
        s.printMethod();   
    }
}
``
 
**this**
 
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
 
### Item 6: Use abstract classes and interface
 
- Abstract classes are declared without implementation.
- Abstract classes cannot be instantiated but can be subclassed.
- Classes containing abstract methods must be declared abstrct themselves.
 
``java
abstract void moveTo(double deltaX, double deltaY);
// or
public abstract class GraphicObject {
   // declare fields
   // declare nonabstract methods
   abstract void draw();
}
``
 
``java
abstract class GraphicObject {
    int x, y;
    ...
    void moveTo(int newX, int newY) {
        ...
    }
    abstract void draw();
    abstract void resize();
}
//
class Circle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}
class Rectangle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}
``
 
**Interfaces**
 
- Interfaces are contracts for software interaction.
 
``java
public interface GroupedInterface extends Interface1, Interface2, Interface3 {
    // constant declarations
    // base of natural logarithms
    double E = 2.718282;
    // method signatures
    void doSomething (int i, double x);
    int doSomethingElse(String s);
}
``
TBC: http://docs.oracle.com/javase/tutorial/java/IandI/usinginterface.html

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

 # Notes from Exams

- flow control requires boolean types to be evaluated, so x/y will not compile. (includes ifs, whiles, etc.)
 
- enhanced loops can iterate over anything that implements the Collection Interface
 
- The default no-args constructor is created in a subclass w/out constructor even if the superclass implements constructors.
 
- A constructor inherits the access modifiers of its class.
 
- The method signature is the name + number and type of paramenters. Return type is not part of the signature.
 
- Constructors can be declared private (ie. Singletons). Construcors are non-static methods.
 
**Scope, References & Garbage Collection**
 
``java
public class TestClass{
  public void testRefs(String str, StringBuilder sb){
    str = str + sb.toString();
    sb.append(str);
    str = null;
    sb = null;
  }
  public static void main(String[] args){
    String s = "aaa";
    StringBuilder sb = new StringBuilder("bbb");
    new TestClass().testRefs(s, sb);
    System.out.println("s="+s+" sb="+sb);
  }
}
``
 
// The null assignments in testRefs does not affect the variables s and sb in the main method. ie. a="aaa" sb="bbbaaabbb"
 
- There is a subtle difference between: int[] i; and int i[]; although in both the cases, i is an array of integers. The difference is if you declare multiple variables in the same statement such as: int[] i, j; and int i[], j;, j is not of the same type in the two cases.
 
- Every class belongs to the no name package by default. This means they share the space with other no-name members but are not importable explicitly.
 
- java.lang gets imported automatically (contains String class)
 
- "." can be called an operator
 
- Strings must be enclosed in double quotes while chars are single quotes. 'a' vs "a"
 
- Remember that a labeled break or continue statement must always exist inside the loop where the label is declared. Here, if(j == 4) break POINT1; is a labelled break that is occurring in the second loop while the label POINT1 is declared for the first loop.
 
- The rule is that an overriding method cannot throw an exception that is a super class of the exception thrown by the overridden method.
 
- Java compiler recognizes the unreachable code in the loop and will refuse to compile.
 
- | evaluates both sides, || evaluates the second side only if if the first evaluates to true. || & && are short-circuit operators.
 
``java
for (int value : arr) {
           if (counter >= 5) {
                 break;
                } else {
            continue; 
               }    
        if (value > 4) {  
            arr[counter] = value + 1; 
               }    
           counter++;  
       }
 
 
- Evaluations occur left to right:
 
``java
class Test{
   public static void main(String[] args){
     int i = 4;
      int ia[][][] = new int[i][i = 3][i];
      System.out.println( ia.length + ", " + ia[0].length+", "+ ia[0][0].length);
   }
}
// ==> 4, 3, 3
``
 
- Operations with Strings are quite good at converting values: String s = 'b'+63+"a";
 
- To construct an instance of a sub class, its super class needs need to be constructed first. Since an instance can only be created via a constructor, some constructor of the super class has to be invoked. Either you explicitly call it or the compiler will add super() (i.e. no args constructor) as the first line of the sub class constructor.
 
- 9 is an int primitive, so new Short(9) is invalid. Use Short s = new Short( (short) 9 ).
 
- **instanceof** - left ar must be an object (not primitive) and right arg must be a class name
 
**Primitive Operator Casting**
 
Remember these rules for primitive types: 1. Anything bigger than an int can NEVER be assigned to an int or anything smaller than int ( byte, char, or short) without explicit cast. 2. CONSTANT values up to int can be assigned (without cast) to variables of lesser size ( for example, short to byte) if the value is representable by the variable.( that is, if it fits into the size of the variable). 3. operands of mathematical operators are ALWAYS promoted to AT LEAST int. (i.e. for byte * byte both bytes will be first promoted to int.) and the return value will be AT LEAST int. 4. Compound assignment operators ( +=, *= etc)  have strange ways so read this carefully:  A compound assignment expression of the form E1 op= E2 is equivalent to E1 = (T)((E1) op (E2)), where T is the type of E1, except that E1 is evaluated only once. Note that the implied cast to type T may be either an identity conversion or a narrowing primitive conversion.
 
- An int can be assigned to a variable of type int, long, float or double/
 
- Subclass overriding methods must return the same primitive type or covariant type of the parent's method.
 
- Casting applies to the first arg on the right. ie. "by" in (long) by/d*3;
 
- switch statements take only be String, byte, char, short, int, and enum values. It's lables must be compile time constants. The type of the input must be able to reach all case lables (memory-wise). Default label can occur in any order.
 
- final variables can't be changed, but they can be shadowed
 
- java.* doesn't import anaything, but is valid code.
 
- Trying to make a method more private in a subclass will cause a compile error. Remember:
private->'no modifier'->protected->public ( public is accessible to all and private is accessible to none except itself.)
 
- String methods include trim but does not include append.
 
- virtual calls mean calls are bound to a method at run time, not compile time.
 
- Both ternary evaluations must return the same type.
 
- Fields defined in an interface are ALWAYS considered as public, static, and final. (Methods are public and abstract.) Even if you don't explicitly define them as such. In fact, you cannot even declare a field to be private or protected in an interface. Therefore, you cannot assign any value to 'type' outside the interface definition.
 
- & and | accept integral (number) primitive pairs as well as boolean (unlike &&).
 
- All final variables must be initialized before the object is instanciated or it won't compile.
 
- Abstract Classes
  - A class is uninstantiable if the class is declared abstract.
  - If a method has been declared as abstract, it cannot provide an implementation i.e. a method body even if empty (and the class containing that method must be declared abstract).
  - If a method is not declared abstract, it must provide a method body (the class can be abstract but not necessarily so).
  - If any method in a class is declared abstract, then the whole class must be declared abstract.
 
- Interfaces
  - Every interface is implicitly abstract. This modifier is obsolete for interfaces and should not be used in new Java programs.
  - An interface can extend any number of other interfaces and can be extended by any number of other interfaces
  - Every field declaration in the body of an interface is implicitly public, static and final. It is permitted, but strongly discouraged as a matter of style, to redundantly specify any or all of these modifiers for such fields. A constant declaration in an interface must not include any of the modifiers synchronized, transient or volatile, or a compile-time error occurs.
  - It is possible for an interface to inherit more than one field with the same name. Such a situation does not in itself cause a compile-time error. However, any attempt within the body of the interface to refer to either field by its simple name will result in a compile-time error, because such a reference is ambiguous.
  - Every method declaration in the body of an interface is implicitly public and abstract, so its body is always represented by a semicolon, not a block.
  - A method in an interface cannot be declared static, because in Java static methods cannot be abstract.
  - A method in an interface cannot be declared native or synchronized, or a compile-time error occurs, because those keywords describe implementation properties rather than interface properties. However, a method declared in an interface may be implemented by a method that is declared native or synchronized in a class that implements the interface.
 
- Overriding Methods
  - A method can be overridden by defining a method with the same signature(i.e. name and parameter list) and return type as the method in a superclass. The return type can also be a subclass of the orginal method's return type.
  - Only methods that are accessible can be overridden. A private method cannot, therefore, be overridden in subclasses, but the subclasses are allowed to define a new method with exactly the same signature.
  - A final method cannot be overridden.
  - An overriding method cannot exhibit behavior that contradicts the declaration of the original method. An overriding method therefore cannot return a different type (except a subtype) or throw a wider spectrum of exceptions than the original method in the superclass.
  - A subclass may have a static method with the same signature as a static method in the base class but it is not called overriding. It is called shadowing because the concept of polymorphism doesn't apply to static members.
 
 
 
**Common Exceptions**
 
- Errors
  - AssertionError
- Exception
  - IOException
    - FileNotFoundException
  - RuntimeException
    - IndexOutOfBoundsException
      - StringIndexOutOfBoundsException
      - ArrayIndexOutOfBoundsException
    - IllegalArgumentException
    - IllegalStateException
    - ClassCastException
    - NullPointerException
    - SecurityException
 
 
- Exceptions
  - The terminology "thrown by the JVM" and "thrown programatically or by the application" is not precise but is used by popular books. If it helps, you can think of the exception categories as "thrown implicitly" and "thrown explicitly". An exception that is thrown even when there is no throw statement, is said to be thrown implicitly. For example, calling a method on null will cause a NullPointerException to be thrown automatically, even though there is no throw statement. On the other hand, a code may throw an exception explicitly by using the throw statement. For example, a method code might check an argument for validity and if it finds the argument inappropriate, it may throw an exception by executing throw new IllegalArgumentException();.
  - Errors are always thrown only by the JVM
 
- You cannot statically import package members, only static members of a class (wildards are fine).
 
- String, StringBuilder, and StringBuffer - all are final classes.
 
- Remember that wrapper classes (java.lang.Boolean, java.lang.Integer, java.lang.Long, java.lang.Short etc.) are also final and so they cannot be extended.
 
- java.lang.Number, however, is not final. Integer, Long, Double etc. extend Number.
 
- java.lang.System is final as well.
 
- Order of declarations:
  - First, static statements/blocks are called IN THE ORDER they are defined.
  - Next, instance initializer statements/blocks are called IN THE ORDER they are defined.
  - Finally, the constructor is called. So, it prints a b c 2 3 4 1
 
- .sublist(1,1) returns an empty list.
 
- The Exception that is thrown in the last, is the Exception that is thrown by the method to the caller.
So, when no exception or any exception is thrown at line 1, the control goes to finally or some catch block. Now, even if the catch blocks throws some exception, the control goes to finally.
 
 
 
**Packages**
 
- java.lang
  - String
- java.util
  - ArrayList
 
 
- Variables are shadowed and methods are overriden. The following code returns 8 Accelerate : Sportscar - the parents variable with the childs method on account of the Car reference type:
 
``java
class Car{
   public int gearRatio = 8;
   public String accelerate() {  return "Accelerate : Car";  }
}
class SportsCar extends Car{
   public int gearRatio = 9;
   public String accelerate() {  return  "Accelerate : SportsCar";  }
   public static void main(String[] args){
      Car c = new SportsCar();
      System.out.println( c.gearRatio+"  "+c.accelerate() );
   }
}
``
 
- **Class Declaration**
  - main method should be static
  - main method must be public
  - the class name must have the file name
  - the main method arg must be a array of strings
 
- null has Object covariant type?
 
- You cannot call class variables from the main method without referencing the object (they're not global vars)
 
- Objects default to null value (this includes Strings)
 
- THE LOOP CONDITION MUST EVALUATE TO BOOLEAN TYPE
 
- Which is these are valid?
 
``java
1. for (int i=5; i=0; i--) { }
2.  int j=5;
      for(int i=0, j+=5;  i<j ; i++) {  j--;  }
3. int i, j;
    for (j=10; i<j; j--) { i += 2; }
4. int i=10;
    for ( ; i>0 ; i--) { }
5. for (int i=0, j=10; i<j; i++, --j) {;}
``
 
4 & 5
 
No 1.
uses '=' instead of '==' for condition which is invalid. The loop condition must be of type boolean.
 
No 2.
uses 'j +=5'. Now, this statement is preceded by 'int i=0,' and that means we are trying to declare variable j. But it is already declared before the for loop. If we remove the int in the initialization part and declare i before the loop then it will work. But if we remove the statement int j = 5; it will not work because compiler will try to do j = j+5 and you can't use the variable before it is initialized. Although the compiler gives a message 'Invalid declaration' for j += 5 but it really means the above mentioned thing.
 
No 3. i is uninitialized.
 
No 4. is valid. It contains empty initialization part.
 
No 5.
This is perfectly valid. You can have any number of comma separated statements in initialization and incrementation part. The condition part must contain a single expression that returns a boolean.
All a for loop needs is two semi colons :-
for( ; ; ) {} This is a valid for loop that never ends. A more concise form for the same is : for( ; ; );
 
``java
package loops; public class JustLooping {     private int j;     void showJ(){         while(j<=5){             for(int j=1; j <= 5;){                 System.out.print(j+" ");                 j++;             }             j++;         }     }     public static void main(String[] args) {         new JustLooping().showJ();     } }
// prints 1 2 3 4 5 siz times.
``
 
 
 
- Local variables are not initialized by default. We have to explicitly initialize local variables other wise they remain uninitialized and it will be a compile time error if such variables are accessed without getting initialized.
Instance variables and static variables receive  a default value if not explicitly initialized. All primitive types get a defaults value equivalent to 0. (eg. int to 0 and float to 0.0f etc) and boolean to false. The type/class of a variable does not affect whether a variable is initialized or not.
 
- Whether a call needs to be wrapped in a try/catch or whether the enclosing method requires a throws clause depends on the class of the reference and not the class of the actual object.
 
-  Within an instance method, you can access the current object of the same class using 'this'
 
- Variables defined in the try block are not visible from the catch block (will not compile)
 
- public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
 
- Encapsulation means that the internal representation of an object is generally hidden from view outside of the object's definition.

 