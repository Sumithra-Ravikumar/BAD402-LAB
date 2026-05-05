from queue import PriorityQueue


class State:
    def __init__(self, lm, lc, boat, rm, rc):
        self.lm = lm
        self.lc = lc
        self.boat = boat
        self.rm = rm
        self.rc = rc


    def is_valid(self):
        if self.lm < 0 or self.lc < 0 or self.rm < 0 or self.rc < 0:
            return False
        if self.lm > 0 and self.lc > self.lm:
            return False
        if self.rm > 0 and self.rc > self.rm:
            return False
        return True


    def __eq__(self, o):
        return (self.lm, self.lc, self.boat, self.rm, self.rc) == \
               (o.lm, o.lc, o.boat, o.rm, o.rc)


    def __hash__(self):
        return hash((self.lm, self.lc, self.boat, self.rm, self.rc))


def successors(s):
    res = []
    for m in range(3):
        for c in range(3):
            if 1 <= m + c <= 2:
                if s.boat == 1:
                    ns = State(s.lm-m, s.lc-c, 0, s.rm+m, s.rc+c)
                else:
                    ns = State(s.lm+m, s.lc+c, 1, s.rm-m, s.rc-c)


                if ns.is_valid():
                    res.append(ns)
    return res


def solve():
    start = State(3,3,1,0,0)
    pq = PriorityQueue()
    pq.put((0,start))
    visited = set()


    while not pq.empty():
        _, s = pq.get()
        if s.lm == 0 and s.lc == 0:
            print("Goal reached")
            return


        visited.add(s)


        for nxt in successors(s):
            if nxt not in visited:
                pq.put((1,nxt))


solve()