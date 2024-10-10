
### Liskov Substitution Principle (LSP)

**Definition**: 
The Liskov Substitution Principle states that:
- If you have a class (let’s call it a **parent class**) and a subclass (let’s call it a **child class**), you should be able to replace the parent class with the child class without causing any errors or changing how the program works.

### Simple Explanation

Think of it like this: If you have a remote control that works with a specific model of a TV, you should be able to use the same remote control with another model of the same brand, and it should work just fine. If it doesn’t, that’s a violation of the Liskov Substitution Principle.

### Example in PHP

Let’s say we have a basic example involving a `Car` class and its subclasses.

#### Correct Implementation

```php
class Car {
    public function start() {
        return "Car is starting";
    }
}

class ElectricCar extends Car {
    public function start() {
        return "Electric car is starting silently";
    }
}

class GasCar extends Car {
    public function start() {
        return "Gas car is starting with a roar";
    }
}

function testCar(Car $car) {
    echo $car->start();
}

// Usage
$myCar = new ElectricCar();
testCar($myCar); // Outputs: Electric car is starting silently

$myOtherCar = new GasCar();
testCar($myOtherCar); // Outputs: Gas car is starting with a roar
```

### Explanation

1. **Parent Class**: We have a `Car` class that has a `start` method.
2. **Child Classes**: We create two subclasses: `ElectricCar` and `GasCar`, both of which inherit from `Car`. They override the `start` method to provide their specific behavior.
3. **Usage**: The `testCar` function takes a `Car` object. We can pass either an `ElectricCar` or a `GasCar`, and it works perfectly. The function behaves as expected regardless of which subclass is used.

### Violation of LSP

Now, let’s look at an example that violates LSP:

```php
class Bird {
    public function fly() {
        return "Bird is flying";
    }
}

class Sparrow extends Bird {
    public function fly() {
        return "Sparrow is flying";
    }
}

class Ostrich extends Bird {
    public function fly() {
        throw new Exception("Ostriches can't fly");
    }
}

function makeBirdFly(Bird $bird) {
    echo $bird->fly();
}

// Usage
$mySparrow = new Sparrow();
makeBirdFly($mySparrow); // Works fine

$myOstrich = new Ostrich();
// makeBirdFly($myOstrich); // This will throw an exception
```

### Explanation

1. **Parent Class**: The `Bird` class has a `fly` method.
2. **Child Classes**: The `Sparrow` class can fly, so it works fine. However, the `Ostrich` class cannot fly and throws an exception when you call the `fly` method.
3. **Violation**: When we try to use `makeBirdFly` with an `Ostrich`, it causes an error. This violates the Liskov Substitution Principle because we cannot replace `Bird` with `Ostrich` without causing issues.

### Conclusion

- The Liskov Substitution Principle helps ensure that subclasses can be used interchangeably with their parent classes without causing errors or unexpected behavior.
- Following this principle leads to more robust and maintainable code.

