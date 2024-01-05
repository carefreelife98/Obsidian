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
> **RadioButton 은 주로 RadioGroup 에 속한 상태로서 사용.**
> - RadioGroup 에 속한 여러 RadioButton 은 상호 배타적인 상태를 유지.
> - **하나의 RadioGroup 에서 하나의 RadioButton 만 선택 가능.**
> 
> <br>
> **RadioButton 은 TextView 를 상속한다.**
> - RadioButton 의 생김새는 TextView 의 클래스 속성인 font face, style, color 등을 사용하여 조작할 수 있다.