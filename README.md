Step 1: Set Up the Tic-Tac-Toe Game Board With Tkinter
To kick things off, you’ll start by creating the game board. Before doing this, you need to decide how to organize the code for your tic-tac-toe game. Because this project will be fairly small, you can initially keep all the code in a single .py file. This way, running the code and executing your game will be more straighforward.

Go ahead and fire up your favorite code editor or IDE. Then create a tic_tac_toe.py file in your current working directory:

# tic_tac_toe.py

"""A tic-tac-toe game built with Python and Tkinter."""
Throughout this tutorial, you’ll be incrementally adding code to this file, so keep it open and near. If you want to get the entire code for this tic-tac-toe game project, then you can click the following collapsible section and copy the code from it:


Having the entire source code beforehand will allow you to check your progress while going through the tutorial.

Alternatively, you can also download the game source code from GitHub by clicking the link in the box below:

Get Source Code: Click here to get access to the source code that you’ll use to build your tic-tac-toe game.

Now that you know what the game’s final code will look like, it’s time to make sure you have the right Tkinter version for this project. Then, you’ll go ahead and create your game board.


Remove ads
Ensure the Right Tkinter Version
To complete this project, you’ll use a standard Python installation. There’s no need to create a virtual environment because no external dependency is required. The only package that you’ll need is Tkinter, which comes with the Python standard library.

However, you need to make sure that you have the right Tkinter version installed. You should have Tkinter greater than or equal to 8.6. Otherwise, your game won’t work.

You can check your current Tkinter version by starting a Python interactive session and running the following code:

>>> import tkinter
>>> tkinter.TkVersion
8.6
If this code doesn’t show a version greater than or equal to 8.6 for your Tkinter installation, then you’ll need to fix that.

On Ubuntu Linux, you may need to install the python3-tk package using the system’s package manager, apt. That’s because typically Ubuntu doesn’t include Tkinter in its default Python installation.

Once you have Tkinter properly installed, then you need to check its current version. If the Tkinter version is lower than 8.6, then you’ll have to install a more recent version of Python either by downloading it from the official download page or by using a tool like pyenv or Docker.

On macOS and Windows, a straightforward option is to install a Python version greater than or equal to 3.9.8 from the download page.

Once you’re sure you have the right Tkinter version, you can get back to your code editor and start writing code. You’ll begin with the Python class that’ll represent the tic-tac-toe game board.

Create a Class to Represent the Game Board
To build the board of your tic-tac-toe game, you’ll use the Tk class, which allows you to create the main window of your Tkinter app. Then you’ll add a display on a top frame and a grid of cells covering the rest of the main window.

Go ahead and import the required objects and define the board class:

# tic_tac_toe.py

import tkinter as tk
from tkinter import font

