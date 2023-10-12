  

## 문제 해결

- eksctl create 중 `Error: checking AWS STS access – cannot get role ARN for current session:` 에러 발생
    
    ```
    eksctl create cluster -f gooloom-Cluster-gen.yaml --profile carefreelife98
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.03.56.png]]
    
    - IAM User 에서 Access key 발급 받은 후 생성한 Profile 을 명령어에 추가하여 해결.