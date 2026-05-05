import heapq


class Node:
    def __init__(self, state, parent=None, action=None, cost=0, heuristic=0):
        self.state = state
        self.parent = parent
        self.action = action
        self.cost = cost
        self.heuristic = heuristic


    def __lt__(self, other):
        return (self.cost + self.heuristic) < (other.cost + other.heuristic)


def parse_graph_input():
    graph = {}
    num_edges = int(input("Enter number of edges: "))
    for _ in range(num_edges):
        u, v, cost = input().split()
        cost = int(cost)
        graph.setdefault(u, []).append((v, cost))
        graph.setdefault(v, []).append((u, cost))
    return graph


def astar(start, goal, graph):
    def heuristic(x):
        return abs(ord(x) - ord(goal))


    pq = []
    heapq.heappush(pq, Node(start, None, None, 0, heuristic(start)))
    visited = set()


    while pq:
        node = heapq.heappop(pq)


        if node.state == goal:
            path = []
            while node.parent:
                path.append(node.state)
                node = node.parent
            return path[::-1]


        visited.add(node.state)


        for neigh, cost in graph[node.state]:
            if neigh not in visited:
                heapq.heappush(pq, Node(neigh, node, None,
                                        node.cost + cost, heuristic(neigh)))


    return None


graph = parse_graph_input()
start = input("Start: ")
goal = input("Goal: ")


print("Path:", astar(start, goal, graph))