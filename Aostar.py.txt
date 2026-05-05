import heapq


class Node:
    def __init__(self, state, parent=None, cost=0):
        self.state = state
        self.parent = parent
        self.cost = cost


    def __lt__(self, other):
        return self.cost < other.cost


def heuristic(s):
    h = {'A':5,'B':3,'C':2,'D':1,'E':2,'G':0}
    return h.get(s, 999)


def ao_star(start, goal, graph):
    pq = []
    heapq.heappush(pq, Node(start, None, 0))
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
                heapq.heappush(pq, Node(neigh, node,
                                        node.cost + cost + heuristic(neigh)))


    return None


graph = {
    'S':[('A',3),('B',2)],
    'A':[('C',4),('D',2)],
    'B':[('E',3)],
    'C':[('G',2)],
    'D':[('G',5)],
    'E':[('G',4)]
}


print("Path:", ao_star('S','G',graph))