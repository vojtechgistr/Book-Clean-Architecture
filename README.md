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
9. [LSP: The Liskov Substitution Principle](#liskov-substitution-principle))
10. [ISP: The Interface Segregation Principle](#interface-segregation-principle)
11. [DIP: The Dependency Inversion Principle](#dependency-inversion-principle)

IV. [Component Principles](#component-principles)

12. [Components](#components)
13. [Component Cohesion](#component-cohesion)
13. [Component Coupling](#component-coupling)

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

- *What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T*

## Good Example:

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/a8b953ba-5171-47f7-93f3-a1aaaa56abb8)

- This design conforms to the LSP because the behavior of the `Billing` application does not depend, in any way, on which of the two subtypes it uses.
- Both of the subtypes are substitutable for the `License` type.

## Bad Example:

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/03895447-5cdb-4a85-8bf9-52fea9a4cb3c)

- In this example, Square is not a proper subtype of Rectangle because the height and width of the Rectangle are independently mutable.
- In contrast, the height and width of the Square must change together.
- Since the User believes it is communicating with a Rectangle, it could easily get confused.

# <a name="interface-segregation-principle">10. ISP: The Interface Segregation Principle</a>

- Depending on something that carries baggage that you don't need can cause you troubles that you didn't expect.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/4c9e4f96-2a5a-430c-8e4f-909db1691dec)

- Imagine that `OPS` is a class written in a statically typed language like Java.
- Clearly, in that case, the source code of `User1` will inadvertently depend on `op2` and `op3`, even though it doesn’t call them.
- This dependence means that a change to the source code of `op2` in `OPS` will force `User1` to be recompiled and redeployed, even though nothing that it cared about has actually changed.
- This problem can be resolved by segregating the operations into interfaces as shown below:

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/fddaaeb2-3f76-459f-a0c2-ce88c4fbcc3d)

- The source code of User1 will depend on `U1Ops`, and `op1`, but will not depend on `OPS`.
- Thus a change to `OPS` that `User1` does not care about will not cause `User1` to be recompiled and redeployed.

# <a name="dependency-inversion-principle">11. DIP: The Dependency Inversion Principle</a>

- The DIP tells us that the most flexible systems are those in which source code dependencies refer only to abstractions, not to concretions.
- In a statically typed language, like Java, this means that the `use`, `import`, and `include` statements should refer only to source modules containing interfaces, abstract classes, or some other kind of abstract declaration. **Nothing concrete should be depended on.**
- This only applies to *volatile* concrete elements of our system - for example in Java, we don't care about very stable classes like `String`.

## Stable Abstractions

- Interfaces are less volatile than implementations.
- Coding practices:
  - **Don’t refer to volatile concrete classes.** Refer to abstract interfaces instead. This rule applies in all languages, whether statically or dynamically typed. It also puts severe constraints on the creation of objects and generally enforces the use of *Abstract Factories*.
  - **Don’t derive from volatile concrete classes.** This is a corollary to the previous rule, but it bears special mention. In statically typed languages, inheritance is the strongest, and most rigid, of all the source code relationships; consequently, it should be used with great care. In dynamically typed languages, inheritance is less of a problem, but it is still a dependency—and caution is always the wisest choice.
  - **Don’t override concrete functions.** Concrete functions often require source code dependencies. When you override those functions, you do not eliminate those dependencies—indeed, you inherit them. To manage those dependencies, you should make the function abstract and create multiple implementations.
  - **Never mention the name of anything concrete and volatile.** This is really just a restatement of the principle itself.

## Factories

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/9f57940e-42ba-452a-a86e-69d03e7c02ce)

- The curved line is an architectural boundary. It separates the abstract from the concrete.
- The `Application` uses the `ConcreteImpl` through the `Service` interface. However, the `Application` must somehow create instances of the `ConcreteImpl`. To achieve this without creating a source code dependency on the `ConcreteImpl`, the `Application` calls the `makeSvc` method of the `ServiceFactory` interface. This method is implemented by the `ServiceFactoryImpl` class, which derives from `ServiceFactory`. That implementation instantiates the `ConcreteImpl` and returns it as a `Service`.
- The concrete component in Figure 11.1 contains a single dependency, so it violates the DIP. This is typical. DIP violations cannot be entirely removed, but they can be gathered into a small number of concrete components and kept separate from the rest of the system.


# <a name="component-principles">IV. Component Principles</a>

- If the SOLID principles tell us how to arrange the bricks into walls and rooms, then the
component principles tell us how to arrange the rooms into buildings. Large software
systems, like large buildings, are built out of smaller components.


# <a name="components">12. Components</a>

- Components are the units of deplyoment - the smallest entities that can be deployed as a part of a system.
- For example in Java - .jar files, in Ruby - gem files, in .NET - DLLs.
- They can be:
  - Linked together into a single executable.
  - Aggregated together into a single archive, such as a `.war` file.
  - Independently deployed as separate dynamically loaded plugins, such as `.jar` / `.dll` / `.exe` files.
- Regardless of how they are eventually deployed, well-designed components always retain the ability to be independently deployable and, therefore, independently developable.

# <a name="component-cohesion">13. Component Cohesion</a>

## REP: The Reuse/Release Equivalence Principle

- Definition: *The granule of reuse is the granule of release*.
- Only components that are released through a tracking system can be effectively reused.
- It's common for developers to be alerted about a new release and decide, based on the changes made in that release, to continue to use the old release or not.
- Components must be separately released, versioned, and tracked to ensure the reusability of the code.
- Without release numbers, there would be no way to ensure that all the reused components are compatible with each other.

## CCP: The Common Closure Principle

- This is the Single Responsibility Principle (SRP) restated for components. Both can be defined as:
  - *Gather together those things that change at the same times and for the same reasons. Separate those things that change at different times or for different reasons.*
- Just as the SRP says that a class should not contain multiple reasons to change, so the Common Closure Principle (CCP) says that a component should not have multiple reasons to change.
- The CCP prompts us to gather together in one place all the classes that are likely to change for the same reasons. This minimizes the workload related to releasing, revalidating, and redeploying the software.
- This principle is closely related to Open Closed Principle (OCP).
- Because 100% closure is not attainable, closure must be strategic. We design our classes such that they are closed to the most common kinds of changes that we expect or have experienced.

## CRP: The Common Reuse Principle

- Definition: *Don't force users of a component to depend on things they don't need*.
- Is yet another principle that helps us to decide which classes and modules should be placed into a component.
- When one component uses another, a dependency is created between the components and because of that dependency, every time the *used* component is changed, the *using* component will likely need corresponding changes.
- **Thus when we depend on a component, we want to make sure we depend on every class in that component. Put another way, we want to make sure that the classes that we put into a component are inseparable --that it is impossible to depend on some and not on the others.**

### Relation to ISP

- The ISP advises us not to depend on classes that have methods we don't use. The CRP advises us not to depeend on components that have classes we don't use.

## The Tension Diagram for Component Cohesion

- Three cohesion principles tend to finght each other. The REP and CCP are *inclusive* - both tend to make components larger.
- The CRP is an *exclusive* principle, driving components to be smaller.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/de819c24-d92d-44cf-97f8-5a4888b03bb2)

- An architect who focuses on just the REP and CRP will find that too many components are impacted when simple changes are made. In contrast, an architect who focuses too strongly on the CCP and REP will cause too many unneeded releases to be generated.

# <a name="component-coupling">14. Component Coupling</a>

- The forces that impinge upon the architecture of a component structure are technical, political, and volatile.

## ADP: The Acyclic Dependencies Principle

- *Allow no cycles in the component dependency graph*
- "morning after syndrome" = you come back to work in the morning but the code that worked yesterday doesn't work, because somebody stayed longer than you and made changes to it's dependencies.
- The "morning after syndrome" occurs in development environments where many developers are modifying the same source files.
- Solution to this is to partition the development environment into releasable components, which become units of work that can be responsibility of a single developer, or a team of developers.
- By this, each team can decide for itself when to adapt its own components to new releases of the components.
- To make it work successfully, **you must manage the dependency structure of components. There can be no cycles**

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/5138613c-b6cc-4a14-b90c-a0ef2d480ac6)

