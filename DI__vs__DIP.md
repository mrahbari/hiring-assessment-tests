تفاوت بین **Dependency Inversion** و **Dependency Injection** به شرح زیر است:

### Dependency Inversion

**Dependency Inversion Principle** (DIP) یکی از اصول SOLID است که می‌گوید:
1. ماژول‌های سطح بالا نباید به ماژول‌های سطح پایین وابسته باشند. هر دو باید به یک abstraction وابسته باشند.
2. Abstraction نباید به جزئیات وابسته باشد. جزئیات باید به abstraction وابسته باشند.

به عبارت دیگر، برای کاهش وابستگی بین ماژول‌ها و افزایش انعطاف‌پذیری سیستم، باید از abstractions (مثل interfaceها) استفاده کرد.

### Dependency Injection

**Dependency Injection** (DI) یک الگوی طراحی است که به ما اجازه می‌دهد وابستگی‌ها (dependencies) را به یک کلاس یا ماژول از خارج تزریق کنیم، به جای اینکه خود کلاس آن‌ها را ایجاد کند. این کار باعث می‌شود کلاس‌ها کمتر به یکدیگر وابسته باشند و کد آسان‌تر تست شود.

### مثال در PHP

در زیر یک مثال ساده از هر دو مفهوم در PHP آورده شده است:

#### بدون استفاده از Dependency Inversion و Dependency Injection

```php
class Database {
    public function connect() {
        // Logic to connect to the database
        return "Connected to Database";
    }
}

class User {
    private $database;

    public function __construct() {
        $this->database = new Database(); // وابستگی به کلاس Database
    }

    public function getUser() {
        return $this->database->connect() . " - Fetching User";
    }
}

$user = new User();
echo $user->getUser();
```

در این مثال، کلاس `User` مستقیماً به کلاس `Database` وابسته است.

#### با استفاده از Dependency Inversion و Dependency Injection

```php
interface DatabaseInterface {
    public function connect();
}

class MySQLDatabase implements DatabaseInterface {
    public function connect() {
        return "Connected to MySQL Database";
    }
}

class User {
    private $database;

    // وابستگی به interface به جای کلاس خاص
    public function __construct(DatabaseInterface $database) {
        $this->database = $database; // تزریق وابستگی
    }

    public function getUser() {
        return $this->database->connect() . " - Fetching User";
    }
}

// ایجاد نمونه از MySQLDatabase و تزریق آن به User
$database = new MySQLDatabase();
$user = new User($database);
echo $user->getUser();
```

### نتیجه‌گیری

- در مثال اول، وابستگی‌ها به‌طور مستقیم در کلاس‌ها تعریف شده‌اند که باعث کاهش انعطاف‌پذیری و سختی در تست می‌شود.
- در مثال دوم، با استفاده از **Dependency Injection** و **Dependency Inversion**، وابستگی‌ها از خارج به کلاس‌ها تزریق می‌شوند و کلاس‌ها به abstractions (interfaceها) وابسته هستند، که این باعث می‌شود کد قابل تست و نگهداری‌تری داشته باشیم.



--------------------------- 


Sure! Here's the explanation of the difference between **Dependency Inversion** and **Dependency Injection** in English, along with a PHP example:

### Dependency Inversion

**Dependency Inversion Principle** (DIP) is one of the SOLID principles, which states that:
1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.

In other words, to reduce dependencies between modules and increase system flexibility, abstractions (such as interfaces) should be used.

### Dependency Injection

**Dependency Injection** (DI) is a design pattern that allows us to inject dependencies into a class or module from the outside, rather than having the class create them itself. This makes classes less coupled and the code easier to test.

### Example in PHP

Here’s a simple example of each concept in PHP:

#### Without Dependency Inversion and Dependency Injection

```php
class Database {
    public function connect() {
        // Logic to connect to the database
        return "Connected to Database";
    }
}

class User {
    private $database;

    public function __construct() {
        $this->database = new Database(); // Dependency on Database class
    }

    public function getUser() {
        return $this->database->connect() . " - Fetching User";
    }
}

$user = new User();
echo $user->getUser();
```

In this example, the `User` class is directly dependent on the `Database` class.

#### With Dependency Inversion and Dependency Injection

```php
interface DatabaseInterface {
    public function connect();
}

class MySQLDatabase implements DatabaseInterface {
    public function connect() {
        return "Connected to MySQL Database";
    }
}

class User {
    private $database;

    // Dependency on interface instead of a specific class
    public function __construct(DatabaseInterface $database) {
        $this->database = $database; // Injecting the dependency
    }

    public function getUser() {
        return $this->database->connect() . " - Fetching User";
    }
}

// Creating an instance of MySQLDatabase and injecting it into User
$database = new MySQLDatabase();
$user = new User($database);
echo $user->getUser();
```

### Conclusion

- In the first example, dependencies are directly defined within the classes, which reduces flexibility and makes testing harder.
- In the second example, by using **Dependency Injection** and **Dependency Inversion**, dependencies are injected from outside into the classes, and the classes depend on abstractions (interfaces), leading to more maintainable and testable code.




