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
