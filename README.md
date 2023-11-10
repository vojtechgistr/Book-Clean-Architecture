# Clean Architecture
My notes from the book *Clean Architecture: A Craftsman's Guide to Software Structure and Design* - by *Robert C. Martin*

# Index

1. [What is Design and Architecture](#what-is-design-architecture)
2. [A Tale of Two Values](#tale-of-two-values)
3. [Paradigm Overview](#paradigm-overview)
4. [Structured Programming](#structured-programming)
5. [Object Orient Programming](#object-oriented-programming)
6. [Functional programming](#functional-programming)

III. [Design Principles](#design-principles)

7. [SRP: The Single Responsibility Principle](#single-responsibility-principle)
8. [OCP: The Open Closed Principle](#open-closed-principle)
9. [LSP: The Liskov Substitution Principle](#liskov-substitution-principle)

---

# <a name="what-is-design-architecture">1. What is Design and Architecture</a>

- There is no difference between design and architecture.
- Word "architecture" is used in high-level context, whereas "design" is used in low-level context, but both are part of the whole software design.
- The goal of software architecture is to minimize the human resources required to build and maintain the required system.
- The best option is for the development organization to recognize and avoid its own overconfidence and to start taking the quality of its software architecture seriously.

# <a name="tale-of-two-values">2. A Tale of Two Values</a>

- Every software system provides two different values to the stakeholders: **behavior and structure**.
- Software developers are responsible for ensuring that both those values remain high.
- Function or architecture? Which of these two provides the greater value?
  - The first value of software —behavior— is urgent but not always particularly important
  - The second value of software —architecture— is important but never particularly urgent
  - **Software priorities lie within these 4 points:**
      * 1. Urgent and important
      * 2. Not urgent and important
      * 3. Urgent and not important
      * 4. Not urgent and not important
   
# <a name="paradigm-overview">3. Paradigm Overview</a>

- A paradigm tells you which programming structures to use, and when to use them.
- There are 3 paradigms:
  - **Structured programming:** imposes discipline on direct transfer of control (if/else/then).
  - **Object orient programming:** imposes discipline on indirect transfer of control (polymorphism/function pointers).
  - **Functional programming:** imposes discipline upon assignment (mathematical symbols).

# <a name="structured-programming">4. Structured programming</a>

- Via structured programming, programmers could break down large proposed systems into modules and components that could be further broken down into tiny provable functions.
- Software is driven by falsifiability, it's science. Software Architects strive to define modules, components, and services that are easily falsifiable (testable).

# <a name="object-oriented-programming">5. Object Orient Programming</a>

- OOP supports the proper admixture of:
  - **Encapsulation:** allows data to be hidden and only functions are known. We see this concept in action as the private data members and the public member functions of a class.
  - **Inheritance:** simply redeclaration of a group of variables and functions within an enclosing scope.
  - **Polymorphism:** is writing the same block of code and giving it the same name, but allowing it to take a different type of input.
- To architects: OOP is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system. It allows the architect to create a plugin architecture, in which modules that contain high-level policies are independent of modules that contain low-level details. The low-level details are relegated to plugin modules that can be deployed and developed independently from the modules that contain high-level policies.
 
# <a name="functional-programming">6. Functional programming</a> 

- Variables in functional languages do not vary - are not modified.
- It is important to consider immutable variables, because **all race conditions, deadlock conditions, and concurrent update problems are due to mutable variables**.
- You cannot have a race condition or a concurrent update problem if no variable is ever updated. You cannot have deadlocks without mutable locks.
- As an architect, you must be asking yourself whether immutability is practicable.

## Segregation of Mutability
- One of the most common compromises in regard to immutability is to **segregate the application** into mutable and immutable components
- Immutable components perform their tasks in a functional way, without using any mutable variables

## Event Sourcing
- Imagine if we added up all the transactions of a bank client, we'd require a lot of processing power.
- However we could use event sourcing and have enough storage to perform that.
- the **event sourcing** stategy here is we store the transactions, but not the state. When state is required, we simply apply all the transactions from the beginning of time.
- We could therefore not update or delete anything, and thus making our application immutable, therefore *functional*
- Event Sourcing ensures that all changes to application state are stored as a sequence of events.

# <a name="design-principles">III. Design Principles (SOLID)</a> 

- The SOLID principles tell us how to arrange our functions and data structures into classes, and how those classes should be interconnected.
- The goal of the principles is creation of mid-level software structures that:
  * 1. Tolerate change
  * 2. Are Easy to understand
  * 3. Are the basis of components taht can be used in many software systems
- The "mid-level" refers to the fact that these princilpes are applied by programmers working at the modile level. They help to define the kinds of software structures used within modules and components.


# <a name="single-responsibility-principle">7. SRP: The Single Responsibility Principle</a>

# The Single Responsibility Principle (SRP)
- It's wrong to assume that SRP means that every module should do just one thing. There is a principle like that, a function should do one thing.
- Software systems are changed to satisfy users and stakeholders - **those users and stakeholders are the “reason to change”**
- However, since users/skateholders change, we'll refer to them as actors. Thus, the **definition** of The Single Responsibility Principle (SRP) is: 
  * **A module should be responsible to one, and only one, actor**
- Module is basically just a source file or a cohesive set of functions and data structures.

## Violations of SRP
### Symptom 1: Accidental Duplication
![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/485bc647-9a39-4a06-91de-52fca22ee881)

- An `Employee` class has three functions `calculatePay()`, `reportHours()`, and `save()`.
- This class violates SRP because those three methods are responsible for very different actors.
  - The `calculatePay()` method is specified by the accounting department, which reports to the CFO
	- The `reportHours()` method is specified and used by human resources department, which reports to the COO
	- The `save()` method is specified by the database administrators (DBAs), who report to the CT
- Having them coupled like this can cause issues, for example, if `reportHours()` and `calculatePay()` use a method like `regularHours()` for their calculations and CFO requests a change in `regularHours()`, it would affect the COO too and the company could lose a millions of dollars if tested poorly after the change.

### Symptom 2: Merges
- Two different developers, possibly from two different teams, check out the Employee class and begin to make changes. Unfortunately their changes collide. The result is a
merge and that is a risky affair.
- One solution is to **seperate code that supports different actors**.

### Solutions
- One way to solve the above `Employee` class is to seperate data from functions.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/b995830e-bfce-4175-9f40-85ab6251be4d)

- However, the downside of this is developers now have 3 classes they have to instantiate and track. A common solution here is to use the Facade pattern.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/db22acdc-3bae-48d7-91c0-9b0e825b9397)

- The `EmployeeFacade` contains very little code, so the solution is to keep the most important method in the original `Employee` class and then using that class as a Facade for the lesser functions.

# <a name="open-closed-principle">8. OCP: The Open-Closed Principle</a>

- Definition: *A software artifact should be open for extension but closed for modification*.
- In other words, the behavior of a software artifact ought to be extendible, without having to modify that artifact.
- Most students of software design recognize the OCP as a principle that guides them in the design of classes and modules.

## Example: Financial Summary System
- We have a website that: displays financial summary, is scrollable, displays negative numbers in red color.
- A request was made to turn the same summary into a report to be printed on a black-and-white printer.
- A good software architecture here would reduce the amount of changed code by separating the things that change for different reasons (SRP), and then organizing the dependencies between those things properly (DIP)

### SRP Approach
- By applying SRP, we might come up with the data-flow:

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/a8b7c991-4725-4898-a67e-0e0d829b8701)

- The essential insight here is that generating the report involves 2 separate responsibilities:
  - The calculation of the reported data
  - The presentation of the data into a web and printer-friendly form

 ![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/a0ea086a-074c-4470-9e5b-69ea6695f346)

- We accomplish this by partitioning the processes into classes, and separating those
classes into components, as shown by the double lines in the diagram
- Classes marked with `<I>` are interfaces; those marked with `<DS>` are data structures. Open arrowheard are *using* relationships; closed arrowheads are *implements* or *inheritance* relationships.
- `FinancialDataMapper` knows about `FinancialDataGateway` through an implements relationship, but `FinancialGateway` knows nothing at all about `FinancialDataMapper`.
- Notice that each double line is crossed in one direction only. This means that all component relationships are unidirectional. These arrows point towards the components that we want to protect from change.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/4f4be0bc-0aec-4ec1-b19d-b78b1d2571a0)

- So if component A should be protected from changes in component B, then **component B should depend on component A**.
- We want to protect the *Controller* from changes in the *Presenters*. We want to protect the *Presenters* from changes in the *Views*. We want to protect the *Interactor* from changes in-—well, anything.
- **Architects separate functionality based on how, why, and when it changes, and then organize that separated functionality into a hierarchy of components. Higher-level components in that hierarchy are protected from the changes made to lower-level components.**

### Information Hiding
- The `FinancialReportRequester` interface purpose is to protect the `FinancialReportController` from knowing too much about the `Interactor`.
- If the interface were not there, the *Controller* would have transitive dependencies on the `FinancialEntities`.
- Our first priority is to protect the *Interactor* from changes to the *Controller*, but we also want to protect the *Controller* from changes to the *Interactor* by hiding the internals of the *Interactor*.

# <a name="liskov-substitution-principle">9. LSP: The Liskov Substitution Principle</a> 
