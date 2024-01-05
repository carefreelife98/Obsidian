---
Author: CarefreeLife98
Date: 2023-11-30T18:02:00
Agenda: 
tags:
---
# Fragment
> **Fragment 는 Activity 와 모든 기능이 동일하지만 독립적으로 존재하
> 지 못한다는 차이점이 존재.**
> - **Single Activity 내부에 여러 개의 Fragment 를 생성함으로서 다중 화면을 구성할 수 있다.**

## Fragment 의 정적 배치
> **정적 배치는 xml 파일의 Tag 및 Name 속성이 정의**한다.
> ![[스크린샷 2023-11-30 오후 6.18.48.png]]
> - 생성 시 아래와 같이 Kotlin file 및 Layout 이 동시에 생성된다.<br>
> ![[스크린샷 2023-11-30 오후 6.19.52.png]]

<br><br>

### 동반 객체 (Companion Object)
> **동반 객체는 단 하나만 생성된다.**
```kotlin
companion object {  

    @JvmStatic  
    fun newInstance(param1: String, param2: String) =  
        FragmentA().apply {  
            arguments = Bundle().apply {  
                putString(ARG_PARAM1, param1)  
                putString(ARG_PARAM2, param2)  
            }  
        }}
```

> - **Single tone 객체 (정적 메소드).**
> 	- @JvmStatic Annotation 을 통해 선언.

<br><br>

### onCreateView()
```kotlin
class FragmentA : Fragment() {  
    override fun onCreateView(  
        inflater: LayoutInflater, container: ViewGroup?,  
        savedInstanceState: Bundle?  
    ): View? {  
        // Inflate the layout for this fragment  
        return inflater.inflate(R.layout.fragment_a, container, false)  
    }  
}
```

> **`return inflater.inflate(R.layout.fragment_a, container, false)`**
> - 가장 마지막 Parameter 는 container 에 담겨 있는 View 를 Root View 로 사용할 것인지 결정하는 Boolean 값.

<br><br>

### 정적 배치의 특징 (xml)
> ![[스크린샷 2023-11-30 오후 6.38.10.png]]
> - Tag 이름에 FragmentContainerView 가 들어감.
> - name 속성에 Fragment 이름이 지정됨.

<br><br>

# Fragment 간 통신
> Fragment 간의 직접 통신은 불가능.
> - Hosting Activity, Interface 를 거쳐 Fragment 간 통신이 이루어짐.