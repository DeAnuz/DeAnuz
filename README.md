class Graph:
    def __init__(self, size):
        self.adj_matrix = [[0] * size for _ in range(size)]
        self.size = size
        self.vertex_data = [''] * size

    def add_edge(self, u, v, weight):
        if 0 <= u < self.size and 0 <= v < self.size:
            self.adj_matrix[u][v] = weight
            #self.adj_matrix[v][u] = weight  # For directed graph

    def add_vertex_data(self, vertex, data):
        if 0 <= vertex < self.size:
            self.vertex_data[vertex] = data

    def bellman_ford(self, start_vertex_data):
        start_vertex = self.vertex_data.index(start_vertex_data)
        distances = [float('inf')] * self.size
        distances[start_vertex] = 0

        for i in range(self.size - 1):
            for u in range(self.size):
                for v in range(self.size):
                    if self.adj_matrix[u][v] != 0:
                        if distances[u] + self.adj_matrix[u][v] < distances[v]:
                            distances[v] = distances[u] + self.adj_matrix[u][v]
                            print(f"Relaxing edge {self.vertex_data[u]}->{self.vertex_data[v]}, Updated distance to {self.vertex_data[v]}: {distances[v]}")

        return distances

g = Graph(6)

g.add_vertex_data(0, 'A')
g.add_vertex_data(1, 'B')
g.add_vertex_data(2, 'C')
g.add_vertex_data(3, 'D')
g.add_vertex_data(4, 'E')
g.add_vertex_data(5, 'F')
g.add_edge(0, 1, 6)  # A -> B, weight 6
g.add_edge(0, 2, 4)  # A -> C, weight 4
g.add_edge(0, 3, 5)  # A -> D, weight 5
g.add_edge(3, 2, -2)  # D -> C, weight -2
g.add_edge(2, 1, -2) # C -> B, weight -2
g.add_edge(1, 4, -1)  # B -> E, weight -1
g.add_edge(2, 4, 3)  # C -> E, weight 3
g.add_edge(3, 5, -1) # D -> F, weight -1
g.add_edge(4, 5, 3)  # E -> F, weight 3

# Running the Bellman-Ford algorithm from A to all vertices
print("\nThe Bellman-Ford Algorithm starting from vertex A:")
distances = g.bellman_ford('A')
for i, a in enumerate(distances):
    print(f"Distance of vertice {g.vertex_data[i]}: {a}")
