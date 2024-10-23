# EXERCISE 2: Error handling, exceptions and printer exercise

## PART A
Inheritance and polymorphism principles are employed in this code in the ways that the exception classes are organised (also know as exception hierarchy) and the ways in which they are handled (exception handling).
* Inheritance principles are employed in the exception hierarchy: “Problem” is an abstract class that extends “Exception”. Being abstract means that we can't create instances of Problem directly, but other classes can inherit from it. This is exactly what happens with “WeirdProblem” and “TrickyProblem”, as both of these classes extend Problem, meaning they inherit from “Problem” and are considered more specific types of “Problem”.
* Polymorphism is employed in the exception handling blocks, meaning that this program can handle different types of errors in a flexible way. The catch Blocks that are implemented throughout are like safety nets that catch specific types of errors when they happen. More specifically:
  * The program first tries to catch WeirdProblem specifically.
  * If it's not that, it tries to catch TrickyProblem.
  * If it's neither, it then catches a general Problem (which includes both of the abovementioned types).

This shows polymorphism because the program can treat a “WeirdProblem” either as itself or as a more general Problem. The catch blocks act like filters that handle different error types, from the most specific to the most general. This makes the program flexible, allowing it to respond flexibly to each type of problem.

## PART B
### COMPARISON OF PRINTER1 AND PRINTER2:
#### Printer1: uses inheritance
“Printer1” directly extends the “Fancy” class by inheriting from it. “Printer1” also inherits behaviour from the parent class, given that it automatically gets the “decorate()” method from the Fancy class.
If we wanted Printer1 to use a different decoration style (maybe adding "~~ " instead), you’d need to change its parent from Fancy to another class. This is less flexible because you can’t change what it inherits while the program is running; it’s decided when you write the code.

#### Printer2: uses composition
Contrarily, “Printer2” doesn’t inherit from “Fancy”. Instead, it holds a “Fancy” object inside it and uses that to decorate. By delegating behaviour, when the method “decorate()” is called, “Printer2” asks the Fancy object to do the decoration. 
Here it is easier to change the decoration style because we can replace the “Fancy” object with another type of decorator and we are even able to do that while the program is running, making it more flexible.

So, in conclusion, Printer1 is like a child that directly takes traits from its parent (“Fancy”). To change those traits, you need to change its parent. “Printer2” is like a person who hires different decorators. It can easily switch decorators to change how things look without needing to change itself.

### EXTENDING FUNCTIONALITY IN “PRINTER3”:
#### OPTION 1: DASH DECORATOR CLASS
If our goal is to add “--” to the start and end of a decorated text, we could create a new class called “DashDecorator” that can add “--” before and after any decorated text. This means that we don’t change the original decoration; we just wrap it with an extra decoration.

Hence, the newly created “DashDecorator Class” would wrap around another decorator (like “Fancy”) to add the extra decoration. When we give it some text, it first decorates it using the existing decorator (e.g., “Fancy” adds "=="), then, it adds “--” at the beginning and end.

#### OPTION 2: ADDING METHOD TO DECORATOR INTERFACE
Option 1 above works as long as the number and type of characters that we need for the extra decoration stay the same. However, if we need to handle a different number of characters of different types, then we would need to add a new method to the Decorator interface.
This method would let each decorator determine how many “--” characters it wants to use. For example, DashDecorator could say, “I use 2 – characters.” This way, each decorator controls its own style. If we place this logic inside the Printer interface, then we can make Printer decide how many “--” characters to use. This keeps the decoration logic separate from the Printer interface.
This is less ideal because Printer should just print the text without worrying about how many characters to add. It also mixes two separate behaviours, namely decorating and printing, which can make the code less readable and more difficult to understand; as one would expect these to be two separate tasks.
