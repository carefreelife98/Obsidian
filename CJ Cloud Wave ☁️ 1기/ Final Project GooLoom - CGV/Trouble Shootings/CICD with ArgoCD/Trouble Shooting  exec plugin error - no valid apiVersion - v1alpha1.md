  

이번 프로젝트를 하며 가장 많이 본 에러 중 하나..

결국은 버전 문제이다.

  

  

# 해결

1. 루트 경로의 .kube/config 를 열어 각 클러스터의 apiVersion을 v1beta1 으로 변경.
    - 뭔가 진행이 더 되긴 하는데 결국 다시 에러.
2. aws eks update-kubeconfig —name (클러스터 이름)
    - update는 되나 에러 해결X
3. **Github 에서 aws cli2 를 사용하면 관련 apiVersion 에러가 발생하지 않는다고 해서 aws cli2 로 업그레이드.**
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_3.59.40.png]]
    
    - 성공!!!! argocd 에 두번째 WAS 클러스터도 추가가 되었다.