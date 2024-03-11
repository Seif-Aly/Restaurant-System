# Restaurant-System
# Accessing the Administrator Interface
Within each established dining establishment, a singular administrator account is automatically generated. To gain entry, the administrator's designated username and password must be provided. Additional accounts generated via app registration are designated as standard guest accounts by default.

# Data Management Details
The storage mechanism for the restaurant's data utilizes a file generated through Java's native serialization capabilities. This file encompasses: account login credentials (with all passwords encrypted via AES), the latest menu status, various restaurant metrics, and comprehensive details on customer orders (including the advancement status of each order, ensuring that the preparation of incomplete orders resumes from their last saved progress upon program restart). Data storage is automatically updated following any user action that alters the stored data or upon program closure.

The data storage file's location is determined through command-line arguments, with 'restaurant.dat' often used during testing. Initializing the program with a path to a non-existent or empty file triggers the automatic creation of a "default" restaurant, equipped solely with an admin account featuring a standard username and password. Any operations conducted within this restaurant are recorded in the designated file, ensuring their availability upon the next program launch.

# Handling of Orders
Order processing is executed in a multithreaded manner, simulating the activity of three chefs, thereby limiting simultaneous order preparation to three. Should all chefs be occupied, incoming orders are queued as "In processing" until a chef becomes available. An order transitions to "Ready" status once preparation begins, and retains this status upon completion.

# Design Patterns Employed
1. Builder Pattern:
The program's dishes are represented as Dish class instances, characterized by numerous attributes. Given the need to query and validate each attribute's value during dish creation, this process is facilitated through a series of methods that set the value of subsequent attributes using a lombok Builder, enhancing the creation process's efficiency.

2. State Pattern:
The application features several user interface menus: a login menu, a menu for authenticated guests, and another for authenticated administrators. Each menu type includes a printMenu() method, the functionality of which depends on the specific menu type. This diversity is managed using the State pattern, representing all menu types through a single Menu class, differentiated by the MenuPrinter interface instances assigned to them, which include the printMenu() method. This design allows for the utilization of a consistent method to display varying menu types.

3. Observer Pattern:
Within the program, each order is represented by an Order class instance, with all of a user's orders stored in an ArrayList<Order>. To enable the removal of completed orders from the pending list directly from the Order class, each order contains a client field linking to the customer who placed the order. This link is established using a specialized setClient method.
