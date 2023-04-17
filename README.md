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
- This class will declare the Delegate that will fire the Kill Event - Declare all the delegates in this class

- HeaderFile
  - Declare a multicast delegate oneparam. dont need to implement this. 

```cpp
DECLARE_MULTICAST_DELEGATE_OneParam(FEnemyKillEvent, int32);

UCLASS()
class INTERFACES_API UDelegateDeclarations : public UObject
{
	GENERATED_BODY()
	
};
```


## Class Event Manager / Interface
- Create a C++ class type Unreal Class : Interface
- This class will declare the function that gets the delegate object that be used to fire the kill event. 
- This function will need to be implemented in the child classes Enemy

- Header file
  - Declare a pure virtual function GetKillEvent() that returns the delegate object FEnemyKilled
    - Gets the event broadcasted when enemy is killed
    - This function doesn't need to be implemented in this class but must be implemented in the classes that inherit from this one because it's a pure virtual function


## Delegate Class: Enemy
- Create a C++ class type AActor
- This class will inherit from the EventManagerInterface parent class
- This class will get the delegate object with the function GetEnemyKillEvent() and use it to fire the kill event
- This will be the class that will contain the delegate function Send() that will invoke / broadcast the kill event, therefore, this class should declare the delegate object FEnemyKillEvent in its header file. Only classes declaring the delegate object can use it to broadcast events. So insert the delegate object inside the class that will trigger the events. 

- Header File
  - #include EventManagerInterface.h
  - #include "public IEventManagerInterface" in the class definition to make it also inherit from EventManagerInterface
  - inherit from public AActor but also from public IEventManagerInterface
  - Declare the delegate object KillEvent. This will be used to trigger the event when the enemy is killed. 
  - Since this class is child of the interface class EventManagerInterface, it must Declare and Implement the GetKillEvent() function and override that of the parent class. This function will get and return the delegate object.
  - Declare a Send() delegate function that will broadcast the kill event.
  - Declare a variable to store the score after each enemy is killed and expose it with UPROPERTY
  
- Implementation File
  - Implement the GetKillEvent() function and make it return the kill event
  - Implement the Send() function. Use the delegate object KillEvent to Bind a lambda function that prints by how much the score increased everytime an enemy was killed.
  - Use the delegate object to broadcast the ScoreIncrease variable with the new score information
  - Call Send() on BeginPlay()


- Create enemy tag in project settings
- Create a blueprint based on this class
- Create an enemy tag inside this blueprint


## Listener Class MyGameMode
- Create a C++ class type AGameModeBase
- On project settings switch the game mode from the standar game mode to MyGameMode.
- This is the class that will implement the function OnEnemyKilled() that will listen to the event

- Header file
  - Declare OnEnemyKilled() that will listen to the kill event
  - Declare a variable for the current/initial score and expose it with UPROPERTY
  - Declare a AEnemy instance that will be used to call the GetEnemyKillEvent() function that will get the delegate object

- Implementation file
  - Find all enemy actors
  - Iterate through all the enemies and Get the delegate instance from each one with the GetEnemyKilledEvent() function and store it inside a delegate local delegate object.
  - Use this delegate object to bind the delegate function to the listener function that increases the score
  - Define the OnEnemyKilled() function that increases the score and prints it and that will be called when the kill event occurs


- Inpute a value in the field Score increase in the Enemy BP and for initial score in the GameMode BP


















