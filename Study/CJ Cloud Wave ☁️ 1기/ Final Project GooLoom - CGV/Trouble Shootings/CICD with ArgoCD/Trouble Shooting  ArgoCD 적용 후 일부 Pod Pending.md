  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.11.34.png]]

- 일부 Pod 들이 계속 Pending 상태 유지.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.17.03.png]]

```
kubectl describe po (pod 이름)
```

- `kubectl describe po (pod 이름)` 해보니 cpu 사용량이 많아 node가 부족해져 preemtion이 걸린 것.

  

## 해결

> 생성된 pod 가 차지하는 resource에 비해 노드의 자원량이 적어 추가적인 CA 를 통해 노드를 늘려주어야 한다 생각.

  

- Karpenter 를 사용해서 노드를 Autoscale 하기로 했다.

  

## Karpenter 설치

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.36.49.png]]

- Karpenter 설치 후 pending 상태이던 Pod 들이 Running 시작.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.37.49.png]]

- ArgoCD 에서의 Health Check 도 성공 상태로 바뀜.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.39.34.png]]

- 동시에 Karpenter 가 자원 확보를 위해 생성한 두 개의 노드도 확인됨.