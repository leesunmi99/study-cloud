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
vpc-01-public-ec2-a1, vpc-01-public-ec2-c1 추가 > Include as pending below



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
