# Interfaces / Refactoring Teddy Bear Destruction

- Now 3 different classes will broadcast the same event
- Our invokers need to inherit from more than one class
- We will use interfaces to do that
- An Interface / Abstract class is a class in which at least one its functions are pure virtual / abstract functions
  - pure virtual / abstract functions: don't have implementation in the parent class so they must be overriden and implemented in the child classes
    - pure virtual / abstract functions are used when we need to create functions that we know must exist in the parent classes but their implementaton varies according to each child class. Eg. Animals class has a Move() function. Every animal moves but each animal moves differently. A bird can implement Move() by Move() { Fly() } a Fish by Move() { Swim() } and a Dog by Move() { Run() }. That's why they must be implemented in the child classes and must override the parent class. 
