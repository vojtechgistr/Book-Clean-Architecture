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
10. [ISP: The Interface Segregation Principle](#interface-segregation-principle)
11. [DIP: The Dependency Inversion Principle](#dependency-inversion-principle)

IV. [Component Principles](#component-principles)

12. [Components](#components)
13. [Component Cohesion](#component-cohesion)
14. [Component Coupling](#component-coupling)
15. [What Is Architecture?](#what-is-architecture)
16. [Independence](#independence)
17. [Boundaries: Drawing Lines](#boundaries-drawing-lines)
18. [Boundary Anatomy](#boundary-anatomy)
19. [19. Policy and Level](#policy-and-level)
20. [20. Business Rules](#business-rules)
21. [21. Screaming Architecture](#screaming-architecture)
22. [22. The Clean Architecture](#the-clean-architecture)
23. [23. Presenters and Humble Objects](#presenters-and-humble-objects)
24. [24. Partial Boundaries](#partial-boundaries)

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
- In other words:
  - the behavior of a software artifact ought to be extendible, without having to modify that artifact.
  - = the behavior of software entities (classes, modules, functions, etc.) ought to be extendible, without modifying those entities.
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
- We want to protect the *Controller* from changes in the *Presenters*. We want to protect the *Presenters* from changes in the *Views*. We want to protect the *Interactor* from changes in-well, anything.
- **Architects separate functionality based on how, why, and when it changes, and then organize that separated functionality into a hierarchy of components. Higher-level components in that hierarchy are protected from the changes made to lower-level components.**

### Information Hiding
- The `FinancialReportRequester` interface purpose is to protect the `FinancialReportController` from knowing too much about the `Interactor`.
- If the interface were not there, the *Controller* would have transitive dependencies on the `FinancialEntities`.
- Our first priority is to protect the *Interactor* from changes to the *Controller*, but we also want to protect the *Controller* from changes to the *Interactor* by hiding the internals of the *Interactor*.

# <a name="liskov-substitution-principle">9. LSP: The Liskov Substitution Principle</a> 

- *What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T.*
- A child class should be able to do everything that a parent class can.

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
- **Thus when we depend on a component, we want to make sure we depend on every class in that component. Put another way, we want to make sure that the classes that we put into a component are inseparable—that it is impossible to depend on some and not on the others.**

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
- In software, many factors can make a component hard to change—for example, its size, complexity, and clarity, among other characteristics.

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

- Definition: *A component should be abstract as it is stable.*
- SAP sets up a relationship between stability and abstractness - it says:
  - stable component should be abstract - easily extended
  - unstable component should be concrete - easily changed
- Thus *dependencies run in the direction of abstraction.*

### Where do we put the high-level policy?

- The software that encapsulates the high-level policies of the system should be placed into stable components (I = 0).
- Unstable components (I = 1) should contain only the software that is volatile—software that we want to be able to quickly and easily change.
- However, if the high-level policies are placed into stable components, then the source code that represents those policies will be difficult to change.
- The answer is found in OCP. This principle tells us that it is possible and desirable to create classes that are flexible enough to be extended without requiring modification - Abstract Classes.

### Measuring Abstraction

- The *A* metric is a measure of the abstractness of a component.
  - *Nc*: The number of classes in the component.
  - *Na*: The number of abstract classes and interfaces in the component.
  - *A*: Abstractness. `A = Na ÷ Nc`.
- *A* metric ranges from 0 (no abstract classes at all) to 1 (contains nothing but abstract classes).

### The Main Sequence

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/62227b97-19c9-4563-b75e-dcfe6db33bfc)

- Since we cannot enforce a rule that all components sit at either (0, 1) or (1, 0), we must assume that there is a locus of points on the *A/I* graph that defines reasonable positions for components.
- A component that sits on the Main Sequence is not “too abstract” for its stability, nor is it “too unstable” for its abstractness. It is neither useless nor particularly painful. It is depended on to the extent that it is abstract, and it depends on others to the extent that it is concrete.
- Good architects strive to position the majority of their components at one of the two endpoints of the Main
Sequence.
- However, in my experience, some small fraction of the components in a large system are neither perfectly abstract nor perfectly stable. Those components have the best characteristics if they are on, or close, to the Main Sequence.

### Distance from the Main Sequence

- *D*: Distance. `D = |A+I–1|`. The range of this metric is `[0, 1]`. A value of `0` indicates
that the component is directly on the Main Sequence. A value of `1` indicates that the
component is as far away as possible from the Main Sequence.
- Any component that has a *D* value that is **not near zero** can be reexamined and restructured.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/4fdb0304-518a-4926-8f05-a71823f9874f)

- We see that the bulk of the components lie along the Main Sequence, but some of them are more than one standard deviation (Z = 1) away from the mean. These aberrant components are worth examining more closely. For some reason, they are either very abstract with few dependents or very concrete with many dependents.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/b06a131d-10c9-43cf-81f0-af898a256946)

- Another way to use the metrics is to plot the D metric of each component over time.

# <a name="what-is-architecture">15. What Is Architecture?</a>

- Software architects may not write as much code as other programmers do, but they continue to engage in programming tasks.
- The architecture of a software system is the shape given to that system by those who build it. The form of that shape is in the division of that system into components, the arrangement of those components, and the ways in which those components communicate with each other.
- The purpose of that shape is to facilitate the development, deployment, operation, and maintenance of the software system contained within it.
- Good architecture makes the system easy to understand, easy to develop, easy to maintain, and easy to deploy.
- The ultimate goal is to minimize the lifetime cost of the system and to maximize programmer productivity.

## Keeping Options Open

- All software systems can be decomposed into two major elements: *policy* and *details*.
- The policy element embodies all the business rules and procedures. The policy is where the true value of the system lives.
- The details are those things that are necessary to enable humans, other systems, and programmers to communicate with the policy, but that do not impact the behavior of the policy at all. They include IO devices, databases, web systems, servers, frameworks, communication protocols, and so forth.

# <a name="independence">16. Independence</a>

- The decoupling of the use cases and layers also affords a high degree of flexibility in deployment. Indeed, if the decoupling is done well, then it should be possible to hotswap layers and use cases in running systems. Adding a new use case could be a simple as adding a few new jar files or services to the system while leaving the rest alone.

## Use Cases

- Means that the architecture of the system must support the intent of the system. If the system is a shopping cart application, then the architecture must support shopping cart use cases. Indeed, this is the first concern of the architect, and the first priority of the architecture.
- However, architecture does not wield much influence over the behavior of the system. There are very few behavioral options that the architecture can leave open. But influence isn’t everything.
- **The most important thing a good architecture can do to support behavior is to clarify and expose that behavior so that the intent of the system is visible at the architectural level.**

## Duplication

- Architects often fall into a trap—a trap that hinges on their fear of duplication.
- There is true duplication, in which every change to one instance necessitates the same change to every duplicate of that instance.
- Then there is false or accidental duplication. If two apparently duplicated sections of code evolve along different paths—if they change at different rates, and for different reasons—then they are not true duplicates. Return to them in a few years, and you’ll find that they are very different from each other.

## Decoupling modes

  - **Source level**. We can control the dependencies between source code modules so that changes to one module do not force changes or recompilation of others (e.g., Ruby Gems). In this decoupling mode the components all execute in the same address space, and communicate with each other using simple function calls. There is a single executable loaded into computer memory. People often call this a monolithic structure.
  - **Deployment level**. We can control the dependencies between deployable units such as jar files, DLLs, or shared libraries, so that changes to the source code in one module do not force others to be rebuilt and redeployed. Many of the components may still live in the same address space, and communicate through function calls. Other components may live in other processes in the same processor, and communicate through interprocess communications, sockets, or shared memory. The important thing here is that the decoupled components are partitioned into independently deployable units such as jar files, Gem files, or DLLs.
  - **Service level**. We can reduce the dependencies down to the level of data structures, and communicate solely through network packets such that every execution unit is entirely independent of source and binary changes to others (e.g., services or microservices).

- As the project matures, the optimal mode may change.


# <a name="boundaries-drawing-lines">17. Boundaries: Drawing Lines</a>

- Which kinds of decisions are premature? Decisions that have nothing to do with the business requirements—the use cases—of the system.
- These include decisions about frameworks, databases, web servers, utility libraries, dependency injection, and the like.
- A good system architecture:
  - Is one in which decisions like these are rendered ancillary and deferrable.
  - A good system architecture does not depend on those decisions.
  - A good system architecture allows those decisions to be made at the latest possible moment, without significant impact.
- To draw boundary lines in a software architecture, you first partition the system into components. Some of those components are core business rules; others are plugins that contain necessary functions that are not directly related to the core business. Then you arrange the code in those components such that the arrows between them point in one direction—toward the core business.
- You should recognize this as an application of the Dependency Inversion Principle and the Stable Abstractions Principle. Dependency arrows are arranged to point from lower-level details to higher-level abstractions.

## Which lines do you draw, and when do you draw them?

- You draw lines between things that matter and things that don’t.
- The GUI doesn’t matter to the business rules, so there should be a line between them. The database doesn’t
matter to the GUI, so there should be a line between them. The database doesn’t matter to the business rules, so there should be a line between them
- Where is the boundary line? The boundary is drawn across the inheritance relationship, just below the `DatabaseInterface`.
- **Boundaries are drawn where there is an axis of change. The components on one side of the boundary change at different rates, and for different reasons, than the components on the other side of the boundary.**

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/9b143f89-ec53-44b3-9a5d-748eae6e4b93)

- The `BusinessRules` use the `DatabaseInterface` to load and save data. The `DatabaseAccess` implements the
interface and directs the operation of the actual `Database`.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/564e5f91-1ca4-4b5b-b44a-e1c3df4d35ca)

- Note the two arrows leaving the `DatabaseAccess` class. Those two arrows point away from the `DatabaseAccess` class. That means that none of these classes knows that the `DatabaseAccess` class exists.
- Now because of this, we can use any database we want without affecting the business rules.

## What about input and output?

- The IO is irrelevant.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/d7ec8773-adcb-4435-a63c-850f05e9cd1b)

- Once again, we see that the less relevant component depends on the more relevant component. The arrows show which component knows about the other and, therefore, which component cares about the other. The `GUI` cares about the `BusinessRules`.
- Having drawn this boundary and this arrow, we can now see that the `GUI` could be replaced with any other kind of interface—and the `BusinessRules` would not care.

## Plugin Architecture

- Indeed, the history of software development technology is the story of how to conveniently create plugins to establish a scalable and maintainable system architecture.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/0c6e6878-434a-4922-964b-1a8a053c763f)

- Because the user interface in this design is considered to be a plugin, we have made it possible to plug in many different kinds of user interfaces.
- They could be web based, client/server based, SOA based, Console based, or based on any other kind of user interface technology.

# <a name="boundary-anatomy">18. Boundary Anatomy</a>

- The architecture of a system is defined by a set of software components and the boundaries that separate them. Those boundaries come in many different forms.
- Most systems, other than monoliths, use more than one boundary strategy.

## Boundary Crossing

- At runtime, a boundary crossing is nothing more than a function on one side of the boundary calling a function on the other side and passing along some data.
- The trick to creating an appropriate boundary crossing is to manage the source code dependencies.
- Managing and building firewalls against this change is what boundaries are all about.

## the Dreaded Monolith

- The simplest and most common of the architectural boundaries has no strict physical representation. It is simply a disciplined segregation of functions and data within a single processor and a single address space.
- The simplest possible boundary crossing is a function call from a low-level client to a higher-level service. Both the runtime dependency and the compile-time dependency point in the same direction, toward the higher-level component.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/4b25df6a-470a-4bbe-b516-04107ced6f74)

- When a high-level client needs to invoke a lower-level service, dynamic polymorphism is used to invert the dependency against the flow of control. The runtime dependency opposes the compile-time dependency.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/aeaf8db5-cee6-44cf-82c2-fcbd01e5de6e)

- Note, however, that all dependencies cross the boundary from right to left toward the higher-level component. Note, also, that the definition of the data structure is on the calling side of the boundary.
- Communications between components in a monolith are very fast and inexpensive. They are typically just function calls. Consequently, communications across source-level decoupled boundaries can be very chatty.

## Deployment Components

- Deployment does not involve compilation. Instead, the components are delivered in binary, or some equivalent deployable form.
- This is the deployment-level decoupling mode
- The act of deployment is simply the gathering of these deployable units together in some convenient form, such as a WAR file, or even just a directory.
- With that one exception, deployment-level components are the same as monoliths.
- Communications across these boundaries can still be very chatty.

## Threads

- Both monoliths and deployment components can make use of threads.
- Threads are not architectural boundaries or units of deployment, but rather a way to organize the schedule and order of execution.
- They may be wholly contained within a component, or spread across many components.

## Local Processes

- A much stronger physical architectural boundary is the local process.
- A local process is typically created from the command line or an equivalent system call. Local processes run in the same processor, or in the same set of processors within a multicore, but run in separate address spaces.
- Memory protection generally prevents such processes from sharing memory, although shared memory partitions are often used.
- Most often, local processes communicate with each other using sockets, or some other kind of operating system communications facility such as mailboxes or message queues
- Each local process may be a statically linked monolith, or it may be composed of dynamically linked deployment components.
- The source code of the higher-level processes must not contain the names, or physical addresses, or registry lookup keys of lower-level processes.
- Communication across local process boundaries involve operating system calls, data marshaling and decoding, and interprocess context switches, which are moderately expensive. Chattiness should be carefully limited.

## Services

- The strongest boundary is a service.
- A service is a process, generally started from the command line or through an equivalent system call.
- Services do not depend on their physical location.
- Two communicating services may, or may not, operate in the same physical processor or multicore.
- The services assume that all communications take place over the network.
- Care must be taken to avoid chatting where possible. Communications at this level must deal with high levels of latency.

# <a name="policy-and-level">19. Policy and Level</a>

- Software systems are statements of policy. A computer program is a detailed description of the policy by which inputs are transformed into outputs.
- In most nontrivial systems, that policy can be broken down into many different smaller statements of policy. Some of those statements will describe how particular business rules are to be calculated. Others will describe how certain reports are to be formatted. Still others will describe how input data are to be validated.

## Level

- A strict definition of “level” is “the distance from the inputs and outputs.”
- The farther a policy is from both the inputs and the outputs of the system, the higher its level. The policies that manage input and output are the lowest-level policies in the system.
- Lower-level components should be plugins to the higher-level components.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/dbd8d411-e811-436c-a66b-16ffeb3e950c)


# <a name="business-rules">20. Business Rules</a>

- The business rules should remain pristine, unsullied by baser concerns such as the user interface or database used.
- Ideally, the code that represents the business rules should be the heart of the system, with lesser concerns being plugged in to them.
- The business rules should be the most independent and reusable code in the system.
- Strictly speaking, business rules are rules or procedures that make or save the business money. Very strictly speaking, these rules would make or save the business money, irrespective of whether they were implemented on a computer. They would make or save money even if they were executed manually.
- Critical Business Rules usually require some data to work with.
- For example, our loan requires a loan balance, an interest rate, and a payment schedule. We shall call this data Critical Business Data. This is the data that would exist even if the system were not automated.
- The critical rules and critical data are inextricably bound, so they are a good candidate for an object. We’ll call this kind of object an **Entity**.

## Entities

- An Entity is an object within our computer system that embodies a small set of critical business rules operating on Critical Business Data.
- The Entity object either contains the Critical Business Data or has very easy access to that data.
- The interface of the Entity consists of the functions that implement the Critical Business Rules that operate on that data.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/55130287-cd56-4250-aac1-412caa37b820)

- This class stands alone as a representative of the business.
- It is unsullied with concerns about databases, user interfaces, or third-party frameworks.
- It could serve the business in any system, irrespective of how that system was presented, or how the data was stored, or how the computers in that system were arranged.
- **The Entity is pure business and nothing else.**

## Use Cases

- A use case is an object. It has one or more functions that implement the application-specific business rules. It also has data elements that include the input data, the output
data, and the references to the appropriate Entities with which it interacts.
- Entities have no knowledge of the use cases that control them.
- Why are Entities high level and use cases lower level? Because use cases are specific to a single application and, therefore, are closer to the inputs and outputs of that system.

## Request and Response Models

- Well-formed use case object should have no inkling about the way that data is communicated to the user, or to any other component. We certainly don’t want the code within the use case class to know about HTMLor SQL!
- The use case class accepts simple request data structures for its input, and returns simple response data structures as its output.
- These data structures are not dependent on anything. They do not derive from standard framework interfaces such as `HttpRequest` and `HttpResponse`. They know nothing of the web, nor do they share any of the trappings of whatever user interface might be in place.
- This lack of dependencies is critical.

# <a name="screaming-architecture">21. Screaming Architecture</a>

- If you look at the system architecture plans, you should see what it *screams*. It's like a plan of a house, when you look at it, it screams (you understand) "LIBRARY", or "HOME".
- Architectures are not (or should not be) about frameworks. Architectures should not be supplied by frameworks.
- Frameworks are tools to be used, not architectures to be conformed to.
- If your architecture is based on frameworks, then it cannot be based on your use cases.
- Your system architecture should be as ignorant as possible about how it will be delivered. You should be able to deliver it as a console app, or a web app, or a thick client app, or even a web service app, without undue complication or change to the fundamental architecture.

## Testability

- If your system architecture is all about the use cases, and if you have kept your frameworks at arm’s length, then you should be able to unit-test all those use cases without any of the frameworks in place.
- You shouldn’t need the web server running to run your tests.
- You shouldn’t need the database connected to run your tests.
- Your Entity objects should be plain old objects that have no dependencies on frameworks or databases or other complications.

# <a name="the-clean-architecture">22. The Clean Architecture</a>

- Every clean architecture produces systems that have the following characteristics:
  - **Independent of frameworks.** The architecture does not depend on the existence of some library of feature-laden software. This allows you to use such frameworks as tools, rather than forcing you to cram your system into their limited constraints.
  - **Testable.** The business rules can be tested without the UI, database, web server, or any other external element.
  - **Independent of the UI.** The UI can change easily, without changing the rest of the system. A web UI could be replaced with a console UI, for example, without changing the business rules.
  - **Independent of the database.** You can swap out Oracle or SQLServer for Mongo, BigTable, CouchDB, or something else. Your business rules are not bound to the database.
  -**Independent of any external agency.** In fact, your business rules don’t know anything at all about the interfaces to the outside world.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/86135e9c-fcb0-4b01-a93a-c4eebe2803ef)

- Nothing in an inner circle can know anything at all about something in an outer circle. In particular, the name of something declared in an outer circle must not be mentioned by the code in an inner circle. That includes functions, classes, variables, or any other named software entity.
- The circles in Figure 22.1 are intended to be schematic: You may find that you need more than just these four. There’s no rule that says you must always have just these four. However, the Dependency Rule always applies.
- The outermost circle consists of low-level concrete details. As you move inward, the software grows more abstract and encapsulates higher-level policies.

## Entities

- Entities encapsulate enterprise-wide Critical Business Rules. An entity can be an object with methods, or it can be a set of data structures and functions.
- If you don’t have an enterprise and are writing just a single application, then these entities are the business objects of the application. They encapsulate the most general and high-level rules.
- They are the least likely to change when something external changes. No operational change to any particular application should affect the entity layer.

## Use cases

- The software in the use cases layer contains application-specific business rules.
- These use cases orchestrate the flow of data to and from the entities, and direct those entities to use their Critical Business Rules to achieve the goals of the use case.
- We do not expect changes in this layer to affect the entities. We also do not expect this layer to be affected by changes to externalities such as the database, the UI, or any of the common frameworks.
- We do, however, expect that changes to the operation of the application will affect the use cases and, therefore, the software in this layer. If the details of a use case change, then some code in this layer will certainly be affected.

## Interface Adapters

- The software in the interface adapters layer is a set of adapters that convert data from the format most convenient for the use cases and entities, to the format most convenient for some external agency such as the database or the web.
- Similarly, data is converted, in this layer, from the form most convenient for entities and use cases, to the form most convenient for whatever persistence framework is being used (i.e., the database).
- No code inward of this circle should know anything at all about the database. If the database is a SQL database, then all SQL should be restricted to this layer.

## Frameworks and Drivers

- The frameworks and drivers layer is where all the details go. The web is a detail. The is a detail. We keep these things on the outside where they can do little harm.

## Crossing Boundaries

- For example, suppose the use case needs to call the presenter.
- This call must not be direct because that would violate the Dependency Rule: No name in an outer circle can be mentioned by an inner circle.
- So we have the use case call an interface (shown in Figure 22.1 as “use case output port”) in the inner circle, and have the presenter in the outer circle implement it.

## Which data crosses the boundaries?

- Typically the data that crosses the boundaries consists of simple data structures.
- You can use basic structs or simple data transfer objects if you like. Or the data can simply be arguments in function calls. Or you can pack it into a hashmap, or construct it into an object.
- **The important thing is that isolated, simple data structures are passed across the boundaries.**
- We don’t want to cheat and pass Entity objects or database rows.
- We don’t want the data structures to have any kind of dependency that violates the Dependency Rule.

![image](https://github.com/DaRealAdalbertBro/Book-Clean-Architecture/assets/56306485/9713cb6b-5cbf-4e1e-be06-139a7119f1ac)

# <a name="presenters-and-humble-objects">23. Presenters and Humble Objects</a>

## The Humble Object Pattern

- Design pattern that was originally identified as a way to help unit testers to separate behaviors that are hard to test from behaviors that are easy to test.
- The idea is very simple: *Split the behaviors into two modules or classes.*
- One of those modules is humble; it contains all the hard-to-test behaviors stripped down to their barest essence. The other module contains all the testable behaviors that were stripped out of the humble object.
- For exampl GUI is the Humble Object (View) - hard to test, but GUI behavior is easy to test (Presenter).
- Using the *Humble Object pattern*, we can separate these two kinds of behaviors into two different classes **called the Presenter and the View**.
- At each architectural boundary, we are likely to find the Humble Object pattern lurking somewhere nearby.

# <a name="partial-boundaries">24. Partial Boundaries</a>

- In many situations, a good architect might judge that the expense of such a boundary is too high—but might still want to hold a place for such a boundary in case it is needed later.
- This kind of anticipatory design is often frowned upon by many in the Agile community as a violation of YAGNI: “You Aren’t Going to Need It.” Architects, however, sometimes look at the problem and think, “Yeah, but I might.” In that case, they may implement a partial boundary.
- The full-fledged architectural boundary uses reciprocal boundary interfaces to maintain isolation in both directions.

## Skip The Last Step
- One way to construct a partial boundary is to do all the work necessary to create independently compilable and deployable components, and then simply keep them together in the same component.
- The reciprocal interfaces are there, the input/output data structures are there, and everything is all set up—but we compile and deploy all of them as a single component.

## One-Dimensional boundaries

![image](https://github.com/vojtechgistr/Book-Clean-Architecture/assets/56306485/0695bd83-7ae0-460e-94d1-f6054e1d804c)

- A simpler structure that serves to hold the place for later extension to a fullfledged boundary is shown in Figure 24.1.
-  The necessary dependency inversion is in place in an attempt to isolate the `Client` from the `ServiceImpl`.
-  It should also be clear that the separation can degrade pretty rapidly, as shown by the nasty dotted arrow in the diagram.

## Facades

- An even simpler boundary is the Facade pattern, dependency inversion is sacrificed.
- The boundary is simply defined by the Facade class, which lists all the services as methods, and deploys the service calls to classes that the client is not supposed to access.

![image](https://github.com/vojtechgistr/Book-Clean-Architecture/assets/56306485/3d32e1a3-84b6-4205-8025-ec51d2a4a8e0)

- Note, however, that the Client has a transitive dependency on all those service classes.
- In static languages, a change to the source code in one of the `Service` classes will force the `Client` to recompile.
- Also, you can imagine how easy backchannels are to create with this structure.
