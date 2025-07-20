# Kotlin Arrays - Complete Reference

## Array Declaration & Initialization

### Creating Arrays

| **Method** | **Example** | **Use Case** |
|:-----------|:------------|:-------------|
| **Fixed size** | `val arr = IntArray(5)` | Known size, default values |
| **With values** | `val arr = intArrayOf(1, 2, 3, 4, 5)` | Known elements |
| **Lambda init** | `val arr = IntArray(5) { it * 2 }` | Calculated values |
| **Generic** | `val arr = arrayOf(1, 2, 3)` | Mixed types allowed |

### Common Array Types

| **Type** | **Declaration** | **Default Value** |
|:---------|:----------------|:------------------|
| **IntArray** | `IntArray(size)` | `0` |
| **BooleanArray** | `BooleanArray(size)` | `false` |
| **CharArray** | `CharArray(size)` | `'\u0000'` |
| **DoubleArray** | `DoubleArray(size)` | `0.0` |

## Essential Properties & Methods

### Properties
```kotlin
val arr = intArrayOf(1, 2, 3, 4, 5)

arr.size           // 5 (length of array)
arr.indices        // 0..4 (valid index range)
arr.lastIndex      // 4 (last valid index)
```

### Access & modification

```kotlin
// Reading
val first = arr[0]        // Get first element (exception if emtpy)
val firstOrNull           // Get first element or null
val last = arr.last()     // Get last element (exception if empty)
val last = arr.lastOrNull()     // Get last element or null

// Writing  
arr[0] = 10              // Set element
arr.fill(0)              // Fill all with value
```

## Array Loops & Iteration

### Functional Style Loops

| **Method** | **Example** | **Use Case** |
|:-----------|:------------|:-------------|
| **forEach** | `array.forEach { item -> doSth() }` | Process each element |
| **forEachIndexed** | `array.forEachIndexed { index, item -> doSth() }` | Need both index and value |

### Traditional For Loops

| **Syntax** | **Range** | **Best For** |
|:-----------|:----------|:-------------|
| `for(i in 0..array.size-1)` | 0 to size-1 (inclusive) | Explicit range control |
| `for(i in 0 until array.size)` | 0 to size-1 (exclusive) | Modern Kotlin style |
| `for(i in array.indices)` | All valid indices | **Recommended** - safe & clear |

### Performance Notes

```kotlin
// Fastest - direct index access
for (i in array.indices) {
    process(array[i])
}

// Clean - functional style  
array.forEach { item ->
    process(item)
}

// Best of both - when you need index
array.forEachIndexed { index, item ->
    process(index, item)
}
````




