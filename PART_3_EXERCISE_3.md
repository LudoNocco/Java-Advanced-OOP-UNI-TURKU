# EXERCISE 3: Shapes and operations

## DESIGN DESCRIPTION
To address the issues in the provided program and make it more maintainable, extensible, and aligned with object-oriented principles, we can refactor the code using inheritance, polymorphism, and encapsulation.  We can create an abstract base class Shape that defines the common behaviour for all shapes. The class will include abstract methods for operations like area() and boundaries(). We can then create specific and concrete classes for each shape (Triangle, Rectangle, Circle) that extend Shape and each class will implement the abstract methods from Shape. In other words, each specific shape class will encapsulate the data and behaviour of the abstract base class. We could use polymorphism to treat all shapes in the same way. This allows us to store different shapes in a single collection and perform operations without knowing the exact type of shape.

## IMPLEMENTATION

### STEP 1: CREATING THE BASE CLASS SHAPE

```java
public abstract class Shape {
    public abstract double area();
    public abstract Point[] boundaries();
}
```
Here, the area() method calculates the area of the shape and the boundaries() method returns an array of Point objects representing the bounding rectangle of the shape.

### STEP 2: CREATING THE CLASSES THAT EXTEND THE BASE CLASS: TRIANGLE, RECTANGLE AND CIRCLE

```java
public class Triangle extends Shape {
    private final Point p1, p2, p3;

    public Triangle(Point p1, Point p2, Point p3) {
        this.p1 = p1;
        this.p2 = p2;
        this.p3 = p3;
    }

    @Override
    public double area() {
        return Math.abs(
            (p1.x() * (p2.y() - p3.y()) +
             p2.x() * (p3.y() - p1.y()) +
             p3.x() * (p1.y() - p2.y())) / 2.0
        );
    }

    @Override
    public Point[] boundaries() {
        int xMin = Math.min(p1.x(), Math.min(p2.x(), p3.x()));
        int xMax = Math.max(p1.x(), Math.max(p2.x(), p3.x()));
        int yMin = Math.min(p1.y(), Math.min(p2.y(), p3.y()));
        int yMax = Math.max(p1.y(), Math.max(p2.y(), p3.y()));
        return new Point[]{new Point(xMin, yMin), new Point(xMax, yMax)};
    }
}

public class Rectangle extends Shape {
    private final Point p1, p2;

    public Rectangle(Point p1, Point p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    @Override
    public double area() {
        return Math.abs((p1.x() - p2.x()) * (p1.y() - p2.y()));
    }

    @Override
    public Point[] boundaries() {
        int xMin = Math.min(p1.x(), p2.x());
        int xMax = Math.max(p1.x(), p2.x());
        int yMin = Math.min(p1.y(), p2.y());
        int yMax = Math.max(p1.y(), p2.y());
        return new Point[]{new Point(xMin, yMin), new Point(xMax, yMax)};
    }
}

public class Circle extends Shape {
    private final Point center;
    private final double radius;

    public Circle(Point center, Point perimeterPoint) {
        this.center = center;
        this.radius = Math.sqrt(
            Math.pow(center.x() - perimeterPoint.x(), 2) +
            Math.pow(center.y() - perimeterPoint.y(), 2)
        );
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }

    @Override
    public Point[] boundaries() {
        int xMin = (int) Math.floor(center.x() - radius);
        int xMax = (int) Math.ceil(center.x() + radius);
        int yMin = (int) Math.floor(center.y() - radius);
        int yMax = (int) Math.ceil(center.y() + radius);
        return new Point[]{new Point(xMin, yMin), new Point(xMax, yMax)};
    }
}
```

