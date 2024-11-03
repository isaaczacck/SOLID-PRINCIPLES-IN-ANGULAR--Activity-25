# "SOLID PRINCIPLES in Angular"

Activity 25:

# Introduction

The SOLID principle is a crucial concept in software development, enhancing code quality and efficiency. It is particularly useful in Angular, a leading web application framework, for creating robust, modular, and extendable applications, making it an essential aspect of the software development process.


# SOLID Principles:

# S - Single Responsibility Principle (SRP):

Adhering to the SRP in Angular ensures that each class or component takes on only a single responsibility. For instance, an AuthService should be responsible exclusively for authentication. This fosters a clear separation of concerns and facilitates maintenance and testing.


```
// Bad Example: Single component handling data fetching and display
@Component({ selector: 'app-product', templateUrl: './product.component.html' })
export class ProductComponent {
    products: any[] = [];
    constructor(private http: HttpClient) {
        this.fetchProducts();
    }
    fetchProducts() {
        this.http.get('/api/products').subscribe(data => { this.products = data; });
    }
}

// Good Example: Separating data-fetching logic into a ProductService
@Injectable({ providedIn: 'root' })
export class ProductService {
    constructor(private http: HttpClient) {}
    getProducts() { return this.http.get('/api/products'); }
}

@Component({ selector: 'app-product', templateUrl: './product.component.html' })
export class ProductComponent {
    products: any[] = [];
    constructor(private productService: ProductService) {
        this.loadProducts();
    }
    loadProducts() { this.productService.getProducts().subscribe(data => { this.products = data; }); }
}
```
Explanation: In Angular, any service, component, or a directive should be able to have just a single responsibility to be fully maintainable.

Real-World Use Case:
An Angular application with separate components for managing user profiles and products can be maintained and tested by isolating business logic in services.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Open/Closed Principle (OCP):
The OCP encourages the extensibility of classes without modifying them. An Angular LoggerService could implement various logger strategies, which can be swapped by Dependency Injection, without altering the class itself. This boosts the flexibility and reusability of code.

```
// Bad Example: Hardcoding different discount types
getDiscount(type: string): number {
    if (type === 'student') return 20;
    if (type === 'senior') return 30;
    return 0;
}

// Good Example: Using interfaces to extend discount types
interface Discount { calculate(): number; }

class StudentDiscount implements Discount { calculate() { return 20; } }
class SeniorDiscount implements Discount { calculate() { return 30; } }

function applyDiscount(discount: Discount) { return discount.calculate(); }
```
Explanation: Classes should be open for extension but closed for modification. In Angular, this principle can be implemented by using interfaces and inheritance, allowing new functionality to be added without altering existing code.

Real-World Use Case: 
In an e-commerce application, new discount types (e.g., "HolidayDiscount") can be introduced by implementing the Discount interface without altering the core discount logic. This makes the system flexible for future changes, supporting rapid development of new promotional features.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Liskov Substitution Principle (LSP):
The LSP ensures that subclasses can extend the functionality of their parent classes without affecting existing functionalities. In Angular, an ExtendedService that inherits from a BaseService can provide additional functions while preserving the basic functionality.

```
interface Shape { getArea(): number; }

class Circle implements Shape {
    constructor(public radius: number) {}
    getArea() { return Math.PI * Math.pow(this.radius, 2); }
}

class Square implements Shape {
    constructor(public side: number) {}
    getArea() { return this.side * this.side; }
}
```
Explanation: This states that a superclass's objects must be replaceable by a subclass's instances without altering the correctness of the program. In Angular, it generally means that a component or service or class that may implement an interface should essentially work the same as something with which it is supposed to replace. In that way, one is almost guaranteed that any and all sub-classes or implementations support the behavior expected and that one has loose, modular code.

Real-World Use Case:
In an Angular application with a dashboard that shows various metrics based on shape areas, adhering to LSP allows you to substitute different Shape types (e.g., Circle, Square, or any other future shapes) without modifying the underlying area calculation logic. By ensuring all shapes adhere to a common Shape interface, the application can dynamically render area calculations for any shape type, supporting easy updates and maintenance as new shapes are introduced.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Interface Segregation Principle (ISP):

In Angular, the ISP requires the division of large interfaces into smaller, more specific ones. This prevents classes from being overloaded with methods they don't need. An example would be a UserService split into specific interfaces like UserAuthentication and UserDataManagement.

```
// Bad Example: Large interface forcing unused methods
interface CRUD { create(): void; read(): void; update(): void; delete(): void; }

class ReadOnlyService implements CRUD {
    create() {} // Not needed
    read() {}
    update() {} // Not needed
    delete() {} // Not needed
}

// Good Example: Smaller interfaces with specific responsibilities
interface Read { read(): void; }
interface Write { create(): void; update(): void; delete(): void; }

class ReadOnlyService implements Read { read() {} }
```
Explanation: Classes should not be forced to implement interfaces they don’t use. In Angular, creating smaller, more specific interfaces prevents unnecessary methods in classes.

Real-World Use Case:
In an Angular application with various services, some may only need to fetch data (read-only), while others may require full CRUD operations. By defining specific interfaces for each functionality, services implement only the methods they need, making code easier to understand and maintain.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Dependency Inversion Principle (DIP):
The DIP dictates that dependencies should be based on abstractions and not on concretizations. A BookComponent should depend on an abstract BookService rather than a specific implementation, reducing coupling and enhancing testability.

```
// Bad Example: Direct instantiation of a logger service
class OrderComponent {
    private logger = new ConsoleLogger();
}

// Good Example: Injecting an interface-compliant service
interface Logger { log(message: string): void; }

@Injectable({ providedIn: 'root' })
class ConsoleLogger implements Logger { log(message: string) { console.log(message); } }

@Component({ selector: 'app-order', templateUrl: './order.component.html' })
class OrderComponent {
    constructor(private logger: Logger) {}
    placeOrder() { this.logger.log('Order placed successfully.'); }
}
```
Explanation: LogService depends on the Logger abstraction rather than a concrete implementation, allowing different loggers to be swapped without modifying LogService, aligning with DIP.

Real-World Use Case:
A Logging system where different logging methods (e.g., console, file, or HTTP logging) are injected based on the environment.


Resources: 

https://angular.love/angular-and-solid-principles/

https://newcubator.com/blog/web/solid-principles-in-angular

