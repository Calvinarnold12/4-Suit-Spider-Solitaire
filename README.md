# 4-Suit Spider Solitaire  

The purpose of this project is to create a fast, non-visual simulation of the 4-Suit Spider Solitaire game and pair it with multiple heuristic algorithms to find one with a high relative success rate.  
This program will be written in **Java** and will use **JUnit** as a testing framework.

---

## Game Rules  

### Setting Up  
1. Two decks of cards are shuffled together so that there are two copies of every card.  
2. The cards are placed sequentially into 10 stacks: the first 4 stacks contain 5 cards each, and the remaining 6 stacks contain 4 cards each.  
3. One final deal is made on top of each stack (face up). The remaining cards form the stock for later dealing.  

### Game Actions  
1. **Move Action**:  
   - You can move a card of value *n* from its stack to another stack if the destination stack has a card of value *n+1* or if the destination stack is empty.  
   - You can move a sequence of cards provided that every card in that sequence is of the same suit and forms a descending sequence. The destination must have a card of value *n+1* relative to the last card in the moved sequence, or the destination stack must be empty.  

2. **Deal**:  
   - You can deal from the stock at any time, provided there are no empty stacks.  
   - One card is dealt sequentially onto each stack until the stock is empty.  

3. **New Game**:  
   - Generates a new game from a random seed.  

### Game Conditions  
1. **Stack Complete**:  
   - If a complete sequence from King to Ace (all of the same suit) is detected after a move, that sequence is removed from the game.  
   - This scores one point. A full completion is 8 points.  

2. **Game Complete**:  
   - The game ends successfully when no stacks remain.  

3. **Game Over**:  
   - If there are no more cards to deal and no useful moves remain.  
   - Example: there are two stacks with an open 8 and one stack with an open 7 — we don’t want the algorithm to get stuck infinitely moving the 7 between both 8s.  

---

## Game Structure & Implementation Logic  

1. A card stack consists of multiple arrays; each array is sorted and of the same suit.  
2. If a card is moved onto another stack and matches the suit, it is added to the existing array.  
3. If a card is moved onto another stack, is numerically *n-1* relative to the destination, and is not of the same suit, a new array is created and pushed on top of the stack.  
4. If an array is moved, it follows the same rules based on its head card.  
5. If the moved array is *n-1* relative to the destination card and is not of the same suit, it is pushed onto the top of the stack as a new array.  
6. When a stack-complete condition is satisfied, the array is removed and a point awarded.  
7. When dealing, if the tail of an array matches *n+1* and the same suit relative to the new card, the card joins the existing array; otherwise, it starts a new array on top of the stack.  
8. If a deal places a card into the middle of an array, the array is split: remaining elements stay, and the new cards form a new array.  

---

## Java Classes  

- **Card.java**  
  - Contains an `int` for card value, an `enum` for suit, and a `boolean` for face-up or face-down state.  
  - May also track times seen and number on board for heuristic decision-making.  

- **DoubleDeck.java**  
  - Represents two decks of cards in a random order.  
  - Constructor shuffles cards and ensures exactly two copies of each card.  

- **FourSuitSS.java**  
  - Core game structure.  
  - Manages 10 stacks and deals cards from a `DoubleDeck`.  
  - Handles moves, splitting arrays, checking legality, scoring completed sequences, checking for game over, and drawing the board in terminal mode.  

- **FourSuitsSSDriver.java**  
  - Player interface for manual gameplay.  

- **FourSuitsSSAlgorithm.java**  
  - Driver for automated/algorithmic play.  
  - Allows multiple strategies and outputs statistics on performance.  

- **FourSuitsSSTest.java**  
  - Unit tests for the `FourSuitSS` game structure using JUnit.  

---

## Simulation & Algorithm Design  

### Strategy Interface  
- Define a `Strategy` interface or abstract class to represent a move-deciding algorithm.  
- Algorithms will implement `Strategy` and can be plugged into `FourSuitsSSAlgorithm`.  

### Heuristic Examples  
- **Greedy Move**: always make the move that frees the most cards.  
- **Suit-Stack Prioritization**: prefer moves that build same-suit sequences.  
- **Empty-Stack-Prioritization**:  prefer moves that lead to open stacks

### Metrics Collected  
- Win rate (percentage of games completed).  
- Average score per game.  
- Average moves per game.  
- Average time per game.  

---

## Testing Plan  

### Unit Tests (FourSuitsSSTest.java)  
- `testValidMove()`  
- `testInvalidMove()`  
- `testDealCards()`  
- `testStackCompletion()`  
- `testGameOverCondition()`  
- `testAlgorithmPerformance()` (simulate multiple games).  

### Integration Tests  
- Simulate a full game from a known seed and verify the outcome.  
- Simulate algorithm runs to ensure stable performance metrics.  

### Coverage Goals  
- Aim for at least 80% line coverage across `FourSuitSS` and related classes.  

---

## Performance & Scaling  

- **Bulk Simulation**:  
  - Provide a method to run thousands of games automatically with different seeds.  
  - Collect aggregated statistics for each algorithm.  

- **Parallelization**:  
  - Allow multiple games to run concurrently using Java threads or `ForkJoinPool` for faster benchmarking.  

- **State Management**:  
  - Add a way to clone or serialize game states for fast rollbacks during heuristic evaluation.  

- **Logging**:  
  - Implement a `GameStats` or logging class to track each game’s progress and output in a structured format (CSV/JSON).  

---

## Class Diagram  

```mermaid
classDiagram
    class Card {
        -int value
        -Suit suit
        -boolean faceUp
        -int timesSeen
        -int numOnBoard
    }

    class DoubleDeck {
        -List<Card> cards
        +shuffle()
        +dealCard() Card
    }

    class FourSuitSS {
        -Stack<Card[]> stacks[10]
        -DoubleDeck deck
        +moveCard(fromStack, toStack)
        +deal()
        +checkIfGameOver()
        +score()
        +drawGameboard()
    }

    class FourSuitsSSDriver {
        +main()
        +getPlayerInput()
    }

    class FourSuitsSSAlgorithm {
        +runAlgorithm(strategy)
        +generateStats()
    }

    class FourSuitsSSTest {
        +testMove()
        +testDeal()
        +testGameOver()
    }

    interface Strategy {
        +chooseMove(gameState)
    }

    FourSuitSS --> DoubleDeck : uses
    FourSuitSS --> Card : contains
    FourSuitsSSDriver --> FourSuitSS : controls
    FourSuitsSSAlgorithm --> FourSuitSS : automates
    FourSuitsSSTest --> FourSuitSS : tests
    FourSuitsSSAlgorithm --> Strategy : uses
