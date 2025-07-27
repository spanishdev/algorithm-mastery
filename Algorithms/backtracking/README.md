# Used for:
* Find the end result and their path
* Permutations
* Combinations
* DFS based algorithm

# Template for all backtracking problems

```kotlin
class BacktrackingTemplate {
    fun backtrackTemplate(): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        
        fun backtrack(/* state parameters */) {
            // Base case: solution complete?
            if (/* solution complete */) {
                result.add(/* copy of current solution */)
                return
            }
            
            // Try all possible choices
            for (/* choice in available choices */) {
                // 1. Choose
                // path.add(choice)
                // used[choice] = true (if needed)
                
                // 2. Explore
                backtrack(/* updated state */)
                
                // 3. Unchoose (backtrack)
                // path.removeLast()
                // used[choice] = false (if needed)
            }
        }
        
        backtrack(/* initial state */)
        return result
    }
}
```
