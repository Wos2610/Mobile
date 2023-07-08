
## **1. Visibility Modifiers**
*Trong phạm vi package: 
- `top-level` funtion: bên trong package nhưng ngoài class/object/interface.

- 4 visibility modifiers: `private`, `public`, `protected`, `internal`.
    +  Default : `public` , sẽ visible ở mọi nơi.
    + `private`: visible trong file chứa declaration.
    + `internal`: visible everywhere in same module.
    + `protected`: không available cho top-level declaration.

## **2. Class Member**
- Class by default là `final`: not inheritable.
- Muốn có class non-final thì khai báo 
`open class`

*Trong phạm vi class:
- `private`: chỉ visible inside this class.
- `protected`: visible inside this class and subclass.
- `internal`: bất kì client nào mà thấy được declaration this class thì có thể thấy `internal` members của class này.
- `public`: Bất kì client nào mà thấy được declaration this class thì có thể thấy `public` members của class này.

## **3.Constructor**
- Có một `primary constructor` và một hoặc nhiều hơn `secondary constructor`

### Primary Constructor
```kotlin
class Person constructor(firstName: String) { /*...*/ }
hoặc
class Person(firstName: String) { /*...*/ }
// có thể bỏ trong 1 số trường hợp
```

### Block Init  
- `Primary constructor` không thể chứa code.
- Nếu muốn tạo `initializer code` thì ta sẽ đặt chúng vào trong `intializer blocks`
- `Initializer blocks` 
```kotlin
class InitOrderDemo(name: String) {
    val firstProperty = "First property: $name".also(::println)
    
    init {
        println("First initializer block that prints $name")
    }
    
    val secondProperty = "Second property: ${name.length}".also(::println)
    
    init {
        println("Second initializer block that prints ${name.length}")
    }
}

fun main() {
    InitOrderDemo("hello")
}
```

- Để các `parameters` trong primary constructor thành các `property` của class thì thêm `val/var` trước các parameters
```kotlin
class Person(val firstName: String, val lastName: String, var isEmployed: Boolean = true)
```

### Secondary Constructor

- Được khai báo bên trong class.
```kotlin
class Person(val pets: MutableList<Pet> = mutableListOf())

class Pet {
    constructor(owner: Person) {
        owner.pets.add(this) // adds this pet to the list of its owner's pets
    }
}
```

- Code trong `initializer code` và `property initializer` sẽ được thực hiện trước mỗi `secondary constructor`.

```kotlin
class Constructors {
    init {
        println("Init block")
    }

    constructor(i: Int) {
        println("Constructor $i")
    }
}
```
- Nếu một `non-abstract class` không có `primary/secondary constructor` thì sẽ mặc định một `public primary constructor`. Nếu muốn private thì 

```kotlin
class DontCreateMe private constructor() { /*...*/ }
```

## **4. Data Classes**
- Khai báo:
```kotlin
data class User(val name: String, val age: Int)
```

- Trình biên dịch sẽ tự động sinh các đoạn code cho các hàm sau của các `property` được khai báo trong `primary constructor`:  
    + equals / hashCode()
    + toString
    + componentN()
    + copy()

- Tuy nhiên `data class` cũng yêu cầu một số điều kiện  
    + `Primary constructor` có ít nhất một `parameter`.
    + Tất cả các `parameters` đều phải là các `properties` của class (nghĩa là phải khai báo val/var).
    + `Data class` thì không thể `abstract`, `open`, `seal`, `inner`.


- Những hàm ở trên mà được tự động sinh ra thì chỉ sử dụng các `parameters` trong `primary constructor` chứ không tính các `properties` khác được khai báo bên trong class.

```kotlin
data class Person(val name: String) {
    var age: Int = 0
}
fun main() {
    val person1 = Person("John")
    val person2 = Person("John")
    person1.age = 10
    person2.age = 20
    println("person1 == person2: ${person1 == person2}")
    // Dù khác tuổi nhưng output : true
}
```

### Copying
- Khi bạn muốn copy một `object` , thay đổi 1 vài `properties` và giữ nguyên phần còn lại thì dùng hàm `copy()`.

```kotlin
data class User(val name : String, val age : Int)
fun main(){
   	val jack = User(name = "Jack", age = 1)
	val olderJack = jack.copy(age = 2) 
    print(olderJack)
}
```

## **5. Enum Class**

```kotlin
enum class Fruit {
    WATERMELON, JACKFRUIT, MANGOSTEEN
}
```

### Anonymous Class
- Là class mà không được khai báo bằng keyword `class`, thường sử dụng một lần.
- Enum constants có thể khởi tạo bằng anonymous class với các phương thức riêng của chúng. Các constants được phân cách với các member khác bằng dấu `;`, giữa các constant là dấu `,`
``` kotlin
enum class ProtocolState {
    WAITING {
        override fun signal() = TALKING
    },

    TALKING {
        override fun signal() = WAITING
    };

    abstract fun signal(): ProtocolState
}
```

## **6. Nested Class**
- Là một class được khởi tạo trong một class khác.
```kotlin
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}
val demo = Outer.Nested().foo() // == 2
```
- **Nested class** không thể truy cập đến **Outer class** members.
- Ta có thể sử dụng các members của **Nested class** mà không phải tạo **object** cụ thể.

## **7. Inner Class**
- Là một **Nested class** nhưng có thêm từ khóa `inner`, giúp **Nested class** có thể truy cập đến các members của **Outer class** (kể cả `private`).

```kotlin
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```





