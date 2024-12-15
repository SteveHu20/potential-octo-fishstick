---
title: "351 Android Unlocking"
date: 2024-12-15
---

## Problem
```
Android devices have a special lock screen with a 3 x 3 grid of dots. Users can set an "unlock pattern" by connecting the dots in a specific sequence, forming a series of joined line segments where each segment's endpoints are two consecutive dots in the sequence. A sequence of k dots is a valid unlock pattern if both of the following are true:

All the dots in the sequence are distinct.
If the line segment connecting two consecutive dots in the sequence passes through the center of any other dot, the other dot must have previously appeared in the sequence. No jumps through the center non-selected dots are allowed.
For example, connecting dots 2 and 9 without dots 5 or 6 appearing beforehand is valid because the line from dot 2 to dot 9 does not pass through the center of either dot 5 or 6.
However, connecting dots 1 and 3 without dot 2 appearing beforehand is invalid because the line from dot 1 to dot 3 passes through the center of dot 2.
```

### Thought

Given solution space is not huge, we can enumerate all the possible solutions and count.

1. Use DFS to count all the possible solutions
2. Use visited to mark and backtrack to avoid revisit.
3. The most tricky part of this problem is when it reach a point, it can still move forward sometimes.


### Core Logic:
```
1. Architecture: try each every starting position
2. DFS Logic:
   - Basic condition: if it's more steps taken, just return. If the step is not enough, add more steps before counting.
   - Iterate all possible steps:
      - Mark it as visited
      - Move to next position
      - Backtrack

```

### Solution
```
class Solution(object):
    def numberOfPatterns(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        self.count = 0
        visited = [[0] * 3 for _ in range(3)]
        dirs = [[-1, 0], [1, 0], [0, -1], [0, 1],[-1, -1], [1, -1], [-1, 1], [1, 1], [1, 2], [-1, -2], [-1, 2], [1, -2], [2, 1], [-2, -1], [-2, 1], [2, -1]]
        def dfs(i, j, step):
            if step >= m and step <= n:
                self.count += 1
            if step > n:
                return

            for dx, dy in dirs:
                x, y = i + dx, j + dy
                if x < 0 or x >= 3 or y < 0 or y >= 3:
                    continue
                if visited[x][y] == 0:
                    visited[x][y] = 1
                    dfs(x, y, step + 1)
                    visited[x][y] = 0
                else:
                    x += dx
                    y += dy
                    if x < 0 or x >= 3 or y < 0 or y >= 3:
                        continue
                    if visited[x][y] == 0:
                        visited[x][y] = 1
                        dfs(x, y, step + 1)
                        visited[x][y] = 0
        for i in range(3):
            for j in range(3):
                visited[i][j] = 1
                dfs(i, j, 1)
                visited[i][j] = 0

        return self.count
  ```
