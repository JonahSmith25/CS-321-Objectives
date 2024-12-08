1\. Tell, Don't Ask 

-   [**Example Violation**](https://github.com/JonahSmith25/battleship/blob/7180df74c41ec3ff30deb0d3cfe0470231ae2265/src/main/java/core/Grid.java#L88C1-L98C6)

Inside of the Grid class, the allShipsAreSunk() method asks for each ships coordinates and then checks if each cell has been hit. This logic is spread out in multiple places and could be refined by using the tell don't ask principle.

-   Changes to Adhere to Principle:

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

Rather than asking Grid objects to check if a ships cells have been hit, we could givethis responsibility to the Ship object to make the needed checks. 

Tradeoffs: 

-   Pros: After making these changes, the Grid class becomes better focused on managing its own functionalities rather than concerning itself with the state of ships objects, in turn making the code more modular. 

-   Cons: If this principal  was used too much the large amounts of delegating tasks would make excessively complex objects. However, this is an appropriate application and fits well within the project.
