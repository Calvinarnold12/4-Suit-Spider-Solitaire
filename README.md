# 4-Suit-Spider-Solitaire
The purpose of this project is to create a fast non-visual simulation of the 4-suit-spider-solitaire game, and pair it with multiple different heuristical algorithms to try and find an algorithm with a high relative success rate.
#The Game Rules
## Setting Up
1. Two decks of cards are shuffled together such that there are two copies of every card in the deck.
2. The cards are placed sequencially such that there are 10 stacks, the first 4 stacks containing 5 cards, and the remainder containing four
3. One last deal is made on the top of each of these stacks face up, and  the remainder of the cards are kept to be dealt later
## Game-Actions
1. Move Action: You can move a card of value n from the stack that it is on to another stack, provided that the destination stack has a card of n+1 value or if the destination stack is empty. You can move a stack of multiple cards provided that each item in the stack you are trying to move, up to the last item, is of the same suit and the destination has a value n+1 relative to the last item or if the destination stack is empty.
2. Deal: You can deal from the remainder deck at any time, provided there are no empty stacks. You deal 1 card sequencially onto each item o
