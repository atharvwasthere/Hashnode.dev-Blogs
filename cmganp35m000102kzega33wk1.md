---
title: "SOLID Principles"
seoTitle: "SOLID Principles Explained with Examples | Design Patterns "
seoDescription: "Learn the SOLID principles of object-oriented design with clear C++ and TypeScript examples. Improve code readability, maintainability, and scalability by m"
datePublished: Fri Oct 03 2025 09:43:10 GMT+0000 (Coordinated Universal Time)
cuid: cmganp35m000102kzega33wk1
slug: solid-principles
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1759481505710/b3b1a341-c4b0-4e55-b70d-1a6d65daf27f.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1759481276460/33b0c304-a49f-4de5-ac9a-ac2c595bb375.png
tags: design-patterns, system-design, liskov-substitution-principle, open-closed-principle, solid-principles, design-principles, low-level-design, lld, dependency-inversion, interface-segregation, single-responsibility-principle-srp

---

When working in companies, it’s essential to ensure that our code remains highly maintainable and easy to read.

It is that one metric in which AI fails poorly (not following design patterns / SOLID etc.)

## What exactly is a Design Pattern?

It is a Solution to a kind of readability, maintainability & extendibility problem…

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Unlike High Level Design(HLD), Low Level Design(LLD) is very closely related to the final coding implementation. We focus on a) Code <strong>Structure</strong> b) Code <strong>Distribution</strong></div>
</div>

> One thing to keep in mind LLD implementation is both LANGUAGE & ALGORITHM `agnostic`

Now, before we study, design principle/pattern we need to study its building blocks and which comes out to be SOLID Principle…

# SOLID Principle

When it comes to SOLID principles, there are a lot of misconceptions and misunderstandings among developers, but ***I can guarantee you, after reading this blog, you will not have any confusion, and you can also apply it in real-life coding.***

We will be using C++& TypeScript as the language but as I said before it is language agnostic if you have some knowledge about OOP concepts you would understand this.

SOLID stands for

* S: Single Responsibility Principle
    
* O: Open Closed Principle
    
* L: Liskov Substitution Principle
    
* I: Interface Segregation
    
* D: Dependency Inversion
    

## 1\. Single Responsibility Principle (SRP)

> Every class should have only one reason to change

This doesn't mean a class should only have one method. It means a class should only be responsible for one specific feature or piece of functionality. The "reason to change" refers to a single source or actor, like a business requirement or stakeholder.

### Why SRP?

* Keeping responsibilities separate reduces merge conflicts and prevents the creation of "***monster classes"*** that are difficult to manage.
    
* if you have too many things in the same piece of code, you have too many reasons to change that code
    
* When working on a larger codebase it is very important to keep SRP in mind as there are very high chances that your class can become a “monster Class”
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759481861758/47803c5b-d240-43ed-9614-ce5da9c713c2.png align="center")

### A common violation is a class that handles data, business logic, and reporting all at once. Let’s see this example

```tsx
class Employee {
    // Responsibility 1: Data
    constructor(public id: string, public name: string, public salary: number) {}

    // Responsibility 2: Finance Logic
    calculatePay(): number {
        // Payroll logic...
        return this.salary / 12;
    }

    // Responsibility 3: HR Logic
    generatePerformanceReport(): string {
        // Reporting logic...
        return `Performance report for ${this.name}...`;
    }
}
```

Now the issue with this code is that, when the logic of salary or performance Calculation changes there is a need to change the existing implementation of the class `employee`

> We refactor this into three distinct classes, each with a single responsibility.

