
```Kotlin
class Person(val name:String, val age:Int)  
  
// kotlin = multi paradigm programming language. 다중 패러다임 언어.  
// 함수형 언어 + 객체 지향 언어  
  
// c(명령형 언어), c++(객체 지향형 언어)  
  
fun main(){ // function. 함수. entry point(진입점).  
    // 함수의 Body(몸체). block=compound statements(복문)  
    val chae = Person("Chae", 25) // on instance of the class Person  
    val park = Person("Park", 26) // on instance of the class Person  
    println(chae.age)  
    println(park.age)  
//    println(PI)  
//    println(abs(-12.7)) //절대값 |-12.7| = 12.7//    println("Hello World!")  
}
```
# val, var, const val
- **val (value) : Immutable variables (Read-Only Variables)**
	```Kotlin
val a: Int = 1 //(직접 할당)
val b = 2 // 'Int' Type 자동 참조
val c: Int // 변수가 초기화 되지 않을 때에는 Type 선언 필요
c = 3 // 선언 이후 값의 할당
	```
- **var (variable) : Mutable variables (Writeable variables)**
	```Kotlin
var x = 5 // 'Int' Type 자동 참조
x += 1
	```
- **const var : 전역 상수 (Immutable) - 함수 내에서 지역 변수로서 사용 불가능**
	- const val 은 무조건 top-level (가장 바깥) 에 지정해주어야 한다.
```Kotlin
const val MY_KEY = 1234 // 타입 생략
const val MY_KEY:Int = 1234

fun main(){
	...
}
```

# Basic Types of Kotlin
- **숫자**
	- Int
	- Float, Double

- **문자, 문자열**
	- Char, String

- **Boolean**
	- true
	- false

- **Any: 모든 Type 과 호환**

- **Number: 모든 숫자 Type 과 호환**

## Basic Types : Number
> **Number** : Kotlin type은 **첫 글자가 대문자.**
> **`Number` Type은 모든 숫자 Type 과 호환이 가능하다.**
- Double(64), Float(32)
- Long(64), Int(32), Short(16), Byte(8)

- **상수(Literal Constants)**
	- `Double` : 123.5, 123.5e10
	- `Float` : 123.5**F**, 123.5**F**
	- `Long` : 123**L**
	- `Hexadecimals` : 0x0F
	- `Binaries` : 0b00001011

- **형 변환 함수 :** `to + type` -> **`toByte()`**
	- toByte()
	- toShort()
	- toLong()
	- toFloat()
	- toDouble()

- [!] **U: Unsigned (부호 없음)**
	- ULong, UInt, UShort, UByte


## Changing Type : 확대 및 축소
- Number Type 을 크기 순으로 정렬
	- **`Byte -> Short -> Int -> Long -> Float -> Double`**
- **Type 확대 (Widening)** : Int -> Float
- **Type 축소 (Narrowing)** : Float -> Int

```Kotlin
package number  
  
class Number {  
    fun number(){  
        val b = 980116  
        val i:Byte = b.toByte()  
        println("${b.toString(2)}, ${b.toString(16)}, $i")  
  
        val f = i.toFloat()  
        println("int $i becomes Float $f")  
    }  
}
```
 
![[스크린샷 2023-09-19 오후 1.04.16.png]]

## Quiz 1
```kotlin
// 다음 코드에서 에러가 발생하는 원인을 찾으시오.

fun quiz_1(){  
    val f = 3.14f  
    val d = 2.718  
  
    val s:Short = f.toShort()
    val b:Byte = d.toByte() 
}
```
- 에러 발생 원인
	![[스크린샷 2023-09-19 오후 1.14.50.png]]
	- Int 로 먼저 형변환 해줘야 함.

## Numbers 예시 
### 예시 1
```kotlin
class NumbersEx {  
    fun ex1(){  
        println("Byte: MIN_VALUE=${Byte.MIN_VALUE}, MAX-VALUE=${Byte.MAX_VALUE}")  
//        println(Byte.MAX_VALUE)  
//        println(Byte.MIN_VALUE)  
    }
```
- println 내부에 `${}` 를 사용하여 변수를 넣어 한번에 출력할 수 있다.
- **Byte Type**은 `MIN_VALUE` 와 `MAX_VALUE` 가 존재한다.
	- **Byte.MAX_VALUE = `127`**
	- **Byte.MIN_VALUE = `-128`**

