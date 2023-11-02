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