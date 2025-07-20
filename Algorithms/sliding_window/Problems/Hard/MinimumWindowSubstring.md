# Minimum Window Substring
[Leetcode Problem](https://leetcode.com/problems/minimum-window-substring/)

## Problem Description
Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such window in s that covers all characters in t, return the empty string "".
Note: If there is such a window, it is guaranteed that there will always be only one unique minimum window in s.

## Examples

```kotlin
Example 1:
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

Example 2:
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.

Example 3:
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

```

## Constraints
- m == s.length
- n == t.length
- 1 <= m, n <= 10^5
- s and t consist of uppercase and lowercase English letters.

## Key Insights
- Use sliding window technique with two pointers
- Expand window by moving right pointer, contract by moving left pointer
- Track character frequencies using HashMap
- Only update minimum window when current window is valid and smaller
- A window is valid when it contains all characters from t with required frequencies

## Solution Approach
- Create frequency map for target string t
- Use two pointers (windowStart, windowEnd) for sliding window
- Expand window by moving windowEnd and adding characters to window map
- When window becomes valid, try to contract it by moving windowStart
- Track the minimum valid window found during the process
- Return the substring of minimum window or empty string if no valid window exists

## Complexity Analysis
- Time Complexity: O(m + n) where m is length of s and n is length of t
- Space Complexity: O(m + n) for the HashMaps storing character frequencies

## Solution

```kotlin
fun minWindow(s: String, t: String): String {
    if(t.length > s.length) return ""
    
    var minStart = 0
    var minLength = Int.MAX_VALUE
    
    val tMap = HashMap<Char, Int>()
    
    for(c in t) tMap[c] = tMap.getOrDefault(c, 0) + 1
    
    var windowStart = 0
    val map = HashMap<Char, Int>()
    
    for(windowEnd in s.indices) {
        val endChar = s[windowEnd]
        map[endChar] = map.getOrDefault(endChar, 0) + 1
        
        while(allFound(tMap, map)) {
            val currentLength = windowEnd - windowStart + 1
            if(currentLength < minLength) {
                minLength = currentLength
                minStart = windowStart
            }
            
            val startChar = s[windowStart]
            map[startChar] = map.getOrDefault(startChar, 0) - 1
            if(map[startChar] == 0) map.remove(startChar)
            windowStart++
        }
    }
    
    return if(minLength == Int.MAX_VALUE) "" else s.substring(minStart, minStart + minLength)
}

fun allFound(target: HashMap<Char, Int>, window: HashMap<Char, Int>): Boolean {
    for((char, count) in target) {
        if(window.getOrDefault(char, 0) < count) {
            return false
        }
    }
    return true
}

```
