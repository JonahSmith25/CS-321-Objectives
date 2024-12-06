1\. Renaming a Parameter: startGame Method in GameDriver.java 

-   [**Permalink**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/GameDriver.java#L21C5-L32C6)

-   Explanation:  Within the startGame method, there is a default temporary grid created that will not be used in the broader context of the game. As such, its name could be changed to tempGrid in order to more clearly define its purpose and use case. 

-   In the startGame method, the grid variable is temporarily named grid, which is fine, but changing its name to tempGrid will make it clearer that it's a temporary instance for the start of the game and not part of a broader grid structure.

-   New code:

```
private void startGame() {
   presenter.displayMessage("Game is starting...");
   // User input for grid size
Grid tempGrid = Grid.defaultGrid(); // Temporary grid
   while (!tempGrid.allShipsAreSunk()) {
       presenter.displayGrid(tempGrid);
       presenter.displayMessage("Insert a coordinate to shoot!");
Coord playerInputCoord = presenter.askForCoordinate(tempGrid);
       tempGrid.shoot(playerInputCoord);
       reportIfShipSunk(tempGrid, playerInputCoord);
   }
}
```

-   Affected lines outside the block: There are no outward effects of this alteraton.




2\. Introducing a New Parameter: reportIfShipSunk Method in GameDriver.java 

-   [**Permalink**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/GameDriver.java#L34C4-L42C6) 

-   Explanation:  The reportIfShipSunk is currently relying on a direct check inside its body. Instead, we could use an isShipSunk parameter to decouple the methods logic and make it easier to test. This will improve the flexibility of the code. 

-   New code:
```
private void reportIfShipSunk(Grid grid, Coord playerInputCoord, boolean  isShipSunk) {
   if (isShipSunk) {
for (Ship ship : grid.getShipList()) {
for (Coord c : ship.getCoordList()) {
               if (c.isEqual(playerInputCoord)) {
                   presenter.displayMessage("You sunk your opponent's " + ship.getName() + "!");
               }
           }
       }
   }
}
```

-   Affected lines outside the block: 
```
// Update the call to reportIfShipSunk
reportIfShipSunk(grid, playerInputCoord, grid.isShipSunk(ship));
```



3\. Extracting a Method: Grid.java 

-   [**Permalink**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/Grid.java#L88C5-L98C6)

-   Explanation:  The allShipsAreSunk method currently checks if all coordinates of each ship have been hit. This could be extracted into an isShipHit method which would break down the code into more readable and understandable sections while also ruducing the size and complexity of the  allShipsAreSunk  method. This also makes it easier to test individual aspects of the method rather than the method as a whole.

-   New code:
```
public boolean  allShipsAreSunk() {
for (Ship ship : shipList) {
       if (!isShipHit(ship)) {
           return false;
       }
   }
   return true;
}

private boolean  isShipHit(Ship ship) {
for (Coord coord : ship.getCoordList()) {
       if (!this.getCell(coord).cellIsHit()) {
           return false;
       }
   }
   return true;
}
```
-   Affected lines outside the block: There are no outward effects of this alteraton.



4\. Renaming a Method: Grid.java 

-   [**Permalink**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/Grid.java#L113C1-L119C6) 

-   Explanation:  The isShipOnGrid method performs a check on the validity of a ships placement, though the name does not directly relay the need for the ship to be in bounds rather than simply on a grid. Given this, renaming it to isShipWithinBounds would be an improvement of clarity and improving its readability.

-   New code:
```
public boolean  isShipWithinBounds(Ship ship) {
for (Coord coord : ship.getCoordList()) {
       if (coord.row < 1 || coord.row > numRows()) return false;
       if (coord.col < 1 || coord.col > numCols()) return false;
   }
   return true;
}
```
-    Affected lines outside the block: Any calls to this method would need to be refactored and altered to the new method name.


5\. Introducing a New Variable: Grid.java 

-   [**Permalink**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/Grid.java#L40C1-L50C6) 

-   Explanation:  The markAllShipCells method currently iterates through the ships and marks their cells. This is currently being done through a loop intermediate variable. Having a new variable which holds the current ship's cells allows for better optimization and clarity. This will in turn reduce the number of redundant calls made.

-   New code:

```
private void markAllShipCells() {
for (Ship ship : shipList) {
List<Coord> shipCoords = ship.getCoordList(); // New variable for clarity
       markShipCells(shipCoords);
   }
}

private void markShipCells(List<Coord> shipCoords) {
for (Coord coord : shipCoords) {
       getCell(coord).setAsShip();
   }
}
```

-   Affected lines outside the block: There are no outward effects of this alteraton.






