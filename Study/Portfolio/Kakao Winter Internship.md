---
Author: CarefreeLife98
Date: 2023-11-20T17:46:00
Agenda: 
tags:
---
CGV - Fast Order Application

K-Digital Training (CJ Olivenetworks 주관)

AWS Cloud Infra, IaC(Terraform, Terraform Cloud), Container Orchestration(Docker, EKS), Spring Boot 2.7.3, Aurora DB(MySQL)

CGV - Fast Order 프로젝트는 현재 CGV 에서 운영중인 Fast Order 앱의 실제 클라우드 인프라 환경을 기반으로 IaC 를 사용, 더 효율적인 클라우드 인프라를 구축한 후 고객사의 요구사항에 맞추어 Java Spring Boot 를 활용한 Fast Order 앱을 CI/CD Pipeline 위에서 개발, 대규모의 사용자가 동시에 사용해도 문제가 없도록 해당 인프라의 Container Orchestration (EKS) 으로 배포하는 경험이 주 목적입니다.

해당 프로젝트에서 주로 사용한 서비스 및 기술은 다음과 같습니다.

[DNS]

Route53, CloudFront

[보안]

WAF, ACM, ASM, OpenVPN, Site-toSite VPN

[백업]

AWS Backup, On-premise Backup, EKS Backup

[가용성]

Aurora DB 이중화, On-premise 이중화, EKS, Karpenter

[모니터링]

Prometheus, Grafana, Slack, Kubecost

[CICD/자동화]

Terraform, Terraform Cloud, ArgoCD, Github Actions, Docker Hub

[WEB]

Apache2, html, thymeleaf

[WAS]

Java Spring Boot 2.7.3, Tomcat, mod_jk Connector

[DB]

Aurora MySQL, JPA

해당 프로젝트에서 저는 가장 흥미로웠던 다음과 같은 파트에 자원하여 진행하였습니다.

클라우드 인프라를 코드로서 구축할 수 있는 IaC(Terraform), 기존에 흥미를 가지고 진행해왔던 Spring boot 를 활용한 Web Application 개발 및 개발한 서비스가 인프라에서 Container Orchestration System 으로서 배포되어 대규모 트래픽을 고가용성을 가지고 처리할 수 있는 솔루션인 K8s(EKS), 그리고 제가 속한 팀이 더욱 효율적인 작업을 할 수 있도록 CI/CD Pipeline 구축 및 다방면으로서의 자동화를 구축하였습니다.

간략한 과정을 설명드리자면, AWS Cloud Infra 를 구축할 수 있는 Terraform code 를 작성한 후, 해당 Code 를 Git repository 로 Push 하게 되면 Terraform Cloud 에 의해 해당 코드를 기반으로 AWS 에 Cloud Infra 가 자동으로 구축 및 수정이 됩니다. Spring Boot 를 사용하여 개발한 Fast Order Web Application은 3-Tier 구조로 설계하였고, Web 은 Apache2 및 AWS S3 에 저장된 정적 리소스들을 사용, WAS 는 Spring Boot 및 내장 톰캣으로 구현하고 JPA 를 사용하여 Aurora MySQL 과 연결됩니다. 해당 코드들도 Github 에 Push 하면  Github Actions 에 의해 Docker Image 를 빌드하여 DockerHub에 push 되고, 새로운 버전을 ArgoCD 가 EKS Cluster에 무중단 배포하게 됩니다

  

  

  

  

우선 제 지원서를 읽어주셔서 감사합니다. 저는 2024년 02월 졸업 예정에 있는, 나침반을 가진 개발자 채승민 이라고 합니다. 저는 본래 비전공자였지만, 우연히 프로그래밍을 접한 후 제 적성과 흥미에 잘 맞아 현재 졸업을 미루며 컴퓨터 공학 복수 전공을 이수하는 과정에 있습니다. 비록 IT 분야에 발을 들인지 1년도 채 되지 않았고, 나이에 비해 남들보다 조금 늦었을 수도 있지만 지금 이 분야에 몸 담기 위해 학습하며 경험하고 있는 저는 그 누구보다 즐겁고 행복하다고 자부합니다. 이에 온갖 풍랑을 겪고 있지만 방향을 잡고 보물을 찾으러 가는 과정이기에 파도가 즐거운, 나침반을 가진 개발자라는 제 소개를 드렸습니다. 제가 이 길로 접어들 수 있었던 첫 번째 계기는 Java 기반 Spring Framework 였습니다. 어릴 적부터 무언가를 만들고 발명하는 것을 좋아했었던 제가 Spring Framework 인터넷 강의를 들으며 이 길로 가야겠다는 확신이 들었습니다. Java 조차 모르는 상태에서 재미있다는 이유 하나로 Spring 을 공부하며 Java 를 익히고 그 과정에서 Backend Server 개발자가 되겠다는 목표를 설정하게 되었고 이는 지금도 제 삶의 최종 목표에 도달하기 위한 과정 중 하나의 목표로서 유효하여 실제 현업 환경을 경험해보고 싶다는 생각에 이번 카카오 동계 인턴 Server 포지션에 지원하였습니다. 

