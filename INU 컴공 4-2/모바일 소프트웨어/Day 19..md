---
Author: CarefreeLife98
Date: 
Agenda: 
tags:
---
# Intent - SubActivity
```kotlin
package com.example.intentexample  
  
import android.content.ComponentName  
import android.content.Intent  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import com.example.intentexample.databinding.ActivityMainBinding  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
  
        binding.button.setOnClickListener {  
            val subIntent = Intent(this@MainActivity, SubActivity::class.java)  
//            val cname: ComponentName = ComponentName("com.example.intentexample", "com.example.intentexample.SubActivity")  
//            val subIntent = Intent()  
//            subIntent.component = cname  
            startActivity(subIntent)  
        }  
    }  
}
```