### 예시 2
```kotlin
fun ex2ChangeType(){  
    val b = 0b011111  
    val i2b:Byte = b.toByte()  
    println("2진수 변환=${i2b.toString(2)}, 16진수 변환=${i2b.toString(16)}") // 2진수, 16진수 변환 한번에 출력  
//  println(i2b.toString(16)) //16진수  
}
```
- `변수 이름 + : + 변수 Type` 을 통해 **변수의 타입을 직접 지정**할 수 있다.
- `Byte 변환 후 toString(int)` 메소드를 통해 **진수 변환**이 가능하다.
	![[스크린샷 2023-09-19 오후 1.40.06.png]]
	![[스크린샷 2023-09-19 오후 1.38.38.png]]

### 예시 3
```kotlin
fun ex3typeCheck(v:Any){ // 형식 인자에는 val & var를 지정하지 않는다.  
    when (v){ // Java 의 Switch-Case 와 비슷함. Break 존재하지 않는다.  
        is Short -> println("The Type od $v is Short.")  
        is Int -> println("The Type od $v is Int.")  
        is Float -> println("The Type od $v is Float.")  
        is Double -> println("The Type od $v is Double.")  
  
        else -> println("unknown type")  
    }  
}

//Main.kt
val f = 2.56f  
val d = 28.56  
val s:Short = 23  
var i = 12  
  
NumbersEx().ex3typeCheck(f) // 실 인자. actual argument  
NumbersEx().ex3typeCheck(d) // 실 인자. actual argument  
NumbersEx().ex3typeCheck(s) // 실 인자. actual argument  
NumbersEx().ex3typeCheck(i) // 실 인자. actual argument  
NumbersEx().ex3typeCheck("Hello")
```
![[스크린샷 2023-09-19 오후 1.56.21.png]]
- `Any` : **모든 형식의 인자를 수용함.**
- `when-is-else` : **Java 의 Switch-Case** 문과 비슷.
	- Break문을 없애어 잦은 오류를 해결함.

## Basic Types : Char
> **Char Type 변수는 `""` 로 묶은 문자만 할당 할 수 있음.**
- ASCII 코드에 해당하는 숫자를 할당할 경우 에러가 발생함.
	- `toChar()` 을 적용하여 **Char Type으로 변환해야 함.**

### 예시 1
```kotlin
fun char(){  
    val code:Int = 65 // 문자 'A'의 ASCII Code    val hanCodeFirst:Char = '\uac00' // UNICode - 첫 번째 한글 음절 '가'  
    val hanCodeLast:Char = '\ud7a3' // UNICode - 마지막 한글 음절 '힣'  
  
    println("${code.toChar()}, ${(code+1).toChar()}") // A, B  
    println("$hanCodeFirst, $hanCodeLast") // '가', '힣'  
}
```
- `toChar()` 메소드를 통해 Int 형 데이터를 해당 ASCII 코드의 문자로 변환 할 수 있다.
- 한글은 `Char = ''` 으로 선언함으로서 `UniCode 변환`을 통해 사용할 수 있다.

### 예시 2
```kotlin
fun char1(){  
  
    for (i in 48..60){  
        val c = i.toChar()  
        if (c.isDigit()) print(c)  
        else print ('*')  
    }    
    println()  
  
    for (i in 65..90) {  
        print("${i.toChar()} ")  
    }  
    println()  
}
```
![[스크린샷 2023-09-19 오후 2.54.31.png]]
- Kotlin 에서의 For 문은 `for (i in 0..10) {}`  과 같이 사용 가능하며 이때 `시작값과 끝 값은 i에 포함`된다.
- `isDigit()`
	- 해당 값이 숫자인지를 판별. (True, False) 반환

### 예시 3 (중요)
```kotlin
fun char_warn(){  
//        println('0'.toInt()) // Deprecated 됨.  
    println('0'.code) // 해당 숫자가 아닌 숫자의 ASCII Code 를 출력함.  
    println('0'.digitToInt()) // 대안  
}
```
![[스크린샷 2023-09-19 오후 3.03.17.png]]
- 기존에 사용하던 `'숫자'.toInt() 메소드는 Deprecated.`
- Intellij 에서 제공하는 대안으로 `'숫자'.code 메소드는 해당 숫자가 아닌 ASCII Code를 반환`해줌.
- **대안**
	- `'숫자'.digitToInt()` 메소드 사용하기.

