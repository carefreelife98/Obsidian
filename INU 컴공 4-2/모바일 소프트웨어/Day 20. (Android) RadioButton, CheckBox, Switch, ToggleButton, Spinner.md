---
Author: CarefreeLife98
Date: 2023-11-23T13:59:00
Agenda: 
tags:
  - Android
---
# CompoundButton
![[스크린샷 2023-11-23 오후 2.25.00.png]]
`[Android 공식 문서](https://developer.android.com/reference/kotlin/android/widget/CompoundButton)`
> **CompoundButton (복합 버튼) 에서 파생된 Class**
> - 모양은 다르나 **항목의 선택이라는 공통 기능**을 가진다.
> 	- **RadioButton**
> 	- **CheckBox**
> 	- **Switch**
> 	- **ToggleButton**
> <br>
> **CompoundButton 에 속한 View 의 이벤트 처리**
> - **Listener Interface : OnCheckedChangeListener**
> 	![[스크린샷 2023-11-23 오후 2.29.31.png]]
> - **Abstract Method (Callback Method) : onCheckedChanged()**

``
```kotlin
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
        binding.checkBox.setOnCheckedChangeListener ( object : CompoundButton.OnCheckedChangeListener{  
            override fun onCheckedChanged(bt: CompoundButton?, checked : Boolean) {  
                TODO("Not yet implemented")  
            }  
        })  
    }  
}
```
> **onCheckedChanged(bt: CompoundButton?, checked : Boolean)**
> - **bt : CompoundButton Type 객체**
> - **checked : Boolean**
> 	- true = 선택
> 	- false = 해제

<br><br>

# RadioGroup 과 RadioButton
> **RadioButton 은 2 가지 상태가 존재.**
> - Checked
> - UnChecked
> 
> <br>
> RadioButton 은 RadioGroup 에 속한 