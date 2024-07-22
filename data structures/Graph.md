A graph is a collection of nodes (vertices) and edges connecting pairs of nodes, used to represent relationships or networks. Two common ways of writing a graph class are with adjacency list and a adjacency matrix.

## Implementation

```python3
class Graph:
    def __init__(self):
        # Initialize the graph with an empty dictionary.
        self.adjacency_list = {}

    def add_vertex(self, vertex):
        # Add a vertex to the adjacency list.
        if vertex not in self.adjacency_list:
            self.adjacency_list[vertex] = []

    def add_edge(self, vertex1, vertex2):
        # Add an edge between vertex1 and vertex2.
        # For an undirected graph, add the edge in both directions.
        if vertex1 not in self.adjacency_list:
            self.add_vertex(vertex1)
        if vertex2 not in self.adjacency_list:
            self.add_vertex(vertex2)
        
        self.adjacency_list[vertex1].append(vertex2)
        self.adjacency_list[vertex2].append(vertex1)

    def remove_vertex(self, vertex):
        # Remove a vertex and all its edges.
        if vertex in self.adjacency_list:
            # Remove the vertex from all other vertices' adjacency lists.
            for adjacent_vertex in self.adjacency_list[vertex]:
                self.adjacency_list[adjacent_vertex].remove(vertex)
            # Remove the vertex from the adjacency list.
            del self.adjacency_list[vertex]

    def remove_edge(self, vertex1, vertex2):
        # Remove an edge between vertex1 and vertex2.
        if vertex1 in self.adjacency_list and vertex2 in self.adjacency_list[vertex1]:
            self.adjacency_list[vertex1].remove(vertex2)
        if vertex2 in self.adjacency_list and vertex1 in self.adjacency_list[vertex2]:
            self.adjacency_list[vertex2].remove(vertex1)

    def display(self):
        # Display the adjacency list representation of the graph.
        for vertex, edges in self.adjacency_list.items():
            print(f"{vertex}: {edges}")
```

```python3
class Graph:
    def __init__(self, num_vertices):
        # Initialize the graph with a specified number of vertices.
        self.num_vertices = num_vertices
        self.adjacency_matrix = [[0] * num_vertices for _ in range(num_vertices)]

    def add_edge(self, vertex1, vertex2):
        # Add an edge between vertex1 and vertex2.
        if vertex1 < self.num_vertices and vertex2 < self.num_vertices:
            self.adjacency_matrix[vertex1][vertex2] = 1
            self.adjacency_matrix[vertex2][vertex1] = 1

    def remove_edge(self, vertex1, vertex2):
        # Remove an edge between vertex1 and vertex2.
        if vertex1 < self.num_vertices and vertex2 < self.num_vertices:
            self.adjacency_matrix[vertex1][vertex2] = 0
            self.adjacency_matrix[vertex2][vertex1] = 0

    def display(self):
        # Display the adjacency matrix representation of the graph.
        for row in self.adjacency_matrix:
            print(' '.join(map(str, row)))
```

## Algorithms

### Depth-First Search (DFS)

Start at root node and explore each branch fully before moving into the next branch. Preferred if we want to visit every node in a graph.

```python3

class Graph:
    def __init__(self):
        self.graph = {}  # Dictionary to store adjacency list

    def add_edge(self, u, v):
        if u not in self.graph:
            self.graph[u] = []
        if v not in self.graph:
            self.graph[v] = []
        self.graph[u].append(v)
        self.graph[v].append(u)  # For undirected graph

    def dfs(self, start, visited=None):
        if visited is None:
            visited = set()
        
        visited.add(start)
        print(start, end=' ')

        for neighbor in self.graph.get(start, []):
            if neighbor not in visited:
                self.dfs(neighbor, visited)
```

### Breadth-First Search (BFS)

Start at root node and explore each branch fully before moving into the next branch. Preferred if we want to find the shortest path between two nodes.

```python3
class Graph:
    def __init__(self):
        self.graph = {}  # Dictionary to store adjacency list

    def add_edge(self, u, v):
        if u not in self.graph:
            self.graph[u] = []
        if v not in self.graph:
            self.graph[v] = []
        self.graph[u].append(v)
        self.graph[v].append(u)  # For undirected graph

    def bfs(self, start):
        visited = set()
        queue = [start]
        visited.add(start)

        while queue:
            node = queue.pop(0)
            print(node, end=' ')

            for neighbor in self.graph.get(node, []):
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
```

### Bidirectional Search

Used to find the shortest path between two nodes. Essentially runs two BFS searches, one from each node until they collide in order to find a path.

```python3
class Graph:
    def __init__(self):
        self.graph = {}  # Dictionary to store adjacency list

    def add_edge(self, u, v):
        if u not in self.graph:
            self.graph[u] = []
        if v not in self.graph:
            self.graph[v] = []
        self.graph[u].append(v)
        self.graph[v].append(u)  # For undirected graph

    def bidirectional_search(self, start, goal):
        if start == goal:
            return [start]

        # Initialize the frontiers
        front_start = {start}
        front_goal = {goal}
        
        # Initialize the explored sets
        explored_start = set()
        explored_goal = set()
        
        # Initialize parent mapping to reconstruct the path
        parent_start = {start: None}
        parent_goal = {goal: None}
        
        while front_start and front_goal:
            # Explore the start frontier
            current_start = front_start.pop()
            explored_start.add(current_start)
            if current_start in front_goal:
                return self._reconstruct_path(parent_start, parent_goal, current_start, 'start')

            for neighbor in self.graph.get(current_start, []):
                if neighbor not in explored_start:
                    front_start.add(neighbor)
                    parent_start[neighbor] = current_start
                    explored_start.add(neighbor)

            # Explore the goal frontier
            current_goal = front_goal.pop()
            explored_goal.add(current_goal)
            if current_goal in explored_start:
                return self._reconstruct_path(parent_start, parent_goal, current_goal, 'goal')

            for neighbor in self.graph.get(current_goal, []):
                if neighbor not in explored_goal:
                    front_goal.add(neighbor)
                    parent_goal[neighbor] = current_goal
                    explored_goal.add(neighbor)
        
        return None  # No path found

    def _reconstruct_path(self, parent_start, parent_goal, meeting_node, meeting_point):
        path_start = []
        path_goal = []

        # Reconstruct path from start to meeting node
        node = meeting_node
        while node is not None:
            path_start.append(node)
            node = parent_start[node]
        path_start.reverse()

        # Reconstruct path from meeting node to goal
        node = meeting_node
        while node is not None:
            path_goal.append(node)
            node = parent_goal[node]

        return path_start + path_goal[1:]  # Remove the meeting node from the second part
```