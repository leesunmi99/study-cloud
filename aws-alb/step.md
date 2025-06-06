# ALB 실습 상세 단계 
> 📚 본 문서는 인프런 강의 **‘스스로 구축하는 AWS 클라우드’**의 실습 내용을 기반으로  
> 직접 따라하며 수행한 결과를 정리한 것입니다. 

## VPC 생성 
- Name: vpc-01
- CIDR: 10.1.0.0/16

## Subnet 생성 
- Name: vpc-01-public-subnet-c
- Availability Zone: ap-northeast-2c
- VPC CIDR block: 10.1.0.0/16
- subnet CIDR block: 10.1.2.0/24


## Route tables 
- Name: vpc-01-public-subnet-rt
- Edit subnet associations > vpc-01-public-subnet-a, vpc-01-public-subnet-c 추가 > Save assocoations


## Instance 
- Name: vpc-01-public-ec2-c1

## Security group
- Name: vpc-01-alb-public-sg
- Description: security group for vpc-01-alb-public
- VPC: vpc-01
- Inbound rules
  - HTTP: 0.0.0.0/0 : allow http from everywhere

## Target group 
- Step 1 ) Specify group details
- Target type: instances
- Target group name: vpc-01-alb-public-tg
- Protocol-Port: HTTP
- Health check path: /AWS.ALB/healthcheck
- Step 2 ) Register targets
- vpc-01-public-ec2-a1, vpc-01-public-ec2-c1 추가 > Include as pending below



## ALB 생성 
- Name: vpc-01-alb-public
- Scheme: Internet-facing (외부에서 들어오는 트래픽 분산)
- Network mapping
  - VPC: vpc-01
  - Available Zones and subnets:
    - ap-northeast-2a: vpc-01-public-subnet-a
    - ap-northeast-2c: vpc-01-public-subnet-c
- Security group: vpc-01-alb-public-sg
- Listeners and routing
  - HTTP:80:vpc-01-alb-public-tg


 ## +) 경로 기반 라우팅
- Target group
   - vpc-01-alb-public-a1-tg
   - vpc-01
   - Health check path: /a1/index.php
   - Register target: vpc-01-public-ec2-a1
- Load balancer > Manage rules > Edit rules > Add rule
  - Name: Path-a1
  - Add condition-Path: /a1*
  - Define rule actions
    - Target group: vpc-01-alb-public-al-tg
  - Set rule priority
    - Path-a1: Priotiry: 100
    - Path-c1: Priority: 200
 ## +) 고정 세션 라우팅
 - Target group > Attributes > Edit > Target selection configuration
 - Stickiness
   - [check] Turn on stickiness
   - Stickness duration: 3
   - Unit of time: seconds

---
# 실습 결과 확인 
## ALB 
![image](https://github.com/user-attachments/assets/6852d619-c4d3-497c-8543-42dc545df252)

![image](https://github.com/user-attachments/assets/85aa4845-d892-4ddd-a859-d441516db09b)
![image](https://github.com/user-attachments/assets/ed1b05b4-7c75-413d-916e-8eb49866147e)

- 트래픽이 분산되어 새로고침 할 때마다 화면이 바뀐다.

## ALB 경로 기반 라우팅 
![image](https://github.com/user-attachments/assets/a007733b-27fc-425e-af06-568bef2bc437)
![image](https://github.com/user-attachments/assets/47b907a9-ce99-4ea6-b925-1801f763b327)

## ALB 고정 세션 라우팅
- 새로 고침 버튼을 눌러도 3초간 세션이 고정되어 화면이 바뀌지 않는다.


---
# ❌ 오류 정리
## 502 Bad Gateway
- 문제
- Instance의 User date가 제대로 입력되지 않아서 php 서버가 실행되지 않은 상태였음 
- 그래서 새로 고침을 할 때마다 php 서버가 실행되지 않은 인스턴스에 접근하면 이 오류가 뜸 
- 해결
- php 서버를 다시 실행시켜줌
- 방지
- Auto Scaling Group에서 MyASG의 ELB도 선택해주었다. 
=> 고가용성 확보
---
# 📝 배운 점 
- web 서버가 죽었을 때 ELB 입장에선 unhealthy로 판단하지만, auto scailing group 입장에선 판단하지 못한다.
- 서비스가 다운되는 다양한 경우를 미리 예측하고 행동하는 것이 중요하다는 것을 깨달았다. 