- This structure has no cycles = it is directed acyclic graph (DAG).
- We can find out who will be affected by changing a component, for example `Presenters`, you just follow dependency arrows backward. Thus `View` and `Main` will both be affected.

### The Effect of a cycle component

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/528b6a49-8103-4817-a6bd-47025b7b38bd)

- This cycle creates some immediate problems. For example, the developers working on the `Database` component know that to release it, the component must be compatible with `Entities`. However, with the cycle in place, the `Database` component must now also be compatible with `Authorizer`. But `Authorizer` depends on `Interactors`.
- This makes `Database` much more difficult to release. `Entities`, `Authorizer`, and `Interactors` have, in effect, become one large component.

### Breaking the cycle

- There are two primary mechanisms for breaking a cycle:
  - Apply the Dependency Inversion Principle (DIP). In the case in Figure 14.3, we could create an interface that has the methods that `User` needs. We could then put that interface into `Entities` and inherit it into `Authorizer`. This inverts the dependency between `Entities` and `Authorizer`, thereby breaking the cycle.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/334d620e-83e6-4572-b9bb-2ccc75c9b0ae)

  - Create a new component that both `Entities` and `Authorizer` depend on. Move the class(es) that they both depend on into that new component.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/f1862c4f-bc0a-415a-9be2-5831d8f5a5cf)

