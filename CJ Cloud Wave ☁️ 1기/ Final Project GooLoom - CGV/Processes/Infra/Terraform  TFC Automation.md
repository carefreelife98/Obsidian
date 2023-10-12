  

# TFC 성공 모습

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.37.58.png]]

  

```
# hpa 테스트$ ab -c 200 -n 200 -t 30 http://$(kubectl get ingress/gooloom-ing -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/contents/aws
```