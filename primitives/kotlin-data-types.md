# Kotlin Data Types - Range Values

## Primitive Types

| **Type** | **Min Value** | **Max Value** |
|:---------|:-------------:|:-------------:|
| **Byte** | `-128` | `127` |
| **Short** | `-32,768` | `32,767` |
| **Int** | `-2,147,483,648` | `2,147,483,647` |
| **Long** | `-9,223,372,036,854,775,808` | `9,223,372,036,854,775,807` |

## Floating Point Types

| **Type** | **Min Value** | **Max Value** |
|:---------|:-------------:|:-------------:|
| **Float** | `1.4E-45` ⚠️ | `3.4028235E38` |
| **Double** | `4.9E-324` ⚠️ | `1.7976931348623157E308` |

## ⚠️ Important Note

**Float.MIN_VALUE** and **Double.MIN_VALUE** are **NOT** the minimum negative values, but the **smallest positive values greater than zero**.

For actual minimum negative values, use:
- `-Float.MAX_VALUE`
- `-Double.MAX_VALUE`

## 🌌 Infinity Constants

| **Type** | **Negative Infinity** | **Positive Infinity** |
|:---------|:--------------------:|:--------------------:|
| **Float** | `Float.NEGATIVE_INFINITY` | `Float.POSITIVE_INFINITY` |
| **Double** | `Double.NEGATIVE_INFINITY` | `Double.POSITIVE_INFINITY` |

## Code Examples

```kotlin
// Safe range checking
val maxInt = Int.MAX_VALUE
val minInt = Int.MIN_VALUE

// Floating point infinities
val negInf = Float.NEGATIVE_INFINITY
val posInf = Float.POSITIVE_INFINITY
