# Python-Word_Search-Generator
A Python based word search generator where the user may input their own words and it automatically creates a PLAYABLE word search game. It takes in the x,y coordinates of the first letter and last letter of the word and reveals If it is correct. 

This documentation provides an overview of a simple word search game implemented in Python. The game allows users to input words, creates a word search grid, and provides options for revealing words or guessing their positions based on coordinates.

## Table of Contents

1. [Introduction](#introduction)
2. [Dependencies](#dependencies)
3. [Game Initialization](#game-initialization)
   - [Word Input](#word-input)
4. [Word Search Board](#word-search-board)
   - [Board Size](#board-size)
   - [Random Filling](#random-filling)
   - [Display](#display)
5. [Word Placement](#word-placement)
   - [Adding Words](#adding-words)
   - [Checking Overlap](#checking-overlap)
6. [User Interaction](#user-interaction)
   - [Guessing Word Positions](#guessing-word-positions)
   - [Revealing Words](#revealing-words)
7. [Game Loop](#game-loop)
8. [Usage](#usage)
9. [License](#license)

## Introduction

This Python program implements a word search game where users can input words, and the program generates a word search grid. Users can then interact with the game by either revealing words or attempting to guess the positions of words on the grid.

## Dependencies

The game relies on the following Python packages:

- **random:** Provides functions for generating random numbers.
- **replit:** Facilitates clearing the console for a cleaner display. This code was hosted on the Replit website.
- **colorama:** Adds color to console output.

```bash
pip install colorama random
```

## Game Initialization

### Word Input

Users can input words for the word search game. The input process continues until the user enters "Done" to finish. The program validates input, ensuring that only letters are accepted, and avoids duplicates.

```python
# Word input loop
loop = True
while loop:
    print("Enter a word (Input \"Done\" to finish).")
    print("Current words:")
    for i in range(len(WordSearch.words)):
        print(f"{i+1}. {Fore.YELLOW + Style.BRIGHT + WordSearch.words[i] + Style.RESET_ALL}")
    word = input(Fore.RED + Style.BRIGHT + ">>> " + Style.RESET_ALL)
    clear()

    if word.capitalize() == "Done":
        if len(WordSearch.words) <= 3:
            clear()
            print("Not enough words! You need at least 4 words.")
            loop = True
        else:
            loop = False
    else:
        # Input validation
        if word.isalpha():
            if word.upper() not in WordSearch.words:
                WordSearch.words.append(word.upper())
            else:
                clear()
                print("You already picked that word!")
                loop = True
        else:
            clear()
            print("You may only input letters!")
            loop = True
```

## Word Search Board

### Board Size

The size of the word search board is dynamically set based on the length of the longest word inputted by the user. The size is adjusted to accommodate the words comfortably.

```python
class WordSearch:
  # .....
  @classmethod
  def set_board_size(cls):
    for i in range(len(cls.words)):
      # Going through every word
      if len(cls.words[i]) > cls.size:
        # If word length is more than current board size, current board size gets set to the word lenght + 2
        if len(cls.words) >= 6:
          add = 3
        else:
          add = 0
        cls.size = len(cls.words[i]) + 3 + add
  # .....
  @staticmethod
  def increase_size():
    for i in range(0, WordSearch.size):
      # Adds a new column in existing rows
      for x in range(0,3):
        #print(i)
        WordSearch.wordsearch[i].append("-")
    increase = 2
    for a in range(0,3):
      WordSearch.wordsearch.append([])
      WordSearch.size += 1

      index = len(WordSearch.wordsearch)-1
      
      for x in range(0, WordSearch.size + increase):
        # Adds new row
        WordSearch.wordsearch[index].append("-")
      increase -= 1
```

### Random Filling

Board is initially filled with empty spaces with a blank canvas, after words have been addded, the empty spaces on the board are filled with random letters.

```python
class WordSearch:
  # .....
  #---Filling board with empty spaces---
  @classmethod
  def fill_board(cls):
    for row in range(0, cls.size):
      
      cls.wordsearch.append([])
      
      for col in range(0, cls.size):
        
        cls.wordsearch[row].append("-")
  # .....
  #---Fill empty spaces with random letters---
  @classmethod
  def randomFill_letters(cls):
  
    LETTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
  
    for row in range(0, cls.size):
  
      for col in range(0, cls.size):
        if cls.wordsearch[row][col] == "-":
          # If the current bit is a "-" (empty), set it a random letter
          randomLetter = random.choice(LETTERS)
          
          cls.wordsearch[row][col] = randomLetter
  # .....
```

### Display

The game provides a visual representation of the word search board, displaying letters in different colors to enhance visibility.

```python
class WordSearch:
  # .....
  @classmethod
  def displayWordsearch(cls, list):
    #-----Make top part of grid-----
  
    #Make the letters
    letter = 65
    
    print("    ", end="")
    
    for i in range(cls.size):
      
      print(f"{Fore.CYAN + Style.BRIGHT + chr(letter)}", end=" ")
      
      letter += 1
  
    #Make the bar
    print("\n   ", end="")
    
    for i in range(cls.size):
      
      print(Style.DIM + Fore.MAGENTA + "__", end="")
      
    print(Style.DIM + Fore.MAGENTA + '_')
  
    #Empty line of grid
    print(Style.DIM + Fore.MAGENTA + "  |", end="")
    
    for i in range(cls.size):
      
      print("  ", end="")
      
    print(Style.DIM + Fore.MAGENTA + " |")
  
    #-----Main Part-----
  
    #Print words & letters on side
    letter = 65
    
    for row in list:
      
      print(f"{Fore.YELLOW + Style.BRIGHT + chr(letter)} {Fore.MAGENTA + Style.DIM}|{Style.RESET_ALL} {' '.join(row)} {Fore.MAGENTA + Style.DIM}|")
      
      letter += 1
  
    #-----Bottom-----
  
    #Empty bar
    print(Style.DIM + Fore.MAGENTA + "  |", end="")
    
    for i in range(cls.size):
      
      print(Style.DIM + Fore.MAGENTA + "__", end="")
      
    print(Style.DIM + Fore.MAGENTA + "_|")
  # .....
```

## Word Placement

### Adding Words

Words inputted by the user are placed on the word search board in various directions, including horizontal, vertical, and diagonal.

```python
class WordSearch:
   # .....
   @classmethod
     def addWord(cls, word):
       direction = ["horizontal", "vertical", "diagonal"]
   
       loop = True
       count = 0
   
       while loop:
   
         if count >= 4:
           # If direction been changed 4 times, increase size of board
           WordSearch.increase_size()
           count = 0
       
         choice = direction[random.randint(0, len(direction)-1)]
     
         if word == "TESTTESTTESTTEST":
           choice = "diagonal"
     
         
         # horizontal
         if choice == "horizontal":
     
           row, col, newDirection = cls.check_no_overlap(word, cls.size-1, cls.size-len(word), choice, 0) # word, row, col, direction, add
       
           if newDirection:
             # If looped 4 times, gotta loop back whole thing to pick a new direction
             loop = True
             count += 1
           else:
           
             for i in range(0, len(word)):
               # Adds letter by letter to the row
               cls.wordsearch[row][col + i] = word[i]
               cls.wordpos.append((row, col + i))
     
             loop = False
     
         # vertical
         elif choice == "vertical":
     
           row, col, newDirection = cls.check_no_overlap(word, cls.size-len(word), cls.size-1, choice, 0) # word, row, col, direction, add
           
           if newDirection:
             # If looped 4 times, gotta loop back whole thing to pick a new direction
             loop = True
             count += 1
           else:
           
             for i in range(0, len(word)):
               # Adds letter by letter to the row
               cls.wordsearch[row + i][col] = word[i]
               cls.wordpos.append((row + i, col))
       
             loop = False
           
         elif choice == "diagonal":
     
           row, col, newDirection = cls.check_no_overlap(word, cls.size-len(word), cls.size-len(word), choice, len(word)-1) # word, row, col, direction, add
   
           if newDirection:
             # If looped 4 times, gotta loop back whole thing to pick a new direction
             loop = True
             count += 1
           else:
     
             for i in range(0, len(word)):
               # Adds letter by letter to the row
               cls.wordsearch[row + i][col + i] = word[i]
               cls.wordpos.append((row + i, col + i))
     
             loop = False
   # .....
   @classmethod
     def add_words_to_board(cls):
       for i in range(len(cls.words)):
         cls.addWord(cls.words[i].upper())
   # .....
```

### Checking Overlap

The program checks for overlap when placing words to ensure that words do not intersect. If overlap is detected, the program attempts to choose a new direction for word placement.

```python
class WordSearch:
  # .....
  @classmethod
  def check_no_overlap(cls, word: str, row: int, col: int, direction: str, add: int):

    count = 0
    loop = True


    while loop:
      overlap = False
      xvec, yvec = 0, 0
      row = random.randint(0, row)#-add)
      col = random.randint(0, col)

      for char in word:
        # Going through each letter in the word
        for ipos, cpos in cls.wordpos:
          # Looking through each coords used already
          if row + yvec == ipos and col + xvec == cpos:
            # Coords already being used
            overlap = True
          else:
            # Coords not being used
            pass
            
        if direction == "horizontal":
          xvec += 1
          
        elif direction == "vertical":
          yvec += 1
          
        else:
          xvec += 1
          yvec += 1
          
      if overlap:
        count += 1
        if count >= 4:
          # If looped 4 times, breaks the loop and returns for the function to pick a new direction
          loop = False
          newDirection = True
        else:
          # Loops picking coords if the limit hasn't been reached yet
          loop = True
      else:
        # Doesn't loop or change direction
        loop = False
        newDirection = False
          
    return row, col, newDirection
  # .....
```

## User Interaction

### Guessing Word Positions

Users can guess the positions of words by providing coordinates (e.g., X1, Y1, X2, Y2). The program validates the coordinates and reveals the word on the board if the guess is correct and the word was in the game.

```python
class WordSearch:
  # .....
  #---Getting word possitions and validation---
  @classmethod
  def guess_word_position(cls, coordslist: list):
    formedword = []
    if len(coordslist) == 4:
      # If there are 4 coordiinates
      # code to check here
      # convert coords to list indices e.g. a,a,c,c --> 0,0,2,2
      # check coord validity
      # get string of coord
      # check if word exists
      # x1 = x2
      # y1 = y2
      # x1-x2 = y1-y2
      
      # converting the alphabet coords to list indices and unpacking it into the 4 variables
      x1, y1, x2, y2 = cls.convert_coords(coordslist)
      validcoords, lengthofword, xvector, yvector = cls.check_coord_validity(x1, y1, x2, y2)
      
      if validcoords:
        # forms the wordsearch board string 
        xpos = x1
        ypos = y1
        for i in range(lengthofword+1):
          formedword.append(cls.wordsearch[ypos][xpos])
          WordSearch.curguesswordpos.append(ypos)
          WordSearch.curguesswordpos.append(xpos)
          xpos += xvector
          ypos += yvector
  
        
        formedword = "".join(formedword)

        WordSearch.check_word(formedword)

        
      # coordinates are invalid 
      else:  
        print("Invalid coordinates inputted.")

    # insufficient coords inputted
    else:
      print("Insufficient coordinates inputted.")
  # .....
  @staticmethod
  def check_word(word):
    pos = WordSearch.curguesswordpos
    wordsearch = WordSearch.wordsearch
    
    if word in WordSearch.words:
      # If word is valid, gets the index and removes from the bank
      index = WordSearch.words.index(word)
      WordSearch.words.pop(index)

      i = 0
      ind = 0


      # makes the correctly guessed word green, letter by letter
      for i in range(int(len(pos)/2)):
        wordsearch[pos[ind]][pos[ind+1]] = Fore.GREEN + wordsearch[pos[ind]][pos[ind+1]] + Style.RESET_ALL 
        ind += 2

    pos.clear()

  @staticmethod
  def convert_coords(coordslist: list):
    newcoordslist = list(map(lambda x: ord(x.upper())-65, coordslist))
    return newcoordslist

  @staticmethod
  def check_coord_validity(x1, y1, x2, y2):
    validcoords = False
    xvector = 0
    yvector = 0
    
    # vertical line
    if x1 == x2:
      validcoords = True
      lengthofword = abs(y2-y1)
      xvector = 0
      yvector = int((y2 - y1)/lengthofword)

    # horizontal line
    elif y1 == y2:
      validcoords = True
      lengthofword = abs(x2-x1)
      yvector = 0
      xvector = int((x2 - x1)/lengthofword)

    # diagonal line
    elif x1-x2 == y1-y2:
      validcoords = True
      # can either be difference the x coords or y coords
      lengthofword = abs(x1-x2)
      xvector = int((x2 - x1)/lengthofword)
      yvector = int((y2 - y1)/lengthofword)

    else:
      validcoords = False
      lengthofword = 0
      xvector = 0
      yvector = 0

    return (validcoords, lengthofword, xvector, yvector)
```

### Revealing Words

Users can choose to reveal all the words on the board, making the letters of the revealed words appear in red.

```python
# .....
if userInput.lower() == "reveal" or userInput == "2":
    wordpos = WordSearch.wordpos
    wordsearch = WordSearch.wordsearch

    ind = 0

    # Gets all the coords of the words and makes the letter become red
    for ipos, cpos in wordpos:
      wordsearch[ipos][cpos] = Fore.RED + wordsearch[ipos][cpos] + Style.RESET_ALL

    loop = False
    clear()
    WordSearch.displayWordsearch(wordsearch)
# .....
```

## Game Loop

The game operates in a loop, allowing users to interact with the board until they choose to reveal the words or exit the game.

```python
# .....
#----INITIATE GAME----

loop = True
while loop:
  print("Enter a word (Input \"Done\" to finish).")
  print("Current words:")
  for i in range(len(WordSearch.words)):
    print(f"{i+1}. {Fore.YELLOW + Style.BRIGHT + WordSearch.words[i] + Style.RESET_ALL}")
  word = input(Fore.RED + Style.BRIGHT + ">>> " + Style.RESET_ALL)
  clear()

  if word.capitalize() == "Done":
    if len(WordSearch.words) <= 3:
      clear()
      print("Not enough words! You need at least 4 words.")
      loop = True
    else:
      loop = False

  else:
    if word.isalpha():
      if word.upper() not in WordSearch.words:
        WordSearch.words.append(word.upper())
      else:
        clear()
        print("You already picked that word!")
        loop = True
    else:
      clear()
      print("You may only input letters!")
      loop = True
        


WordSearch.set_board_size()
WordSearch.fill_board()
WordSearch.add_words_to_board()
WordSearch.randomFill_letters()

loop = True
while loop:
  # displaying the board and asking for user input
  clear()

  WordSearch.displayWordsearch(WordSearch.wordsearch)

  print("WORDS TO FIND:")
  for i in range(len(WordSearch.words)):
    print(f"{i+1}. {WordSearch.words[i]}")

  print() # left blank
  
  for i in range(len(WordSearch.actions)):
      print(f"{i+1}. {WordSearch.actions[i]}")
  userInput = input(Fore.RED + Style.BRIGHT + ">>> " + Style.RESET_ALL)

  if userInput.lower() == "reveal" or userInput == "2":
    wordpos = WordSearch.wordpos
    wordsearch = WordSearch.wordsearch

    ind = 0

    # Gets all the coords of the words and makes the letter become red
    for ipos, cpos in wordpos:
      wordsearch[ipos][cpos] = Fore.RED + wordsearch[ipos][cpos] + Style.RESET_ALL

    loop = False
    clear()
    WordSearch.displayWordsearch(wordsearch)
    
  elif "," in userInput:
    # Converts the coords to a list
    userInputlist = list(userInput)
    i = 0
    while i < len(userInputlist):
      if userInputlist[i] == " " or userInputlist[i] == ",":
        # Removes all the spaces and commas in the coords
        userInputlist.pop(i)
      else:
        i += 1
  
    WordSearch.guess_word_position(userInputlist)
# .....
```

## Usage

To run the game, execute the Python script. Follow the prompts to input words and interact with the word search board.

```bash
python main.py
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
