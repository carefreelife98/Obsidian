Bastion에서 EKS 클러스터 접속 시도 시 에러 내용

```
Unable to connect to the server: dial tcp: lookup FD981C5BDE01D3672C2EB218B9DE462.gr7.ap-northeast-2.eks.amazonaws.com on 10.0.0.2:53: no such host
```

- 해결 방법:
    
    1. `kubectl edit -n kube-system configmap/aws-auth`
        - 위 파일의 user? 에 master 권한 부분에 자신이 이용하는 IAM 사용자의 ARN을 등록해준다.
    
      
    
    1. `aws eks --region [EKS_Region] update-kubeconfig --name [EKS_Cluster_Name]`
        - 위 명령어를 실행하여 Context를 Update 해준다.