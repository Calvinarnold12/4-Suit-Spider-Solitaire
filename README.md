# 4-Suit-Spider-Solitaire
The purpose of this project is to create a fast non-visual simulation of the 4-suit-spider-solitaire game, and pair it with multiple different heuristical algorithms to try and find an algorithm with a high relative success rate. This program will be written in java, and will use JUnit as a testing framework.
#The Game Rules
## Setting Up
1. Two decks of cards are shuffled together such that there are two copies of every card in the deck.
2. The cards are placed sequencially such that there are 10 stacks, the first 4 stacks containing 5 cards, and the remainder containing four
3. One last deal is made on the top of each of these stacks face up, and  the remainder of the cards are kept to be dealt later
## Game-Actions
1. Move Action: You can move a card of value n from the stack that it is on to another stack, provided that the destination stack has a card of n+1 value or if the destination stack is empty. You can move a stack of multiple cards provided that each item in the stack you are trying to move, up to the last item, is of the same suit and the destination has a value n+1 relative to the last item or if the destination stack is empty.
2. Deal: You can deal from the remainder deck at any time, provided there are no empty stacks. You deal 1 card sequencially onto each stack. You can deal until there are no cards left in the deal pile.
3. New Game: Generates a new game from a random seed.
## Game-Conditions
1. Stack-Complete: If the game detects a complete stack after a move action(King-A sequencially all of the same suit), the stack will be eliminated, leaving the last item in the Column stack as the new top card. This will score the algorithm a point, with an 8 point game being a full completion.
2. Game-Complete: If there are no more Stacks left then the game will end with success.
3. Game-Over: If there are no more deals left, and no more useful moves that can be taken. Ex(there are two stacks with an open 8, and one stack with an open 7, we do not want the algorithm to get stuck infinitely moving the 7 between both 8's
## Game-Structure and implementation logic
1. A card stack is a stack of multiple arrays, each array will be in sorted order and of the same suit.
2. If a card is to be moved onto another stack, and that card is of the same suit, then this element will be added to the existing array.
3. If a card is to be moved onto another stack, is numerically n-1 related to the destination and is not of the same suit, a new array is created and pushed onto the top of that stack containing that element.
4. If an array is moved, it follows the above rules for the first element in that array. So if suit of head of the list is same suit and n-1 in relation to the destination then the destination array and the current array are combined
5. If the moved array is n-1 in relation to destination card and is not of the same suit, it is pushed on to the top of that stack.
6. If stack complete is satisfied, the array is popped off of the card stack. A point is awarded.
7. If deal is called, and there are cards left to be dealt, each time a card is dealt the program checks if the tail in that array is both n+1 relative to the card being added and of the same suit. If yes, the item becomes the new array, if no the item becomes the head of a new array pushed on top of the stack.
##Java Class Diagram
Card.java - Contains an int to distinguish card value and an enumeration to distinguish card suit.
DoubleDeck.java - An indexed data structure that is 2 decks of cards in a random order. Its constructor will shuffle the cards. It will need to contain only 2 copies of each card.
FourSuitSS.java - The game structure for our solitaire game, given actions
