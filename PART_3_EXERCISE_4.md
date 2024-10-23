# EXERCISE 4: Choosing the most appropriate data structures

## PART A
The most suitable class construction for these requirements would be a Java record, for the following reasons:
* Immutability: records don’t let you change their data once they’re created. This is good for making sure the data stays consistent.
* Features that make the code more concise: records automatically create useful methods like:
  * equals(): which checks if two records are the same.
  * hashCode(): which helps store records in data structures like hash maps.
  * toString(): which turns the record into a readable string .
Given the above, a record can easily store both the temperature value (like 20.0) and the scale (like Celsius or Fahrenheit).

I would not use:
* Classes, because to get the immutability functionality, you have to write a lot of extra code yourself.
* Interfaces or Abstract Classes: because they are more like templates and don’t hold actual data themselves.
* Enums: because they are good for data that’s inherently immutable (like days of the week, as we saw in one of the exercises for the 05.10 final Exam of the Basic course) but are not appropriate for storing data that’s subject to changes and fluctuations, as is the case for temperature values. However, enums could be used to store the type of scale (Farenheit, Celsius or Kelvin), but given it’s only three of them, I don’t think the trade off of implementing two different data structures would be worth it.

## PART B
A regular class would be suitable for representing diseases, for the following reasons:
* Flexibility: we could create instances of the class as new diseases are discovered.
* Attributes: we would be able to encapsulate name and symptoms, with methods operating on Human objects.

I would rule out the use of:
* Enums: as they wouldn’t be suitable due to the dynamic nature of diseases, which are constantly evolving and changing;
* Interfaces/Abstract Classes: since interfaces and abstract classes only provide templates or definitions (e.g., method signatures, but not full implementations), they wouldn't suffice because we need to instantiate actual disease objects that represent specific diseases. In other words, using an interface or abstract class would limit us to defining "what a disease should look like", but we wouldn't be able to directly create new disease objects without further subclassing and defining concrete details.

## PART C
I would use a Java record to represent each row from the database, for the following reasons:
* Immutability: Java records are immutable by default, meaning that once a record is created, its fields cannot be changed. This immutability is ideal for data structures like database records, where the data should remain consistent and unchanged once retrieved.
* Convenience: records automatically generate key methods like toString(), equals(), hashCode(), and a constructor, without the need to write extra boilerplate code. This is particularly useful when working with simple data structures, as it reduces the amount of repetitive code we would need to write in a traditional class. Records allow us to focus on the essential logic rather than manually implementing these methods.
* Clear structure: the fields (Name, Postal Address, Email Address, Degree Program, Number of Credits) are defined directly in the record's declaration, making the code easy to understand and work with. It enforces a clear structure for the data, which is beneficial when dealing with database queries that return fixed sets of values.

I would rule out the following options:
* Classes: a regular class would require writing a lot of boilerplate code (e.g., getters, setters, constructors, toString, equals, and hashCode methods) just to represent simple data. For this task, which involves handling simple records (student details from the database), this extra effort is unnecessary. Since Java records automatically provide these functionalities, they are a more concise and efficient choice for representing data.
* Enums: these are used to represent a fixed set of constants (like days of the week, status codes, etc.), but they are not suitable for data that can vary. In this case, the student data (name, address, credits, etc.) is dynamic and can change frequently, which makes enums unsuitable because enums are inherently static and not designed to represent variable content like student records.
* Interfaces / Abstract Classes: these are meant to define common behaviour or structures that can be shared across multiple implementations. However, they are not appropriate for storing actual data since they only provide method signatures (interfaces) or partial implementations (abstract classes). In this case, the need is for concrete, immutable data structures that represent actual student records, not just a blueprint for how student records should behave.

## PART D
I would achieve the requirements by using Java's functional interfaces and streams, for the following reasons:
* Reusability: by defining the filtering logic in a “Predicate (hasEnoughCredits)”, the logic becomes reusable across multiple places in the program. A Predicate is a functional interface provided by Java that represents a boolean-valued function, making it a great fit for filters like this where a condition has to be met. We can use hasEnoughCredits not only in this specific filtering operation but also in other parts of the code where similar filtering is needed (for example, for a different student list or even other types of data with a similar requirement). This would be similar to Python’s functional capabilities using lambda expressions, even though lambda expressions can return any type of value, not just boolean like in this case, but the functional programming principles and advantages are very similar.
* Conciseness: streams allow for a more declarative approach to working with collections. Instead of focusing on the how (like a for-loop does), we focus on what you want to achieve: filtering out students with less than 280 credits. This results in cleaner, more readable code that will be easier to maintain.

I would not employ the following options:
* For-loops: while they can achieve the same result, they are more verbose and require more manual steps (initializing the loop, writing the condition, updating the index, manually adding matching rows to a new list). This can lead to more boilerplate and repetitive code, especially if we need to reuse this logic in multiple places. The use of for-loops also makes the filtering process less declarative and harder to read, as the reader has to mentally parse the loop's steps to understand its purpose.
* Custom interfaces: these would be unnecessary when Java already provides a functional interface like Predicate in the java.util.function package. By using built-in functional interfaces, the code is simpler, more consistent with Java's standard library, and easier for other developers to understand and maintain. Custom interfaces would add unnecessary complexity and reduce the benefits of leveraging Java's own functional programming capabilities.

## PART E
### First point definition (with int [ ] values array):
This Point class uses an array (int [ ] values) to store the x and y coordinates. Even though the values array is marked as final, the contents of the array can still be changed after the Point is created. If multiple parts of a program (or "clients") are sharing the same Point object, they can accidentally modify the x or y values by changing the array directly. This can cause unexpected changes and errors. Considering this, this design isn't very safe because it allows the point to be modified after creation, which can lead to bugs.

### Second point definition (immutable record with int x, int y):
This is a record in Java, which makes the x and y values immutable—they can’t be changed once the Point is created. Since it's immutable, this Point is safe to share between different parts of your program, as no one can accidentally modify its values, so it’s more reliable and predictable. This is ideal for representing data that shouldn’t change, like a fixed point on a map. It's easier to work with because there’s no risk of unexpected changes.

### Third point definition (record with mutable Number objects):

This Point class looks immutable from the outside, but it actually uses a custom Number class for x and y, and the value inside each Number can be changed. This can be confusing, as the reader might expect the Point to be unchangeable, but if multiple parts of the program share the same Point, changes to x or y (through Number.value) will unexpectedly affect the point’s coordinates. This could cause bugs because the behaviour isn’t obvious. It’s not a great design for something that’s supposed to be immutable because it behaves in a surprising way. It looks safe to share, but it’s not, which could lead to confusion and errors.
