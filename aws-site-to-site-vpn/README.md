# 프로젝트 제목
가상의 하이브리드 환경 구축을 위한 AWS Site-to-Site VPN 구성 실습

## 📌 개요
이 프로젝트는 AWS의 Virtual Private Gateway와 Customer Gateway를 활용해 
두 개의 VPC 간 Site-to-Site VPN을 구성한 실습입니다.  


## 🎯 목표
- AWS에서 VPN 연결 개념과 구성 경험
- 라우팅 및 터널 상태 점검 방법 학습

## 🗺️ 아키텍처
![구성도](diagram.png)  <!-- 이미지 넣을 경우 -->

## 🛠️ 사용 서비스 / 도구
- AWS VPC, VPN Gateway, Customer Gateway, Route Table
- Untangle (Firewall/VPN 솔루션)

## ⚙️ 구성 절차
- VPC1 / VPC2 생성
- EC2 생성
- Virtual Private Gateway / Customer Gateway 구성
- Site-to-Site VPN 설정
- Static Route 설정
- 터널 상태 확인 및 Ping 테스트
- Untangle 연동

> 📄 자세한 과정은 `steps.md`에 정리되어 있습니다.

## 🧪 테스트 결과
- VPN 터널 상태: **UP**
- VPC 간 ping 성공 여부: **성공**
- Untangle에서 로그 확인: **정상**

## 🚧 문제 해결 경험
- 터널 연결 실패 → 보안 그룹과 라우팅 테이블 수정
- Untangle과의 IPSec 설정 차이 → 암호화 알고리즘 재조정

## 📝 느낀 점
- 네트워크 설정은 논리 흐름이 핵심이다.
- AWS 콘솔만 클릭한 게 아니라, 구조를 이해하면서 설계해야 VPN이 정상 작동함을 느낌.