## Top-Down Design

- The component structure cannot be designed from top down. It is not one of the first things about the system that is designed, but rather evolves as the system grows and changes.
- Components has very little to do with the function of the application. Instead, they are a map to the *buildability* and *maintainability* of the application.
- **That's why they are not designed at the beginning of the project. There is no software to build or maintain, so there is no need for a build and maintainance map.**
- But as more and more modules accumulate in the early stages of implementation and design, there is a growing need to manage the dependencies so that the project can be developed without the "morning after syndrome".
- Moreover, we want to keep changes as localized as possible, so we start paying attention to the SRP and CCP and collocate classes that are likely to change together.
- **If we tried to design the component dependency structure before we designed any classes, we would likely fail rather badly. We would not know much about common closure, we would be unaware of any reusable elements, and we would almost certainly create components that produced dependency cycles. Thus the component dependency structure grows and evolves with the logical design of the system.**

## SDP: The Stable Dependencies Principle

- By conforming to the SDP, we ensure that modules that are intended to be easy to change are not dependend on by modules that are harder to change.

### Stability

- Stability is related to amount of work required to make a change - it is "not easily moved".
- In software, many factors can make a component hard to change --for example, its size, complexity, and clarity, among other characteristics.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/e49eb0cc-fc46-4839-94c7-b4cabe3e5002)

- `X` is a stable component. Three components depend on `X`, so it has three good reasons not to change. We say that `X` is responsible to those three components.
- Conversely, `X` depends on nothing, so it has no external influence to make it change. We say it is independent.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/c06b24f6-f071-4548-957f-0ae77d6a62dc)

- `Y` is a very unstable component. No other components depend on `Y`, so we say that it is irresponsible.
- `Y` also has three components that it depends on, so changes may come from three external sources. We say that Y is dependent.

### Stability Metrics

- How can we measure the stability of a component? One way is to count the number of dependencies that enter and leave that component. These counts will allow us to calculate the positional stability of the component.
  - *Fan-in*: Incoming dependencies. This metric identifies the number of classes outside this component that depend on classes within the component.
  - *Fan-out*: Outgoing depenencies. This metric identifies the number of classes inside this component that depend on classes outside the component.
  - *I*: Instability: `I = Fan-out / (Fan-in + Fan-out)`. This metric has the range `[0, 1]`. `I = 0` indicates a maximally stable component. `I = 1` indicates a maximally unstable component.
- The SDP says that the *I* metric of a component should be larger than the *I* metrics of the comonents that it depends on = *I* metrics should *decrease* in the direction of dependency.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/9c7d4a3b-c01e-49e8-8cb2-30ac4e488767)

### Not all components should be stable

- If all the components in a system were maximally stable, the system would be unchangeable.
- However, we want to design our component structure so that some components are unstable and some are stable.
- Putting the unstable components at the top of a diagram is a useful convention because any arrow that points *up* is violating the SDP (and we shall see later, the ADP).

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/ad0893d2-d61f-4576-a0b3-4872a51846c1)

- The diagram below shows how the SDP can be violated.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/87a3429d-ca3b-4dec-9244-6534b766981c)

- `Flexible` is a component that we have designed to be easy to change - we want it to be unstable.
- However in component `Stable`, some developer has hung a dependency on `Flexible`. This violates SDP because the *I* metric for `Stable` is much smaller than the *I* metric for `Flexible` = `Flexible` is no longer easy to change because a change will force us to deal with `Stable` and all its dependents.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/9de33918-54c8-48f5-bf12-6b9386d9207c)

- We can fix this by employing the DIP.
- This breaks the dependency of `Stable` on `Flexible` and forces both components to depend on `UServer`.
- `Userver` is very stable (*I = 0*), and `Flexible` retains its necessary instability (*I = 1*). All dependencies now flow in the direction of decreasing *I*

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/5c91a6fb-cd09-40be-bc39-50f25273e02c)

### Abstract Components

- In statically typed languages, it is common, and necessary, to use components that use nothing but an interface.
- These abstract components are very stable and, therefore, are ideal targets for less stable components to depend on.
- In dynamically typed languages, these abstract components doesn't exist at all, nor the dependencies that would have targeted them. Dependency structures in these languages are much simpler because dependency inversion does not require either the declaration or the inheritance of interfaces.

## SAP: The Stable Abstraction Principle

- 