class TicTacToeBoard(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Tic-Tac-Toe Game")
        self._cells = {}
In this code snippet, you first import tkinter as tk to bring the module’s name to your current namespace. Using the tk abbreviation is a common practice when it comes to using Tkinter in your code.

Then you import the font module directly from tkinter. You’ll use this module later in this tutorial to tweak the font of your game display.

The TicTacToeBoard class inherits from Tk, which makes it a full-fledged GUI window. This window will represent the game board. Inside .__init__(), you first call the superclass’s .__init__() method to properly initialize the parent class. To do this, you use the built-in super() function.

The .title attribute of Tk defines the text to show on the window’s title bar. In this example, you set the title to the "Tic-Tac-Toe Game" string.

The ._cells non-public attribute holds an initially empty dictionary. This dictionary will map the buttons or cells on the game board to their corresponding coordinates—row and column—on the grid. These coordinates will be integer numbers reflecting the row and column where a given button will appear.

To continue with the game board, you now need to create a display where you can provide information about the game’s state and result. For this display, you’ll use a Frame widget as the display panel and a Label widget to show the required information.

Note: The line numbers in the code samples in this tutorial are intended to facilitate the explanation. Most of the time, they won’t match the line numbers in your final script.

Now go ahead and add the following method to your TicTacToeBoard class:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    # ...

    def _create_board_display(self):
        display_frame = tk.Frame(master=self)
        display_frame.pack(fill=tk.X)
        self.display = tk.Label(
            master=display_frame,
            text="Ready?",
            font=font.Font(size=28, weight="bold"),
        )
        self.display.pack()
Here’s a breakdown of what this method does line by line:

Line 8 creates a Frame object to hold the game display. Note that the master argument is set to self, which means that the game’s main window will be the frame’s parent.

Line 9 uses the .pack() geometry manager to place the frame object on the main window’s top border. By setting the fill argument to tk.X, you ensure that when the user resizes the window, the frame will fill its entire width.

Lines 10 to 14 create a Label object. This label needs to live inside the frame object, so you set its master argument to the actual frame. The label will initially show the text "Ready?", which indicates that the game is ready to go, and the players can start a new match. Finally, you change the label’s font size to 28 pixels and make it bold.

Line 15 packs the display label inside the frame using the .pack() geometry manager.

Cool! You already have the game display. Now you can create the grid of cells. A classic tic-tac-toe game has a three-by-three grid of cells.

Here’s a method that creates the grid of cells using Button objects:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    # ...

    def _create_board_grid(self):
        grid_frame = tk.Frame(master=self)
        grid_frame.pack()
        for row in range(3):
            self.rowconfigure(row, weight=1, minsize=50)
            self.columnconfigure(row, weight=1, minsize=75)
            for col in range(3):
                button = tk.Button(
                    master=grid_frame,
                    text="",
                    font=font.Font(size=36, weight="bold"),
                    fg="black",
                    width=3,
                    height=2,
                    highlightbackground="lightblue",
                )
                self._cells[button] = (row, col)
                button.grid(
                    row=row,
                    column=col,
                    padx=5,
                    pady=5,
                    sticky="nsew"
                )
Wow! This method does a lot! Here’s an explanation of what each line does:

Line 8 creates a Frame object to hold the game’s grid of cells. You set the master argument to self, which again means that the game’s main window will be the parent of this frame object.

Line 9 uses the .pack() geometry manager to place the frame object on the main window. This frame will occupy the area under the game display, all the way to the bottom of the window.

Line 10 starts a loop that iterates from 0 to 2. These numbers represent the row coordinates of each cell in the grid. For now, you’ll have 3 rows on the grid. However, you’ll change this magic number later and provide the option of using a different grid size, such as four by four.

Line 11 and 12 configure the width and minimum size of every cell on the grid.

Line 13 loops over the three column coordinates. Again you use three columns, but you’ll change this number later to provide more flexibility and get rid of magic numbers.

Lines 14 to 22 create a Button object for every cell on the grid. Note that you set several attributes, including master, text, font, and so on.

Line 23 adds every new button to the ._cells dictionary. The buttons work as keys, and their coordinates—expressed as (row, col)—work as values.

Lines 24 to 30 finally add every button to the main window using the .grid() geometry manager.

Now that you’ve implemented ._create_board_display() and ._create_board_grid(), you can call them from the class initializer. Go ahead and add the following two lines to .__init__() in your TicTacToeBoard class:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Tic-Tac-Toe Game")
        self._cells = {}
        self._create_board_display()
        self._create_board_grid()
These two lines put together the game board by adding the display and grid of cells. Isn’t that cool?

With these updates, you can almost run your app and see how your tic-tac-toe game will look. You just need to write a few more lines of boilerplate code. You need to instantiate TicTacToeBoard and call its .mainloop() method to launch your Tkinter app.

Go ahead and add the following piece of code to the end of your tic_tac_toe.py file:

# tic_tac_toe.py
# ...

def main():
    """Create the game's board and run its main loop."""
    board = TicTacToeBoard()
    board.mainloop()

if __name__ == "__main__":
    main()
This code snippet defines a main() function for your game. Inside this function, you first instantiate TicTacToeBoard and then run its main loop by calling .mainloop().

The if __name__ == "__main__": construct is a common pattern in Python applications. It allows you to control the execution of your code. In this case, the call to main() will only happen if you run the .py file as an executable program, as opposed to an importable module.

That’s it! You’re now ready to run your game for the first time. Of course, the game isn’t playable yet, but the board is ready. To run the game, go ahead and execute the following command on your command line:

$ python tic_tac_toe.py
Once you’ve run this command, then you’ll get the following window on your screen:

Tic-Tac-Toe Game Board
Cool! Your tic-tac-toe game is starting to look like the real thing. Now you need to make this window respond to the players’ actions on the board.


Remove ads
Step 2: Set Up the Tic-Tac-Toe Game Logic in Python
Up to this point, you’ve put together a suitable tic-tac-toe game board using Tkinter. Now you need to think of how to deal with the game’s logic. This logic will consist of code that processes a player’s move and determines if this player has won the game or not.

Several ideas are key when it comes to implementing the logic of a tic-tac-toe game. First, you need a valid representation of the players and their moves. You also need a higher-level class to represent the game itself. In this section, you’ll define classes for these three logic concepts.

You can download the source code for this step by clicking the link below and navigating to the source_code_step_2/ folder:

Get Source Code: Click here to get access to the source code that you’ll use to build your tic-tac-toe game.

Define Classes for the Players and Their Moves
In the first round, you’ll define classes to represent players and their moves on the game board. These classes will be pretty bare-bones. All they need is a few attributes each. They don’t even need to have any methods.

You can use a few tools to build classes that fulfill these requirements. For example, you can use either a data class or a named tuple. In this tutorial, you’ll use a named tuple for both classes because this kind of class provides all you need.

Instead of using the classic namedtuple from the collections module, you’ll use the NamedTuple class from typing as a way to provide initial type hint information in your classes.

Go back to your code editor and add the following code at the beginning of your tic_tac_toe.py file:

# tic_tac_toe.py

import tkinter as tk
from tkinter import font
from typing import NamedTuple

class Player(NamedTuple):
    label: str
    color: str

class Move(NamedTuple):
    row: int
    col: int
    label: str = ""

# ...
Here’s a breakdown of this code snippet:

Line 5 imports NamedTuple from typing.

Lines 7 to 9 define the Player class. The .label attribute will store the classic player signs, X and O. The .color attribute will hold a string with a Tkinter color. You’ll use this color to identify the target player on the game board.

Lines 11 to 14 define the Move class. The .row and .col attributes will hold the coordinates that identify the move’s target cell. The .label attribute will hold the sign that identifies the player, X or O. Note that .label defaults to the empty string, "", which means that this specific move hasn’t been played yet.

With these two classes in place, now you can define the class that you’ll use to represent the game logic.

Create a Class to Represent the Game Logic
In this section, you’ll define a class to manage the game’s logic. This class will take care of processing the moves, finding a winner, toggling players, and performing a few other tasks. Get back to your tic_tac_toe.py file and add the following class right before your TicTacToeBoard class:

# tic_tac_toe.py
import tkinter as tk
from itertools import cycle
from tkinter import font
# ...

class TicTacToeGame:
    def __init__(self, players=DEFAULT_PLAYERS, board_size=BOARD_SIZE):
        self._players = cycle(players)
        self.board_size = board_size
        self.current_player = next(self._players)
        self.winner_combo = []
        self._current_moves = []
        self._has_winner = False
        self._winning_combos = []
        self._setup_board()

class TicTacToeBoard(tk.Tk):
    # ...
Here, you first import cycle() from the itertools module. Then you define TicTacToeGame, whose initializer takes two arguments, players and board_size. The players argument will hold a tuple of two Player objects, representing players X and O. This argument defaults to DEFAULT_PLAYERS, a constant that you’ll define in a moment.

The board_size argument will hold a number representing the size of the game board. In a classic tic-tac-toe game, this size would be 3. In your class, the argument defaults to BOARD_SIZE, another constant that you’ll define soon.

Inside .__init__(), you define the following instance attributes:

Attribute	Description
._players	A cyclical iterator over the input tuple of players
.board_size	The board size
.current_player	The current player
.winner_combo	The combination of cells that defines a winner
._current_moves	The list of players’ moves in a given game
._has_winner	A Boolean variable to determine if the game has a winner or not
._winning_combos	A list containing the cell combinations that define a win
The ._players attribute calls cycle() from the itertools module. This function takes an iterable as an argument and returns an iterator that cyclically yields items from the input iterable. In this case, the argument to cycle() is the tuple of default players passed in through the players argument. As you go through this tutorial, you’ll learn more about all the attributes in the above table.

Regarding the last line in .__init__(), it calls ._setup_board(), which is a method that you’ll also define in a moment.

Now go ahead and define the following constants right below the Move class:

# tic_tac_toe.py
# ...

class Move(NamedTuple):
    # ...

BOARD_SIZE = 3
DEFAULT_PLAYERS = (
    Player(label="X", color="blue"),
    Player(label="O", color="green"),
)

class TicTacToeGame:
    # ...
As you already learned, BOARD_SIZE holds the size of the tic-tac-toe board. Typically, this size is 3. So, you’ll have a three-by-three grid of cells on the board.

On the other hand, DEFAULT_PLAYERS defines a two-item tuple. Each item represents a player in the game. The .label and .color attributes of each player are set to suitable values. Player X will be blue, and player O will be green.


Remove ads
Set Up the Abstract Game Board
Managing the game’s state in every moment is a fundamental step in the game’s logic. You need to keep track of every move on the board. To do this, you’ll use the ._current_moves attribute, which you’ll update whenever a player makes a move.

You also need to determine which cell combinations on the board determine a win. You’ll store these combinations in the ._winning_combos attribute.

Here’s the ._setup_board() method, which computes initial values for ._current_moves and ._winning_combos:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

    def _setup_board(self):
        self._current_moves = [
            [Move(row, col) for col in range(self.board_size)]
            for row in range(self.board_size)
        ]
        self._winning_combos = self._get_winning_combos()
In ._setup_board(), you use a list comprehension to provide an initial list of values for ._current_moves. The comprehension creates a list of lists. Each inner list will contain empty Move objects. An empty move stores the coordinates of its containing cell and an empty string as the initial player’s label.

The last line of this method calls ._get_winning_combos() and assigns its return value to ._winning_combos. You’ll implement this new method in the following section.

Figure Out the Winning Combinations
On a classic tic-tac-toe board, you’ll have eight possible winning combinations. They’re essentially the rows, columns, and diagonals of the board. The following illustration shows these winning combinations:

Tic-Tac-Toe Winning Combinations
How would you get the coordinates of all these combinations using Python code? There are several ways to do it. In the code below, you’ll use four list comprehensions to get all the possible winning combinations:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

    def _get_winning_combos(self):
        rows = [
            [(move.row, move.col) for move in row]
            for row in self._current_moves
        ]
        columns = [list(col) for col in zip(*rows)]
        first_diagonal = [row[i] for i, row in enumerate(rows)]
        second_diagonal = [col[j] for j, col in enumerate(reversed(columns))]
        return rows + columns + [first_diagonal, second_diagonal]
The main input for this method is the ._current_moves attribute. By default, this attribute will hold a list containing three sublists. Each sublist will represent a row on the grid and have three Move objects in it.

The first comprehension iterates over the rows on the grid, getting the coordinates of every cell and building a sublist of coordinates. Each sublist of coordinates represents a winning combination. The second comprehension creates sublists containing the coordinates of each cell in the grid columns.

The third and fourth comprehensions use a similar approach to get the coordinates of every cell in the board diagonals. Finally, the method returns a list of lists containing all possible winning combinations on the tic-tac-toe board.

With this initial setup for your game board, you’re ready to start thinking about processing the players’ moves.

Step 3: Process the Players’ Moves on the Game’s Logic
In this tic-tac-toe game, you’ll mostly handle one type of event: players’ moves. Translated to Tkinter terms, a player’s move is just a click on the selected cell, which is represented by a button widget.

Every player’s move will trigger a bunch of operations on the TicTacToeGame class. These operations include:

Validating the move
Checking for a winner
Checking for a tied game
Toggling the player for the next move
In the following sections, you’ll write the code to handle all these operations in your TicTacToeGame class.

To download the source code for this step from GitHub, click the link below and navigate to the source_code_step_3/ folder:

Get Source Code: Click here to get access to the source code that you’ll use to build your tic-tac-toe game.


Remove ads
Validate Players’ Moves
You need to validate the move every time a player clicks a given cell on the tic-tac-toe board. The question is: What defines a valid move? Well, at least two conditions make a move valid. Players can only play if:

The game doesn’t have a winner.
The selected move hasn’t already been played.
Go ahead and add the following method to the end of TicTacToeGame:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

    def is_valid_move(self, move):
        """Return True if move is valid, and False otherwise."""
        row, col = move.row, move.col
        move_was_not_played = self._current_moves[row][col].label == ""
        no_winner = not self._has_winner
        return no_winner and move_was_not_played
The .is_valid_move() method takes a Move object as an argument. Here’s what the rest of the method does:

Line 9 gets the .row and .col coordinates from the input move.

Line 10 checks if the move at the current coordinates, [row][col], still holds an empty string as its label. This condition will be True if no player has made the input move before.

Line 11 checks if the game doesn’t have a winner yet.

This method returns a Boolean value that results from checking if the game has no winner and the current move hasn’t been played yet.

Process Players’ Moves to Find a Winner
Now it’s time for you to determine if a player has won the game after their last move. This may be your chief concern as the game designer. Of course, you’ll have many different ways to find out if a given player has won the game.

In this project, you’ll use the following ideas to determine the winner:

Every cell on the game board has an associated Move object.
Each Move object has a .label attribute that identifies the player who has made the move.
To find out if the last player has won the game, you’ll check if the player’s label is present in all the possible moves contained in a given winning combination.

Go ahead and add the following method to your TicTacToeGame class:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

    def process_move(self, move):
        """Process the current move and check if it's a win."""
        row, col = move.row, move.col
        self._current_moves[row][col] = move
        for combo in self._winning_combos:
            results = set(
                self._current_moves[n][m].label
                for n, m in combo
            )
            is_win = (len(results) == 1) and ("" not in results)
            if is_win:
                self._has_winner = True
                self.winner_combo = combo
                break
Here’s a breakdown of what this new method does line by line:

Line 7 defines process_move(), which takes a Move object as an argument.

Line 9 gets the .row and .col coordinates from the input move.

Line 10 assigns the input move to the item at [row][col] in the list of current moves.

Line 11 starts a loop over the winning combinations.

Lines 12 to 15 run a generator expression that retrieves all the labels from the moves in the current winning combination. The result is then converted into a set object.

Line 16 defines a Boolean expression that checks if the current move determined a win or not. The result is stored in is_win.

Line 17 checks the content of is_win. If the variable holds True, then ._has_winner is set to True and .winner_combo is set to the current combination. Then the loop breaks and the function terminates.

On lines 12 to 16, a few points and ideas need clarification. To better understand lines 12 to 15, say that all the labels in the moves associated with the cells of the current winning combination hold an X. In that case, the generator expression will yield three X labels.

When you feed the built-in set() function with several instances of X, you get a set with a single instance of X. Sets don’t allow repeated values.

So, if the set in results contains a single value different from the empty string, then you have a winner. In this example, that winner would be the player with the X label. Line 16 checks both of these conditions.

To wrap up the topic of finding the winner of your tic-tac-toe game, go ahead and add the following method at the end of your TicTacToeGame class:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

    def has_winner(self):
        """Return True if the game has a winner, and False otherwise."""
        return self._has_winner
This method returns the Boolean value stored in ._has_winner whenever you need to check if the current match has a winner or not. You’ll use this method later in this tutorial.


Remove ads
Check for Tied Games
In the tic-tac-toe game, if the players play on all the cells and there’s no winner, then the game is tied. So, you have two conditions to check before declaring the game as tied:

All possible moves have been played.
The game has no winner.
Go ahead and add the following method at the end of TicTacToeGame to check these conditions:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

    def is_tied(self):
        """Return True if the game is tied, and False otherwise."""
        no_winner = not self._has_winner
        played_moves = (
            move.label for row in self._current_moves for move in row
        )
        return no_winner and all(played_moves)
Inside .is_tied(), you first check if the game has no winner yet. Then you use the built-in all() function to check if all the moves in ._current_moves have a label different from the empty string. If this is the case, then all the possible cells have already been played. If both conditions are true, then the game is tied.

Toggle Players Between Turns
Every time a player makes a valid move, you need to toggle the current player so that the other player can make the next move. To provide this functionality, go ahead and add the following method to your TicTacToeGame class:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

    def toggle_player(self):
        """Return a toggled player."""
        self.current_player = next(self._players)
Because ._players holds an iterator that cyclically loops over the two default players, you can call next() on this iterator to get the next player whenever you need it. This toggling mechanism will allow the next player to take their turn and continue the game.

Step 4: Process Players’ Moves on the Game Board
At this point, you’re able to handle the players’ moves on the game logic. Now you have to connect this logic to the game board itself. You also need to write the code to make the board respond to players’ moves.

First, go ahead and inject the game logic into the game board. To do this, update the TicTacToeBoard class as in the code below:

# tic_tac_toe.py
# ...

class TicTacToeGame:
    # ...

class TicTacToeBoard(tk.Tk):
    def __init__(self, game):
        super().__init__()
        self.title("Tic-Tac-Toe Game")
        self._cells = {}
        self._game = game
        self._create_board_display()
        self._create_board_grid()

    # ...

    def _create_board_grid(self):
        grid_frame = tk.Frame(master=self)
        grid_frame.pack()
        for row in range(self._game.board_size):
            self.rowconfigure(row, weight=1, minsize=50)
            self.columnconfigure(row, weight=1, minsize=75)
            for col in range(self._game.board_size):
                # ...
In this code snippet, you first add a game argument to the TicTacToeBoard initializer. Then you assign this argument to an instance attribute, ._game, which will give you full access to the game logic from the game board.

The second update is to use ._game.board_size to set the board size instead of using a magic number. This update also enables you to use different board sizes. For example, you can create a four-by-four board grid, which can be an exciting experience.

With these updates, you’re ready to dive into handling the players’ moves on the TicTacToeBoard class.

As usual, you can download the source code for this step by clicking the link below and navigating to the source_code_step_4/ folder:

Get Source Code: Click here to get access to the source code that you’ll use to build your tic-tac-toe game.

Handle a Player’s Move Event
The .mainloop() method on the Tk class runs what’s known as the application’s main loop or event loop. This is an infinite loop in which all the GUI events happen. Inside this loop, you can handle the events on the application’s user interface. An event is a user action on the GUI, such as a keypress, mouse move, or mouse click.

When a player in your tic-tac-toe game clicks a cell, a click event occurs inside the game’s event loop. You can process this event by providing an appropriate handler method on your TicTacToeBoard class. To do this, get back to your code editor and add the following .play() method at the end of the class:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    # ...

    def play(self, event):
        """Handle a player's move."""
        clicked_btn = event.widget
        row, col = self._cells[clicked_btn]
        move = Move(row, col, self._game.current_player.label)
        if self._game.is_valid_move(move):
            self._update_button(clicked_btn)
            self._game.process_move(move)
            if self._game.is_tied():
                self._update_display(msg="Tied game!", color="red")
            elif self._game.has_winner():
                self._highlight_cells()
                msg = f'Player "{self._game.current_player.label}" won!'
                color = self._game.current_player.color
                self._update_display(msg, color)
            else:
                self._game.toggle_player()
                msg = f"{self._game.current_player.label}'s turn"
                self._update_display(msg)
This method is fundamental in your tic-tac-toe game because it puts together almost all the game logic and GUI behavior. Here’s a summary of what this method does:

Line 7 defines play(), which takes a Tkinter event object as an argument.

Line 9 retrieves the widget that triggered the current event. This widget will be one of the buttons on the board grid.

Line 10 unpacks the button’s coordinates into two local variables, row and col.

Line 11 creates a new Move object using row, col, and the current player’s .label attribute as arguments.

Line 12 starts a conditional statement that checks if the player’s move is valid or not. If the move is valid, then the if code block runs. Otherwise, no further action takes place.

Line 13 updates the clicked button by calling the ._update_button() method. You’ll write this method in the next section. In short, the method updates the button’s text to reflect the current player’s label and color.

Line 14 calls .process_move() on the ._game object using the current move as an argument.

Line 15 checks if the game is tied. If that’s the case, then the game display gets updated accordingly.

Line 17 checks if the current player has won the game. Then line 18 highlights the winning cells, and lines 19 to 21 update the game display to acknowledge the winner.

Line 22 runs if the game isn’t tied and there’s no winner. In this case, lines 23 to 25 toggle the player for the next move and update the display to point out the player who will play next.

You’re almost there! With a few more updates and additions, your tic-tac-toe game will be ready for its first-ever match.

The next step is to connect every button on the game board to the .play() method. To do this, get back to the ._create_board_grid() method and update it as in the code below:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    # ...

    def _create_board_grid(self):
        grid_frame = tk.Frame(master=self)
        grid_frame.pack()
        for row in range(self._game.board_size):
            self.rowconfigure(row, weight=1, minsize=50)
            self.columnconfigure(row, weight=1, minsize=75)
            for col in range(self._game.board_size):
                # ...
                self._cells[button] = (row, col)
                button.bind("<ButtonPress-1>", self.play)
                # ...
The highlighted line binds the click event of every button on the game board with the .play() method. This way, whenever a player clicks a given button, the method will run to process the move and update the game state.


Remove ads
Update the Game Board to Reflect the Game State
To complete the code for processing the players’ moves on the game board, you need to write three helper methods. These methods will complete the following actions:

Update the clicked button
Update the game display
Highlight the winning cells
To kick things off, go ahead and add ._update_button() to TicTacToeBoard:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    # ...

    def _update_button(self, clicked_btn):
        clicked_btn.config(text=self._game.current_player.label)
        clicked_btn.config(fg=self._game.current_player.color)
In this code snippet, ._update_button() calls .config() on the clicked button to set its .text attribute to the current player’s label. The method also sets the text foreground color, fg, to the current player’s color.

The next helper method to add is ._update_display():

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    # ...

    def _update_display(self, msg, color="black"):
        self.display["text"] = msg
        self.display["fg"] = color
In this method, instead of using .config() to tweak the text and color of the game display, you use a dictionary-style subscript notation. Using this type of notation is another option that Tkinter provides for accessing a widget’s attributes.

Finally, you need a helper method to highlight the winning cells once a given player makes a winning move:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    # ...

    def _highlight_cells(self):
        for button, coordinates in self._cells.items():
            if coordinates in self._game.winner_combo:
                button.config(highlightbackground="red")
The loop inside ._highlight_cells() iterates over the items in the ._cells dictionary. This dictionary maps buttons to their row and column coordinates on the board grid. If the current coordinates are in a winning cell combination, then the button’s background color is set to "red", highlighting the cell combination on the board.

With this last helper method in place, your tic-tac-toe game is ready for the first match!

Run Your Tic-Tac-Toe Game for the First Time
To finish putting together the logic and the user interface of your tic-tac-toe game, you need to update the game’s main() function. Up to this point, you have a TicTacToeBoard object only. You need to create a TicTacToeGame object and pass it over to the TicTacToeBoard updated constructor.

Get back to main() and update it like in the code below:

# tic_tac_toe.py
# ...

def main():
    """Create the game's board and run its main loop."""
    game = TicTacToeGame()
    board = TicTacToeBoard(game)
    board.mainloop()
In this code, the first highlighted line creates an instance of TicTacToeGame, which you’ll use to handle the game logic. The second highlighted line passes the new instance to the TicTacToeBoard class constructor, which injects the game logic into the game board.

With these updates in place, you can now run your game. To do this, fire up a terminal window and navigate to the directory containing your tic_tac_toe.py file. Then run the following command:

$ python tic_tac_toe.py
Once this command has run, then your game’s main window will appear on your screen. Go ahead and give it a try! It’ll behave something like this:


Wow! Your game project looks amazing so far! It allows two players to share their mouse and play a classic tic-tac-toe match. The game GUI looks nice and, in general, the game works as expected.

In the following section, you’ll write code to allow the players to restart the game and play again. You’ll also provide the option of exiting the game.


Remove ads
Step 5: Provide Options to Play Again and Exit the Game
In this section, you’ll provide your tic-tac-toe game with a main menu. This menu will have an option to restart the game so that the players can start another match. It’ll also have an option to exit the game once the players have finished playing.

Main menus are often an essential component of many GUI applications. So, learning how to create them in Tkinter is a good exercise to improve your GUI-related skills beyond the game development itself.

This is an example of how building your own games is a powerful learning experience because it allows you to integrate knowledge and skills that you can later use in other non-game projects.

The complete source code for this step is available for download. Just click the link below and navigate to the source_code_step_5/ folder:

Get Source Code: Click here to get access to the source code that you’ll use to build your tic-tac-toe game.

Build the Game’s Main Menu
To add a main menu to a Tkinter application, you can use the tkinter.Menu class. This class allows you to create a menu bar on top of your Tkinter window. It also allows you to add dropdown menus to the menu bar.

Here’s the code that creates and adds a main menu to your tic-tac-toe game:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    def __init__(self, game):
        # ...

    def _create_menu(self):
        menu_bar = tk.Menu(master=self)
        self.config(menu=menu_bar)
        file_menu = tk.Menu(master=menu_bar)
        file_menu.add_command(
            label="Play Again",
            command=self.reset_board
        )
        file_menu.add_separator()
        file_menu.add_command(label="Exit", command=quit)
        menu_bar.add_cascade(label="File", menu=file_menu)
Here’s what this code does line by line:

Line 8 defines a helper method called ._create_menu() to handle the menu creation in a single place.

Line 9 creates an instance of Menu, which will work as the menu bar.

Line 10 sets the menu bar object as the main menu of your current Tkinter window.

Line 11 creates another instance of Menu to provide a File menu. Note that the master argument in the class constructor is set to your menu bar object.

Lines 12 to 15 add a new menu option to the File menu using the .add_command() method. This new option will have the label "Play Again". When a user clicks this option, the application will run the .reset_board() method, which you provided through the command argument. You’ll write this method in the following section.

Line 16 adds a menu separator using the .add_separator() method. Separators are useful when you need to separate groups of related commands in a given dropdown menu.

Line 17 adds an Exit command to the File menu. This command will make the game exit by calling the quit() function.

Line 18 finally adds the File menu to the menu bar by calling .add_cascade() with appropriate arguments.

To actually add the main menu to your game’s main window, you need to call ._create_menu() from the initializer of TicTacToeBoard. So, go ahead and add the following line to the class’s .__init__() method:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
    def __init__(self, game):
        super().__init__()
        self.title("Tic-Tac-Toe Game")
        self._cells = {}
        self._game = game
        self._create_menu()
        self._create_board_display()
        self._create_board_grid()

    # ...
With this final update, your game’s main menu is almost ready for use. However, before using it, you must implement the .reset_board() method. That’s what you’ll do in the following section in order to allow the players to play again.

Implement the Play Again Option
To reset the game board and allow the players to play again, you need to add code to both classes, TicTacToeGame and TicTacToeBoard.

In the game logic class, you need to reset the ._current_moves attribute to hold a list of initially empty Move objects. You also need to reset the ._has_winner and .winner_combo to their initial state. On the other hand, in the game board class, you need to reset the board display and cells to their initial state.

Get back to TicTacToeGame in your code editor and add the following method right at the end of the class:

# tic_tac_toe.py
# ...

class TicTacToeGame(tk.Tk):
        # ...

    def reset_game(self):
        """Reset the game state to play again."""
        for row, row_content in enumerate(self._current_moves):
            for col, _ in enumerate(row_content):
                row_content[col] = Move(row, col)
        self._has_winner = False
        self.winner_combo = []
The for loop in .reset_game() sets all the current moves to an empty Move object. An empty move’s main characteristic is that its .label attribute holds the empty string, "".

After updating the current moves, the methods sets ._has_winner to False and .winner_combo to an empty list. These three resets ensure that the game’s abstract representation is ready to start a new match.

Note that reset_game() doesn’t reset the player back to X when preparing the game for a new match. Typically, the winner of the previous match gets to go first in the next one. So, there’s no need to reset the player here.

Once you’ve provided the required new functionality in the game logic, then you’re ready to update the game board functionality. Go ahead and add the following method to the end of TicTacToeBoard:

# tic_tac_toe.py
# ...

class TicTacToeBoard(tk.Tk):
        # ...

    def reset_board(self):
        """Reset the game's board to play again."""
        self._game.reset_game()
        self._update_display(msg="Ready?")
        for button in self._cells.keys():
            button.config(highlightbackground="lightblue")
            button.config(text="")
            button.config(fg="black")
This .reset_board() method works as follows:

Line 9 calls .reset_game() to reset the board’s abstract representation.

Line 10 updates the board display to hold the initial text, "Ready?".

Line 11 starts a loop over the buttons on the board grid.

Lines 12 to 14 restore every button’s highlightbackground, text, and fg properties to their initial state.

That’s it! With this last feature, your tic-tac-toe game project is complete. Go ahead and give it a try!# TicTacToe
