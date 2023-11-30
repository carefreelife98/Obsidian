---
Author: CarefreeLife98
Date: 2023-11-30T18:12:00
Agenda: 
tags:
  - ToyProject
---
# \[Toy Project] Carefree OCR
> **어머니께서 Excel 을 사용하여 일하시는데 책자 내부에 존재하는 Data 들을 일일히 손으로 Typing 하여 Excel 에 옮기시는 모습이 힘들어 보였다.**
> - **이에 이번 토이 프로젝트 - [Carefree OCR] 을 진행하게 되었다.**

## Main Idea
> **휴대폰의 Camera 를 사용하여 찍은 책자 이미지를 OCR 기술을 이용해서 Web 화면에 띄어준다면 해당 Text 만 복사하여 어머니께서 잘 사용하실 수 있을 것.**
> <br>
> **\[Tools]**
> - **Naver Cloud Platform - Naver Clova OCR API**
> 	- 부분 유료 / API 호출 당 지불
> - **AWS EC2 - t2.micro / Amazon linux 2**
> 	- Apache 2, Tomcat
> - **Github Actions**
> 	- Workflow File
> 	- Github Secrets
> - **Spring Boot 3.1.5**
> 	- Dependency
> 		- implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'  
> 		- implementation 'org.springframework.boot:spring-boot-starter-web'  
> 		- implementation("com.googlecode.json-simple:json-simple:1.1.1")  
> 		- compileOnly 'org.projectlombok:lombok'  
> 		- annotationProcessor 'org.projectlombok:lombok'  
> 		- testImplementation 'org.springframework.boot:spring-boot-starter-test'