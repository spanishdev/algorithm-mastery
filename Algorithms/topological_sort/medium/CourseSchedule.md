# Course Schedule
[Leetcode Problem](https://leetcode.com/problems/course-schedule/description/)

## Problem Description
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return true if you can finish all courses. Otherwise, return false.

## Examples

```kotlin
Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.

Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

```

## Constraints
- 1 <= numCourses <= 2000
- 0 <= prerequisites.length <= 5000
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses

## Key Insights
- This is a cycle detection problem in a directed graph
- If there's a cycle in the dependency graph, it's impossible to complete all courses
- We can model this as a topological sorting problem using Kahn's Algorithm
- Indegree represents the number of prerequisites for each course
- Courses with indegree = 0 can be taken immediately (no dependencies)

## Solution Approach

### Kahn Algorithm
- Build the dependency graph and calculate indegrees for each course
- Find all courses with no prerequisites (indegree = 0) and add them to a queue
- Process courses one by one:
  - Remove a course from the queue (simulate taking it)
  - For each course that depended on it, reduce its indegree by 1
  - If any course's indegree becomes 0, add it to the queue
- Check if we processed all courses:
  - If yes → no cycles exist → return true
  - If no → cycles exist → return false
  - 
###  DFS with Cycle Detection
DFS approach uses graph coloring to detect back edges (cycles):
- Build the dependency graph from prerequisites
- Use three-color DFS traversal:
  - PENDING (white): Unvisited node
  - PROCESSING (gray): Currently being visited (in recursion stack)
  - FINISHED (black): Completely processed
- For each unvisited course, **start DFS**
  - Mark current course as PROCESSING
  - Recursively visit all dependent courses
  - If we encounter a PROCESSING node → cycle detected → return false
  - Mark current course as FINISHED when done
- If no cycles found in any DFS traversal → return true
 
## Complexity Analysis

### Kahn's Algorithm
- Time Complexity: O(V + E) where V = numCourses and E = prerequisites.length
  - Building the graph: O(E)
  - Processing each course: O(V)
  - Processing each edge: O(E)

- Space Complexity: O(V + E)
  - Graph storage: O(E)
  - Indegree array: O(V)
  - Queue: O(V) in worst case

### DFS Approach:
- Time Complexity: O(V + E) where V = numCourses and E = prerequisites.length
  - Building the graph: O(E)
  - DFS traversal visits each node and edge exactly once: O(V + E)
- Space Complexity: O(V + E)
  - Graph storage: O(E)
  - State array: O(V)
  - Recursion stack: O(V) in worst case (for deep graphs)

## Solution

### Kahn's Algorithm

```kotlin
fun canFinish(numCourses: Int, prerequisites: Array<IntArray>): Boolean {
    // Step 1 & 2: Build graph and calculate indegrees
    val indegree = IntArray(numCourses)
    val graph = Array(numCourses) { mutableListOf<Int>() }
    
    for(prereq in prerequisites) {
        val course = prereq[0]      // Course that has prerequisite
        val dependency = prereq[1]  // Prerequisite course
        graph[dependency].add(course)  // dependency -> course
        indegree[course]++             // Count dependencies
    }
   
    // Step 3: Create queue with courses that have no prerequisites
    val queue = ArrayDeque<Int>()
    indegree.forEachIndexed { course, degree ->
        if(degree == 0) queue.add(course)
    }
    
    // Step 4: Process courses using Kahn's Algorithm
    var coursesDone = 0
    while(queue.isNotEmpty()) {
        coursesDone++
        val current = queue.removeFirst()  // "Take" this course
        
        // Unlock all courses that depended on current course
        for(dependent in graph[current]) {
            indegree[dependent]--  // One less prerequisite
            
            if(indegree[dependent] == 0) {
                queue.add(dependent)  // Course is now ready
            }
        }
    }
    
    // Step 5: Check if we could take all courses
    return coursesDone == numCourses
}
```
### Approach 2: DFS with Cycle Detection

```kotlin
enum class Status { FINISHED, PROCESSING, PENDING }

fun canFinish(numCourses: Int, prerequisites: Array<IntArray>): Boolean {
    val graph = Array(numCourses) { mutableListOf<Int>() }
    val state = Array(numCourses) { Status.PENDING }
    
    // Build the dependency graph
    for(prereq in prerequisites) {
        val course = prereq[0]
        val dependency = prereq[1]
        graph[dependency].add(course)
    }
    
    // Check for cycles starting from each unvisited course
    for(i in 0 until numCourses) {
        if(state[i] == Status.PENDING && !canFinishDFS(i, state, graph)) {
            return false
        }
    }
    return true
}

//If detects a cycle, returns false
fun canFinishDFS(current: Int, state: Array<Status>, graph: Array<MutableList<Int>>): Boolean {
    state[current] = Status.PROCESSING  // Mark as currently being processed
    
    for(dependent in graph[current]) {
        if(state[dependent] == Status.FINISHED) continue  // Already processed
        if(state[dependent] == Status.PROCESSING ||       // Cycle detected!
           !canFinishDFS(dependent, state, graph)) {
            return false
        }
    }
    
    state[current] = Status.FINISHED  // Mark as completely processed
    return true
}
```
