---
Author: CarefreeLife98
Date: 2023-11-14T18:07:00
Agenda: 
tags:
  - Android
---
> **setPositiveButton / setNegativeButton**
> - **[중요] which 는 무엇인가?**
> 	- 별 의미가 없는 Parameter 이다 ㅋㅋ;

> **모든 Pop up 창을 띄우기 위해서는 항상 마지막에 show() 붙히기.**

<br><br>

```kotlin
val builder = MaterialAlertDialogBuilder(this@MainActivity)
```
> - **Context Parameter 정확하게 이해하기.**
> - 무명 클래스 에서 this 사용 시 Error.


```kotlin
setCancelable(false) // 뒤로 가기 버튼 사용 불가능.
```


# Single Choice Items
```kotlin
package com.example.alertdialog  
  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import android.view.View  
import com.example.alertdialog.databinding.ActivityMainBinding  
import com.google.android.material.dialog.MaterialAlertDialogBuilder  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        val items = resources.getStringArray(R.array.colors)  
        var mSelect = -1  
  
        binding.callButton.setOnClickListener {  
            MaterialAlertDialogBuilder(this@MainActivity)  
                .apply { // scope function, this. 생략 가능  
                    setTitle("색상 선택")  
                    setIcon(R.drawable.ic_launcher)  
                    setCancelable(false) // 뒤로 가기 버튼 사용 불가능.  
                    setSingleChoiceItems(items, -1) { _, which -> mSelect = which }  
                    setPositiveButton("SELECT") { _, _ ->  
                        binding.colorTextView.apply {  
                            visibility = View.VISIBLE  
                            text = getString(R.string.color_selected, items[mSelect])  
                        }  
                    }                    setNegativeButton("CANCEL") { _, _ ->  
                        mSelect = -1  
  
                        binding.colorTextView.apply {  
                            visibility = View.INVISIBLE  
                            text = ""  
                        }  
                    }                    show()  
            }  
        }    }  
}
```

# 다중 항목 선택

```kotlin
package com.example.alertdialog  
  
import android.content.DialogInterface  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import android.view.View  
import com.example.alertdialog.databinding.ActivityMainBinding  
import com.google.android.material.dialog.MaterialAlertDialogBuilder  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        val items = resources.getStringArray(R.array.colors)  
        var statusArr = Array(items.size) { false }  
  
        binding.callButton.setOnClickListener {  
            MaterialAlertDialogBuilder(this@MainActivity)  
                .apply { // scope function, this. 생략 가능  
                    setTitle("색상 선택")  
                    setIcon(R.drawable.ic_launcher)  
                    setCancelable(false) // 뒤로 가기 버튼 사용 불가능.  
  
                    setMultiChoiceItems(items, null) {  
                            _, which, isChecked -> statusArr[which] = isChecked  
                    }  
                    setPositiveButton("SELECT") { _, _ ->  
                        var s = ""  
                        if (statusArr.none { it }) {  
                            s += "아무것도 선택하지 않았습니다."  
                        } else {  
                            var tmp = ""  
                            statusArr.forEachIndexed { index, b ->  
                                if (b) tmp += "${items[index]} "  
                            }  
                            s += getString(R.string.color_selected, tmp)  
                        }  
                        binding.colorTextView.apply {  
                            visibility = View.VISIBLE  
                            text = s  
                        }  
                    }                    setNegativeButton("CANCEL") { _, _ ->  
                                            }  
                    show()  
            }  
        }    }  
}
```

