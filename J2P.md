### **AIShips Class**

1.  **Role and Responsibilities:**\
    The `AIShips` class manages the placement of ships onto the grid for the AI aspect of this battleship game. It interacts with the `Grid` and `ShipSpec` classes in order to randomly place ships onto the board. In doing so, it ensures that there are no overlapping ships and that no ship exceeds the boundaries of the board. In short, it generates and validates ship placements within the restrictions of the Board/grid.

    -   **Relation to the Rest of the Project:**\
        The `AIShips` class interacts with the `Grid` (game board) and `ShipSpec` (Ship details) and in doing so interacts with other classes such as the `Ship` class, `Cell` class, etc. Not only does it interact with many of the other classes indirectly or directly, but it also proves integral to the playability of the game. Sure, without some form of presenter/interface for the player it's all a moot point, but the `AIShips` class uses the other classes to form a playable aspect of the game.
2.  **Fields and Their Class Types:**

    -   **`Grid grid`:**\
        The `Grid` class is a coded representation of the game board and is a central aspect of the game. It is needed to ensure proper placement of the ships onto the game board. This type choice is meaningful as the ships need to use the `Grid` class information to guarantee valid placement.

    -   **`List<Ship> ships`:**\
        This is used to store the ships that have been placed by the AI. The use of `List` is fitting, as it allows dynamic additions while also removing the need for a fixed-size structure.

    -   **`ShipSpec[] shipSpecs`:**\
        This is an array of objects that define the name and size of each ship. This is a fitting choice as the number of ships is fixed and well-defined. However, a `List<ShipSpec>` could be used instead under circumstances where the number of ships would be altered. Here, the array offers the best performance in a fixed application, while a `List` would offer better flexibility.

3.  **Methods:**

    -   [**setShips()**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/AIShips.java#L19C1-L38C6):

        -   **Return type:** `void` --- This is a fitting use of the `void` return type given that the method is used to modify the state of a class and is not intended to return any value.
        -   **Parameters:** None --- This is appropriate since the method does not need any input other than the class's state.
        -   **Access modifier:** `public` --- Given that other parts of the game need access to this method to initialize the ships, this is fitting.
    -   [**checkShipSizes()**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/AIShips.java#L40C1-L46C6):

        -   **Return type:** `void` --- Given that there is no distinct need for a return value as it simply raises an exception for invalid ship sizes, this is a good use case.
        -   **Parameters:** None
        -   **Access modifier:** `private` --- Since this method is only used within its own class to validate information, it does not need to be accessible to other classes.
4.  **Additional Methods:**

    -   **Method 1: `getRemainingShips()`**

        -   **Return type:** `List<Ship>`
        -   **Parameters:** None
        -   **Purpose:** This method would be used to return a list of ships that have yet to be placed on the grid. This could be useful when checking that the AI has successfully completed the placing of all the ships.
    -   **Method 2: `resetShips()`**

        -   **Return type:** `void`
        -   **Parameters:** None
        -   **Purpose:** This method would wipe the board and clear off all the ships, which could be useful for re-initializing AI ship placements for a game restart.

* * * * *

### **Ship Class**

1.  **Role and Responsibilities:**\
    The `Ship` class is used to represent an individual ship within the game. To accomplish this task, it manages the coords, size, direction, and name. It allows us to check whether or not a specific coordinate is part of a ship, to check if two ships overlap, and to generate a list of occupied coordinates on the grid. This is a central part of the physical representation of ships on the game grid.

    -   **Relation to the Rest of the Project:**\
        The `Ship` class interacts closely with the `Coord` class and the `Grid` class, relying on the coordinates for placement and grid position checking. To the rest of the classes, the `Ship` class is a key part of placing and tracking ships during gameplay.
2.  **Fields and Their Class Types:**

    -   **`Coord startCoordinate`:**\
        This is used to represent the start position of a ship on the grid. `Coord` is an appropriate choice as it contains both the row and column of the start point and does so in a way that meshes well with other grid operations.

    -   **`int size`:**\
        The ship size represents the number of cells that a given ship occupies. This field is a key component of generating a ship's coordinates and verifying whether or not a ship fits on the grid. The use of `int` is fitting given that the size is just a numeric value.

    -   **`Direction direction`:**\
        This field is used to determine the horizontal or vertical orientation of a given ship. This allows a ship to occupy more than one cell along a row or column. Its use of enums is a fitting choice, as it gives clarity to the use case and intentions.

    -   **`String name`:**\
        This is used for the name of a ship/ships. The use of strings is fitting as it is needed to give a title to the ship.

    -   **`List<Coord> coordList`:**\
        This contains a list of coordinates on the grid that a ship occupies. The use of `List` is fitting given that the number of coordinates varies based on a ship's size and orientation.

3.  **Methods:**

    -   [**containsCoord(Coord coord)**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/Ship.java#L47C4-L54C6):

        -   **Return type:** `boolean` --- This works since we only need to know if a coordinate is a part of the ship.
        -   **Parameters:** `Coord coord` --- This parameter is necessary since the method is used to check if the specific coordinate passed through the parameter is part of the ship.
        -   **Access modifier:** `public` --- This method being public is acceptable given that other parts of the code may want to check if a shot hit a ship.
    -   [**isOverlapping(Ship other)**](https://github.com/skiadas/battleship/blob/6e2b7dff2e2b947be6aefa0ae29c3c6385df32e0/src/main/java/core/Ship.java#L74C1-L81C6):

        -   **Return type:** `boolean` --- This method needs to return if the current ship overlaps with another. Given that this is a yes or no/true or false type of need, boolean is a fitting return type.
        -   **Parameters:** `Ship other` --- The other ship is a required parameter to make comparisons with the current ship.
        -   **Access modifier:** `public` --- Since this may be used by the `AIShips` class, remaining public is a valid choice.
4.  **Additional Methods:**

    -   **Method 1: `getShipStatus()`**

        -   **Return type:** `String`
        -   **Parameters:** None
        -   **Purpose:** This method would return the current status of a ship, such as if it's been hit in all of its cells, if it's not been completely hit, or if it is unscathed. It could be used to return strings like "unscathed", "hit", "sunk".
    -   **Method 2: `rotateShip()`**

        -   **Return type:** `void`
        -   **Parameters:** None
        -   **Purpose:** This would allow the rotation of a ship's orientation. This would be used for modifying the orientation of the ship during gameplay.
