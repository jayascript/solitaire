# Python Strategic Solitaire Simulator
### Jaya Z. Moore

This code uses object-oriented programming in Python to simulate Solitaire games that implement the **Solitaire Strategy Guide** found on [chessandpoker.com](http://www.chessandpoker.com/solitaire_strategy.html). Furthermore, the program implements a simple betting strategy to mimic playing against the House, with a mandatory pay-to-play fee, earnings for each card played to an Ace stack, and bonus winnings for filling up all Aces stacks (i.e. "winning" the game).

Compared to the original codebase ([found here](https://github.com/suryadutta/solitaire)], this simulation results in a significant increase in the win rate (from 4% according to the developer to nearly 40% according to my own calculations), one that rivals [that of professional Solitaire players](http://www.jupiterscientific.org/sciinfo/KlondikeSolitaireReport.html).

## Getting Started

Before you run the program, be sure to have the requirements installed:

```
python3 -m pip install -r requirements.txt
```

Activate your virtual environment in the current working directory. To play one simulated game of Solitaire, run the following command in your terminal:

```
python3 solitaire.py
```

You'll be presented with 3 questions asking you to choose some options for how the game will run:

```
--- STRATEGIC SOLITAIRE ---

How much would you like to bet? Input an amount with numbers and
decimals only (e.g. 1.50).
 >>
Unknown input. Using default value: $1.50

Would you like to see each move? Y/N:
 >>
Unknown input. Using default value: verbose = False

Choose a mode:
1) Strategic
2) Simple
 >>
Unknown input. Using default value: strategic

You won! Total winnings: 26.4
```

If you don't put in any values, or your values can't be read by the program, then the simulation will run with default values. If you choose to see each move, then the console will output each action and the state of the board after each move:

```
{   'ace stacks': {'♠': '', '♡': '', '♢': '', '♣': ''},
    'deck': '5 ♠, 8 ♠, 7 ♡, 9 ♡, K ♡, 3 ♢, 7 ♢, 4 ♡, 9 ♣, 10 ♠, J ♡, 10 ♣, 9 '
            '♢, 4 ♣, K ♠, 8 ♡, 4 ♢, 8 ♣, A ♠, 6 ♣, 3 ♠, J ♢, 6 ♡, 8 ♢',
    'stacks': [   'Q ♠',
                  '1 cards face down., J ♣',
                  '2 cards face down., 5 ♢',
                  '3 cards face down., 2 ♣',
                  '4 cards face down., 5 ♣',
                  '5 cards face down., A ♣',
                  '6 cards face down., 2 ♢']}


Move 0: Play A ♣ from board to Ace stack

{   'ace stacks': {'♠': '', '♡': '', '♢': '', '♣': 'A ♣'},
    'deck': '5 ♠, 8 ♠, 7 ♡, 9 ♡, K ♡, 3 ♢, 7 ♢, 4 ♡, 9 ♣, 10 ♠, J ♡, 10 ♣, 9 '
            '♢, 4 ♣, K ♠, 8 ♡, 4 ♢, 8 ♣, A ♠, 6 ♣, 3 ♠, J ♢, 6 ♡, 8 ♢',
    'stacks': [   'Q ♠',
                  '1 cards face down., J ♣',
                  '2 cards face down., 5 ♢',
                  '3 cards face down., 2 ♣',
                  '4 cards face down., 5 ♣',
                  '5 cards face down.',
                  '6 cards face down., 2 ♢']}
```

At the end of the run, the console will print out whether or not the run achieved a win state and how much the player would receive in earnings. In the original codebase, the developer notes that this happened about 4% of the time. In this updated codebase, however, the Solitaire strategy implemented in this program achieves a win state **nearly 40% of the time.**

## Definitions

* **Deck**:  The stack of cards from which the player can draw. This code implements one card draw.
* **Play Stacks**:  The seven piles that cards are added to and shuffled around on the board. Each pile contains a mixture of face up and face down cards. Any subsequent face up cards must alternate suit color and decrease in value.
* **Ace Stacks**: Also called foundations. The four stacks (one for each suit) that build up from Ace to King. When all ace stacks are filled, the player wins the game.

Note that in this betting implementation, the player may "win" money even though they didn't "win" the game.

## Code Overview

The code has three files:

* `setup.py` contains three classes: **Card**, **Stack**, and **Deck**. Each of these classes have methods and variables that persist throughout the game.

* `strategy.py` contains one class: **Game**. This class contains methods for initializing a game of solitaire, updating the game status, and simulating a complete game using the professional algorithm discussed below.

* `solitaire.py` contains a simple script to simulate one game and output the elements and result. If verbosity is enabled, the user will be able to scroll through the console and view each move made during the simulation.

## Algorithm

The original developer explains in his documentation that the strategy he implemented was "just the simplest one [he] could think of that made sense and could sometimes win." He linked to the strategy listed on [chessandpoker.com](http://www.chessandpoker.com/solitaire_strategy.html) and noted that it might be a more successful algorithm. My goal for this project was to implement this new Solitaire strategy and compare it to the original strategy developed by Surya.

### ChessandPoker.com Solitaire Strategy
The **ChessandPoker.com Solitaire Strategy Guide** says to move through the game in the following order:

1. Always play an Ace or Deuce wherever you can immediately.
2. Always make the play or transfer that frees (or allows a play that frees) a downcard, regardless of any other considerations.
3. When faced with a choice, always make the play or transfer that frees (or allows a play that frees) the downcard from the biggest pile of downcards.
4. Transfer cards from column to column only to allow a downcard to be freed or to make the columns smoother.
5. Don't clear a spot unless there's a King IMMEDIATELY waiting to occupy it.
6. Only play a King that will benefit the column(s) with the biggest pile of downcards, unless the play of another King will at least allow a transfer that frees a downcard.
7. Only build your Ace stacks (with anything other than an Ace or Deuce) when the play will:
    * Not interfere with your Next Card Protection
    * Allow a play or transfer that frees (or allows a play that frees) a downcard
    * Open up a space for a same-color card pile transfer that allows a downcard to be freed
    * Clear a spot for an IMMEDIATE waiting King (it cannot be to simply clear a spot)
8. Don't play or transfer a 5, 6, 7 or 8 anywhere unless at least one of these situations will apply after the play:
    * It is smooth with it's next highest even/odd partner in the column
    * It will allow a play or transfer that will IMMEDIATELY free a downcard
    * There have not been any other cards already played to the column
    * You have ABSOLUTELY no other choice to continue playing (this is not a good sign)
9. When you get to a point that you think all of your necessary cards are covered and you just can't get to them, IMMEDIATELY play any cards you can to their appropriate Ace stacks. You may have to rearrange existing piles to allow blocked cards freedom to be able to go to their Ace stack. Hopefully this will clear an existing pile up to the point that you can use an existing pile upcard to substitute for the necessary covered card.

For more details, [read the full guide](http://www.chessandpoker.com/solitaire_strategy.html).

I played through a game of Solitaire myself using this strategy, and was surprised to find that I won it easily! So, I was excited to implement this algorithm in Python and see what the machine could do.

Not all aspects of this algorithm are implemented perfectly. There's definitely room for improvement. However, each strategy is implemented in some aspect in this program.

Just like the original program, the algorithm will pick the first item on the list that is successful, and restart from the top on the next turn. This ensures that the best strategy is always followed first before moving on to the lesser options. If there are no possible moves left, the game will end.

### Results

My implementation of this algorithm consistently showed a success rate (i.e. all cards played to their respective Ace stacks) of nearly 40%. In fact, I chose to stop tweaking when I did when I learned that this is just about the odds of a professional player winning any given game of Solitaire using their best plays. A report on [Jupiter Scientific](http://www.jupiterscientific.org/sciinfo/index.html) referenced [this paper](http://web.engr.oregonstate.edu/~afern/papers/solitaire.pdf) which showed that only around 82% of Solitaire games are likely winnable to begin with. Of those, the report concluded that the odds of winning with good plays are around 43%, whereas consistently performing the best plays should increase that percentage significantly.

As I mentioned earlier, there's room for improvement in the development of the strategic Solitaire algorithm. However, even this rudimentary implementation gets success rates that are pretty close to what one would expect from a professional human player. If you're interested in contributing to the repo and beefing up this algorithm, then open a pull request and let's see how high the success rate can go!
