# Clean Architecture
My notes from the book *Clean Architecture: A Craftsman's Guide to Software Structure and Design* - by *Robert C. Martin*

# Index

1. [What is Design and Architecture](#what-is-design-architecture)
2. [A Tale of Two Values](#tale-of-two-values)
3. [Paradigm Overview](#paradigm-overview)
4. [Structured Programming](#structured-programming)
5. [Object Orient Programming](#object-oriented-programming)
6. [Functional programming](#functional-programming)

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
