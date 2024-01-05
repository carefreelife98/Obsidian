```kotlin
package assignment.hw1  
  
import kotlin.math.PI  
import kotlin.math.abs  
  
class Person (var name:String, var age: Int)  
const val MY_KEY = 980116  
  
class Hw1 {  
    fun p2() {  
        println("Hello World!")  
    }  
  
    fun p3() {  
        println(PI)  
        println(abs(-12.6))  
        println("Hello World!")  
    }  
  
    fun p4() {  
        var Chae = Person("Chae", 26)  
        println(Chae.age)  
    }  
  
    fun p6() {  
        val a: Int = 1  
        val b = 2  
        val c: Int  
        c = 3  
  
        var x = 5  
        x += 1  
    }  
  
    fun p7() {  
        val x: Any = "Hello"  
        val y: Number = 3.17  
    }  
  
    fun p9() {  
        println("Byte: MIN_VALUE=${Byte.MIN_VALUE}, MAX-VALUE=${Byte.MAX_VALUE}, SIZE_BITS=${Byte.SIZE_BITS}")  
        println("Short: MIN_VALUE=${Short.MIN_VALUE}, MAX-VALUE=${Short.MAX_VALUE}, SIZE_BITS=${Short.SIZE_BITS}")  
        println("Int: MIN_VALUE=${Int.MIN_VALUE}, MAX-VALUE=${Int.MAX_VALUE}, SIZE_BITS=${Int.SIZE_BITS}")  
        println("Long: MIN_VALUE=${Long.MIN_VALUE}, MAX-VALUE=${Long.MAX_VALUE}, SIZE_BITS=${Long.SIZE_BITS}")  
        println("Float: MIN_VALUE=${Float.MIN_VALUE}, MAX-VALUE=${Float.MAX_VALUE}, SIZE_BITS=${Float.SIZE_BITS}")  
        println("Double: MIN_VALUE=${Double.MIN_VALUE}, MAX-VALUE=${Double.MAX_VALUE}, SIZE_BITS=${Double.SIZE_BITS}")  
    }  
  
    fun p10(){  
        val b = 980116  
        val i:Byte = b.toByte()  
        println("${b.toString(2)}, ${b.toString(16)}, $i")  
  
        val f = i.toFloat()  
        println("int $i becomes Float $f")  
    }  
  
    fun p11() {  
        val f = 3.14f  
        val d = 2.718  
  
//        val s:Short = f.toShort() // Int 로 먼저 형변환 후 Byte 형변환 가능  
        val s:Short = f.toInt().toShort()  
//        val b: Byte = d.toByte()  // Int 로 먼저 형변환 후 Byte 형변환 가능  
        val b: Byte = d.toInt().toByte()  
    }  
  
    fun p12(v:Any){ // 형식 인자에는 val & var를 지정하지 않는다.  
        when (v){ // Java 의 Switch-Case 와 비슷함. Break 존재하지 않는다.  
            is Short -> println("The Type od $v is Short.")  
            is Int -> println("The Type od $v is Int.")  
            is Float -> println("The Type od $v is Float.")  
            is Double -> println("The Type od $v is Double.")  
  
            else -> println("unknown type")  
        }  
    }  
  
    fun p13(){  
        val code:Int = 65 // 문자 'A'의 ASCII Code        val hanCodeFirst:Char = '\uac00' // UNICode - 첫 번째 한글 음절 '가'  
        val hanCodeLast:Char = '\ud7a3' // UNICode - 마지막 한글 음절 '힣'  
  
        println("${code.toChar()}, ${(code+1).toChar()}") // A, B  
        println("$hanCodeFirst, $hanCodeLast") // '가', '힣'  
    }  
  
    fun p14(){  
        for (i in 48..60){  
            val c = i.toChar()  
            if (c.isDigit()) print(c)  
            else print ('*')  
        }        println()  
  
        for (i in 65..90) {  
            print("${i.toChar()} ")  
        }  
        println()  
    }  
  
    fun p15(){  
//        println('0'.toInt()) // Deprecated 됨.  
        println('0'.code) // 해당 숫자가 아닌 숫자의 ASCII Code 를 출력함.  
        println('a'.code)  
        println('0'.digitToInt()) // 대안  
//        println('a'.digitToInt()) // Error  
    }  
  
    fun p16(){  
        var foo: Boolean = true  
        val bar = false  
  
        println(foo && bar)  
        println(foo || bar)  
        println(!foo)  
  
        foo = !foo // toggle : Debug 시 Break Point        println(foo)  
    }  
  
    fun p17(){  
        val foo: String = "My First Kotlin"  
        val c: Char = 's'  
  
//        println(foo.length)  
        val size = foo.length  
        println("first char = ${foo[0]}, last char = ${foo[size-1]}") // indexing (Kotlin 에서 권장하는 방법)  
//        println("${foo.get(0)}, ${foo.get(size-1)}") // getter, 권장하지 않음  
  
        val ch1: Char = foo.get(3)  
        val ch2: Char = foo[9]  
        println("length = $size, ch1 = $ch1, ch2 = $ch2")  
  
        for(i in 0 until size)  
            print(foo[i])  
        println()  
    }  
  
    fun p18() {  
        val foo: String = "My First Kotlin"  
  
        println(foo[9])  
        println(foo.substring(0, 9) + "Python") // 0 ~ 8 까지  
        println(foo)  
  
        val foo2 = foo.replace("Kotlin", "C++")  
        println(foo2)  
        println(foo == foo2)  
    }  
  
    fun p19() {  
        val pi = PI.toFloat() // Double  
        val digit = 10  
        val str = "CarefreeLife"  
        val c = '\uAC00'  
        val length = 3000  
  
  
        // 서식 지정: .format("String, %(숫자: 출력 시 해당 칸 수 만큼 띄어줌)변수 타입", 변수)  
        val lengthStr: String = String.format("길이 = %5d meters", length)  
        // %.4f : 소수점 아래 4 자리수에서 반 올림 하여 해당 자리까지 출력  
        println("pi = %.4f %3d %10s %c".format(pi, digit, str, c))  
        println(lengthStr)  
    }  
  
    fun p20() {  
        var a = 1  
        val s1 = "a is $a"  
  
        a = 2  
        val s2 = "${s1.replace("is", "was")}, but now is $a"  
        println(s2)  
  
        val s = "abc"  
        println("$s.length is ${s.length}")  
    }  
  
    fun p21() {  
        val expr = "My First Kotlin"  
        val amount = 10  
  
        val lengthStr = "test length: ${expr.length}"  
        val priceStr = "price : USD ${'$'}$amount"  
        val priceStr2 = "price : USD \$$amount"  
  
        println(lengthStr)  
        println(priceStr)  
        println(priceStr2)  
  
        println("\"Hey\" , I have only $amount\$.")  
    }  
  
    fun p22p23() {  
        // 변수의 type 직후에 ? (= nullable type) 를 붙혀 nullable 변수로 만들 수 있다.  
        var myFavoriteSinger: String? = null  
        println(myFavoriteSinger)  
  
        myFavoriteSinger = "Westlife"  
        println(myFavoriteSinger)  
    }  
  
    fun p24() {  
        var myFavoriteSinger: String? = null  
  
        // 에러. nullable type 변수는 메소드 적용 시에도 nullable 하게 작성 해주어야 한다.  
//        myFavoriteSinger.length  
        myFavoriteSinger = "Westlife"  
        println(myFavoriteSinger.length)  
    }  
  
    fun p25_1(s: String?) {  
        if (s == null) {  
            println("\"$s\" is null")  
        }  
        else println("\"$s\" is NOT null")  
    }  
    fun p25_2(s: String?) {  
        if (s?.isEmpty() == true) {  
            println("\"$s\" is empty")  
        }  
        else println("\"$s\" is NOT empty")  
    }  
    fun p26() {  
        var mfs: String? = "BTS"  
        println(mfs!!.length)  
    }  
  
    fun p27() {  
        var singer:String? = null // Nullable Type  
  
        val size = singer?.length ?: 0 // Elvis Operator  
        print(size)  
  
//        // Java Style Code  
//        if (singer != null) {  
//            println(singer.length)  
//        }  
//        else print(0)  
    }  
  
    fun p28(x: Any) {  
        if (x is Int) {  
            println("$x is ${x.javaClass}")  
        }  
        else if (x !is Int) {  
            println("$x is NOT Int. The type is ${x.javaClass}")  
        }  
        var num: Long = 8L  
        val str: Any  
  
        p28(num)  
        p28(8)  
        var ld = num as Long  
        p28(ld)  
  
        str = "Hello, Kotlin"  
        if (str is String) {  
            println("\"$str\" is ${str.javaClass}")  
        }  
    }  
  
    fun p30p32() {  
        val trafficLightColor = "Red"  
        if (trafficLightColor == "Red")  
            println("Stop")  
        else if (trafficLightColor == "Yellow")  
            println("Slow")  
        else  
            println("Go")  
    }  
  
    fun p33(a:Int, b:Int) {  
  
        if (a > b) {  
            println("$a 가 $b 보다 큽니다.")  
        }else if (a < b) {  
            println("$b 가 $a 보다 큽니다.")  
        } else {  
            println("$a == $b")  
        }  
        print("Enter the Score: ")  
        val score = readLine()!!.toInt()  
        val grade = when (score) {  
            in 90..100 -> 'A'  
            in 80..89 -> 'B'  
            in 70..79 -> 'C'  
            in 60..69 -> 'D'  
            else -> 'F'  
        }  
        println("grade = $grade")  
    }  
  
    fun p35() {  
        val trafficLightColor = "Green"  
  
        when (trafficLightColor) {  
            "Red" -> println("Stop")  
            "Yellow" -> println("Slow")  
            "Green" -> println("Go")  
            else -> println("Light color Invalid")  
        }  
    }  
  
    fun p36(x:Int) {  
        when (x) {  
            1 -> println("x is 1")  
            2 -> println("x is 2")  
            3, 4 -> println("x is 3 or 4")  
            else -> {  
                println("x is greater than or equal to 5")  
            }  
        }  
        p36(1)  
        p36(3)  
        p36(5)  
    }  
  
    enum class Color {  
        RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET  
    }  
  
    fun p37(color: Color) {  
        println(  
            when (color) {  
                Color.RED -> "Richard"  
                Color.ORANGE -> "Of"  
                Color.YELLOW -> "York"  
                Color.GREEN -> "Gave"  
                Color.BLUE -> "Battle"  
                Color.INDIGO -> "In"  
                Color.VIOLET -> "Vain"  
                else -> {  
                    "Not a defined color"  
                }  
            }  
        )  
    }  
  
    fun p38() {  
        print("Enter the Name: ")  
        val name = readLine()!!.toString()  
        if (name == "carefreelife") {  
            print("Enter the Score: ")  
            val score = readLine()!!.toInt()  
            val credit = when (score) {  
                    in 90..100 -> "Credit 10"  
                    in 80..89 -> "Credit 8"  
                    in 70..79 -> "Credit 6"  
                    in 60..69 -> "Credit 4"  
                    else -> "No Credit"  
                }  
            println("Credit = $credit")  
        } else {  
            println("You are NOT Authorized")  
        }  
    }  
  
    fun p39() {  
        val tlc = "Black"  
  
        val message = if (tlc == "Red") "Stop"  
        else if (tlc == "Yellow") "Slow"  
        else if (tlc == "Green") "Go"  
        else "Invalid Light Color"  
  
        println(message)  
    }  
  
    fun p40() {  
        val tlc = "Amber"  
  
        val message = when (tlc) {  
            "Red" -> println("Stop")  
            "Yellow", "Amber" -> "Proceed with caution"  
            "Green" -> "Go"  
            else -> "Invalid Light Color"  
        }  
        println(message)  
    }  
  
    fun p41(i: Int): String {  
        return when {  
            i % 15 == 0 -> "FizzBuzz"  
            i % 3 == 0 -> "Fizz"  
            i % 5 == 0 -> "Buzz"  
            else -> "$i"  
        }  
    }  
  
    fun p41Loop() {  
        for (i in 1..100) {  
            print(p41(i))  
            if (i % 10 == 0) println()  
        }        for (i in 100 downTo 1 step 2) {  
            print(p41(i))  
            if (i % 10 == 0) println()  
        }  
        // while  
        var j = 1  
        while (j <= 100) {  
            println(p41(j))  
            if (j % 10 == 0) println()  
            j++  
        }  
  
        // do - while  
        var x = 1  
        do {  
            println(p41(x))  
            if (x % 10 == 0) println()  
            x++  
        } while (x <= 100)  
    }  
  
    fun p43() {  
        val numbers = arrayOf(1, 2, 3)  
        val mixedArray = arrayOf(7, "Kotlin", false)  
  
        val intOnlyArray = arrayOf<Int>(1, 2, 3)  
        val intOnlyArray2 = intArrayOf(4, 5, 6, 7)  
  
        var i = intOnlyArray[0]  
        val i2 = intOnlyArray2.get(2)  
        println("i=$i, i2 =$i2")  
        println("${intOnlyArray.size}, ${intOnlyArray2.size}")  
        println("${intOnlyArray.first()}, ${intOnlyArray2.last()}")  
  
        intOnlyArray[0] = 0  
        intOnlyArray2.set(3, 9)  
        println(intOnlyArray.contentToString())  
        println(intOnlyArray2.contentToString())  
    }  
  
    fun p44_1() {  
        val intArray: Array<Int> = arrayOf(1, 2, 3)  
        var intArray2: IntArray = intArrayOf(4, 5, 6, 7)  
  
        println("${intArray.size}, ${intArray.first()}, ${intArray.last()}")  
        println("${intArray2[0]}, ${intArray2.get(3)}")  
  
        intArray2[2] = 8  
        intArray2.set(3, 9)  
        println("${intArray2.contentToString()}")  
  
        // Lambda 를 활용한 식.  
        val intArray3: Array<Int> = Array(3) {i -> intArray[i]}  
    }  
  
    fun p44_2(arr : IntArray) {  
        for (e in arr) {  
            print("$e ")  
        }  
        println()  
    }  
  
    fun p45() {  
        val intOnlyArray = arrayOf<Int>(1, 2, 3)  
        val intOnlyArray2 = intArrayOf(4, 5, 6, 7)  
  
        intOnlyArray.forEach { element -> print("$element") }  
        println()  
        intOnlyArray2.forEachIndexed { i, e -> println("intOnlyArray2[$i] = $e") }  
  
        val iter: Iterator<Int> = intOnlyArray.iterator()  
        while (iter.hasNext()) {  
            val e = iter.next()  
            print("$e ")  
        }  
        println()  
    }  
  
    fun p46() {  
        val words: Array<String> = arrayOf("Python", "Kotlin", "Swift")  
        val intOnlyArray = arrayOf(4, 5, 6, 7)  
  
        val foo: (Array<Int>) -> Unit = fun (arr:Array<Int>){  
            for (i in arr) println(i)  
        }  
        val bar: (Array<String>) -> Unit = fun (arr:Array<String>){  
            for (i in arr) println(i)  
        }  
  
        bar(words)  
        foo(intOnlyArray)  
    }  
  
    fun p46_2() {  
        val words: Array<String> = arrayOf("Python", "Kotlin", "Swift")  
        val intOnlyArray = arrayOf(4, 5, 6, 7)  
  
//        val unified = fun(arr: Array<>) {  
//            for (i in arr) println(i)  
//        }  
//        unified(words)  
//        unified(intOnlyArray)  
    }  
  
    fun p47() {  
        val array1 = arrayOf(1, 2, 3)  
        val array2 = arrayOf(4, 5, 6)  
        val array3 = arrayOf(7, 8, 9)  
  
        val arr2d = arrayOf(array1, array2, array3)  
  
        for (e1 in arr2d) {  
            for (e2 in e1) {  
                print("$e2")  
            }  
            println()  
        }  
  
        println(arr2d[0][1])  
        println(arr2d[1][1])  
        println(arr2d[2][1])  
    }  
  
}  
  
  
  
// Hw1 Class 내의 모든 메소드 실행 (메소드 이름이 "p + 숫자" 인 경우만 실행)  
fun main() {  
    // function name (대부분 p + 숫자 2 ~ 47) list 생성  
    val functionNames = mutableListOf<String>()  
    for (i in 0..47) {  
        functionNames.add(i, "p$i")  
    }  
    // 호출할 함수를 포함하는 클래스의 인스턴스 생성  
    val targetObject = Hw1()  
  
    // 함수 이름 리스트를 기반으로 Hw1 클래스 내 거의 모든 함수 실행  
    for (functionName in functionNames) {  
        try {  
            // 함수 이름을 사용하여 함수를 호출  
            val function = targetObject::class.java.getMethod(functionName)  
            println("-----------------[${functionName}] Start -----------------")  
            println("function : ${function}")  
            println("[run ${functionName}]")  
            function.invoke(targetObject)  
            println("-----------------[${functionName}] End -----------------\n")  
        } catch (e: NoSuchMethodException) {  
            println("Function $functionName not found.\n")  
            continue  
        }  
    }  
}
```