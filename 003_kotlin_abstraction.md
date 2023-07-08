
## **1. Interface**

- Cách khai báo interface:
```kotlin
interface MyInterface {
    fun bar()
    fun foo() {
      // optional body
    }
}
```

- Có thể chứa:
    + Abstract method: function without the body.
    + Method implementation.

- Interface chỉ bao gồm các **abstract method**, **default method implementations**, không chứa các **state** (properties with initial values)
> Khác biệt giữa interface và abstract class

```kotlin
interface Vehicle {
    fun start()
    fun stop()
    fun accelerate(speed: Double)
}
```
- Các class mà implement interface thì phải implement toàn bộ những **abstract methods**, **properties** đã được khai báo trong interface.

- Một class có thể implement một hoặc nhiều interfaces.

- Interface không thể khai báo constructor.

- Trong interface, tất cả **members** đều ngầm định `public` và không thể thay đổi **access visibilities**.


## **2. Abstract Class**
- Syntax:
```kotlin
abstract class class_name{

}
```
- Có thể bao gồm:
    + abstract methods: without a body.
    + concrete methods: with a body.  

- Không thể tạo instance của một abstract class.

- Trong abstract class nếu ta không khai báo properties/methods là `abstract` thì sẽ mặc định là **non-abstract**. Khi muốn **override** lại những members này thì ta phải sử dụng keyword `open`.

- Những **properties** hoặc **abstract function** bắt buộc phải **override** lại ở **derived class** bằng cách sử dụng keyword `override`.

```kotlin
//abstract class
abstract class Employee(val name: String,val experience: Int) { // Non-Abstract
																// Property
	// Abstract Property (Must be overridden by Subclasses)
	abstract var salary: Double
	
	// Abstract Methods (Must be implemented by Subclasses)
	abstract fun dateOfBirth(date:String)

	// Non-Abstract Method
	fun employeeDetails() {
		println("Name of the employee: $name")
		println("Experience in years: $experience")
		println("Annual Salary: $salary")
	}
}
// derived class
class Engineer(name: String,experience: Int) : Employee(name,experience) {
	override var salary = 500000.00
	override fun dateOfBirth(date:String){
		println("Date of Birth is: $date")
	}
}
fun main(args: Array<String>) {
	val eng = Engineer("Praveen",2)
	eng.employeeDetails()
	eng.dateOfBirth("02 December 1994")
}
```