---
Author: CarefreeLife98
Date: 2023-11-28T19:12:00
Agenda: 
tags:
---
# WordQuiz
```kotlin
package com.example.wordquiz  
  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import android.widget.RadioButton  
import android.widget.Toast  
import androidx.core.view.forEach  
import androidx.core.view.forEachIndexed  
import com.example.wordquiz.databinding.ActivityMainBinding  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
    private val wordBank: List<Word> = listOf<Word>(  
        Word("부주의한, 소홀한", arrayOf("degenerate", "unity", "inadvertent", "array"), 3),  
        Word("쇠약하게 하다", arrayOf("vanity", "underhand", "enslave", "debilitate"), 4),  
        Word("(위험·곤란)제거하다", arrayOf("artisan", "sadistic", "glossy", "obviate"), 4),  
        Word("우아한", arrayOf("prostrate", "delude", "urbane", "renowned"), 3),  
        Word("활기있게 하다", arrayOf("bereave", "enliven", "occult", "besiege"), 2)  
    )  
  
    private var currentIndex = 0  
    private lateinit var radioArray: Array<RadioButton>  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        radioArray = arrayOf<RadioButton>(  
            binding.radioButton1,  
            binding.radioButton2,  
            binding.radioButton3,  
            binding.radioButton4  
        )  
  
        updateQuiz()  
  
        radioArray.forEachIndexed { idx, rb ->  
            rb.setOnCheckedChangeListener { _, isChecked ->  
                if (isChecked) checkAnswer(idx + 1)  
            }  
        }  
        binding.nextButton.setOnClickListener {  
            currentIndex = (currentIndex + 1) % wordBank.size  
            updateQuiz()  
        }  
        binding.prevButton.setOnClickListener {  
            if (currentIndex == 0) {  
                currentIndex = wordBank.size - 1  
            } else {  
                currentIndex -= 1  
            }  
            updateQuiz()  
        }  
    }  
  
    private fun updateQuiz() {  
        binding.wordTextView.text = wordBank[currentIndex].question  
        radioArray.forEachIndexed { idx, rb ->  
            rb.text = wordBank[currentIndex].numbers[idx]  
        }  
    }  
  
    private fun checkAnswer(usrAns: Int) {  
        val correctAns = wordBank[currentIndex].answer  
        val tmp =  
            if (usrAns == correctAns)  
                resources.getString(R.string.right_ans_msg)  
            else  
                resources.getString(R.string.wrong_ans_msg)  
        Toast.makeText(applicationContext, tmp, Toast.LENGTH_SHORT).show()  
    }  
}
```


# WordQuiz (View Model)
```kotlin

```