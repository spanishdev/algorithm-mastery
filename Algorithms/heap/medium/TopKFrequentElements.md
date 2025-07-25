# Top K Frequent Elements
[Leetcode Problem](https://leetcode.com/problems/top-k-frequent-elements/description/)

## Problem Description
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

## Examples

```kotlin
Example 1:

Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

Example 2:

Input: nums = [1], k = 1
Output: [1]

```

## Constraints
- 1 <= nums.length <= 105
- -104 <= nums[i] <= 104
- k is in the range [1, the number of unique elements in the array].
- It is guaranteed that the answer is unique.

## Key Insights
### Problem Analysis:
Need to find elements with highest frequency (count)
Two-step process: Count frequencies → Find top K
Multiple approaches with different trade-offs

### Core Challenge:
How to efficiently find the K elements with highest counts?

### Heap Strategy Counter-intuitive Insight:
Min-heap of size K is more efficient than max-heap of size N
Why? Keep only K elements in heap vs all N elements

### Approaches Ranking:
Min-heap (size K) - Most efficient for small K
Max-heap (all elements) - More intuitive but less efficient
Sorting - Simple but O(n log n)
QuickSelect - O(n) average, but complex

## Solution Approach
### Approach 1: Min-Heap of Size K (Most Efficient)
Count frequencies with HashMap
Use min-heap to keep track of K most frequent elements
Heap size never exceeds K

### Approach 2: Max-Heap (More Intuitive)
Count frequencies with HashMap
Put all elements in max-heap ordered by frequency
Extract top K elements

### Approach 3: Sorting
Count frequencies
Sort by frequency
Take last K elements

### Approach 4: QuickSelect (Advanced)
Count frequencies
Use quickselect to find Kth largest frequency
Collect all elements with frequency >= Kth largest


## Complexity Analysis
## Min-Heap (size K):

Time: O(n log k) where n = array length
Space: O(n + k) for frequency map + heap

## Max-Heap (all elements):
Time: O(n log n) - putting all elements in heap
Space: O(n) for frequency map + heap

## Sorting:
Time: O(n log n) - sorting by frequency
Space: O(n) for frequency map + sorting

## QuickSelect:
Time: O(n) average, O(n²) worst case
Space: O(n) for frequency map


## Solutions

### Approach 1: Min-Heap of Size K (Most Efficient)
```kotlin
fun topKFrequent(nums: IntArray, k: Int): IntArray {
        // Step 1: Count frequencies
        val frequencyMap = mutableMapOf<Int, Int>()
        for (num in nums) {
            frequencyMap[num] = frequencyMap.getOrDefault(num, 0) + 1
        }
        
        // Step 2: Use min-heap to keep top K frequent elements
        // Counter-intuitive: min-heap keeps LARGEST frequencies by removing smallest
        val minHeap = PriorityQueue<Int> { a, b -> 
            frequencyMap[a]!! - frequencyMap[b]!!  // Compare by frequency
        }
        
        for (num in frequencyMap.keys) {
            minHeap.offer(num)
            if (minHeap.size > k) {
                minHeap.poll()  // Remove element with smallest frequency
            }
        }
        
        // Step 3: Extract result
        return minHeap.toIntArray()
    }

```


### Approach 2: Max-Heap (More Intuitive)

```kotlin
 fun topKFrequentMaxHeap(nums: IntArray, k: Int): IntArray {
        // Step 1: Count frequencies
        val frequencyMap = mutableMapOf<Int, Int>()
        for (num in nums) {
            frequencyMap[num] = frequencyMap.getOrDefault(num, 0) + 1
        }
        
        // Step 2: Use max-heap to get top K elements
        val maxHeap = PriorityQueue<Int> { a, b -> 
            frequencyMap[b]!! - frequencyMap[a]!!  // Compare by frequency (descending)
        }
        
        // Add all elements to heap
        for (num in frequencyMap.keys) {
            maxHeap.offer(num)
        }
        
        // Step 3: Extract top K
        val result = IntArray(k)
        for (i in 0 until k) {
            result[i] = maxHeap.poll()
        }
        
        return result
    }


```
### Approach 3: Sorting (Simple)

```kotlin
  fun topKFrequentSorting(nums: IntArray, k: Int): IntArray {
        // Step 1: Count frequencies
        val frequencyMap = mutableMapOf<Int, Int>()
        for (num in nums) {
            frequencyMap[num] = frequencyMap.getOrDefault(num, 0) + 1
        }
        
        // Step 2: Sort by frequency
        val sortedByFreq = frequencyMap.toList().sortedBy { it.second }
        
        // Step 3: Take last K elements (highest frequency)
        return sortedByFreq.takeLast(k).map { it.first }.toIntArray()
    }
```

### Approach 4: Bucket Sort (Optimal O(n) for specific cases)

```kotlin
  fun topKFrequentBucket(nums: IntArray, k: Int): IntArray {
        // Step 1: Count frequencies
        val frequencyMap = mutableMapOf<Int, Int>()
        for (num in nums) {
            frequencyMap[num] = frequencyMap.getOrDefault(num, 0) + 1
        }
        
        // Step 2: Bucket sort by frequency
        // Index = frequency, Value = list of numbers with that frequency
        val buckets = Array<MutableList<Int>?>(nums.size + 1) { null }
        
        for ((num, freq) in frequencyMap) {
            if (buckets[freq] == null) {
                buckets[freq] = mutableListOf()
            }
            buckets[freq]!!.add(num)
        }
        
        // Step 3: Collect top K from highest frequency buckets
        val result = mutableListOf<Int>()
        for (i in buckets.size - 1 downTo 0) {
            buckets[i]?.let { bucket ->
                for (num in bucket) {
                    result.add(num)
                    if (result.size == k) return result.toIntArray()
                }
            }
        }
        
        return result.toIntArray()
    }
```



### Approach 5: QuickSelect (Advanced O(n) average)

```kotlin
 fun topKFrequentQuickSelect(nums: IntArray, k: Int): IntArray {
        // Step 1: Count frequencies
        val frequencyMap = mutableMapOf<Int, Int>()
        for (num in nums) {
            frequencyMap[num] = frequencyMap.getOrDefault(num, 0) + 1
        }
        
        val uniqueNums = frequencyMap.keys.toIntArray()
        
        // Step 2: Use quickselect to find kth largest frequency
        quickSelect(uniqueNums, 0, uniqueNums.size - 1, k, frequencyMap)
        
        // Step 3: Return top k elements
        return uniqueNums.sliceArray(uniqueNums.size - k until uniqueNums.size)
    }
    
    private fun quickSelect(nums: IntArray, left: Int, right: Int, k: Int, freqMap: Map<Int, Int>) {
        if (left == right) return
        
        val pivotIndex = partition(nums, left, right, freqMap)
        
        when {
            pivotIndex == nums.size - k -> return
            pivotIndex < nums.size - k -> quickSelect(nums, pivotIndex + 1, right, k, freqMap)
            else -> quickSelect(nums, left, pivotIndex - 1, k, freqMap)
        }
    }
    
    private fun partition(nums: IntArray, left: Int, right: Int, freqMap: Map<Int, Int>): Int {
        val pivot = freqMap[nums[right]]!!
        var i = left
        
        for (j in left until right) {
            if (freqMap[nums[j]]!! < pivot) {
                nums[i] = nums[j].also { nums[j] = nums[i] }
                i++
            }
        }
        
        nums[i] = nums[right].also { nums[right] = nums[i] }
        return i
    }

```
