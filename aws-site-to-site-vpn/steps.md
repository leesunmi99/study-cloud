# 📝 Site-to-Site VPN 실습 상세 단계

## ✅ 1. VPC 생성

### VPC A (Seoul Region)
- Name: vpc-01
- CIDR: 10.1.0.0/16
- 서브넷: 10.0.1.0/24 (public)
- 인터넷 게이트웨이 연결: vpc-01-igw
- Enable DNS hostnames 

### VPC B (Tokyo Region)
- Name: vpc-04
- CIDR: 10.4.0.0/16
- 서브넷: 192.168.1.0/24 (public)
- 인터넷 게이트웨이 연결: vpc-04-igw
- Enable DNS hostnames 

---
## ✅ 2. Internet Gateway 생성 및 연결 
### IGW (Seoul Region)
- Name: vpc-01-igw

### IGW (Tokyo Region)
- Name: vpc-04-igw

---
## ✅ 3. Subnet 생성 
### Subnet (Seoul Region)
- Name: vpc-01-public-subnet-a
- CIDR: 10.1.1.0/24

### Subnet (Tokyo Region) 
- Name: vpc-04-public-subnet-a
- CIDR: 10.4.1.0/24

---
## ✅ Security Group 생성 및 수정
### SG (Seoul Region)
- Name: vpc-01-private-ec2-sg


### SG (Tokyo Region)
- Name: vpc-04-public-ec2-a1




---

## ✅ 4. EC2 인스턴스 생성
### EC2 (Seoul Region)
- Name: vpc-01-public-ec2-a1
- AMI: Amazon Linux 2
- Instance type: t2.micro
- Key pair: ec2-public-seoul
  - Key pair type: RSA
  - Private key file format: .pem
- Security Group:
  - Create security Group
    - Name: vpc-01-public-ec2-sg
    - Description: security group for vpc-01-public-ec2
- Advanced details
  - Metadata version: V1 and V2 (token optional)
  - Allow tags in metadata: Enable

- User data
```bash
#!/bin/bash
dnf update -y
dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
dnf install -y mariadb105-server
systemctl start httpd
systemctl enable httpd
usermod -a -G apache ec2-user
chown -R ec2-user:apache /var/www
chmod 2775 /var/www
find /var/www -type d -exec chmod 2775 {} \;
find /var/www -type f -exec chmod 0664 {} \;
```

### EC2 (Tokyo Region)
- Name: vpc-04-public-ec2-a1
- Security Group:
  - Create security Group
    - Name: vpc-04-public-ec2-sg
    - Description: security group for vpc-04-public-ec2




---
## ✅ 5. Elastic IP 생성 및 할당 
- Name: eip-vpc-01-public-ec2-a
Actions > Associate Elastic IP address > Instance(vpc-01-public-ec2-a1)

--- 
## ✅ 6. Route Table 생성 
### RT Seoul
- Name: vpc-01-public-subnet-rt
- VPC: vpc-01
Subnet associations > Edit subnet associations > vpc-01-subnet-a
Routes > Edit routes > Destination 0.0.0.0/0: Target vpc-01-igw

### RT Tokyo
- Name: vpc-04-public-subnet-rt






---
## ✅ 3. Virtual Private Gateway 설정

- Name: vpc-01-vgw
Attach to VPC > 

---




## ✅ 4. Customer Gateway 설정

- Name: vpc-04-cgw
- IP 주소: `x.x.x.x` (Untangle 외부 Public IP)
- Routing: Static
- BGP: 사용 안함

---

## ✅ 5. Site-to-Site VPN Connection 설정

- Name: vpc-0104-vpn
- Virtual Private Gateway: vpc-01-vgw
- Customer Gateway: vpc-04-cgw
- Static IP prefixes: 10.4.0.0/16
Download configuration > Openswan (Libereswan의 베이스) 
