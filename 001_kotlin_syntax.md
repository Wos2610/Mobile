- val :  value (read-only value)
- var :  variable (mutable value)

val a = 1
val a : Int  
## **2. Function**
- Syntax:
```kotlin
fun function_name (parameter_name : parameter_type) : function_type {/*...*/}
```

- Unit = hàm void, không cần viết cũng được

```kotlin
fun printHello(name: String?): Unit {...}

fun printHello(name: String?) { ... }
```
> String? : Dấu hỏi chấm ở đây là để có thể gán giá trị(truyền vào) null cho biến name (type: String)

- Single-expression functions
```kotlin
fun sum(a : Int, b : Int) : Int {
    return a + b
} 

fun sum (a: Int, b: Int) : Int = a + b 

fun sum (a: Int, b: Int) = a + b 
```

## **Null safety**
```kotlin
var a: String = "abc" // Regular initialization means non-null by default
a = null // compilation error

var b: String? = "abc" // can be set to null
b = null
```

- Nhưng khi ta muốn access đến một property của nullable var thì ta có thể dùng **safe call operator** `?.`

```kotlin
val l = b.length // error: variable 'b' can be null
println(b?.length) // return Int? if b != null, return null nếu ngược lại
```

- Khi ta chỉ muốn làm việc với non-null value của một nullable val/var thì ta dùng `?.let`
```kotlin
var listWithNulls: List<String?> = listOf("Kotlin", null)
    for (item in listWithNulls) {
         item?.let { println(it) } // prints Kotlin and ignores null
    }
```

**Elvis operator `?:`**
```kotlin
val l: Int = if (b != null) b.length else -1
// Khi sử dụng ?:
val l = b?.length ?: -1
```
- Nếu bên trái `?:` != null thì return trái, còn nếu bên trái = null thì return bên phải.


## **Equality**
### Structural quality
- == tương đương với equals().

### Referential quality  
- a === b: true nếu a, b cùng trỏ đến 1 đối tượng.  
- Đối với primitive types thì === tương đương với ==.

## **3. Condition**

```kotlin
if (a > b) {
    max = a
} else {
    max = b
}

// As expression
max = if (a > b) a else b

// You can also use `else if` in expressions:
val maxLimit = 1
val maxOrLimit = if (maxLimit > a) maxLimit else if (a > b) a else b
```

- when tương tự switch-case nhưng không cần lệnh break.
- when có thể sử dụng như `expression` hoặc `statement`.

## **4. Loop**
### 4.1 Repeat
- 1 vòng lặp
- Nhiều vòng lặp

### 4.2 While

### 4.3 For
- Giống for-each trong java.
```kotlin
for (item in collection) {...}
```

- Iterate over a range of numbers  
    + a until b : [a, b)
    + a dowto b : [a, b]
```kotlin
for (i in 1..3) { // i >= 1 && i <= 3
    print(i)
}
// 123

for (i in 1 until 10) {  // i >= 1 && i < 10
    print(i)
}
// 123456789

for (i in 6 downTo 0 step 2) {
    print(i)
}
// 6420

```

- Iterate through an array or a list with an index
```kotlin
for (i in array.indices) {
    println(array[i])
}
```

### 4.4 Break / Continue
- Có thể gán label cho các vòng lặp để sử dụng break and continue cho từng vòng lặp.

```kotlin
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (...) break@loop
    }
}
```

# **Collections**
- Gồm List, Set, Map

- Collection<`T`> : read-only.
- MutableCollection<`T`> : Collection + write operations. 

*List<`T`>:
- Có xét thứ tự các elements và cung cấp index để truy cập đến chúng.
- `Indices` : 0 -> n - 1
- List (immutable)
```kotlin
val arr = listOf(1, 2, 3)
```

- List (mutable)
```kotlin
val arr = mutableListOf(1, 2, 3)
numbers.add(5)
numbers.removeAt(1)
```

> Muốn gán cho arr một list/MutableList mới thì phải khai báo arr là var

  
```kotlin
val numbers = listOf("one", "two", "three", "four")
println("Number of elements: ${numbers.size}")
println("Third element: ${numbers.get(2)}")
println("Fourth element: ${numbers[3]}")
println("Index of element \"two\" ${numbers.indexOf("two")}")

/* Number of elements: 4
Third element: three
Fourth element: four
Index of element "two" 1 */
```

*Set<`T`>
- Mỗi element chỉ xuất hiện duy nhất một lần, không quan trọng thứ tự sắp xếp.

- Set (immutable)
```kotlin
val arr = setOf(1, 2, 3, 1)
// [1, 2, 3]
```

- Set (mutable) : mutableSet có thể chỉnh sửa các giá trị trong nó. Khi thêm 1 giá trị mới thì nó sẽ kiểm tra xem đã xuất hiện phần tử đó chưa, nếu chưa thì phần tử đó sẽ được thêm vào mutableSet.
```kotlin
val arr = mutableSetOf(1, 2, 3) // [1, 2, 3]
arr.add(5) // [1, 2, 3, 5]
arr.remove(2) // [1, 3, 5]
```
> mutableSet không sử dụng index nên không có removeAt(), thay vào đó là remove() trực tiếp giá trị muốn xoá.


*Map<`K`, `V`>
- Không kế thừa `Collection`.
- Key không được trùng nhau nhưng value của các key khác nhau có thể bằng nhau.
- Nếu gọi riêng keys hoặc values của Map thì sẽ trả về một List.

- Map (immutable)
```kotlin
val numbersMap = mapOf("key1" to 1, 
                        "key2" to 2, 
                        "key3" to 3, 
                        "key4" to 1)

println("All keys: ${numbersMap.keys}")
// All keys: [key1, key2, key3, key4]
println("All values: ${numbersMap.values}")
// All values: [1, 2, 3, 1]
```

- MutableMap
```kotlin
val numbersMap = mutableMapOf("one" to 1, 
                              "two" to 2)
numbersMap.put("three", 3)
numbersMap["one"] = 11

println(numbersMap)
// {one=11, two=2, three=3}
```