### STEP 3: UPDATING THE REST OF THE PROGRAM IN LIGHT OF THE NEW DESIGN
Lastly, we can refactor the main program to utilize our new structure:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Exercise3 {
    public static void main(String[] args) {
        System.out.println("Exercise 3");
        System.out.println("A circle is defined by a centre and a perimeter point; others by corner points.");

        Scanner scanner = new Scanner(System.in);
        List<Shape> shapes = new ArrayList<>();

        System.out.print("How many shapes do you want to enter? ");
        int numShapes = scanner.nextInt();

        for (int i = 0; i < numShapes; i++) {
            Shape shape = readShape(scanner);
            if (shape != null) {
                shapes.add(shape);
            }
        }

        double totalArea = 0;
        for (Shape shape : shapes) {
            totalArea += shape.area();
        }
        System.out.printf("Sum of area covered by the patterns:\n%f\n\n", totalArea);

        if (!shapes.isEmpty()) {
            Point[] initialBounds = shapes.get(0).boundaries();
            int xMin = initialBounds[0].x();
            int yMin = initialBounds[0].y();
            int xMax = initialBounds[1].x();
            int yMax = initialBounds[1].y();

            for (Shape shape : shapes) {
                Point[] bounds = shape.boundaries();
                xMin = Math.min(xMin, bounds[0].x());
                yMin = Math.min(yMin, bounds[0].y());
                xMax = Math.max(xMax, bounds[1].x());
                yMax = Math.max(yMax, bounds[1].y());
            }

            System.out.printf("The common boundaries of the patterns:\n(%d, %d) x (%d, %d)\n\n",
                    xMin, yMin, xMax, yMax);
        } else {
            System.out.println("No shapes entered.");
        }
    }

    static Shape readShape(Scanner scanner) {
        System.out.print("Enter the pattern type (triangle, quadrilateral, circle): ");
        String type = scanner.next().toLowerCase();
        switch (type) {
            case "triangle":
                System.out.println("Enter the three corner points of the triangle:");
                Point t1 = readPoint(scanner);
                Point t2 = readPoint(scanner);
                Point t3 = readPoint(scanner);
                return new Triangle(t1, t2, t3);
            case "quadrilateral":
                System.out.println("Enter the two opposite corner points of the rectangle:");
                Point q1 = readPoint(scanner);
                Point q2 = readPoint(scanner);
                return new Rectangle(q1, q2);
            case "circle":
                System.out.println("Enter the center point and a point on the perimeter of the circle:");
                Point c1 = readPoint(scanner);
                Point c2 = readPoint(scanner);
                return new Circle(c1, c2);
            default:
                System.out.println("Unknown shape type.");
                return null;
        }
    }

    static Point readPoint(Scanner scanner) {
        System.out.print("Enter the x-coordinate of the point: ");
        int x = scanner.nextInt();
        System.out.print("Enter the y-coordinate of the point: ");
        int y = scanner.nextInt();
        return new Point(x, y);
    }
}
```

## WHY THIS IMPLEMENTATION?

### DESIGN
By defining an abstract class Shape, we establish a common contract for all shapes. This means every shape must implement the area() and boundaries() methods. This makes it easy to add new shapes in the future. For example, to add a Pentagon, we'd create a new class Pentagon that extends Shape and implements the required methods. This solution can also handle errors more easily: using inheritance and polymorphism allows the compiler to enforce contracts and catch errors.

### POLYMORPHISM
Using polymorphism, we can store different types of shapes in a single List<Shape>. This allows us to iterate over all shapes and perform operations without knowing their specific types. When we call shape.area(), the correct method is invoked based on the actual object's class (e.g., Triangle, Circle).

### ENCAPSULATION
Each shape class encapsulates its own data (like points and radius) and behaviour (calculations specific to that shape). This modularity makes the code easier to maintain and understand.

### “SEPARATION OF CONCERNS”

In the Pyhon MOOC taught by Erkki Kaila, in part 10 I learned about “separation of concerns”. In this solution, I applied this principles, as adding new shapes or operations will not require us to modify the existing code, adhering to the Open/Closed Principle of software design.

### HOW TO USE THIS IMPLEMENTATION

To add a new shape, create a class that extends Shape and implement the area() and boundaries() methods. Update the readShape() method to handle user input for the new shape. If we wanted to support more operations (like calculating the perimeter), we can add new abstract methods to the Shape class. Each shape class would then implement these new methods as appropriate.
