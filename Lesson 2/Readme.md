# Lesson 2
In this lesson, we are going to understand what is a searching algorithm. Before that, let's talk about what is a graph.

## What is a graph?
Imagine instagram. There are some people, and each person can follow another person. This is called a Graph. In a graph, there are a set of entities called Nodes (like the people in instagram). These nodes can be connected to each other. The connections are called Edge (like the followings in instagram). You may follow a well-known acter, but he probably will not follow you. This is called a directed edge; meaning that a node may have an edge to another one, but the opposite direction may be unavailable.

## How to construct a graph?
Now that we know about a graph, we can model many every-day problems into graphs. Assume we are given a maze like this:
![A simple maze](https://github.com/MAN1986/pyamaze/blob/main/Picture1.png)

And we want to go from the bottom right corner to the green square. How are we going to do that? The answer is to search for the path from the start position to the end position. We can make the graph representation of this maze. For each node (square), we connect it to nodes that is next to them and there's no wall between them. This means we can go from this node to that other node by only one move. But now comes the real question: how we can find this path? And probably the optimal one?

## The A star game
There are many algorithms that can do this for us. The most widely-used one is called the $A^\star$ algorithm. For getting familiar with how it works, let's play a game!

[The A star game](https://pathfindingdemo.gamelet.online)

## A star coding
Now that you have a basic understanding of the $A^\star$ algorithm, let's do a simple code example using an already-written library.

### PyMaze
Pymaze is a simple python game that has a built-in maze (as you saw one of its mazes earlier). The coordinates system in this maze workd is like this:
![Coordinate System](https://github.com/MAN1986/pyamaze/blob/main/Picture3.png)

To run it, install the `pyamaze` package as explained in the Homework section. Then run this code:
```python
from pyamaze import maze
m=maze(10,10)
m.CreateMaze()
m.run()
```
It will show a random maze. For further reading please refer to the main documentation:
[PyMaze Library](https://github.com/MAN1986/pyamaze?tab=readme-ov-file)

### Networkx
`networkx` is a python library that gives you the option to create a graph and do some calculations on that graph. And the calculation we talk about is the $A^\star$ function.

1. Create a graph:
```python
import networkx
G = networkx.Graph()
```
2. Add some nodes to your graph:
```python
G.add_node(node)
```
3. Add adges to your graph (an edge is the connection between two nodes):
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
The `heuristic` should be a function that given two nodes, it shold estimate the shortest possible distance they have from each other.

### Womework
1. Install visual studio code, python, and jupiter notebook extention on your vscode if you haven't.
2. Open a new jupiter notebook file, select a python interpreter, and then save your file.
3. Install libraries `pyamaze` and `networkx`using this command in your terminal:
```bash
pip install pyamaze
pip install networkx
```
If may want to install these using your jupiter notebook, add ! mark before each and run the cell.
4. Copy this code into your notebook (try putting each code section in a different code cell, so it is easier to run each seperately).
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
5. Implement these two functions using `networkx` and the basic knowledge you have about what is a heuristic:
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
8. Run the game and see the agent slowly moving to the destination:
```python
a=agent(m,filled=True,footprints=True)
# a.position=(5,4)
m.tracePath(solution_path)
m.run()
```
