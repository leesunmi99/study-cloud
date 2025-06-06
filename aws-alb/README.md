# AWS Application Load Balancer 정리


## ELB란 
- 다수의 서비스에 트래픽을 분산 시켜주는 서비스
- Health check: 직접 트래픽을 발생시켜 Instance가 살아있는지 check
- 여러 가용 영역에서 애플리케이션 부하를 처리할 수 있음 
- 지속적으로 IP 주소가 바뀌므로 IP 고정 불가능 : **항상 도메인 기반으로 사용** (NLB는 예외적으로 EIP도 가능)
실무) 화이트리스트 설정을 위해 엔드포인트 IP 주소 알려줘라 -> 우리는 Address 기반이다! 
### ELB의 종류
Application Load Balancer
- 트래픽을 모니터링라여 라우팅 가능
- 주소를 인식함(image.sample.com -> 이미지 서버로)

Network Load Balancer
- TCP 기반 빠른 트래픽 분산 

Classic Load Balancer 
- 현재는 잘 사용하지 않음 

Gateway Load Balancer
먼저 트래픽을 체크함 
가상 어플라이언스 배포, 확장 관리를 위한 서비스 
## ALB의 라우팅 대상 
- Instance
- IP(Private IP Address)
- Lambda
- ALB 


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

> 📘 참고: 
- 쉽게 설명하는 AWS 기초 강좌 13:Elastic Load Balancer(ELB) (https://www.youtube.com/watch?v=mqtUMduyKjk)
- 인프런 강의-스스로 구축하는 AWS 클라우드(노마드 콘텐츠 랩)
