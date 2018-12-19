# Object Oriented Programming Paradigms

## 1. UML Class Diagrams
### 1.1 Creating a UML diagram
A UML diagram for an RPG game can be found [here.](https://github.com/LBruni98/Object-Oriented-Programming-Paradigm/blob/master/The%20Citadel%20of%20Chaos%20-%20UML.png)

### 1.2 Defining Class Diagrams for specific patterns
The class UML document above is built from multiple class diagrams, to build up the program’s gameplay elements and characters. These specific patterns contribute to each element and are used accordingly.

The character state is one single class to which all the other characters subclasses are assigned to, making it a façade design-based class. All the character-based statistics and other stats are assigned within this class, called character and the boss character, the player character and the mob characters come from subclasses linked to this class. Any instance of an intended character will come from the subclasses.

![Facade](https://github.com/LBruni98/Object-Oriented-Programming-Paradigm/blob/master/Facade.PNG)

Then we come to the elements of the game that determine specific characteristics of the game. In the game, there will be a combat section that the player will engage in and works by taking the player to a separate mode in the game that makes a player and NPC character fight. The class that makes up this section uses state design, as the player is essentially placed in an ‘inFight == True/False’ situation. Within this class, a separate set of classes are done to simulate the fighting and even has its own damage system that deals damage based on body part.

![State](https://github.com/LBruni98/Object-Oriented-Programming-Paradigm/blob/master/State.PNG)

## 2. Implementing code with design patterns
### 2.1 State Machine
The code I built was a state machine for a gumball dispenser. The code can be found [here.](https://github.com/LBruni98/Gumball-State-Machine/blob/master/main.cpp)

### 2.2 Code with a Design Pattern in Mind
