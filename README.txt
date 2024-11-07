SUDOKU SOLVING AS A CONSTRAINT SATISFACTION PROBLEM

My solution models Sudoku as a Constraint Satisfaction Problem and uses the combination of constraint propagation and depth-first-search to solve. A CSP is defined by the following:

1. A set of variables
The variables are the 81 cells of the 9x9 sudoku.

2. A set of domains
For a blank sudoku grid, each cell's domain will be any digit from 1 to n, where n is the size of the board.
In the case of this assignment, n=9, and the domain of each variable is {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}.

3. A set of constraints
Each element of the domain (digit) must appear once and only once in each of the 9 rows.
Each element of the domain (digit) must appear once and only once in each 9 columns.
Each element of the domain (digit) must appear once and only once in each 9 sub-boxes.

DATA STRUCTURES

I chose to use a multi-dimensional array to store the current assignment of variables and domains (i.e. the state of the grid). The assignment array has the following index: assignment[row][column][domain]. The combination of the row and column index identifies a variable, and the domain index identifies the domain of that variable. This seemed the most intuitive data structure to use, although care had to be taken due to the mutable nature of arrays.

CONSTRAINT PROPAGATION

I used a simple form of constraint propagation commonly known as "forward checking". When assigning a value to a variable, forward checking involves updating the domain of all other variables in the assigned variable's row, column and box (sometimes known as the variable's 'peers) by removing the assigned variable from their domain to ensure that constraints 1, 2 & 3 are fulfilled. This elimination may trigger further values to be assigned to variables and therefore cause a chain reaction.

DEPTH FIRST SEARCH

I found that the constraint propagation part of my algorithm solved some sudokus by itself. However, the harder sudokus reached a point where a search was needed in order to proceed. My depth_first_search() function selects a variable and attempts to assign it a value from it's domain. The algorithm uses assign() to see if the consequent assignment and eliminations solve the puzzle. If the value cannot be successfully assigned, a different value from the variable's domain is picked and the process starts again.

Variable ordering

My selectVariable() function implemented the Minimum Remaining Value (MRV) heuristic when choosing a variable to begin a depth first search with. This heuristic chooses variables with the least possible values i.e. the smallest domains. If two variables have the same smallest domain, my function simply picked the one furthest down the grid. Performance could have been improved by choosing the variable involved in the most constraints, which would cause failures and 'dead-ends' more quickly.

Value ordering

My algorithm simply orders a variable's domain by size of digit. When depth_first_search() tries to assign a value to a chosen variable, it therefore chooses the smallest value in the variable's domain. The domain could instead be ordered by a Least Constraint Value heuristic, where the first value to be assigned to the chosen variable would be that which appears the least in the domains of the chosen variable's row/column/box. This provides the search more flexibility for the remaining unassigned variables.

In conclusion, my algorithm performed well on the tests, with no sudoku exceeding a time of 0.6 seconds to solve and therefore my choices felt justified. However, further heuristics could always be added to improve performance.