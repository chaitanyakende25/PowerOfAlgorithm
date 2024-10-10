## PowerOfAlgorithm
# Travelling Salesman Problem (TSP): 
Given a set of cities and the distance between every pair of cities, the problem is to find the shortest possible route that visits every city exactly once and returns to the starting point. Note the difference between Hamiltonian Cycle and TSP. The Hamiltonian cycle problem is to find if there exists a tour that visits every city exactly once. Here we know that Hamiltonian Tour exists (because the graph is complete) and in fact, many such tours exist, the problem is to find a minimum weight Hamiltonian Cycle. 

```java
import java.io.*;
import java.util.*;

public class TSE {
    static int n = 4;

    static int MAX = 1000000;

    static int[][] dist = {
        { 0, 10, 15, 20 }, // distances from node 1
        { 5,  0,  9, 10 }, // distances from node 2
        { 6, 13,  0, 12 }, // distances from node 3
        { 8,  8,  9,  0 }  // distances from node 4
    };

    static int[][] memo = new int[n][(1 << n)];

    static int tsp(int pos, boolean[] visited) {
        if (allVisited(visited)) {
            return dist[pos][0]; 
        }

        int mask = createMask(visited);
        if (memo[pos][mask] != 0) {
            return memo[pos][mask];
        }

        int res = MAX; 

        for (int next = 0; next < n; next++) {
            if (!visited[next]) {
                visited[next] = true; 
                res = Math.min(res, dist[pos][next] + tsp(next, visited)); 
                visited[next] = false; 
            }
        }

        return memo[pos][mask] = res; 
    }

    static boolean allVisited(boolean[] visited) {
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                return false;
            }
        }
        return true;
    }

    
    static int createMask(boolean[] visited) {
        int mask = 0;
        for (int i = 0; i < n; i++) {
            if (visited[i]) {
                mask |= (1 << i);
            }
        }
        return mask;
    }

    public static void main(String[] args) {
        for (int[] row : memo) {
            Arrays.fill(row, 0);
        }

        boolean[] visited = new boolean[n]; 
        visited[0] = true; // Start by visiting node 1 (position 0)

        int ans = tsp(0, visited); 

        System.out.println("The cost of the most efficient tour = " + ans);
    }
}

```

This Java code solves the Traveling Salesman Problem (TSP) using a recursive dynamic programming approach with memoization. The problem aims to find the minimum cost of visiting all the cities exactly once and returning to the starting city. Let's break down each part of the code:
# Variables and Constants
* n: The number of nodes (cities) is set to 4.
* MAX: A large value (1000000) representing infinity is used to initialize costs where necessary.
* dist: A 2D array that represents the distance matrix. The value dist[i][j] gives the distance between city i+1 and city j+1. For example, dist[0][1] = 10 means the distance from city 1 to city 2 is 10 units.

# Memoization Array
* memo: A 2D array for memoization, which stores intermediate results to avoid recalculating the minimum cost multiple times. The memoization is done using a combination of the current city and a mask representing the cities that have been visited.

# Recursive Function: tsp() 
The function tsp() calculates the minimum cost of visiting all cities starting from the current city pos while keeping track of visited cities using a boolean array visited.
* 1.Base Case: If all cities are visited, return the cost of returning to the starting city (dist[pos][0]).
```java
      if (allVisited(visited)) {
          return dist[pos][0];
      }
```
* 2.Memoization Check: If the result for the current position pos and visited cities (represented by a mask) is already computed, return the stored result:
```java
      int mask = createMask(visited);
    if (memo[pos][mask] != 0) {
        return memo[pos][mask];
    }
```
* 3.Recursive Exploration: For each unvisited city, mark it as visited, recursively call tsp() to calculate the minimum cost from that city, then backtrack by unmarking it.
```java
for (int next = 0; next < n; next++) {
    if (!visited[next]) {
        visited[next] = true;
        res = Math.min(res, dist[pos][next] + tsp(next, visited));
        visited[next] = false;
    }
}
```
* 4.Return Memoized Result: Store the computed minimum cost in the memo array and return it.
  # Helper Functions
  * 1.allVisited(): Checks if all cities have been visited by iterating through the visited array. If any city is unvisited, it returns false.
```java
static boolean allVisited(boolean[] visited) {
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            return false;
        }
    }
    return true;
}
```
* 2.createMask(): Creates a unique integer (mask) based on the visited array, where each visited city corresponds to a bit set to 1.
```java
  static int createMask(boolean[] visited) {
      int mask = 0;
      for (int i = 0; i < n; i++) {
          if (visited[i]) {
              mask |= (1 << i);
          }
      }
      return mask;
  }
```
# Main Method
In the main() method:
1.Memo Initialization: The memo array is initialized with zeros.
```java
for (int[] row : memo) {
    Arrays.fill(row, 0);
}

```
2.Visited Array Initialization: A boolean array visited is used to keep track of visited cities, with city 1 (index 0) marked as visited.
```java
boolean[] visited = new boolean[n];
visited[0] = true;

```
3.Recursive Call: The tsp() function is called, starting from city 1 (position 0).
```java
int ans = tsp(0, visited);
```
4.Result Output: The minimum cost of the tour is printed.
```java
System.out.println("The cost of the most efficient tour = " + ans);
```

# Key Points
* TSP: Finds the minimum cost to visit all cities and return to the start.
* Dynamic Programming + Recursion: Memoization is used to store already computed costs for certain states to avoid redundant calculations.
* Bit Masking: A unique mask is created to represent the set of visited cities.
  
This code efficiently solves the TSP by exploring all possible paths through recursive backtracking and memoization.

## Time Complexity : O(n² * 2ⁿ)
## Space Complexity: O(n⋅2^n)
