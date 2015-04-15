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

Continues at http://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

### Item 2: Define the structure of Java class

- There are three types of comments:
  - /* multiline */
  - /** multiline that gets picked up by javadoc */
  - // singleline

Continues at http://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html


### Item 3: Create executable Java applications with a main method




----

## Section 2: Working with Java Data Types

## Section 3: Using Operators and Decision Constructs

## Section 4: Creating and Using Arrays

## Section 5: Using Loop Constructs

## Section 6: Working with Methods and Encapsulation

## Section 7: Working with Inheritance

## Section 8: Handling Exceptions