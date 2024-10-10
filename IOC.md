In simple terms, **Inversion of Control (IoC)** in Spring is a way to manage how objects in your code are created and used. Normally, when you write a program, each object is responsible for creating its dependencies. But with IoC, Spring takes over this responsibility. Instead of objects creating or finding what they need, Spring gives them the needed objects (called **dependencies**).

For example, if a `Car` class needs an `Engine`, the `Car` doesn’t have to create an `Engine` itself. Spring will inject the `Engine` into the `Car` for you, either when the `Car` is created (using constructor injection) or by setting it later (using setter injection).

The main benefit is that your code becomes cleaner, more flexible, and easier to maintain. It also makes testing easier because you can swap out dependencies without changing your code.

In simple terms, **Spring IoC** means Spring controls the creation and management of objects, so you don't have to!


--------------------------------------------


در فریم‌ورک Spring، مفهوم **IOC (Inversion of Control)** یکی از اصول اصلی است که نحوه مدیریت وابستگی‌ها را تعریف می‌کند. Inversion of Control به معنی "معکوس کردن کنترل" است، به این صورت که به جای آنکه برنامه به صورت مستقیم مسئول ایجاد و مدیریت اشیاء (Object) باشد، فریم‌ورک Spring این کار را انجام می‌دهد. این باعث می‌شود تا وابستگی‌ها به صورت خودکار تزریق شوند.

در سیستم‌های سنتی، زمانی که یک شیء به اشیاء دیگر نیاز دارد، خودش آن‌ها را ایجاد یا درخواست می‌کند. اما در IOC، کنترل ساخت و مدیریت این اشیاء به فریم‌ورک منتقل می‌شود. این رویکرد باعث می‌شود تا کد شما انعطاف‌پذیرتر، قابل تست‌تر و مقیاس‌پذیرتر شود.

### چگونه IOC در Spring پیاده‌سازی می‌شود؟
در Spring، **IOC Container** مسئول ایجاد و مدیریت اشیاء و تزریق وابستگی‌ها است. این کانتینر وابستگی‌ها را از طریق یک فایل پیکربندی (XML یا Java annotations) مدیریت می‌کند. دو الگوی رایج برای تزریق وابستگی در Spring وجود دارند:
1. **Constructor Injection**: وابستگی‌ها از طریق سازنده‌ی کلاس تزریق می‌شوند.
2. **Setter Injection**: وابستگی‌ها از طریق متدهای setter تزریق می‌شوند.

مثال ساده از IOC:
```java
public class Car {
    private Engine engine;
    
    // Constructor Injection
    public Car(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.run();
    }
}
```
در اینجا شیء Car به شیء Engine نیاز دارد، اما به جای اینکه Car خودش Engine را ایجاد کند، از بیرون تزریق می‌شود.

### مزایای استفاده از IOC:
- **قابلیت تست بهتر**: اشیاء به راحتی می‌توانند با اشیاء ماک تست شوند.
- **کاهش کوپلینگ (وابستگی) بین کلاس‌ها**: کلاس‌ها به صورت مستقیم وابسته به هم نیستند.
- **کد تمیزتر و ساده‌تر**: برنامه‌نویسان فقط بر روی منطق کسب‌وکار تمرکز می‌کنند و نیاز به مدیریت وابستگی‌ها ندارند.