## Basic Types : Boolean
> **Boolean**
> 값 : `true(참)` , `false(거짓)`
- **논리 연산 기호** : `&&(AND)` , `||(OR)` , `!(NOT)`

```kotlin
class boolean {  
    fun boolean(){  
        var foo: Boolean = true  
        val bar = false  
  
        println(foo && bar)  
        println(foo || bar)  
        println(!foo)  
  
        foo = !foo // toggle : Debug 시 Break Point        println(foo)  
    }  
}
```
- **! (toggle)**
	- 대상의 상태가 true, false 사이에서 고르게 변화함.
	- 개발 시 잘 사용하면 유용한 기능.

## Basic Types : String
> **String (문자열, String of Characters)**
- String 을 구성하는 **특정 위치의 Character 접근하기**
	- `String객체.get(index)`
	- `String객체[index]`
- String 자료구조는 `Immutable(변경 불가, Read-Only)` 함.
	- String 의 값을 변경하게 되면 덮어 씌워지는 것이 아니다.
	- 새로운 메모리 공간에 새로운 값이 쓰여지고 그것을 가리키게 되는 것.
	- 기존 값은 Garbage 가 되어 Garbage Collector 가 회수하기 전까지 메모리 공간을 낭비하게 됨.
	- 따라서 **String의 값을 바꾸기 위해서는** `String Buffer` 나 `String Builder` 를 사용해야 한다.

### 예시 1 - Indexing, Get()
```kotlin
fun string(){  
    val foo: String = "My First Kotlin"  
    val c: Char = 's'  
  
//        println(foo.length)  
    val size = foo.length  
    println("${foo[0]}, ${foo[size-1]}") // indexing (Kotlin 에서 권장하는 방법)  
//        println("${foo.get(0)}, ${foo.get(size-1)}") // getter  
    for(i in 0 until size)  
        print(foo[i])  
    println()  
}
```
- **String 에 속한 각 Char 에 접근하는 방법**
	- **Indexing (권장)** : `String[index]`
	- getter : `String.get(index)`
- **for (i in 0 until 10)**
	- **범위 :** `0 <= i <10`
	- `i in 0 ..< 10` 으로도 표현 가능.

### 예시 2 - substring(), replace()
```kotlin
fun string2() {  
    val foo: String = "My First Kotlin"  
  
    println(foo[9])  
    println(foo.substring(0, 9) + "Python") // 0 ~ 8 까지  
    println(foo.replace("Kotlin", "C++"))  
}
```
- `.substring(시작, ~이전)` 
	- 해당 문자열을 char 기준으로 범위 지정하여 자른다.
- `.replace("a", "b")`
	- 해당 문자열의 "a" 를 "b" 로 치환한다.

### 예시 3 - Format()
```kotlin
fun string3() {  
    val pi = PI // Double  
    val digit = 10  
    val str = "CarefreeLife"  
    val c = '\uAC00'  
    val length = 3000  
  
    // 서식 지정: .format("String, %(숫자: 출력 시 해당 칸 수 만큼 띄어줌)변수 타입", 변수)  
    println(String.format("길이 = %5d meters", length))  
    // %.4f : 소수점 아래 4 자리수에서 반 올림 하여 해당 자리까지 출력  
    println("%.4f %3d %10s %c".format(pi, digit, str, c))  
}
```
- **.format("%5d", 변수)**
	- 5칸 띄워 Int형 변수를 출력
		- % 뒤의 형식에 따라 해당 자리에 변수값을 삽입.
		- `d: Int` , `f: Float` , `s: String` , `c: Char`
	- **변수의 개수 만큼 %형식이 필요함.**
	- **소수점이 존재하는 형태의 변수 반올림 가능**
		- **%.4f** : 소수점 아래 4자리에서 `반올림 후 해당 자리까지 출력.`

### 예시 4 - Escape Character ( \ )
```kotlin
fun string4() {  
    val amount = "10"  
  
    println("가격은 USD \$ $amount") // escape character \ . 특수 문자 출력  
}
```
- **특수 문자의 출력은 Escape Character 를 사용. ( \ )**
	- `\$` , `\'` ...


## Basic Types : NULL
```kotlin
fun NULL() {  
  
    // 변수의 type 직후에 ? (= nullable type) 를 붙혀 nullable 변수로 만들 수 있다.  
    var myFavoriteSinger: String? = null  
    println(myFavoriteSinger)  
    
    myFavoriteSinger = "Westlife"  
    println(myFavoriteSinger)  
}
```