```tsx
class EmployeeData {
    constructor(public readonly id: string, public readonly name: string, public readonly salary: number) {}
}

class PayCalculator {
    public calculatePay(employee: EmployeeData): number {
        return employee.salary / 12;
    }
}

class PerformanceReporter {
    public generateReport(employee: EmployeeData): string {
        return `Performance report for ${employee.name}...`;
    }
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759481952822/ab2eb261-86db-4283-b294-9e1ec3830d5a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759481962707/77dd94b1-da3e-406e-82a4-d03d6e8c7520.png align="center")

<div data-node-type="callout">
<div data-node-type="callout-emoji">🗒</div>
<div data-node-type="callout-text">Also point to be noted that SRP is subjective, what is a better class to me might not be to you …as there is no hard and fast rule to it</div>
</div>

**<mark>if you look at a file I/O module of any language you would find, both read and the write functions are in the same class</mark>**

`<iostream> class in cpp has both cin and cout`

Most people confuse SRP by thinking that a class should only contain one method, but that’s wrong. It can have multiple methods. Some people think SRP, like a class, handles only a single thing. But thinking like that is very confusing.

So just remember that SRP means a class should only contain that set of methods which change in future or do any code inside that class in future due to a single source or reason.

Let’s take a look at another example

Think about it, does this follow SRP ?

```cpp
class HtmlProcessor
{
public:
    std::string readFile(const std::string &filename)
    {
	  
	  }

    void writeFile(const std::string &filename, const std::string &data)
    {
        
    }
    std::string convertToText(const std::string &html)
    {
        // Logic
    }
};
```

The answer is : this does not follow SRP

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759482337003/0e810d4c-261d-439c-b42e-86fe54087a82.png align="left")

```cpp
// Handles reading and writing files only
class FileProcessor {
public:
    std::string readFile(const std::string &filename) {
    }
    void writeFile(const std::string &filename, const std::string &data) {
    }

};

// Handles test/HTML content processing logic only
class TestProcessor {
public:
    std::string convertToText(const std::string &html) {
        // logic
      }

};
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759482349579/38b371db-7323-4b67-b24e-205eb2144842.png align="left")

Now you may be thinking that in this `FileProcessor` class breaks SRP, but it’s not how SRP works

Despite it having both Read and Write file logic, they are a part of I/O based tasks…

## 2\. Open Closed Principle (OCP)

> The code should be open for extension but closed for modification..

We can add new functionalities if we want, but we can’t modify the existing ones, modifying the existing one <mark>means re-writing all of unit tests again, making it not developer friendly</mark>

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Now, a good question to ask is why can’t we use inheritance to solve it ? It won’t be useful everywhere but will be used somewhere</div>
</div>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759482503609/a8dbb84f-321f-4455-b8aa-48007dd3d8bb.png align="left")

> Instead of depending on concrete classes, your code should depend on interfaces.

Hence, we use polymorphism for this…

Imagine a service that sends notifications. A naive approach uses conditionals, which violates OCP.

```tsx
// VIOLATES OCP
class NotificationService {
    public sendNotification(type: string, userId: string, message: string): void {
        if (type === 'email') {
            console.log(`Email to ${userId}: ${message}`);
        } else if (type === 'sms') {
            console.log(`SMS to ${userId}: ${message}`);
        }
        // Adding a new type requires modification here.
    }
}
```

We will have an interface for it, implementing the core common function of any notification service…

We introduce a Notifier interface. The service depends on this abstraction, and new notification types are added by creating new classes.

```tsx

interface Notifier {
    notify(userId: string, message: string): void;
}

class EmailNotifier implements Notifier {
    public notify(userId: string, message: string): void {
        console.log(`Email to ${userId}: ${message}`);
    }
}

class SMSNotifier implements Notifier {
    public notify(userId: string, message: string): void {
        console.log(`SMS to ${userId}: ${message}`);
    }
}

// The service is now closed for modification but open for extension.
class NotificationService {
    constructor(private notifiers: Notifier) {}

    public sendNotifications(userId: string, message: string): void {
        for (const notifier of this.notifiers) {
            notifier.notify(userId, message);
        }
    }
}
```

***Overriding*** the `notify` function

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759482617137/2c918859-9baa-4a32-9304-386095a467fc.png align="left")

