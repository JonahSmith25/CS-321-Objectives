1\. Tell, Don't Ask 

-   [**Example Violation**](https://github.com/JonahSmith25/battleship/blob/7180df74c41ec3ff30deb0d3cfe0470231ae2265/src/main/java/core/Grid.java#L88C1-L98C6)

Inside of the Grid class, the allShipsAreSunk() method asks for each ships coordinates and then checks if each cell has been hit. This logic is spread out in multiple places and could be refined by using the tell don't ask principle.

-   Changes to Adhere to Principle:
  
Rather than asking Grid objects to check if a ships cells have been hit, we could givethis responsibility to the Ship object to make the needed checks. 

``` 

public boolean allShipsAreSunk() { 
    for (Ship ship : shipList) { 
        if (!ship.isSunk()) {  // Delegate responsibility to Ship class 
            return false; 
        } 
    } 
    return true; 
} 
 
public class Ship { 
    // Add a method to encapsulate this logic inside Ship 
    public boolean isSunk() { 
        for (Coord coord : coordList) { 
            if (!getCell(coord).cellIsHit()) { 
                return false; 
            } 
        } 
        return true; 
    } 
} 
``` 
 
Tradeoffs: 

-   Pros: After making these changes, the Grid class becomes better focused on managing its own functionalities rather than concerning itself with the state of ships objects, in turn making the code more modular. 

-   Cons: If this principal  was used too much the large amounts of delegating tasks would make excessively complex objects. However, this is an appropriate application and fits well within the project.


2\. Single Responsibility Principle (SRP) 

-   [**Example Violation**](https://github.com/JonahSmith25/battleship/blob/7180df74c41ec3ff30deb0d3cfe0470231ae2265/src/main/java/core/GameDriver.java#L1C1-L36C2):

The GameDriver class handles game flow as well as interactions with the Grid class. This is a violation of the SPR given that it has more than one responsibility.

-   Changes to Adhere to Principle: 

We should separate these responsibilities by extracting the game flow control into a new Game class, and  keep the GameDriver focused on controlling UI with the Presenter. 

``` 

public class Game { 
    private Grid grid; 
 
    public Game() { 
        this.grid = Grid.defaultGrid(); 
    } 
 
    public void start() { 
        while (!grid.allShipsAreSunk()) { 
            // game logic (display grid, shoot, etc.) 
        } 
    } 
} 
 
public class GameDriver { 
    private final Presenter presenter; 
 
    public GameDriver(Presenter presenter) { 
        this.presenter = presenter; 
    } 
 
    public void start() { 
        presenter.displayMessage("Welcome to Battleship!"); 
        Map<String, Runnable> gameOptions = new HashMap<>(); 
        gameOptions.put("start", this::startGame); 
        gameOptions.put("stop", this::stopGame); 
        presenter.displayOptions("Choose an action: start or stop", gameOptions); 
    } 
 
    private void startGame() { 
        Game game = new Game(); 
        game.start(); 
    } 
 
    private void stopGame() { 
        presenter.displayMessage("Game is stopping..."); 
    } 
} 
``` 

Tradeoffs: 

-   Pros: Now the GameDriver class solely handles UI which reduces its complexity and allows it to fit into the guidelines of the SRP. 

-   Cons: In the short term, introducing a new Game class may increase the complexity of the project. However, doing so makes the code more manageable in the long term.


3\. Dependency Inversion Principle (DIP) 

-   [**Example Violation**](https://github.com/JonahSmith25/battleship/blob/7180df74c41ec3ff30deb0d3cfe0470231ae2265/src/main/java/core/GameDriver.java#L6C1-L11C6): 

The GameDriver class currently depends directly on the Presenter interface for all of its interactions, but does not use dependency injection. This causes the GameDriver and all of its dependencies to be tightly paired.

-   Changes to Adhere to Principle: 

To avoid this issue, we could bypass the Presenter using its constructor. Here, we would be using abstractions for dependencies. This change would use mock presenters for testing and to swap implementations without altering the core of the GameDriver class. 
```
public class GameDriver { 
    private final Presenter presenter; 
 
    // Dependency injection 
    public GameDriver(Presenter presenter) { 
        this.presenter = presenter; 
    } 
} 
```
Tradeoffs: 

-   Pros: After making the above changes the code becomes more flexible and allows for easier testing as GameDriver is no longer responsible for creating its own dependencies.  

-   Cons: Doing this adds more complexity as the dependencies have to be managed either through manual effort or some form of framework.