> **Nullable 변수 만들기**
1. 변수 선언 후 해당 변수의 Type 직후에 `?` 추가.
2. 해당 Type 은 **Nullable Type** 으로 변경이 된다.
3. **Nullable Type 으로 설정된 해당 변수에 NULL 값을 사용할 수 있게 된다.**

### (중요) 안전 호출 연산자 (Safe Call Operator) : ?
- **Nullable Type 변수를 참조 할 때에는 필수적으로 ? 사용.**
- 객체 호출 시 해당 객체 직후에 `?` 사용 후 `.` 으로 참조.
	- 해당 **객체가 NULL 이더라도 Exception 발생하지 않고 실행**이 된다.
	- 또한, 해당 객체가 NULL 이 아니더라도 그대로 동작한다.
- **객체의 NULL 상태와 상관없이 안전하게 프로그램을 실행시킬  수 있다.**

```kotlin
// NULL.kt
fun nullCheck(s: String?) {  
    if (s == null) {  
        println("\"$s\" is null")  
    }  
    else println("\"$s\" is NOT null")  
}  
  
fun emptyCheck(s: String?) {  
    if (s?.isEmpty() == true) {  
        println("\"$s\" is empty")  
    }  
    else println("\"$s\" is NOT empty")  
}

// Main.kt
fun main() { // Top-level Function  
    var str: String? = null  
    basicTypes.NULL().nullCheck(str)  
    basicTypes.NULL().emptyCheck(str)  
  
    var str2: String? = ""  
    basicTypes.NULL().nullCheck(str2)  
    basicTypes.NULL().emptyCheck(str2)  
}
```
**실행 결과**
![[스크린샷 2023-09-21 오전 5.45.04.png]]

### Assertion Operator : !!
> **변수 값이 절대 NULL 이 아님을 보증하는 연산자.**
> `nullable variable !!. method/property`

```kotlin
fun assertionOperator() {  
    var mfs: String? = "BTS"  
    println(mfs!!.length)  
}
```

### Elvis Operator : ?:
```kotlin
fun elvisOperator() {  
    var singer:String? = null // Nullable Type  
    
    val size = singer?.length ?: 0 // Elvis Operator 
    print(size)  
  
//        // Java Style Code  
//        if (singer != null) {  
//            println(singer.length)  
//        }  
//        else print(0)  
}
```

> Nullable 객체를 유용하게 사용할 수 있는 연산자.
- **Nullable 객체가 실제 NULL인 경우 Default 값을 반환.**
	- **해당 객체가 NULL 이 아닌 경우 참조 결과에 따라 값을 반환.**

### Smart Cast & Type Check
> **Smart Cast : 타입 추론.**
> Type Check 를 사용하여 객체의 Type을 확인 할 수 있음.
- **Smart Cast**
	- Number, Any 등과 같이 포괄적인 Type 을 변수에 지정 시, 추후 컴파일러가 자동으로 변수에 할당된 Data Type에 따라 Type을 지정해준다.
	- `typeCheck()` 및 `is` 메소드를 사용하여 Type 을 확인하거나 Type에 대한 조건을 생성할 수 있다.

```kotlin
// Smart Cast  
fun typeCheck(x: Any) {  
    if (x is Int)  
        println("$x is INT")  
    else  
        println("$x is NOT INT")  
}

// Main.kt
var num: Number = 8  
var str: Any = "CarefreeLife"  
  
typeCheck(num)
```
**실행 결과**
![[스크린샷 2023-09-21 오전 6.24.36.png]]




## If 문을 수식으로 할당하기 & Elvis Operator
```kotlin
fun ifAsEquation() {  
    var equation: String? = "CarefreeLife"  

	// 아래와 같이 If 문 자체를 변수에 수식으로서 할당 할 수 있다.	
    var length = if (equation != null) {  
        equation.length  
    } else {  
        0  
    }  
  
    println("length is $length")  
}
```
- Kotlin 에서는 **If 문 자체를 변수에 수식으로서 할당이 가능하다.**
- 하지만 다음과 같이 **Elvis Operator** 를 사용하여 더욱 간결하게 같은 코드를 구현할 수 있다.

```kotlin
# Elvis Operator 사용 시
fun ifAsEquation2() {  
    var equation: String? = "CarefreeLife"
    # Elvis Operator ?: 사용
    var length = equation?.length ?: 0  
    println("length is $length")  
}
```
