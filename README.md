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
- Actors broadcasts death > Event manager registers the actors as invokers and the Hud as a listener > Hud listens to kill event and increases the count. 
- Use the interface to define the functions actors should have (broadcast kill message). Actors inherit from Interface but implement functions differently. 

# Exercise:

## Class Delegates Declarations
- Create a C++ class type UObject
- This class will declare the Delegate that will fire the Event - Declare all the delegates in this class

- HeaderFile
  - Declare a multicast delegate oneparam. dont need to implement this. 


## Class Event Manager / Interface
- Create a C++ class type Unreal Class : Interface
- This class will declare the function that gets the event. 

- Header file
  - Declare a pure virtual function GetKillEvent() that returns the kill event broadcasted by the delegate
    - Gets the event broadcasted when enemy is killed
    - This function doesn't need to be implemented in this class but must be implemented in the classes that inherit from this one because it's a pure virtual function


## Class Enemy
- Create a C++ class type AActor
- This class will fire the event
- This will be the class that will invoke / broadcast the kill event, therefore, this class should declare the event. Only classes containing the event can broadcast it. So insert the event inside the class that will trigger it. 

- Header File
  - #include EventManagerInterface.h
  - include "public IEventManagerInterface" in the class definition to make it also inherit from EventManagerInterface
  - Declare the event object KillEvent based on the Delegate. This event will be fired by the delegate when the enemy is killed. 
  - Declare the GetKillEvent() function that will override that of the event manager and get the kill event and store it inside the event object.
  - Declare a variable to store the score after each enemy is killed and expose it with UPROPERTY
  
- Implementation File
  - Define the GetKillEvent() function and make it return the kill event
  - OnBeginPlay, Bind a function to the event that prints by how much the score increased everytime an enemy was killed


- Create enemy tag in project settings
- Create a blueprint based on this class
- Create an enemy tag inside this blueprint


## Class GameMode
- Create a C++ class type AGameModeBase
- This is the class that will implement the function that will listen to the event

- Implementation file
  - Find all enemy actors
  - Iterate through all the enemies and Get the delegate instance from each one
  - Use this delegate instance to bind to the function that increases the score
  - Define the function that increases the score that will be called when the kill event occurs


- Define Score increase in the Enemy BP
- Define initial score in the GameMode BP


















