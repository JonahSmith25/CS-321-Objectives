1\. Tell, Don't Ask 

-   What it says: 

The principal says that we should tell objects what to do rather than asking them for information. 

-   Elaboration: 

The "Tell, don't ask" principal is a part of Object Oriented Programming which states that objects should control their own behavior rather than relying on external code to determine behavior. Instead of asking an object for information and then making decisions based on that information, you tell the object what to do using methods that contain the needed logic. Use of this method leads to the state and behavior of an ojbect being tightly connected which reduces the opportunity for unintended behaviors to arise. It also means that the code can more easily be read, implemented, and altered given that objects and methods are defined and given a particular purpose.  

-   Example of violation:
```
class Order { 
    private int quantity; 
    private double price; 
    public Order(int quantity, double price) { 
        this.quantity = quantity; 
        this.price = price; 
    } 
    public int getQuantity() { 
        return quantity; 
    } 
    public double getPrice() { 
        return price; 
    } 
    public double calculateTotal() { 
        return quantity * price; 
    } 
} 
class OrderProcessor { 
    public double processOrder(Order order) { 
        int quantity = order.getQuantity(); 
        double price = order.getPrice(); 
        return quantity * price; 
    } 
} 
```
-   Refactored code: 

```
class Order { 
    private int quantity; 
    private double price; 
    public Order(int quantity, double price) { 
        this.quantity = quantity; 
        this.price = price; 
    } 
    public double calculateTotal() { 
        return quantity * price; //
    } 
} 
class OrderProcessor { 
    public double processOrder(Order order) { 
        return order.calculateTotal(); 
    } 
}
```


2\. Open-Closed Principle

-   What it says: 

The Open-Closed Principle states that entities should be open for extension but closed for modification. 

-   Elaboration:
  
The "Open-Closed Principal" says that systems should be designed in a way that allows functionality to be extended without making alterations or modifications to the code. Its central theme is that code should be written in such a way that its core logic does not need to be changed in order to be used in the future. This means that methods and systems can be scaled and are more flexible in their use cases, allowing for the creation of new classes or methods without having to tamper with existing ones.

-   Example of violation:

```
class DiscountCalculator {
    public double calculateDiscount(String customerType, double price) {
        if (customerType.equals("regular")) {
            return price * 0.1; // 10% discount for regular customers
        } else if (customerType.equals("vip")) {
            return price * 0.2; // 20% discount for VIP customers
        } else {
            return 0;
        }
    }
}
```

-   Refactored code: 

```
abstract class Customer {
    public abstract double calculateDiscount(double price);
}
class RegularCustomer extends Customer {
    public double calculateDiscount(double price) {
        return price * 0.1; // 10% discount for regular customers
    }
}
class VipCustomer extends Customer {
    public double calculateDiscount(double price) {
        return price * 0.2; // 20% discount for VIP customers
    }
}
class DiscountCalculator {
    public double calculateDiscount(Customer customer, double price) {
        return customer.calculateDiscount(price); // Delegate to customer-specific logic
    }
}
```


3\. Single Responsibility Principle

-   What it says:

The “Single Responsibility Principal” says that any given class should only have one job or responsibility. 

-   Elaboration: 

The goal of this principal is to guarantee that each class has one purpose and will not be given any other responsibilities. With classes that have only one purpose, a system is much easier to maneuver and maintain as there is no question to what does what, as each location should be well named and contain an individual purpose. This increases the flexibility of classes and makes it easier to implement in the future. 

-   Example of Error:

```
class UserManager {
    public void createUser(String username, String password) {
        // Logic to create user
        System.out.println("User " + username + " created.");
    }

    public void sendEmailConfirmation(String email) {
        // Logic to send email confirmation
        System.out.println("Email sent to " + email);
    }
}

```

-   Refactored Code:

```
class UserCreator {
    public void createUser(String username, String password) {
        // Logic to create user
        System.out.println("User " + username + " created.");
    }
}

class EmailSender {
    public void sendEmailConfirmation(String email) {
        // Logic to send email confirmation
        System.out.println("Email sent to " + email);
    }
}

public class Main {
    public static void main(String[] args) {
        UserCreator creator = new UserCreator();
        EmailSender sender = new EmailSender();

        creator.createUser("Guy", "password123");
        sender.sendEmailConfirmation("Guy@example.com");
    }
}

```


4\. Interface Segregation Principle

-   What it says:
-   
The “Interface Segregation Principal” says that users should not have to depend on interfaces which are not used by them.

-   Elaboration:

The goal of this principle is to avoid oversized interfaces and to break interfaces down into smaller and more specific interfaces. This means that multiple smaller interfaces should be created and implemented only when their specific use case is required. In doing so, users avoid unneeded interaction with interfaces they don’t need, and programmers can have use case specific interfaces that are not oversized and serve a designed and focused purpose.  

-   Example of Error:

```
interface Machine {
    void start();
    void print();
}

class Car implements Machine {
    @Override
    public void start() {
        System.out.println("Car is starting.");
    }
    @Override
    public void print() {
        // Cars don't print, but we are forced to implement this method
        throw new UnsupportedOperationException("Cars can't print.");
    }
}

```

-   Refactored Code:

```
interface Startable {
    void start();
}

interface Printable {
    void print();
}

class Car implements Startable {
    @Override
    public void start() {
        System.out.println("Car is starting.");
    }
}

class Printer implements Printable {
    @Override
    public void print() {
        System.out.println("Printer is printing.");
    }
}

```
