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
The following code is developed using the state design pattern, where the behaviour influences how certain features act. The main idea was to develop a state machine that dispenses gumballs once the user has inserted cash. Once the cash was entered, the user can get a gumball from the machine and that affects the number of cranks on the machine and the number of gumballs inside. The specialty of the machine is that the user can choose the input methods; inserting a coin, turning the crank or ejecting the coin.

```cpp
//Inserting the coins
	void insertQuarter() {
		if (state == NO_QUARTER) { //If the machine hasn't a quarter when the user inserts $0.25
			std::cout << "Quarter Inserted\n"; //Notifies the user
			setState(HAS_QUARTER); //The machine now has a quarter
		}
		else {
			std::cout << "There's already one in there!\n"; //Otherwise it will note that it has one in there
		}
	}
```

```cpp
//This function works like insertQuarter() but a bit backwards
	void ejectQuarter() {
		if (state == HAS_QUARTER) { //If the machine has a quarter in it and the user ejects...
			std::cout << "Quarter Ejected\n";
			std::cout << "No change in the Gumball Machine\n"; //Messages will note that the quarter is ejected
			setState(NO_QUARTER); //The machine is set to not have a quarter in it
		}
		else {
			std::cout << "Machine's empty, mate\n"; //Unless it's empty still, however, and the customer is notified
		}
	}
```

```cpp
//Function for turning the crank
void turnCrank() {
  if (state == HAS_QUARTER) { //If the machine has a quarter in it,..
    crank++; //...The customer can turn the crank and it keeps note of an added crank turn...
    gumballs--; //...and dispenses a gumball, knocking one off the counter
    setState(NO_QUARTER); //It also sets the machine to no quarter
	} else { //Otherwise...
    std::cout << "Put a quarter in the machine first, cheapskate!\n"; //...This message appears if the customer is trying to get a freebie
	}
}
```

Special Functions were included with their own states and actively change the dynamic of the gumball machine. For instance, there is a function that allows for a bonus gumball at every 10 cranks. The only instances of where the bonus gumball cannot be dispensed is when there is only one gumball left or the machine has no more gumballs.

```cpp
//Bonus Gumball
	void bonusGumball() {
		//This if statement was set to get the function working as desired
		if ((crank % 10) == 0 && gumballs > 1 && crank != 0 && state == 0) { //After every ten cranks and more than one gumball... and the crank isn't 0... AND the machine has no quarter in it...
			gumballs = gumballs - 1; //...Another gumball is negated, meaning one is given for free
			std::cout << "YOU ARE THE TENTH CUSTOMER! HAVE ANOTHER GUMBALL ON THE HOUSE!\n"; //The customer gets a nice message
		}
		else if ((crank % 10) && gumballs == 1) { //However,there were 10 cranks but only one gumball left...
			std::cout << "Well... at this point you would get an extra gumball but, we're down to one. Sorry, mate!"; //It only displays the message and only deposits one as usual
		}
	}
```

Once the machine runs out of gumballs, then a prompt will appear whether if the user wants a refill of 50 gumballs, otherwise the program can end.

```cpp
//Function for when the gumballs run out
	void outOfBalls() {
		char choice; //This will be where the customer makes their selection
		while (gumballs == 0) { //Once the gumballs have run out...
			//Messages display that the machine is out of gumballs
			std::cout << "We're all out of gumballs!\n";
			std::cout << "Do you want us to refill the system or leave it and come back next time?\n";
			std::cout << "Y/N?\n";
			std::cin >> choice; //The customer inputs their choice

			switch (choice) {
				//If their choice is yes
				case 'Y': //Uppercase
				case 'y': //Or lowercase
					std::cout << "'Ere ya go, mate!\n"; //Message Displays
					setGumballs(50); //and the gumballs are set to 50
					break; //Breaks the loop
				//Otherwise if the choice is no
				case 'N': //Uppercase
				case 'n': //Or lowercase
					std::cout << "Ah well! Come back next time!\n"; //Message Displays
					isBankrupt = false; //Boolean set to false, that will be used later
					//setState(HAS_QUARTER);
					return; //This technically ends the loop and goes back to the main
					//break;
				default: //If any other choice is made
					std::cout << "Mate, be clear! I don't understand that\n"; //Message tells the customer to get their head on straight
			}
		}
	}
```

## 3. Design Patterns in a scenario
### 3.1 Range of Design Patterns
Creational mainly focus on the method of creating objects. Focusing on a suitable manner for creating such objects, where just creating the object basically would cause problems or add complexity. Creational patterns do allow for a more organised method of object creation.
One such creational Pattern is the builder that can build up complex objects by utilising the other subclasses. The representation between that of the objects in the main class and the representation are clear, with a good understanding of how the item works. As the design supports change, it can allow the user the ability to change the function workings, adding to flexibility.

Structural Patterns are design patterns that work to provide effective solutions and stress the importance of class composition and object structures. Different structural patterns work with classes to create a program and doesn’t change the overall foundations of a system due to one part of said system.
A known structural pattern is the bridge pattern, so called because it bridges the implementation and abstraction to create an independence between each other, implementing change with no negative consequence.

Behavioural Patterns use the objects interaction and their responsibility to improve the communications between the class, allowing for additional flexibility of that communication. This provides easy communications with the object and the class and makes them loosely coupled between them.
A specific pattern of this class is the state class, where the object changes behaviour based on its internal state. This can make for different functions to execute once a specific state has changed in one class if associated with that object responsible.

### 3.2 Patterns for a given scenario
To determine what would make for an effective design pattern, you would have to consider the situation at hand. When developing an app, the pattern and the type of pattern would have to make the app work properly, but also considering other factors such as ease of development and the objects and classes involved.

Two scenarios given to utilise specific design patterns were a game and a gumball dispenser. The gumball machine was more directed in supplying the user and notifying the user of the gumball machine’s availability, meaning we had to tell the user about the gumball stock, the turn of the cranks and if there is any money in there. All that to make the machine function.

The appropriate design pattern to use here was the state pattern. As stated before the state pattern determined actions based on the states of each classes or objects. This was essential to create a user-friendly program that ensured the user of what was going on internally, displaying if there was a coin inside or if the user was entitled to a free gumball. The state machine was used as such, to give the user a gumball if the machine had a coin, base the number of cranks to a multiple of 10 to give a bonus gumball, etc.

However, in the sense of the second scenario, the RPG game, the requirements were so diverse in terms of features that a single design pattern wouldn’t be enough to organise or make the program function.

How this was done was to employ that of distinctive design patterns, however they aren’t the same in type. The state machine was used to determine if the player was in a battle, in a trade or in a specific city, allowing for creating functions based on the state the player was in. However, in terms of more difficult and more complex elements, I used the façade pattern, which allowed for a huge class to be made up of subclasses, but all linked to a huge class that allowed for different instances relating to a subclass but utilising the main class. This proved useful for the whole character dynamic, as the entire game’s characters would be made up of one base that the rest of the characters can use, meaning you would have an enemy or an NPC, which can be entirely different to one another but share the same stats between them.
