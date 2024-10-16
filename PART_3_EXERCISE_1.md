# EXERCISE 1: CommandLineApp

## PART A 
This is a case of single implementation inheritance.
The class LoginScreen extends the abstract class CommandLineApp and via specialization inheritance, it inherits its methods and properties. 
It is also a more detailed and specific version of CommandLineApp, adding specialised behaviour by using the lock() method. 
This method is a simple authentication mechanism that works by continuously prompting the user for a username and password until the correct combination of user name and password is entered. 

## PART B
### DOES IT WORK / NOT WORK?
The code does NOT work because the NaturalResource interface is not being implemented in the correct way by the Hydroelectric class.
I see two main issues, mainly with access modifiers and exception handling in the “spend(float amount) method”:
* Exception handling Issue: the method declares throws Exception, but it does not specify any particular checked exceptions. In Java, an overriding method is not allowed to throw a more general checked exception than the one declared by the method it overrides.
* Access modifiers: the abovementioned method is declared as “private”, but by default all methods in an interface are public. Making class implementations as “private” violates the interface contract.
* Breaking polymorphism: linked to the access modifier issue above, this code is currently breaking polymorphism principles. Polymorphism shoud allow objects of different classes that share a common interface or superclass to be treated as objects of that common type. However, here this rule is broken because you cannot use the spend method consistently across the different NaturalResource implementations (e.g. “Coal” would allow the method spend to be called, but “Hydroelectric” would not.

### BENEFITS AND DRAWBACKS
#### Benefits:
From a conceptual standpoint, the intention behind the code is reasonable, as it tries to model a renewable natural resource, which by default is inherently limitless, meaning that it cannot be depleted.
#### Drawbacks:
As mentioned previously, the incorrect implementation violates the principles of polymorphism, interface implementation and exception handling, which will lead to compilation errors.
#### Potential solution:
The solution could be to provide a public implementation of the spend method in the Hydroelectric class, without declaring any checked exceptions. If spending is not applicable, we can throw an unchecked exception.
```java
      class Hydroelectric implements NaturalResource {
          private float left = 100;
      
          @Override
          public float amountLeft() {
              return left;
          }
      
          @Override
          public void spend(float amount) {
              throw new UnsupportedOperationException("Renewable resource cannot be spent.");
          }
      }
```

## PART C

### WHY DOES IT WORK / NOT WORK? 
This code works for two main reasons linked to “covariant return types” and correct “method overriding”.
* Covariant return types: when a subclass overrides a method from a parent class, it can change the method's return type to a more specific type. In this case, the generate() method in RandomGenerator returns a very generic Object type. In the RandomIntegerGenerator class, it appears that we need to return an Integer, so to achieve this, the overridden generate() method is allowed to return a different type of value (Integer) which is more specific than Object and better suited to our needs.

* Method overriding: as mentioned above, the generate() method in RandomIntegerGenerator replaces the version from RandomGenerator (the parent class). This means that when you call generate() on an object of RandomIntegerGenerator, it will use the version that returns an Integer, even though the original method in the parent class is set to return an Object.
### BENEFITS AND DRAWBACKS
#### Benefits:
The main benefits lie in providing “type safety” and “specialization”. As for type safety, it means that we can work with more specific integer types without the need for casting. As for specialisation, the child class RandomIntegerGenerator provides us with a more specialised version of the generate() method.
#### Drawbacks:
I think that the code poses a limitation in terms of polymorphism. polymorphism makes you give up the more precise behaviour of the child class (RandomIntegerGenerator) and forces you to interact with it as if it were the more general parent class (RandomGenerator). This reduces the convenience and type safety that you would otherwise have by using RandomIntegerGenerator directly. 
Imagine you have a class called RandomIntegerGenerator that has a generate() method. This method is meant to give you an Integer directly. However, if you treat that same RandomIntegerGenerator object like it’s just a RandomGenerator (or in other words you use it through a reference of type RandomGenerator); then when you call generate(), Java thinks it’s supposed to give you an Object instead of an Integer.
So even though RandomIntegerGenerator knows it’s supposed to give you an Integer, by treating it like a RandomGenerator, you lose that guarantee. Now, you might need to manually check or convert the result back to an Integer, which makes things more labour intensive.
