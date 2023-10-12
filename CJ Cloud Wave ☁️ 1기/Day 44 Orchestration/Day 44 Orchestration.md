  

# [CloudFormation]

  

## 장점

- 인프라를 프로그램처럼 사용 가능
- 빠르게 생성, 관리 및 삭제가 가능
- 사용한 만큼만 요금을 지불

  

## 개요

- 상태를 기재한 설정 파일(템플릿) 을 바탕으로 AWS 구축을 자동화 할 수 있는 서비스.
- 템플릿에는 리소스 설정 정보를 JSON이나 YAML 형식으로 기술한다.
- CLoudFormation은 작성한 리소스 집합체를 **스택** 이라는 단위로 관리한다.
- 추가 요금 없음. (프로비져닝 되었다) AWS 자원 요금만 발생.

  

## Infrastructure as Code (IaC)

- 인프라를 코드로 정의하고 운용하는 접근법.

  

## CloudFormation 용어

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.31.12.png]]

  

  

실습

1. CloudFormation 스택 생성 - 샘플 템플릿
    1. URL 이동 및 YAML 파일 복사
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.43.57.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.45.11.png]]
        
          
        
    2. 스택 이름 및 파라미터 지정
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.46.26.png]]
        
        - True 시 Master / StandBy DB 구성 가능
        
          
        
          
        
          
        

## AWS CLI로 CloudFormation Stack 생성하기

  

  

  

  

## WAF Work Shop → Juice Shop

1. URL 접속 후 US East Virginia에 CloudFormation Stack 생성
    
    > [!info] Workshop Studio  
    > Discover and participate in AWS workshops and GameDays  
    > [https://catalog.us-east-1.prod.workshops.aws/workshops/c2f03000-cf61-42a6-8e62-9eaf04907417/en-US/00-introduction](https://catalog.us-east-1.prod.workshops.aws/workshops/c2f03000-cf61-42a6-8e62-9eaf04907417/en-US/00-introduction)  
    
      
    
2. WebApp Stack 출력에 나타나는 URL로 Juice page 접근 가능
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.27.28.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.27.35.png]]
    
      
    
3. AWS WAF deployment 에서 가장 중요한 리소스는 [web ACL (Web Access Control List)](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) 이다.
    1. WAF를 실행하는 것에 있어 가장 빠른 방법은 [AWS Managed Rule Group for AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-list.html)를 WebACL에 적용하는 것.
        - [AWS Managed Rule Group for AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-list.html)
            - 일반적인 공격 (SQL, Command Line Attacks)에 대한 방어를 제공하는 Rule의 집합.
            - 아래와 같은 rule group을 제공한다.
                - _Amazon IP Reputation list_
                - _Known Bad Inputs_
                - _Core rule set_.
4. 문제 1
    
    - `_You are the sole developer for the start up Juice Shop. Your website is a simple web application backed by a SQL Database._`
    - `_For some reason, a group of Milkshake bandits have started attacking your site!_`
    - `_Luckily, you recently attended a workshop on AWS WAF. You decide to implement your own WAF to protect your site._`
    
      
    
    ## 해결하기
    
    1. **WAF 생성 및 Managed Rules 적용**
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.40.32.png]]
        
        - Resource Type은 CloudFront로 설정.
        
          
        
        **Add AWS resources**
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.44.28.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.45.44.png]]
        
          
        
        **Add Managed Rule Groups**
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.48.38.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.50.36.png]]
        
        - Core Rule Set 활성화
            - 넓은 범위의 웹 애플리케이션 보안 취약점을 방어
        
          
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.51.58.png]]
        
        - SQL database 활성화
            - SQL Injection 과 같은 비허가된 쿼리를 삽입하는 공격을 방어
        
          
        
        이후 아래와 같은 명령어를 사용
        
        ```
        $ export JUICESHOP_URL=<Your Juice Shop URL>
        ```
        
          
        
        ```
        # This imitates a Cross Site Scripting attack# This request should be blocked.$ curl -X POST  $JUICESHOP_URL -F "user='<script><alert>Hello></alert></script>'"
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.13.30.png]]
        
          
        
        ```
        # This imitates a SQL Injection attack# This request should be blocked.curl -X POST $JUICESHOP_URL -F "user='AND 1=1;"
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.14.22.png]]
        
          
        
        만약 Rule이 정상적으로 작동된다면 아래와 같은 HTML Response를 받을 것이다.
        
        ```
        <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"><html>  <head>    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />    <title>ERROR: The request could not be satisfied</title>  </head>  <body>    <h1>403 ERROR</h1>    <!-- Omitted -->  </body></html>
        ```
        
          
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.17.50.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.18.56.png]]
        
        - Rule을 삭제한 후 해당 공격성 명령어가 작동되는 것을 볼 수 있다.
        
          
        
    2. **Custom Rules 생성**
        
        WAF Console에서