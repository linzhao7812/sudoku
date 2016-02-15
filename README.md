# Sudoku Solver

A simple iOS application to demonstrate a brute force sudoku solver implemented in a recursive fashion.


### Features

- A native application written in Objective-c for iOS 9 using Xcode 7 Beta
- Open source (https://github.com/linzhao7812/Sudoku)
- Easy to be deployed on devices such as iPhone, iPad running iOS 9 or up


### Getting the source code

##### Option 1: Download Source Code

- Go to: https://github.com/linzhao7812/Sudoku

##### Option 2: Git clone source

```bash
$ git clone --recursive https://github.com/linzhao7812/Sudoku.git
```


### What's there
A Sudoku game is pre-filled in a 9x9 grid. Click "Solve It!" button will solve the game and fill up the open space. A "Reset" button on the top right corner to reset the 9x9 grid with the current Sudoku game. The app also provides three new games to demonstrate how it handles special cases such as an empty grid which will actually fill up the board with a valid solution, or an invalid Sudoku game which will then show an error message. 



### Documentation
SimpleSudoku app starts with a set of pre-defined 9X9 Sudoku games to demonstrate how a brute force searching algorithm was implemented to solve the game or handle the error cases. The key searching algorithm is implemented in a recursive fashion. Each recursive call takes care of the first open space still to be handled. That will be replaced tentatively with a number (from 1 to 9). If the number is valid based on Sudoku rules, then go recurse which will try the next open space. If it fails, undo the number replacement, and return false since any number tried on this search branch is invalid. And this forces the caller (up to the root) to try the next number. 

##### Key class:
- SudokuSolver.m: This contains the implementation of the search algorithm. Below is key function:
```Objective-C
- (BOOL)solve : (TwoDimentionalArray *)array {
    for (int row = 0; row < ROW_COLUMN_CELL_NUM; row ++) {
        for (int col = 0; col < ROW_COLUMN_CELL_NUM; col ++) {
            NSString *value = [array getValueFromArray:row cols:col];
            if ([value caseInsensitiveCompare:@"."] == NSOrderedSame) {
                for (int k = 1; k <= ROW_COLUMN_CELL_NUM; k ++) {
                    // Tentatively assign the number to the open space
                    [array setValuesToArray:row cols:col value:[NSString stringWithFormat:@"%i", k]];
                    // Validate the board and solve the next open space recursively
                    if ([self isValid:array rows:row cols:col] && [self solve:array]) {
                        return true;
                    } else {
                        // Undo the previous assignment
                        [array setValuesToArray:row cols:col value:@"."];
                    }
                }
                return false;
            }
        }
    }
    
    return true;
}
```

##### Helper classes:
- TwoDimentionalArray.m: A definition of a 2-D array implemented to describe the 9X9 sudoku board. It helps to set and get values to and from a specific cell.
- Constants.m: A helper class to define all the constants shared among classes. 

##### UI classes:
- ViewController.m: A custom UIViewController class to render the application data.
- Main.storyboard: UI layout of the app. All the buttons, labels are created and constrains are set to support different screen resolutions.

