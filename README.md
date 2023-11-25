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

and Tkinter
by Leodanis Pozo Ramos 9 Comments
 gamedev gui intermediate
Share Share Email
Table of Contents

Demo: A Tic-Tac-Toe Game in Python
Project Overview
Prerequisites
Step 1: Set Up the Tic-Tac-Toe Game Board With Tkinter
Ensure the Right Tkinter Version
Create a Class to Represent the Game Board
Step 2: Set Up the Tic-Tac-Toe Game Logic in Python
Define Classes for the Players and Their Moves
Create a Class to Represent the Game Logic
Set Up the Abstract Game Board
Figure Out the Winning Combinations
Step 3: Process the Players’ Moves on the Game’s Logic
Validate Players’ Moves
Process Players’ Moves to Find a Winner
Check for Tied Games
Toggle Players Between Turns
Step 4: Process Players’ Moves on the Game Board
Handle a Player’s Move Event
Update the Game Board to Reflect the Game State
Run Your Tic-Tac-Toe Game for the First Time
Step 5: Provide Options to Play Again and Exit the Game
Build the Game’s Main Menu
Implement the Play Again Option
Conclusion
Next Steps
Remove ads
Playing computer games is a great way to unwind or challenge yourself. Some people even do it professionally. It’s also fun and educational to build your own computer games. In this tutorial, you’ll build a classic tic-tac-toe game using Python and Tkinter.

With this project, you’ll go through the thought processes required for creating your own game. You’ll also learn how to integrate your diverse programming skills and knowledge to develop a functional and fun computer game.

In this tutorial, you’ll learn how to:

Program the classic tic-tac-toe game’s logic using Python
Create the game’s graphical user interface (GUI) using the Tkinter tool kit
Integrate the game’s logic and GUI into a fully functional computer game
As mentioned, you’ll be using the Tkinter GUI framework from the Python standard library to create your game’s interface. You’ll also use the model-view-controller pattern and an object-oriented approach to organize your code. For more on these concepts, check out the links in the prerequisites.

To download the entire source code for this project, click the link in the box below:

Get Source Code: Click here to get access to the source code that you’ll use to build your tic-tac-toe game.

Demo: A Tic-Tac-Toe Game in Python
In this step-by-step project, you’ll build a tic-tac-toe game in Python. You’ll use the Tkinter tool kit from the Python standard library to create the game’s GUI. In the following demo video, you’ll get a sense of how your game will work once you’ve completed this tutorial:


Your tic-tac-toe game will have an interface that reproduces the classic three-by-three game board. The players will take turns making their moves on a shared device. The game display at the top of the window will show the name of the player who gets to go next.

If a player wins, then the game display will show a winning message with the player’s name or mark (X or O). At the same time, the winning combination of cells will be highlighted on the board.

Finally, the game’s File menu will have options to reset the game if you want to play again or to exit the game when you’re done playing.

If this sounds like a fun project to you, then read on to get started!


Remove ads
Project Overview
Your goal with this project is to create a tic-tac-toe game in Python. For the game interface, you’ll use the Tkinter GUI tool kit, which comes in the standard Python installation as an included battery.

The tic-tac-toe game is for two players. One player plays X and the other plays O. The players take turns placing their marks on a grid of three-by-three cells. If a given player gets three marks in a row horizontally, vertically, or diagonally, then that player wins the game. The game will be tied if no one gets three in a row by the time all the cells are marked.

With these rules in mind, you’ll need to put together the following game components:

The game’s board, which you’ll build with a class called TicTacToeBoard
The game’s logic, which you’ll manage using a class called TicTacToeGame
The game board will work as a mix between view and controller in a model-view-controller design. To build the board, you’ll use a Tkinter window, which you can create by instantiating the tkinter.Tk class. This window will have two main components:

Top display: Shows information about the game’s status
Grid of cells: Represents previous moves and available spaces or cells
You’ll create the game display using a tkinter.Label widget, which allows you to display text and images.

For the grid of cells, you’ll use a series of tkinter.Button widgets arranged in a grid. When a player clicks one of these buttons, the game logic will run to process the player’s move and determine whether there’s a winner. In this case, the game logic will work as the model, which will manage the data, logic, and rules of your game.

Now that you have a general idea of how to build your tic-tac-toe game, you should check out a few knowledge prerequisites that’ll allow you to get the most out of this tutorial.

Prerequisites
To complete this tic-tac-toe game project, you should be comfortable or at least familiar with the concepts and topics covered in the following resources:

Python GUI Programming With Tkinter
Object-Oriented Programming (OOP) in Python 3
Python “for” Loops (Definite Iteration)
When to Use a List Comprehension in Python
Model-View-Controller (MVC) Explained – With Legos
Dictionaries in Python
How to Iterate Through a Dictionary in Python
Main Functions in Python
Write Pythonic and Clean Code With namedtuple
If you don’t have all of the suggested knowledge before starting this tutorial, that’s okay. You’ll learn by doing, so go ahead and give it a whirl! You can always stop and review the resources linked here if you get stuck.

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

Demo: A Tic-Tac-Toe Game in Python
Project Overview
Prerequisites
Step 1: Set Up the Tic-Tac-Toe Game Board With Tkinter
Step 2: Set Up the Tic-Tac-Toe Game Logic in Python
Step 3: Process the Players’ Moves on the Game’s Logic
Step 4: Process Players’ Moves on the Game Board
Step 5: Provide Options to Play Again and Exit the Game
