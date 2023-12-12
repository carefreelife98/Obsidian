---
Author: CarefreeLife98
Date: 2023-12-12T15:41:00
Agenda: 
tags:
---
# View Model
## ViewModel 생성
```kotlin
class MainActivity : AppCompatActivity() {
	...
	// ViewModel 을 사용하기 위한 코드. 기계적 Typing 필요.  
	private val wordViewModel: WordViewModel by lazy {  
	    ViewModelProvider(this).get(WordViewModel::class.java)  
	}
	...
}
```

## ViewModel 정의
```kotlin
package com.example.wordquiz  
  
import androidx.lifecycle.ViewModel  
  
class WordDictViewModel : ViewModel(){
	var curIndexDict = 0  
	
	// Data Class 를 통한 Model 정의
	private val wordDictBank = listOf<WordDict>(  
	    WordDict("degenerate", "퇴화(퇴보)하다"),  
	    WordDict("vanity", "덧없음, 무상함, 공허"),  
	    WordDict("artisan", "기능공"),  
	    WordDict("prostrate", "넘어뜨리다, 항복시키다"),  
	    WordDict("bereave", "(생명, 희망을)빼앗다")  
	)
	
	// getter 생성 - MainActivity 등 다른 Class 에서 사용할 수 있게 된다.
	val curWord1 get() = wordDictBank[curIndexDict].word1  
	val curWord1Meaning get() = wordDictBank[curIndexDict].word1meaning  
	val curWord2 get() = wordDictBank[curIndexDict].word2  
	val curWord2Meaning get() = wordDictBank[curIndexDict].word2meaning  
	val curWord3 get() = wordDictBank[curIndexDict].word3  
	val curWord3Meaning get() = wordDictBank[curIndexDict].word3meaning  
	val curWord4 get() = wordDictBank[curIndexDict].word4  
	val curWord4Meaning get() = wordDictBank[curIndexDict].word4meaning

}
```

## Data Class
> **Data Class 로서 DTO 를 정의**하고 ViewModel 에서 꺼내쓰는 느낌

```kotlin
package com.example.wordquiz  
  
data class WordDict(  
    val word1: String,  
    val word1meaning: String,  
    val word2: String,  
    val word2meaning: String,  
    val word3: String,  
    val word3meaning: String,  
    val word4: String,  
    val word4meaning: String  
)
```

## Modular 순환
```kotlin
// currentIndex 변경 로직  
fun moveToNext() {  
    currentIndex = (currentIndex + 1) % wordBank.size  
}  
  
fun moveToPrevious() {  
    if (currentIndex == 0) {  
        currentIndex = wordBank.size - 1  
    } else {  
        currentIndex -= 1  
    }  
}
```

# Fragment

## Fragment 파일 생성
> Fragment 도 Binding 이 가능함.
```kotlin
class FragmentA : Fragment() {  
    private lateinit var binding: FragmentABinding
```

## MainActivity - Fragment 동적 초기화
> 앱 실행 시 첫 Fragment 가 보여야 하는 경우 초기화 하는 방법

```kotlin
// MainActivity
override fun onCreate(savedInstanceState: Bundle?) {  
    super.onCreate(savedInstanceState)  
    binding = ActivityMainBinding.inflate(layoutInflater)  
    setContentView(binding.root)  
  
    val fr: Fragment? = supportFragmentManager.findFragmentById(R.id.fragment_container)  
    
    // 어떠한 Fragment 가 배치되어 있지 않으면 FragmentA 를 배치  
    if (fr == null) {  
        supportFragmentManager.beginTransaction().apply { 
	        // transaction 객체 생성 (Single tone) 
            this.add(R.id.fragment_container, FragmentA())            
            this.addToBackStack(null)
            this.commit()  
        }  
    }
```

## Fragment 간의 통신
> **Hosting Acvtivity (Main Activity 를 거쳐 이루어짐. 직접 x)**


### 3단계 구현 과정 (Interface)
- 1단계: FragmentA가 인터페이스(MyListener) 정의
	- 추상 메소드(onMethodA) 선언
	- FragmentA의 뷰에 이벤트 발생  
		- Hosting Activity에서 구현한 메소드(onMethodA( ) ) 호출 
- 2단계: FragmentB가 FragmentA가 요구하는 기능을 메소드 (methodB)로 구현     
- 3단계: Hosting Activity는 FragmentA가 정의한 인터페이스 상속 • 추상메소드구현  
	- FragmentB의 메소드(methodB() ) 호출