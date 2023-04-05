# Interfaces / Refactoring Teddy Bear Destruction

# Interfaces Definition
- Interfaces are classes in which all of its functions are pure virtual.
- By making its functions pure virtual we are ensuring that its child classes implement all these functions. 
- The benefit of that is that we can make sure that all child classes comply to the same functionality standards while allowing flexibility to the child classes to implement the functions in their own specific way. 
  - Eg: a game that has multiple weapons that the player can use. Each weapon has different properties and behavior, but they all need to be able to fire and reload.
    - pure virtual / abstract functions: don't have implementation in the parent class so they must be overriden and implemented in the child classes
    - pure virtual / abstract functions are used when we need to create functions that we know must exist in the child classes but their implementaton varies according to each child class. Eg. All animals move but each type of animal move in a different way. Animals class has a Move() function. A bird can implement Move() by Move() { Fly() } a Fish by Move() { Swim() } and a Dog by Move() { Run() }. That's why they cannot be defined in the parent class, must be implemented in the child classes and must override the parent class. 


# Exercise

## Ingridients
- An actor interface (parent class)
- 3 Actors (child classes)
- A HUD 
- An Event Manager

## Preparation
- The Actor dies and the HUD increases the counts for actors killed
- Actors broadcasts death > Event manager registers the actors as invokers and the Hud as a listener > Hud listens to kill event and decreases the count. 
- Use the interface to define the functions actors should have (broadcast kill message). Actors inherit from Interface but implement functions differently. 

# Exercise:

## Class Delegates Declarations
- UObject
- Declare all the delegates in this class
- Declare a multicast delegate oneparam


## Class Event Manager / Interface
- Declare a multicast delegate oneparam
- Unreal Class : Interface
- Gets the event broadcasted when enemy is killed
- Declare a pure virtual function GetKillEvent() that returns a delegate event
  - This function needs to be implemented in the classes that inherit from this one


## Class Enemy
- AActor > also inherits from EventManagerInterface
- Create enemy tag in project settings
- Create a blueprint based on this class
- Create an enemy tag inside this blueprint
- #include EventManagerInterface.h

- Header file
  - Declare a delegate instance that will store the event to be called when enemy is killed
  - Declare the GetKillEvent() function that will get the kill event and store it inside the delegate instance
  
- Implementation File
  - Define the GetKillEvent() function
  - Use the delegate instance and 


## Class GameMode
- AGameModeBase

- Implementation file
  - Find all enemy actors
  - Iterate through all the enemies and Get the delegate instance from each one
  - Use this delegate instance to bind to the function that increases the score
  - Define the function that increases the score that will be called when the kill event occurs





















