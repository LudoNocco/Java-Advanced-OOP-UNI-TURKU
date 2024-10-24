# EXERCISE 3: comparing challenge1 and challenge2
## PART A: main feature

The main feature here is “Generics with multiple bounds”. In challenge1, the method only accepts a Bird type as a parameter. But in challenge2, it uses a generic type X that can be any object, as long as it implements both the Winged and Bipedal interfaces. This means challenge1 can only work with birds, but challenge2 is more flexible and can work with any object that can both fly and walk, even if it’s not a bird.
## PART B: differences between challenge1 and challenge2
The difference between these methods is in their flexibility. challenge1 is limited to Bird objects or their subclasses. On the other hand, challenge2 can accept any object that implements both the Winged and Bipedal interfaces, regardless of its class. Even though both methods perform the same actions (flying and walking), challenge2 can be used with more types of objects, making it more versatile.
## PART C: Penguin example to show the advantages of challenge2 over challenge1
```java
class Penguin extends Bird {
    @Override
    public String toString() {
        return "Penguin";
    }

    @Override
    public void fly() {
        System.out.println(this + " can't fly, but it tries!");
    }
}

public class Exercise3 {
    public Exercise3() {
        System.out.println("Exercise 3");

        challenge1(new Crow());  // Works with Crow
        challenge2(new Crow());  // Works with Crow

        challenge2(new Penguin());  // Works with Penguin
        // challenge1(new Penguin());  // Doesn't work, because it's limited to Bird type
    }

    void challenge1(Bird b) {
        System.out.println("In this challenge, we fly and then we walk!");
        b.fly();
        b.walk();
    }

    <X extends Winged & Bipedal> void challenge2(X b) {
        System.out.println("In this challenge, we fly and then we walk!");
        b.fly();
        b.walk();
    }

    public static void main(String[] args) {
        new Exercise3();
    }
}
```
In this example we see in practice how challenge1 is limited to Bird objects, so it can’t accept other Winged and Bipedal animals like Penguin. Here is where we can appreciate the flexibility of challenge2 afforded by the fact that it can work with any object that can both walk and fly, like Crow and Penguin. Even though we know that a Penguin can't really fly, it is nevertheless an animal that is at the same time winged and bipedal and so it should be allowed to implement both Winged and Bipedal, which we can only do by using challenge2.
