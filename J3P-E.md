1\. Composition 

[**Example in the Code**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/Grid.java#L13C5-L13C27): 

A representation of composition in our project can be seen in the grid class where there is a call to the Cell object in a 2D array used to form a grid. This grid contains multiple Cell objects, each of which represents an individual location upon the game board.

Why Composition is Useful: 

Composition gives room to create more flexible and reusable code. For instance, rather than creating all the individual instances of the cells and containing  all the code for modifications and checks inside of the grid class, this task was offloaded to another class that can focus on its own individual purpose. This allows the Grid class to focus on simply creating the grid with objects it can trust are functional by interacting with the Cell class. 

2\. Depending on an Interface Instead of an Implementation 

[**Example in the Code**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/GameDriver.java#L6C1-L11C6):

Within the GameDriver class, the Presenter interface is used to interact with the player. Rather than defaulting to an implementation like TextPresenter, the game depends on he Presenter interface. This leaves room for flexibility down the line, where any changes to the format of presentation can be done in the without changes being required in the GameDriver. 

Why This Concept is Useful: 

Depending on an interface rather than on an implementation allows you to block a class (in this case the driver) from knowing more than it needs to. In the case of our project, the GameDriver has no concern over the nature of Presenter; it could be text based or graphical, either way it does not concern the GameDriver. This allows for more flexibility and allows changes to be made without having to concern much else other than the presenter class itself. 

3\. Subclasses and Inheritance 

[**Example in the Code**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/GameDriver.java#L6C1-L11C6):

Although it is an enum rather than a subclass, an example of inheritance can be seen by the Direction enum in the Ship class, which contains two possible values: HORIZONTAL and VERTICAL. This enum restricts the ship class by giving it two directions to work with, managing the code efficiently. Its use could be extended however; for instance, an added DIAGONAL direction could add more opportunities to the ship class without having to alter any present logic for the ship class itself.  

Why Subclasses and Inheritance Are Useful: 

The use of inheritance grants reusability to existing functionalities of a class while stretching it into more specific behaviors. For instance, we could use the Ship class as a base while extending into subclasses of differing kinds of ships. Subclasses also allow you to build a structure where foundational classes can branch out to their subclasses and so forth, creating a logical and traceable hierarchy  of classes.
