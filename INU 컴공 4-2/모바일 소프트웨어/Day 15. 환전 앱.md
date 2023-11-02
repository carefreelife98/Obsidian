---
Author: CarefreeLife98
Date: 2023-11-02T18:13:00
Agenda: 
tags:
  - Android
---
# 환전 Application (2)
> res -> values -> strings.xml
> - **위에 명시된 string 을 가져올 때에는 getString(R.string. ~) 을 사용한다.**

## imeOptions
![[스크린샷 2023-11-02 오후 6.38.58.png]]
> **IME (Input Method Editor)**
> - Application 내에서 입력을 위해 사용하는 Soft Keyboard 의 Enter 기능을 원하는 동작을 수행하도록 변환하는 것.
> - **`.setOnEditorActionListener()`**
> 	- Soft Keyboard 의 Enter 입력 처리 리스너.
> 	- 무명 클래스 object 사용하여 onEditorAction 함수를 override 후 사용.
> 		![[스크린샷 2023-11-02 오후 6.53.08.png]]

**전체 코드**
```kotlin
package com.example.exchange  
  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import android.view.KeyEvent  
import android.view.LayoutInflater  
import android.view.inputmethod.EditorInfo  
import android.widget.EditText  
import android.widget.TextView  
import android.widget.TextView.OnEditorActionListener  
import android.widget.Toast  
import com.example.exchange.databinding.ActivityMainBinding  
  
class MainActivity : AppCompatActivity() {  
    private var rate = ""  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        rate = getString(R.string.exchange_rate)  
        binding.currentRateTextView.text = rate  
  
        // Soft Keyboard 의 Enter 키 기능 Customizing        binding.wonEditText.setOnEditorActionListener( object : OnEditorActionListener {  
            override fun onEditorAction(v: TextView?, actionId: Int, event: KeyEvent?): Boolean {  
                if (actionId == EditorInfo.IME_ACTION_DONE) {  
                    doExchangeCurrency((v as EditText).text.toString())  
                    return true  
                }  
                return false  
            }  
        })  
  
        binding.convertButton.setOnClickListener {  
            val wonValue: String = binding.wonEditText.text.trim().toString()  
            doExchangeCurrency(wonValue)  
        }  
    }  
  
    private fun doExchangeCurrency(valueStr: String?): Unit {  
        if (valueStr == null) return  
        if (valueStr.isNotBlank()) {  
            val usd: Double = valueStr.toDouble() / rate.toDouble()  
            binding.amountTextView.text = "%.1f".format(usd)  
        } else {  
            Toast.makeText(applicationContext, "금액을 입력하세요",  
                Toast.LENGTH_LONG).show()  
        }  
    }  
}
```

<br><br>
## EditText 를 TextInputLayout 으로 변환하기

## Dimenstion Resource