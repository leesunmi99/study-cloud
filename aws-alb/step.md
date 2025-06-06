# ALB 실습 상세 단계 

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
Edit subnet associations > vpc-01-public-subnet-a, vpc-01-public-subnet-c 추가 > Save assocoations


## Instance 
- Name: vpc-01-public-ec2-c1



## Target group 
- Target type: instances
- Target group name: vpc-01-alb-public-tg
- Protocol-Port: HTTP
- Health check path: /AWS.ALB/healthcheck
