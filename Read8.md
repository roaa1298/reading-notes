# OO Design

### SOLID principles intro

Single-responsibility Concept This principle argues that a class should have just one cause to change, or in other words, a single task.

Open-closed Principle This concept argues that things or entities should be able to be extended but not changed. A class should be able to be extended without having to change the class itself.

Substitution Principle of Liskov Let q(x) be a property provable about objects of x of type T, according to this concept. Then, for objects y of type S, where S is a sub-type of T, q(y) should be provable. This phrase means that every subclass or derived class should be used in place of its base or parent class.

Principle of Interface Segregation This concept asserts that customers should never be compelled to implement an interface that they do not use, or to rely on methods that they do not utilize.

The Principle of Dependency Inversion Entities must rely on abstractions rather than concretions, according to this concept. It specifies that the high-level module should rely on abstractions rather than the low-level module. Decoupling is also possible as a result of this.   

### OO SOLID principles in real life

The single responsibility principle states that a class or module should only change for one cause.

The open/closed principle refers to a class that performs what it should and can be expanded without needing to be altered afterwards.

Any child type of a parent type should be able to stand in for that parent type, according to the Liskov substitution principle.

Clients should not be forced to rely on stuff they don't require because of the interface segmentation concept.

The inversion of dependence is based on abstractions rather than real details.

example:

    S - the "duck" vehicle are street legal and water-capable, and they only change from driving on land to going out on the water, and from being driven in the water to land.

    O - a smart phone that doesn't need to be changed itself, but you can add apps on the phone.

    L - Cooking stew with various ingredients in the stew, while asking if the ingredients are edible or not.

    I - The soup of the day on a menu at a restuarant. The reason why it only says soup of the day is because it changes everyday.

    D - Using a credit card to pay groceries with, and the clerk swiping your credit card with a visa machine.
