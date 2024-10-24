# EXERCISE 1: Zipper and TestZipper
## PART A: identify and describe each class 
### Exercise1 Class (Exercise1.java)
A regular class with a constructor. It shows how to use the TestZipper class by creating an instance and running it within a block that ensures resources (like files) are properly managed when the program is done with them.
### TestZipper Class (TestZipper.java)
This is a specific class that extends another class called Zipper and uses a feature called an anonymous inner class. TestZipper gives a concrete implementation of the createHandler method. Inside this method, it creates an unnamed class (anonymous inner class) that processes files.
### Zipper Class (Zipper.java)
This class is abstract, which means it provides a basic structure but leaves some methods for other classes to fill in. It also uses an interface (AutoCloseable) to handle resource cleanup. Zipper defines a special inner class (Handler) that acts as a template for managing files. This Handler class is abstract and needs to be completed by subclasses.
### Zipper.Handler Inner Class
This is an inner class inside Zipper, designed to process files after they are unzipped. It is protected and abstract, meaning subclasses can access and use it, but they must define how it works. The fact that it's static means it doesn’t rely on an instance of Zipper to work.
### Anonymous Inner Class in TestZipper
In the TestZipper class, an anonymous inner class extends Handler to give specific instructions on how to process files. This unnamed class is used directly within the createHandler method, making it quick to implement without needing to define a separate class.
## PART B: Why was this structure used?
### Exercise1 Class
A regular class with a constructor works well to start the program. It organises the code clearly and keeps things structured. We could use a static method or a main method instead, but using an instance method is more flexible and reusable.
### TestZipper Class
By extending Zipper, TestZipper benefits from (i.e. inherits) the common functionality provided by the parent class while adding its own features. This approach uses inheritance, method overriding, and anonymous inner classes to keep the code short. We could use a named inner class instead of an anonymous one, but that would just add extra and potentially unnecessary code.
### Zipper Class
An abstract class is perfect here because it offers a shared base for subclasses while forcing them to implement certain methods. Using AutoCloseable makes it so that resources are cleaned up automatically. An interface wouldn’t work because it can’t share the logic, and a regular class wouldn’t force the implementation of the methods.
### Zipper.Handler Inner Class
This inner class is protected and static, making it accessible to subclasses but independent of the outer Zipper class. We could use a top-level class here, but it would expose more of the class to other parts of the program (i.e. the client), making the implementation less safe and more open to potential bugs.
### Anonymous Inner Class in TestZipper
This type of class provides a fast way to implement the required handle method without having to create a whole new class. It’s efficient because really we only need the class in this one place. A named class could be an alternative, but I don’t see it as a valuable trade-off.

## PART C: temporary folder
The temporary folder is created by the Zipper class when we make a new Zipper object. This happens inside the Zipper constructor (Zipper(String zipFile)). The folder gets deleted when the close() method is called, which happens automatically when the program finishes using Zipper because of how AutoCloseable works. This means the folder exists only while we are using the Zipper object and is cleaned up right after, so no files are left behind.

## PART D: Class Handler and the use of Static
The Handler class can only be used by other classes that extend Zipper, keeping it hidden from the client. The word "abstract" means that other classes must complete its design by adding a handle() method. The word "static" means Handler can be used without needing to create a Zipper object. This is good because Handler doesn’t need anything from the outer Zipper class, and making it static helps avoid problems like using too much memory.