and last but not the least we will have the main `NotificationService` class which will have a key (for letting it know if the Notification type is `sms, email , popUp etc..` and the Notify function tamed according to it.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">It is a good idea to be dependent upon abstractions rather than depending on concrete classes</div>
</div>

> The IF-ELSE is not a problem

This should be enough to understand OCP completely.

At last, remember these:

* Don’t use `if-else` or `switch` to decide behavior. Instead, use interfaces/abstractions.
    
* Don’t edit existing classes to add cases. Instead, extend using new classes.
    

## 3\. Liskov Substitution Principle (LSP)

You may not know it, but you have either seen it many times or yourself must have implemented if you have been coding for some time.

> Objects of a superclass shall be replaceable with the objects of its subclass without breaking any thing.

In simple terms, If a function works with a base class, it must also work with any of its subclasses without any special checks.

Imagine a banking system. We all know accounts: savings, current, fixed deposit etc.

```tsx
class Account {
public:
    virtual void withdraw(double amount) = 0;
    virtual void deposit(double amount) = 0;
    virtual double getBalance() = 0;
    virtual void transfer(Account* to, double amount) = 0;
    virtual void printStatement() = 0;
};
```

```cpp
class SavingsAccount : public Account {
public:
    void withdraw(double amount) override; 
    void deposit(double amount) override;
    double getBalance() override;
    void transfer(Account* to, double amount) override;
    void printStatement() override;
    // savings-specific
    void applyInterest();
};

class CurrentAccount : public Account {
public:
    void withdraw(double amount) override;
    void deposit(double amount) override;
    double getBalance() override;
    void transfer(Account* to, double amount) override;
    void printStatement() override;
    // current-specific
    void provideOverdraft(double limit);
};
```

Works fine. Substitute `SavingsAccount` or `CurrentAccount` anywhere `Account*` is expected. System won’t break.

### The Violation

If some dev adds fixed deposit accounts into the same hierarchy:

```tsx
class FixedDepositAccount : public Account {
public:
    void withdraw(double amount) override {
        throw std::runtime_error("Cannot withdraw before maturity");
    }

    void deposit(double amount) override {
        throw std::runtime_error("Deposits only allowed at account creation");
    }

    double getBalance() override; 
    void transfer(Account* to, double amount) override {
        throw std::runtime_error("Transfer not allowed for FD");
    }

    void printStatement() override;
    // FD-specific
    void calculateMaturityValue();
};
```

this breaks **LSP** badly:

* parent said: every account can withdraw → FD says “nope”.
    
* parent said: every account can deposit → FD says “only once”.
    
* parent said: transfer is available → FD throws error.
    

Any generic code that works with `Account*` will crash when given an FD.

### The “Wrong Inheritance”

We tried to make `FixedDepositAccount` a subclass of `Account`.

But an FD is not a “normal withdrawable account” its behavior is fundamentally different.

So inheritance forces it to pretend to support methods like `withdraw()` or `transfer()`, which it can’t without breaking the rules.

That’s why it ended up throwing errors → **LSP broken**.

### The Fix: Use Composition(has-a relationship)+ Interfaces

Instead of one fat parent class, break the behaviors into smaller **capabilities**.

```cpp
// Base Account class for all accounts
class Account {
protected:
    std::string accountNo;
    std::string owner;
public:
    virtual double getBalance() = 0;
    virtual void printStatement() = 0;
    virtual ~Account() {}
};

// Capabilities as separate interfaces
class Withdrawable {
public:
    virtual void withdraw(double amount) = 0;
    virtual ~Withdrawable() {}
};

class Depositable {
public:
    virtual void deposit(double amount) = 0;
    virtual ~Depositable() {}
};

class Transferable {
public:
    virtual void transfer(Account* to, double amount) = 0;
    virtual ~Transferable() {}
};
```

Now model accounts by **composing only the behaviors they support**:

```cpp
class SavingsAccount : public Account, public Withdrawable,
                       public Depositable, public Transferable {
public:
    double getBalance() override;
    void withdraw(double amount) override;
    void deposit(double amount) override;
    void transfer(Account* to, double amount) override;
    void applyInterest();
};

class FixedDepositAccount : public Account {
public:
    double getBalance() override;
    void printStatement() override;

    // FD-specific
    void calculateMaturityValue();
    // no withdraw, no deposit, no transfer
};
```

---

### Why this avoids LSP issues

* No class is “forced” into methods it can’t logically implement.
    
* Client code depending on `Withdrawable*` is guaranteed to get only accounts that can withdraw.
    
* Substitution is safe, because contracts are cleanly separated.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759482809090/b34f60ad-2724-4f17-954b-1978f57781a5.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759482812753/eb59d567-3921-4457-a15c-d0eab912c4b4.png align="left")

