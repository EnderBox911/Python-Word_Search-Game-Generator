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
- **replit:** Facilitates clearing the console for a cleaner display.
- **colorama:** Adds color to console output.

```bash
pip install colorama
```

## Game Initialization

### Word Input

Users can input words for the word search game. The input process continues until the user enters "Done" to finish. The program validates input, ensuring that only letters are accepted, and avoids duplicates.

## Word Search Board

### Board Size

The size of the word search board is dynamically set based on the length of the longest word inputted by the user. The size is adjusted to accommodate the words comfortably.

### Random Filling

Empty spaces on the board are filled with random letters to create a more challenging word search experience.

### Display

The game provides a visual representation of the word search board, displaying letters in different colors to enhance visibility.

## Word Placement

### Adding Words

Words inputted by the user are placed on the word search board in various directions, including horizontal, vertical, and diagonal.

### Checking Overlap

The program checks for overlap when placing words to ensure that words do not intersect. If overlap is detected, the program attempts to choose a new direction for word placement.

## User Interaction

### Guessing Word Positions

Users can guess the positions of words by providing coordinates (e.g., X1, Y1, X2, Y2). The program validates the coordinates and reveals the word on the board if the guess is correct.

### Revealing Words

Users can choose to reveal all the words on the board, making the letters of the revealed words appear in red.

## Game Loop

The game operates in a loop, allowing users to interact with the board until they choose to reveal the words or exit the game.

## Usage

To run the game, execute the Python script. Follow the prompts to input words and interact with the word search board.

```bash
python word_search_game.py
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