복수 전공 이수 과정 중 더 많은 경험을 원했기에 여러 교육 과정 및 많은 교내, 교외 동아리에 지원했지만 번번히 떨어졌었습니다. 하지만 계속된 도전 끝에 탈락 할 것이라 생각되었던 CJ Olivenetworks 에서 주관하는 클라우드 및 K8s 교육에 테스트 및 면접 합격 후 참여할 수 있게 되었고, 해당 교육은 약 2개월에 걸쳐 network, linux 와 같은 기초 과목 부터 Docker, K8s, Public Cloud, 자동화 까지 배우는 과정이었습니다. 이는 제가 줄곧 해오던 Backend 개발과는 다소 거리가 있는 교육이었습니다. 하지만 ChatGPT 와 같은 자연어 처리 모델이 사회적 이슈를 사던 가운데, 이 분야에서 경쟁력을 가지려면 트렌디한 기술을 많이 사용해보며 더 배우고 더 큰 경험을 해봐야 한다는 생각에 최선을 다해서 지식과 경험을 얻어냈고, 그 과정의 끝에서 진행했던 짧지만 강렬했던 한 프로젝트가 제 나침반이 되어주었습니다.

해당 프로젝트는 CGV 에서 실제로 운영중인 Cloud infra 설계를 바탕으로 비용 및 성능적으로 더 효율적이고 창의적인 Cloud Infra를 설계한 후 고객사의 요구사항을 충족하는 Fast Order 서비스를 자유롭게 개발하여 배포하되 대규모 사용자가 동시에 사용해도 무리가 가지 않는 고가용성을 가진 환경을 구축하는 것이었습니다. 이전 공통 프로젝트에서 팀원 간 소통 및 의견 제시 과정이 원활하지 않아 아쉬운 결과가 나왔었고, 이에 소통 및 협업 툴을 단일화 할 필요가 있었습니다. 따라서 소통의 장을 Slack 으로 선정하여 모든 팀원이 같은 곳에서 의견을 나눌 수 있었고 프로젝트의 시작점이 달라지는 것을 보며 협업 초기에 아주 작은 것부터 기준을 잡고 체계화 해가는 것이 중요하다는 것을 깨달았습니다. 해당 경험에 기반하여 각 팀원의 역할을 각자 의견에 맞추어 배정하였고, 제 개인 역할의 수행 역량에 영향이 가지 않는 선에서 팀원에게 도움을 줄 수 있는 부분을 찾아 도움을 주며 같이 성장하는 선순환 구조가 생겨났습니다. 그 결과로 프로젝트를 대표하는 발표자로 선정이 되었고 함께 성장해온 팀 내 문화 덕분에 제 역할이 아니었던 파트에 대한 발표 준비도 원활하게 할 수 있었으며, 실제로 해당 파트를 쉽게 이해할 수 있었습니다.

작은 기준 및 체계 구축으로부터 이와 같은 팀 내 분위기와 환경이 조성되면서 자연스럽게 저 역시 더욱 정성을 다해 프로젝트를 진행 할 수 있었고, 그에 기반하여 CI/CD Pipeline 의 존재를 모르던 제가 자의적으로 해당 부분 학습을 하여 팀과 제 자신을 위해 구축하고 실 적용까지 하게 되었습니다. 

위 프로젝트를 진행하며 제일 크게 깨달은 점은 기술적인 부분도 아니며 교육 및 프로젝트의 결과도 아닙니다. 프로젝트를 시작하기 전, 가장 기초적인 Public Cloud 도 많이 사용해보지 않았고, Container Orchestration 및 IaC, CI/CD 등의 개념도 잘 몰랐지만 학습의 부족은 학습으로 채울 수 있다는 것을 느꼈습니다. 하지만 추후 실제 프로젝트 투입 시에 협업하는 과정에서 사람과 사람 사이의 원활한 소통의 부족으로 발생하는 문제는 프로젝트가 진행될수록 돌이키기 어렵다는 것을 크게 느꼈고 이를 방지하기 위한 특정 기준이나 틀을 제시할 수 있는 능력이 필요함을 느꼈습니다.

위 경험에 빗대어 앞서 말씀드린 협업 능력에 이번 카카오 동계 인턴을 통해 배움과 경험의 깊이까지 더해진다면 추후 제 삶의 최종 목표인 DevOps 라는 보물을 찾을 수 있을 것 같아 나침반을 가지고 찾아왔습니다. 감사합니다.

java / kotlin spring - 2, python - 2