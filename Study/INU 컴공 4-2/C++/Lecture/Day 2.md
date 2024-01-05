  

## Stack VS Heap

- Stack
- Heap

  

## [중요] References

- 하나의 자료형 당 두 개의 추가적인 자료형이 존재
    - int (Origin 자료형)
    - int * (Pointer)
    - int & (Reference)

  

## BlockChain Trilemma → 이론적으로 세 가지 요소를 전부 충족할 수 없다.

블록체인 삼각포석 (Blockchain Trilemma)은 블록체인 기술의 세 가지 주요 요소 간의 균형을 나타내는 개념입니다. 이 세 가지 주요 요소는 다음과 같습니다:

1. 보안 (Security): 블록체인은 안전하고 변경 불가능한 데이터 저장을 제공해야 합니다. 이것은 해킹, 공격 또는 데이터 변조로부터 보호하는 것을 의미합니다.
2. 속도 (Scalability): 블록체인은 높은 트랜잭션 처리량을 지원하고 실시간 거래에 대한 빠른 응답 시간을 제공해야 합니다. 블록체인이 규모가 확장되면서도 성능을 유지해야 합니다.
3. 분산 (Decentralization): 분산은 중앙 중심의 제어를 피하고 블록체인 네트워크가 중앙화되지 않도록 하는 것을 의미합니다. 블록체인은 여러 참여자 간에 균형을 유지해야 합니다.

블록체인 트라이레마는 이 세 가지 요소 간의 상충 관계를 강조합니다. **한 요소를 향상시키면 다른 요소에 영향을 미치는 경우가 발생**할 수 있으며, 이로 인해 블록체인 개발자들은 적절한 균형을 찾기 위해 노력합니다. 예를 들어, 더 높은 보안을 달성하기 위해 블록 크기를 증가시키면 분산성이 감소할 수 있습니다.

블록체인 트라이레마를 극복하기 위한 다양한 기술적 솔루션과 접근 방식이 연구되고 있으며, 이를 통해 블록체인 기술의 성능과 안전성을 지속적으로 향상시키고 있습니다.

  

### → 어느 정도의 Requirement 는 적당히 충족하여 균형적인 개발을 할 것.

  

  

## Constants and Typedef

Const : 상수 (변경 불가능 - Immutable)

  

## C (절차 지향) / 객체 지향

- 차이 → 변수에 접근 제어를 할 수 있다. (Public / Private)

  

## Functions

- Function 의 Signature or Prototype 을 분석하기 위한 세 가지 중점
    - Return Type
    - Function Name
    - Argument List

  

## Memo

- C++ Function 의 argument 부분을 보면 Reference Type 앞에 const 가 붙어있는 것을 같이 볼 수 있다.
    
    - 가독성을 위해서.
    - Reference로 내가 참조하고 있는 값은 절대 값이 바뀌지 않는다는 것을 빠르게 알 수 있음.
    - 해당 값에 대해서 Write는 절대 실행하지 않고 Read만 실행하겠다는 것.
    
      
    

## Function Overloading

- Overloading 은 `다형성`과 연관이 있다.

  

## Operator Overloading