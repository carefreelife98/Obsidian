---
Author: CarefreeLife98
Date: 2023-11-09T18:04:00
Agenda: 
tags:
  - Android
---
# Check Box
> **Radio Button**
> - 단일 선택
> <br>
> **Check Box**
> - 다중 선택

## Check Box 사용 방식 두 가지
```kotlin
package com.example.selectcheckbox  
  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import android.widget.CheckBox  
import android.widget.Toast  
import com.example.selectcheckbox.databinding.ActivityMainBinding  
import java.lang.StringBuilder  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        val coffeeArr: Array<String> = resources.getStringArray(R.array.coffee_menus)  
        val orderedArr: Array<CheckBox> = arrayOf<CheckBox>(  
            binding.americanoCheckBox,  
            binding.latteCheckBox,  
            binding.decafCheckBox  
        )  
        val boolArr: Array<Boolean> = Array(coffeeArr.size) { false }  
  
  
  
        orderedArr.forEachIndexed { idx, cb ->  
            cb.text = coffeeArr[idx]  
        }  
  
        orderedArr.forEachIndexed { index, cb ->  
            cb.setOnCheckedChangeListener { _, isChecked ->  
                boolArr[index] = isChecked  
                showToast(boolArr, coffeeArr)  
            }  
        }    }  
  
    private fun showToast(bArray: Array<Boolean>, cArray: Array<String>) {  
        val sb = StringBuilder()  
  
        if (bArray.none { it }) {  
            sb.append("Please Select at least 1 Menu.")  
        } else {  
            bArray.forEachIndexed { index, b ->  
                if (b) sb.append("${cArray[index]} ")  
            }  
            sb.append("are Ordered. Thanks!")  
        }  
        Toast.makeText(applicationContext, sb.toString(), Toast.LENGTH_SHORT).show()  
    }  
}  
  
//binding.selectButton.setOnClickListener {  
//    val sb = StringBuilder()  
//  
//    if (orderedArr.none { it.isChecked }) {  
//        sb.append("음료를 선택해 주세요.")  
//    } else {  
//        orderedArr.forEach { if (it.isChecked) sb.append("${it.text} ") }  
//        sb.append(" are ordered. Thanks!")  
//    }  
//    showToast(sb.toString())  
//}  
  
//        binding.americanoCheckBox.text = coffeeArr[0]  
//        binding.latteCheckBox.text = coffeeArr[1]  
//        binding.decafCheckBox.text = coffeeArr[2]  
  
  
//        binding.selectButton.setOnClickListener {  
//            val sb = StringBuilder()  
//  
//            // Check Box 는 독립적으로 동작하므로  
//            // If Else 가 아닌 IF 를 통해 각각 독립적으로 실행 되어야 함.  
//            if (binding.americanoCheckBox.isChecked){  
//                sb.append("${binding.americanoCheckBox.text} ")  
//            }  
//            if (binding.latteCheckBox.isChecked){  
//                sb.append("${binding.latteCheckBox.text} ")  
//            }  
//            if (binding.decafCheckBox.isChecked){  
//                sb.append("${binding.decafCheckBox.text} ")  
//            }  
//            sb.append(" are ordered. Thanks!")  
//            showToast(sb.toString())  
//        }  
//    }
```