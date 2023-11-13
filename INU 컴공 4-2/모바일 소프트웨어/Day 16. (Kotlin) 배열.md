---
Author: CarefreeLife98
Date: 2023-11-07T18:09:00
Agenda: 
tags:
  - Android
  - Kotlin
---
# 배열 객체를 생성하는 방법 - Array(), arrayOf(), (Type)ArrayOf()
> **배열 객체를 생성하는 방법에는 세 가지가 존재함.**
> 1. Array()
> 2. arrayOf()
> 3. IntArrayOf() - 잘 사용하지 않음
```kotlin
package com.example.androidarray  
  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import com.example.androidarray.databinding.ActivityMainBinding  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        binding.button.setOnClickListener {  
            // arrayOf<>() : 원소의 개수가 제한적이고 각 원소의 초기 값이 다를 때 사용.
            // type: Array<type>            
            val strArr: Array<String> = arrayOf<String>("Red", "Green", "Blue")  
  
            // Array<>() : 원소의 개수가 많으며 초기 값이 같을 때 사용.  
            // type: Array<type>            
            val strArr2: Array<String> = Array(strArr.size) { idx -> strArr[idx] }  
  
            // (type)ArrayOf : 대부분의 자료형을 사용할 수 있지만 String Type 은 사용하지 못한다.  
            // type: (Type)Array            
            val boolArr = booleanArrayOf(true, false, false)  
            val intArr:IntArray = intArrayOf(9, 8, 0, 1, 1, 6)  
            // stringArrayOf 사용 불가.  
            // val stringArr = stringArrayOf("red", "green", "blue")
            
            binding.textView.text = boolArr.contentToString()  
        }  
    }  
}
```
> - **Array() & arrayOf()**
> 	- **Array() 와 arrayOf() 의 자료형은 Array\<type> 으로 동일**하다.
> <br><br>
> - **(type)ArrayOf()**
> 	- **(type)ArrayOf() 의 자료형은 (Type)Array** 로서 다르게 표현됨.
> 	- **대부분의 자료형을 지원하지만 String 자료형은 지원하지 않음.**
> 		- stringArrayOf() 는 사용불가.

<br><br>

# 배열과 List Collection

# 문자열 배열 리소스를 Array 객체에 저장

**<중요>** **arrayOf<>() 의 반환 Type 은 Array\<String>** 

```kotlin
package com.example.myarray  
  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import com.example.myarray.databinding.ActivityMainBinding  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        binding.button.setOnClickListener {  
            // arrayOf 는 원소의 개수가 제한적이고 다양한 값을 가질 때 사용  
            val strArr: Array<String> = arrayOf<String>("Red", "Green", "Blue")  
            val strList: List<String> = listOf<String>("Red", "Green", "Blue") // immutable list. 수정 불가. 불변  
            var strMutableList = mutableListOf("Red", "Green", "Blue") // 수정 가능. 가변  
//            val strArr = Array<String>(3) { index:Int -> "$index arg" }  
//            val strArr = Array<String>(3) { "$it arg" }  
//            val intArr = Array<Int>(10) { it + 3 }  
//            val strArr2 = Array<String>(strArr.size) { strArr[it] }  
//            val strArr3: BooleanArray = booleanArrayOf(false, false, true)  
  
//            binding.textView.text =  
//                strList.map { "COLOR $it" }.toString() // map : 모든 원소에 대하여 공통적으로 적용  
//            binding.textView.text =  
//                strList.filter { it.length > 3 }.toString() // filter : 모든 원소에 대하여 Boolean function 적용  
//            binding.textView.text =  
//                strList.none { it.length > 10 }.toString() // none : 모든 원소에 대하여 Boolean function 적용 결곽가 전부 false 인 경우 True 반환.  
             binding.textView.text =  
                strList.any { it.length > 10 }.toString() // none : 모든 원소에 대하여 Boolean function 적용 결곽가 전부 true 인 경우 True 반환.  
        }  
    }  
}
```
# Compound Button