If a child can’t fully honor the parent’s promises, split responsibilities and compose instead.

### **At last, remember these:**

* A subclass must honor the behavioral contract of its superclass.
    
* Subclasses should not throw new exceptions for methods defined in the parent.
    
* If you find yourself using `instanceof` checks, you are likely violating LSP.
    

## 4\. Interface Segregation Principle

> There should be not any kind of forceful behavior imposed on a class which does not require it.

If there is an interface which has a lot of methods to implement and those methods will not be relevant for every class using that, so then it is better to break it into multiple interfaces and use them when required.

And if you don’t have interface and have a class then you can’t even achieve it using inheritance polymorphism saves us from it…

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759482912886/07128955-6898-4f79-910a-14f1d33ea3da.png align="left")

We still have some <mark> code duplication</mark> and if later the logic is changed (both switched to the same algo)

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">These kinds of code duplication problems are solved by<mark> design patterns precisely strategic design pattern.</mark></div>
</div>

Think about it or else I will be releasing another blog on “How to design a chess Engine” in that I will cover this…

### **At last, remember these:**

* Split large interfaces into small, role-based contracts.
    
* Don't force classes to depend on things they don't use
    
* Avoid `throw new Error "Not implemented".`
    

## 5\. Dependency Inversion Principle

> High level modules should not depend on concrete implementations of low level modules directly instead, they should depend on abstractions

High-level modules (business logic) should <mark>not be tightly coupled</mark> to low-level modules (database access, network calls). Instead of `BusinessLogic -> Database`, the dependency should be inverted: `BusinessLogic -> Repository <- Database.`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759483180985/6492d472-2436-4bab-b079-b1c15e9905bf.png align="left")

service classes are the main algorithm driven classes

### Example → An `OrderService` should not be hard-coded to use a specific database like MySQL.

```tsx
// Low-level module
class MySQLRepository {
    public insertOrder(orderData: any): void {
        console.log("Order inserted into MySQL.");
    }
}

// VIOLATES DIP
class OrderService {
    private repo: MySQLRepository = new MySQLRepository(); // Tight coupling

    public createOrder(orderData: any): void {
        this.repo.insertOrder(orderData);
    }
}
```

Here the problem is **tight coupling…**

### The `OrderService` depends on an `OrderDBRepository` interface. The concrete database implementation is "injected" at runtime.

So all they now do is just ***override the pre-written functions*** (you have to comply to the interface)

```tsx
// Abstraction defined by the high-level module
interface OrderDBRepository {
    insertOrder(orderData: any): void;
}

// Low-level modules implement the abstraction
class MySQLRepository implements OrderDBRepository {
    public insertOrder(orderData: any): void { /*... */ }
}
class MongoRepository implements OrderDBRepository {
    public insertOrder(orderData: any): void { /*... */ }
}

// High-level module depends on the abstraction
class OrderService {
    constructor(private repo: OrderDBRepository) {} // Dependency is injected

    public createOrder(orderData: any): void {
        this.repo.insertOrder(orderData);
    }
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759483186432/c3304207-de9b-43c8-a6fb-e19974f5c62a.png align="left")

> here order service will have a composition relationship (has - a ) and thus abstraction will be followed it will be now decided on runtime which whether we will use MYSQL or Mongo

<mark>This process of supplying required information and concrete implementations to abstractions at runtime is known as </mark> ***<mark>Dependency Injection</mark>***

## Up until now we have always heard people say that classes must represent a noun and all, but <mark>classes can also represent behavior</mark>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759483190759/dcdd7caf-6317-48dc-b27a-c78da5ec21ca.png align="left")