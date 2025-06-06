# AWS Application Load Balancer 정리

## 1. 아키텍처

- **트래픽 분산 네트워크 환경 구성**
- **경로 기반 라우팅 (Path-based Routing)**  
  : 요청 경로나 조건에 따라 트래픽을 서로 다른 대상 그룹(Target Group)으로 분산
- **고정 세션 라우팅 (Sticky Session Routing)**  
  : 일정 시간 동안 동일한 사용자의 요청을 동일한 서버로 전달

---

## 2. Application Load Balancer (ALB)

- **Elastic Load Balancer(ELB)**의 한 종류
- 주로 **HTTP/HTTPS 트래픽**을 처리
- **웹 어플리케이션**에 최적화된 Layer 7 로드 밸런서
- 다양한 기능 지원:
  - 경로 기반 라우팅
  - 호스트 기반 라우팅
  - WebSocket 지원
  - 인증 및 SSL 종료(Offloading)

---

## ✅ 로드 밸런서를 사용하는 이유

- 로드 밸런서 없이도 웹 서비스는 동작 가능
- 하지만 트래픽이 증가하면:
  - 특정 서버에 과부하
  - 성능 저하 및 장애 발생 가능성 증가

➡ **로드 밸런서를 도입하면** 트래픽을 여러 서버로 분산시켜 **안정적인 서비스 제공** 가능

---

## 🌐 Elastic Load Balancing(ELB) 특징

- 여러 **가용 영역(AZ)**에 걸쳐 대상(Target)으로 트래픽을 자동 분산
- **고가용성(High Availability)**과 **내결함성(Fault Tolerance)** 보장
- EC2 인스턴스, 컨테이너, Lambda 등을 대상 그룹으로 사용 가능

---

> 📘 참고: 더 많은 AWS 실습 자료는 [gptonline.ai/ko](https://gptonline.ai/ko/)에서 확인하실 수 있습니다.
