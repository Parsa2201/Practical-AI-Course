# Lesson 2
In this lesson, we will learn what a searching algorithm is. But before that, let’s first understand what a graph is.

## What is a graph?
Imagine Instagram. There are many users, and each user can follow other users. This is an example of a graph. In a graph, there are entities called nodes (like Instagram users), which can be connected to each other. These connections are called edges (like the “follow” relationships on Instagram). 

For example, you may follow a popular actor, but they may not follow you back. This is called a directed edge, meaning a connection exists in one direction but not necessarily in the opposite direction.

## How to construct a graph?
Now that we understand graphs, we can model many real-world problems using them. Suppose we are given a maze like this:
![A simple maze](https://github.com/MAN1986/pyamaze/blob/main/Picture1.png)

Our goal is to go from the bottom-right corner to the green square. How can we do that? The answer is searching for a path from the start position to the goal.

We can represent this maze as a graph. Each cell is a node, and we connect it to adjacent cells that are not blocked by walls. This means we can move from one node to another in a single step.

But the real question is: how do we find this path, and preferably the optimal one?

## The A star game
There are many algorithms to solve this problem, and the most widely used is the $A^\star$ algorithm. To get familiar with how it works, try this interactive game:

[The A star game](https://pathfindingdemo.gamelet.online)

## A star coding
Now that you have a basic understanding of the $A^\star$ algorithm, let’s look at a simple coding example using existing libraries.

### PyMaze
`pymaze` is a Python library with built-in mazes (like the one shown above). The coordinate system in this maze works like this:
![Coordinate System](https://github.com/MAN1986/pyamaze/blob/main/Picture3.png)

To run it, install the `pyamaze` package as explained in the Homework section, then run this code:
```python
from pyamaze import maze
m=maze(10,10)
m.CreateMaze()
m.run()
```
This will show a random maze. For more details please refer to the official documentation:
[PyMaze Library](https://github.com/MAN1986/pyamaze?tab=readme-ov-file)

### NetworkX
`networkx` is a Python library for creating graphs and performing calculations on them. Here, we will use it to implement the $A^\star$ algorithm.

1. Create a graph:
```python
import networkx
G = networkx.Graph()
```
2. Add nodes to the graph:
```python
G.add_node(node)
```
3. Add edges between nodes (edges represent connections between nodes):
```python
G.add_edge(nodeA, nodeB)
```
4. Call the $A^\star$ function on that graph and your path is ready (`source_node` is the place you are starting and `destination_node` is were you  want to reach)!
```python
path_coords = nx.astar_path(
        G=G,
        source=source_node,
        target=destination_node,
        heuristic=heuristic
    )
```
The `heuristic` is a function that estimates the shortest possible distance between two nodes.

## Womework
1. Install Visual Studio Code, Python, and Jupiter Notebook extention if you haven't already.
2. Open a new Jupiter Notebook file, select a python interpreter, and save your file.
3. Install the required libraries using this terminal command:
```bash
pip install pyamaze
pip install networkx
```
If using Jupyter Notebook, add ! before each command:
```python
!pip install pyamaze
!pip install networkx
```
4. Copy the following code into your notebook (split each section into separate cells for easier execution):
```python
from pyamaze import maze, agent
import networkx

class MazeGraph:
    """
    This class provides a simplified toolkit for solving a pyamaze maze.
    It helps the student get the information needed to build a graph
    without needing to understand the complex details of the pyamaze object.
    """
    def __init__(self, maze_object: maze):
        """Initializes the solver with the maze to be solved."""
        
        # --- Attributes for the student to use ---
        self.start_node = (maze_object.rows, maze_object.cols)
        self.goal_node = (1, 1)
        
        # --- Internal attributes (hidden from the student's view) ---
        self._maze = maze_object
        self._agent = agent(self._maze, footprints=True, filled=True, color='cyan')

    def get_all_cells(self) -> list[tuple[int, int]]:
        """
        Returns a list of all cell coordinates in the maze.
        Use this to loop through every cell to build your graph.
        Example: for cell in solver.get_all_cells(): ...
        """
        return self._maze.maze_map.keys()

    def get_neighbors(self, cell) -> list[tuple[int, int]]:
        """
        For any given cell, this returns a list of its open neighbors.
        Use this to find which edges to add to your graph.
        Example: neighbors = solver.get_neighbors(cell)
        """
        # This logic is hidden from the student.
        neighbors = []
        for direction, is_open in self._maze.maze_map[cell].items():
            if is_open:
                if direction == 'E': neighbors.append((cell[0], cell[1] + 1))
                elif direction == 'W': neighbors.append((cell[0], cell[1] - 1))
                elif direction == 'S': neighbors.append((cell[0] + 1, cell[1]))
                elif direction == 'N': neighbors.append((cell[0] - 1, cell[1]))
        return neighbors

    def format_path(self, coordinate_path):
        """
        A helper to convert the list of coordinates from A* into the
        string format that pyamaze needs to draw the path.
        """
        path_string = ''
        for i in range(len(coordinate_path) - 1):
            curr, nxt = coordinate_path[i], coordinate_path[i+1]
            if nxt[0] < curr[0]: path_string += 'N'
            elif nxt[0] > curr[0]: path_string += 'S'
            elif nxt[1] > curr[1]: path_string += 'E'
            elif nxt[1] < curr[1]: path_string += 'W'
        
        return {self._agent: path_string}
```
5. Implement these two functions using `networkx` and your understanding of heuristics:
```python
def heuristic(cell1, cell2):
    # Your code

def solve_maze(solver: MazeGraph):
    # Your code
```
6. Create a 20x20 maze:
```python
m = maze(20, 20)
m.CreateMaze()
```
7. Solve it with your solver:
```python
solver_toolkit = MazeGraph(m)
solution_path = solve_maze(solver_toolkit)
```
8. Run the game and see the agent slowly move to the destination:
```python
a=agent(m,filled=True,footprints=True)
m.tracePath(solution_path)
m.run()
```
