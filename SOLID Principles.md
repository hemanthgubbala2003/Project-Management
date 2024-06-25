**Single Responsibility Principle(SRP):**

__A class or package or component should have one and only one reason to change._
_Note: No no to monolithic classes and yes to breaking down responsibility, allows for easy testing, lower coupling and better organization.__

Example:

Bad example: The UserService class handles both user authentication and email notifications
```
public class UserService {
    public void authenticate(String username, String password) {
        // Authentication logic
    }

    public void sendWelcomeEmail(String email) {
        // Email sending logic
    }
}
```
Good example: Separate classes for authentication and email notifications
```
public class AuthService {
    public void authenticate(String username, String password) {
        // Authentication logic
    }
}

public class EmailService {
    public void sendWelcomeEmail(String email) {
        // Email sending logic
    }
}

```

**Open/Closed Principle(abstraction):**

_Software entities should be open for extension but closed for modification.
Note: I think it is asking for abstraction. To create loosely coupled systems._

Bad example: Modifying a class to add new functionality
```
public class Rectangle {
    public double width;
    public double height;
}

public class AreaCalculator {
    public double calculateRectangleArea(Rectangle rectangle) {
        return rectangle.width * rectangle.height;
    }

    // Adding a new shape requires modifying this class
    public double calculateCircleArea(Circle circle) {
        return Math.PI * circle.radius * circle.radius;
    }
}
```
Good example: Using polymorphism to extend functionality
```
public abstract class Shape {
    public abstract double calculateArea();
}

public class Rectangle extends Shape {
    public double width;
    public double height;

    @Override
    public double calculateArea() {
        return width * height;
    }
}

public class Circle extends Shape {
    public double radius;

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
```


Liskov Substitution Principle(polymorphism):

_Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.
"The derived types must not change the behavior of the base types" means that it must be possible to use a derived type as if you were using the base type. For instance, if you are able to call x = baseObj.DoSomeThing(123) you also must be able to call x = derivedObj.DoSomeThing(123). The derived method should not throw an exception if the base method didn't. A code using the base class should be able to work well with the derived class as well. It should not "see" that it is using another type. This does not mean that the derived class has to do exactly the same thing; that would be pointless. In other words using a derived type should not break the code that was running smoothly using the base type._

Bad example: Subclass changes the expected behavior of the superclass
```
public class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

public class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostriches can't fly");
    }
}
```

Good example: Using a more appropriate hierarchy
```
public abstract class Bird {
    public abstract void move();
}

public class FlyingBird extends Bird {
    @Override
    public void move() {
        System.out.println("Flying");
    }
}

public class WalkingBird extends Bird {
    @Override
    public void move() {
        System.out.println("Walking");
    }
}

public class Ostrich extends WalkingBird {
    // Ostrich-specific behavior
}
```


Interface Segregation Principle:

_The dependency of one class to another should depend on the smallest possible interface.
I.E Be careful when creating interfaces._

Bad example: A large interface with many methods
```
public interface Worker {
    void work();
    void eat();
}
```

Good example: Split into smaller, more specific interfaces
```
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}

public class HumanWorker implements Workable, Eatable {
    @Override
    public void work() {
        // Working logic
    }

    @Override
    public void eat() {
        // Eating logic
    }
}

public class RobotWorker implements Workable {
    @Override
    public void work() {
        // Working logic
    }
}
```


Dependency Inversion Principle :

_Depend on abstractions than concrete implementations._

Examples:

Bad example: High-level module depends on a low-level module
```
public class Light {
    public void turnOn() {
        System.out.println("Light on");
    }

    public void turnOff() {
        System.out.println("Light off");
    }
}

public class Switch {
    private Light light;

    public Switch(Light light) {
        this.light = light;
    }

    public void operate() {
        light.turnOn();
    }
}
```
Good example: Both high-level and low-level modules depend on an abstraction
```
public interface Switchable {
    void turnOn();
    void turnOff();
}

public class Light implements Switchable {
    @Override
    public void turnOn() {
        System.out.println("Light on");
    }

    @Override
    public void turnOff() {
        System.out.println("Light off");
    }
}

public class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}
```
