# Interfaces / Refactoring Teddy Bear Destruction

- Now 3 different classes will broadcast the same event
- Our invokers need to inherit from more than one class
- We will use interfaces to do that
- In interface classes, all methods must be abstract
  - Abstract methods must be overriden in the child classes that inherit from the interface class
  - The difference between Abstract and Virtual classes is that the methods in virtual classes CAN be overriden 
