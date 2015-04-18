# Java Primer

----

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

----

## Section 2: Working with Java Data Types

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

## Section 3: Using Operators and Decision Constructs

## Section 4: Creating and Using Arrays

## Section 5: Using Loop Constructs

## Section 6: Working with Methods and Encapsulation

## Section 7: Working with Inheritance

## Section 8: Handling Exceptions