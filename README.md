# Artificial Intelligence Nanodegree
## Introductory Project: Diagonal Sudoku Solver

# Question 1 (Naked Twins)
Q: How do we use constraint propagation to solve the naked twins problem?  
A: 
*  Filter the boxes that could be potential naked twins, and then we search for the peers of this potential naked twins. Basically **eliminate** all the possibilities that don't fulfil the constraints criteria.  
    
    If the box in question has a twin then both box are stored in a list:
    
```python
naked_twins = [[box1,box2] for box1 in potential_twins \
                   for box2 in peers[box1] \
                   if values[box1]==values[box2]]
```
 * Then from all the commun peers of the two boxes the two values present in them are eliminated from all the peers, because they are not part of the choices for a possible solution.
    
```python
for k,v in enumerate(naked_twins):
        box1 = v[0]
        box2 = v[1]
        # intersection of peers
        peers1 = peers[box1]
        peers2 = peers[box2]
        peers_intersec = [box for box in peers1.intersection(peers2) if len(values[box]) > 2] 
        # delete the digits in naked twins from all common peers.
        for peer in peers_intersec:
            for rm_val in values[box1]:
                values[peer] = values[peer].replace(rm_val,'')
```

On key part is that to make a constraint propagation this process need to be done repeatedly along all the naked twins, that is done in using the **for** statement in the list of naked_twins 

# Question 2 (Diagonal Sudoku)
Q: How do we use constraint propagation to solve the diagonal sudoku problem?  
A:

How do we use constraint propagation to solve the diagonal sudoku problem?

* Filter the boxes that belong to the diagonals, eliminating the box from the universe of boxes

```python
diagonal_units = [[row[k] for k, row in enumerate(row_units)], [row[len(row_units) -1- k] for k, row in enumerate(row_units)]]
```

* With the reduce_puzzle function the has inherent the constraint propagation paradigm, is applied searching for the possible solution eliminating possibilities and assigning only choices

```python
def reduce_puzzle(values):
    values = grid_values(values)
    stalled = False
    while not stalled:
        # Check how many boxes have a determined value
        solved_values_before = len([box for box in values.keys() if len(values[box]) == 1])
        #  Use the Eliminate Strategy
        values = eliminate(values)
        # Use the Only Choice Strategy
        values = only_choice(values)
        # Use the Naked Twins Strategy
        values = naked_twins(values)
        # Check how many boxes have a determined value, to compare
        solved_values_after = len([box for box in values.keys() if len(values[box]) == 1])
        # If no new values were added, stop the loop.
        stalled = solved_values_before == solved_values_after
        # Sanity check, return False if there is a box with zero available values:
        if len([box for box in values.keys() if len(values[box]) == 0]):
            return False

    return values
    pass
```

### Install

This project requires **Python 3**.

We recommend students install [Anaconda](https://www.continuum.io/downloads), a pre-packaged Python distribution that contains all of the necessary libraries and software for this project. 
Please try using the environment we provided in the Anaconda lesson of the Nanodegree.

##### Optional: Pygame

Optionally, you can also install pygame if you want to see your visualization. If you've followed our instructions for setting up our conda environment, you should be all set.

If not, please see how to download pygame [here](http://www.pygame.org/download.shtml).

### Code

* `solution.py` - You'll fill this in as part of your solution.
* `solution_test.py` - Do not modify this. You can test your solution by running `python solution_test.py`.
* `PySudoku.py` - Do not modify this. This is code for visualizing your solution.
* `visualize.py` - Do not modify this. This is code for visualizing your solution.

### Visualizing

To visualize your solution, please only assign values to the values_dict using the `assign_value` function provided in solution.py

### Submission
Before submitting your solution to a reviewer, you are required to submit your project to Udacity's Project Assistant, which will provide some initial feedback.  

The setup is simple.  If you have not installed the client tool already, then you may do so with the command `pip install udacity-pa`.  

To submit your code to the project assistant, run `udacity submit` from within the top-level directory of this project.  You will be prompted for a username and password.  If you login using google or facebook, visit [this link](https://project-assistant.udacity.com/auth_tokens/jwt_login) for alternate login instructions.

This process will create a zipfile in your top-level directory named sudoku-<id>.zip.  This is the file that you should submit to the Udacity reviews system.

