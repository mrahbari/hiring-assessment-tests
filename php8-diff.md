PHP 8 introduced several major changes and improvements over PHP 7.4. Here are some key changes with examples:

### 1. **JIT (Just-In-Time) Compilation**
PHP 8 introduces a JIT compiler, which improves performance, particularly for CPU-heavy tasks. However, the impact on typical web applications is marginal.

```php
// No code change required to benefit from JIT.
// Enable it in the `php.ini` file with the following:
opcache.jit_buffer_size = 100M
```

### 2. **Union Types**
PHP 8 allows declaring multiple types for parameters, return values, or properties using union types. This was not possible in PHP 7.4.

**PHP 7.4 Example:**
```php
function calculate($a, $b) {
    return $a + $b;
}
```
Here, you cannot strictly enforce types.

**PHP 8 Example:**
```php
function calculate(int|float $a, int|float $b): int|float {
    return $a + $b;
}
```
Now, you can accept both `int` and `float` as valid types.

### 3. **Named Arguments**
PHP 8 introduces named arguments, allowing you to pass arguments by their names instead of their order. This makes the code more readable, especially with functions that have many optional parameters.

**PHP 7.4 Example:**
```php
function createUser($name, $email, $role = 'subscriber') {
    // ...
}
createUser('Mojtaba', 'mojtaba@example.com', 'admin');
```

**PHP 8 Example:**
```php
createUser(name: 'Mojtaba', email: 'mojtaba@example.com', role: 'admin');
```

### 4. **Attributes (Annotations)**
PHP 8 introduces attributes (metadata annotations) that can be added to classes, methods, properties, etc. Previously, this was achieved via PHPDoc comments in 7.4.

**PHP 7.4 Example (PHPDoc):**
```php
/**
 * @Route("/path", methods={"GET"})
 */
class MyController {
    // ...
}
```

**PHP 8 Example (Attributes):**
```php
#[Route("/path", methods: ["GET"])]
class MyController {
    // ...
}
```

### 5. **Match Expression**
PHP 8 introduces the `match` expression, which is similar to `switch` but more flexible. Unlike `switch`, it supports return values and does not require a `break` statement.

**PHP 7.4 Example:**
```php
$role = 'admin';
switch ($role) {
    case 'admin':
        echo 'Access granted';
        break;
    case 'user':
        echo 'Limited access';
        break;
    default:
        echo 'No access';
}
```

**PHP 8 Example:**
```php
$role = 'admin';
echo match ($role) {
    'admin' => 'Access granted',
    'user' => 'Limited access',
    default => 'No access',
};
```

### 6. **Nullsafe Operator**
The nullsafe operator (`?->`) makes it easier to work with objects that might be `null`. In PHP 7.4, this required verbose null checks.

**PHP 7.4 Example:**
```php
if ($user !== null) {
    $username = $user->getProfile()->getUsername();
}
```

**PHP 8 Example:**
```php
$username = $user?->getProfile()?->getUsername();
```
This safely handles `null` and avoids errors.

### 7. **Constructor Property Promotion**
PHP 8 allows for concise class property definitions and initialization directly in the constructor, reducing boilerplate code.

**PHP 7.4 Example:**
```php
class User {
    public $name;
    public $email;
    
    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
    }
}
```

**PHP 8 Example:**
```php
class User {
    public function __construct(
        public string $name,
        public string $email
    ) {}
}
```

### 8. **Throw Expression**
In PHP 8, `throw` is now an expression, meaning it can be used in places where expressions are allowed, such as in `null` coalescing or ternary operators.

**PHP 7.4 Example:**
```php
if (!$user) {
    throw new Exception("User not found");
}
```

**PHP 8 Example:**
```php
$user = $user ?? throw new Exception("User not found");
```

### 9. **Stringable Interface**
PHP 8 adds the `Stringable` interface, automatically implemented by any class that defines the `__toString()` method.

**PHP 7.4 Example:**
You had to explicitly check for `__toString()`.

**PHP 8 Example:**
```php
function printName(Stringable $object) {
    echo $object;
}
```

### 10. **Weak Maps**
Weak maps in PHP 8 allow you to store object references without preventing their garbage collection.

**PHP 8 Example:**
```php
$map = new WeakMap();
$object = new stdClass;
$map[$object] = 'value';

unset($object); // The object is garbage collected and removed from the map.
```

