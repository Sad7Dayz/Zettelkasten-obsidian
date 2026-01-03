# 📖 UML

## 📜 목차
- [[#Diagram|다이어그램]]
- [plantUML사용법](https://frootjy.tistory.com/73)
- [plantUML Web](https://plantuml.com/ko-dark/sequence-diagram)
- [PlantUML가이드](https://pdf.plantuml.net/PlantUML_Language_Reference_Guide_ko.pdf)
- [draw.io 설계](https://www.youtube.com/live/5UTeppzK9zA?si=thSOZkRAcayMO7DV)
- [js2flowchart.js](https://youtu.be/05eYKgmmHXY?si=oI4oMLQ7UMTICz24)
- [js2flowchart.js Market](https://marketplace.visualstudio.com/items?itemName=kostasx.live-html-previewer-v2)
- [CodeToFlowChart](http://www.codetoflow.com/)
- [mermaid FlowChart](https://www.freecodecamp.org/news/how-to-make-flowcharts-with-mermaid/)

---

## Diagram

UML DataType
![[Pasted image 20250329032344.png]]

Full notation
![[Pasted image 20250329190657.png]]

UML Visibility
![[Pasted image 20250329190408.png]]

## UML Class Notation

class notation is a graphical representation used to depict classes and their relationships in object-oriented modeling.

![Class-Notation](https://media.geeksforgeeks.org/wp-content/uploads/20240118123645/Class-Notation.webp)

1. ****Class Name:****
    - The name of the class is typically written in the top compartment of the class box and is centered and bold.
2. ****Attributes:****
    - Attributes, also known as properties or fields, represent the data members of the class. They are listed in the second compartment of the class box and often include the visibility (e.g., public, private) and the data type of each attribute.
3. ****Methods:****
    - Methods, also known as functions or operations, represent the behavior or functionality of the class. They are listed in the third compartment of the class box and include the visibility (e.g., public, private), return type, and parameters of each method.
4. ****Visibility Notation:****
    - Visibility notations indicate the access level of attributes and methods. Common visibility notations include:
        - `+` for public (visible to all classes)
        - `-` for private (visible only within the class)
        - `#` for protected (visible to subclasses)
        - `~` for package or default visibility (visible to classes in the same package)

### ****Parameter Directionality****

In class diagrams, parameter directionality refers to the indication of the flow of information between classes through method parameters. It helps to specify whether a parameter is an input, an output, or both. This information is crucial for understanding how data is passed between objects during method calls.

![class-notation-with-parameter-directionality](https://media.geeksforgeeks.org/wp-content/uploads/20240118123748/class-notation-with-parameter-directionality.jpg)

There are three main parameter directionality notations used in class diagrams:

- ****In (Input):****
    - An input parameter is a parameter passed from the calling object (client) to the called object (server) during a method invocation.
    - It is represented by an arrow pointing towards the receiving class (the class that owns the method).
- ****Out (Output):****
    - An output parameter is a parameter passed from the called object (server) back to the calling object (client) after the method execution.
    - It is represented by an arrow pointing away from the receiving class.
- ****InOut (Input and Output):****
    - An InOut parameter serves as both input and output. It carries information from the calling object to the called object and vice versa.
    - It is represented by an arrow pointing towards and away from the receiving class.
## Relationships between classes

In class diagrams, relationships between classes describe how classes are connected or interact with each other within a system. There are several types of relationships in object-oriented modeling, each serving a specific purpose. Here are some common types of relationships in class diagrams:

![relationship](https://media.geeksforgeeks.org/wp-content/uploads/20240209094815/relationship.webp)

### ****1. Association****

An association represents a bi-directional relationship between two classes. It indicates that instances of one class are connected to instances of another class. Associations are typically depicted as a solid line connecting the classes, with optional arrows indicating the direction of the relationship.

Let’s understand association using an example:

> Let’s consider a simple system for managing a library. In this system, we have two main entities: `Book` and `Library`. Each `Library` contains multiple `Books`, and each `Book` belongs to a specific `Library`. This relationship between `Library` and `Book` represents an association.

The “Library” class can be considered the source class because it contains a reference to multiple instances of the “Book” class. The “Book” class would be considered the target class because it belongs to a specific library.

![association](https://media.geeksforgeeks.org/wp-content/uploads/20240209094944/association.webp)

### 2. Directed Association

A directed association in a UML class diagram represents a relationship between two classes where the association has a direction, indicating that one class is associated with another in a specific way.

- In a directed association, an arrowhead is added to the association line to indicate the direction of the relationship. The arrow points from the class that initiates the association to the class that is being targeted or affected by the association.
- Directed associations are used when the association has a specific flow or directionality, such as indicating which class is responsible for initiating the association or which class has a dependency on another.

> Consider a scenario where a “Teacher” class is associated with a “Course” class in a university system. The directed association arrow may point from the “Teacher” class to the “Course” class, indicating that a teacher is associated with or teaches a specific course.

- The source class is the “Teacher” class. The “Teacher” class initiates the association by teaching a specific course.
- The target class is the “Course” class. The “Course” class is affected by the association as it is being taught by a specific teacher.

![directed-association](https://media.geeksforgeeks.org/wp-content/uploads/20240209095108/directed-association.webp)

### 3. ****Aggregation****

Aggregation is a specialized form of association that represents a “whole-part” relationship. It denotes a stronger relationship where one class (the whole) contains or is composed of another class (the part). Aggregation is represented by a diamond shape on the side of the whole class. In this kind of relationship, the child class can exist independently of its parent class.

Let’s understand aggregation using an example:

> The company can be considered as the whole, while the employees are the parts. Employees belong to the company, and the company can have multiple employees. However, if the company ceases to exist, the employees can still exist independently.

![aggregation](https://media.geeksforgeeks.org/wp-content/uploads/20240209095240/aggregation.webp)

### 4. ****Composition****

Composition is a stronger form of aggregation, indicating a more significant ownership or dependency relationship. In composition, the part class cannot exist independently of the whole class. Composition is represented by a filled diamond shape on the side of the whole class.

Let’s understand Composition using an example:

> Imagine a digital contact book application. The contact book is the whole, and each contact entry is a part. Each contact entry is fully owned and managed by the contact book. If the contact book is deleted or destroyed, all associated contact entries are also removed.

This illustrates composition because the existence of the contact entries depends entirely on the presence of the contact book. Without the contact book, the individual contact entries lose their meaning and cannot exist on their own.

![relationship-(1)](https://media.geeksforgeeks.org/wp-content/uploads/20240212183015/relationship-(1).webp)

### 5. ****Generalization(Inheritance)****

Inheritance represents an “is-a” relationship between classes, where one class (the subclass or child) inherits the properties and behaviors of another class (the superclass or parent). Inheritance is depicted by a solid line with a closed, hollow arrowhead pointing from the subclass to the superclass.

> In the example of bank accounts, we can use generalization to represent different types of accounts such as current accounts, savings accounts, and credit accounts.

The Bank Account class serves as the generalized representation of all types of bank accounts, while the subclasses (Current Account, Savings Account, Credit Account) represent specialized versions that inherit and extend the functionality of the base class.

![generalization](https://media.geeksforgeeks.org/wp-content/uploads/20240209095454/generalization.webp)

### 6. ****Realization (Interface Implementation)****

Realization indicates that a class implements the features of an interface. It is often used in cases where a class realizes the operations defined by an interface. Realization is depicted by a dashed line with an open arrowhead pointing from the implementing class to the interface.

Let’s consider the scenario where a “Person” and a “Corporation” both realizing an “Owner” interface.

- ****Owner Interface:**** This interface now includes methods such as “acquire(property)” and “dispose(property)” to represent actions related to acquiring and disposing of property.
- ****Person Class (Realization):**** The Person class implements the Owner interface, providing concrete implementations for the “acquire(property)” and “dispose(property)” methods. For instance, a person can acquire ownership of a house or dispose of a car.
- ****Corporation Class (Realization):**** Similarly, the Corporation class also implements the Owner interface, offering specific implementations for the “acquire(property)” and “dispose(property)” methods. For example, a corporation can acquire ownership of real estate properties or dispose of company vehicles.

> Both the Person and Corporation classes realize the Owner interface, meaning they provide concrete implementations for the “acquire(property)” and “dispose(property)” methods defined in the interface.

![realization](https://media.geeksforgeeks.org/wp-content/uploads/20240209095608/realization.webp)

### 7. Dependency Relationship

A dependency exists between two classes when one class relies on another, but the relationship is not as strong as association or inheritance. It represents a more loosely coupled connection between classes. Dependencies are often depicted as a dashed arrow.

Let’s consider a scenario where a Person depends on a Book.

- ****Person Class:**** Represents an individual who reads a book. The Person class depends on the Book class to access and read the content.
- ****Book Class:**** Represents a book that contains content to be read by a person. The Book class is independent and can exist without the Person class.

> The Person class depends on the Book class because it requires access to a book to read its content. However, the Book class does not depend on the Person class; it can exist independently and does not rely on the Person class for its functionality.

![dependency](https://media.geeksforgeeks.org/wp-content/uploads/20240209095731/dependency.webp)

### 8. Usage(Dependency) Relationship

A usage dependency relationship in a UML class diagram indicates that one class (the client) utilizes or depends on another class (the supplier) to perform certain tasks or access certain functionality. The client class relies on the services provided by the supplier class but does not own or create instances of it.

- In UML class diagrams, usage dependencies are typically represented by a dashed arrowed line pointing from the client class to the supplier class.
- The arrow indicates the direction of the dependency, showing that the client class depends on the services provided by the supplier class.

> Consider a scenario where a “Car” class depends on a “FuelTank” class to manage fuel consumption.

- The “Car” class may need to access methods or attributes of the “FuelTank” class to check the fuel level, refill fuel, or monitor fuel consumption.
- In this case, the “Car” class has a usage dependency on the “FuelTank” class because it utilizes its services to perform certain tasks related to fuel management.

![usage](https://media.geeksforgeeks.org/wp-content/uploads/20240209095836/usage.webp)
## Purpose of Class Diagrams

The main purpose of using class diagrams is:

- This is the only UML that can appropriately depict various aspects of the OOPs concept.
- Proper design and analysis of applications can be faster and efficient.
- It is the base for deployment and component diagram.
- It incorporates forward and reverse engineering.

## Benefits of Class Diagrams

Below are the benefits of class diagrams:

- Class diagrams represent the system’s classes, attributes, methods, and relationships, providing a clear view of its architecture.
- They shows various relationships between classes, such as associations and inheritance, helping stakeholders understand component connectivity.
- Class diagrams serve as a visual tool for communication among team members and stakeholders, bridging gaps between technical and non-technical audiences.
- They guide developers in coding by illustrating the design, ensuring consistency between the design and actual implementation.
- Many development tools allow for code generation from class diagrams, reducing manual errors and saving time.

참고사이트
https://en.wikipedia.org/wiki/Class_diagram
https://www.geeksforgeeks.org/unified-modeling-language-uml-object-diagrams/?ref=next_article_top
https://www.freecodecamp.org/news/uml-diagrams-full-course/