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
### Subnet A (public)
- Subnet name: vpc-01-public-subnet-a
- CIDR: 10.1.1.0/24


---

## ✅ 4. EC2 인스턴스 생성
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
  - SSH (22), ICMP 허용
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

---
## ✅ 5. Elastic IP 생성 및 할당 
- Name: eip-vpc-01-public-ec2-a
Actions > Associate Elastic IP address > Instance 

--- 
## ✅ 6. Route Table 생성 
- Name: vpc-01-public-subnet-rt
- VPC: vpc-01
Subnet associations > Edit subnet associations > vpc-01-subnet-a
Routes > Edit routes > Destination 0.0.0.0/0: Target vpc-01-igw









---
## ✅ 3. Virtual Private Gateway 설정

- 이름: `vpn-gw-a`
- 연결 대상 VPC: `vpc-site-a`

---




## ✅ 4. Customer Gateway 설정

- 이름: `untangle-cgw`
- IP 주소: `x.x.x.x` (Untangle 외부 Public IP)
- Routing: Static
- BGP: 사용 안함

---

## ✅ 5. Site-to-Site VPN Connection 설정

- 이름: `vpn-a-to-b`
- Virtual Private Gateway: `vpn-gw-a`
- Customer Gateway: `untangle-cgw`
- Static routes: `192.168.0.0/16`
- 암호화 설정: 기본값 사용 (IKEv2)




---

## ✅ 7. Untangle에서 IPSec VPN 구성

- Peer IP: AWS에서 제공한 터널 IP
- PSK: AWS에서 생성된 Pre-Shared Key 입력
- 암호화 설정: AES256 / SHA1 / DH Group 2 등 AWS와 일치시킴

---

## ✅ 8. 연결 테스트

- 터널 상태 확인: `UP` 상태 확인
- EC2 A → EC2 B ping 전송 → **성공**
- Untangle 로그에서 데이터 전송 확인됨
