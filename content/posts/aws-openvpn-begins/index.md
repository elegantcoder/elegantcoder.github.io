---
title: "원격근무의 필수조건, AWS에 OpenVPN 구축기"
author: elegantcoder
date: 2018-11-17T16:55:43
publishDate: 2018-11-17T16:55:43
summary: "근무중인 세븐핀테크는 설립 때부터 리모트 근무에 높은 가치를 두고 실천해오고 있다. 각 멤버는 인천 송도, 천안, 제주에서 근무하고 있다. 세븐핀테크는 주식 고수의 실시간 계좌를 열람하고 거래 알림을 받는 모바일 앱 월스트릿파이터를 개발, 운영하고 있다. 집이나 도서관, 카페 등 본인이 편한 곳에서 근무하기 때문에 모든 사내 리소스는 물리적인 공간에 두지 않으며 클라우드(주로 AWS)에서 운영된다. 그러나 보안 [&hellip;]
"
url: "/posts/aws-openvpn-begins"
aliases: ["/aws-openvpn-begins"]
titleImage: "Untitled-copy.jpg"
draft: false
lastmod: 2020-06-30T22:49:06
isCJKLanguage: true
categories:
  - aws
  - dev
tags:

  - outdated
params:
  images:
    - "Untitled-copy.jpg"
---
근무중인 세븐핀테크는 설립 때부터 리모트 근무에 높은 가치를 두고 실천해오고 있다. 각 멤버는 인천 송도, 천안, 제주에서 근무하고 있다. 세븐핀테크는 주식 고수의 실시간 계좌를 열람하고 거래 알림을 받는 모바일 앱 월스트릿파이터를 개발, 운영하고 있다.

[![계좌까고 말해라 월스트릿파이터](배너2.png)](https://wlfa.us/campaign/mobile-app-store.html?utm_source=elegantcoder.com&utm_medium=referral&utm_campaign=OpenVPN)

집이나 도서관, 카페 등 본인이 편한 곳에서 근무하기 때문에 모든 사내 리소스는 물리적인 공간에 두지 않으며 클라우드(주로 AWS)에서 운영된다. 그러나 보안 면에서 아이피 주소 제한에 손이 많이가고 그에 따라 보안이 느슨해지는 이슈가 발생했다. 이슈의 해결을 위해 VPN 을 도입했다.

우리는 아래 사항에 중점을 두고 구축 작업을 진행했다.

-   AWS EC2에 OpenVPN을 사용해 구축
-   사내 리소스에 접근할 때만 VPN을 통하기
-   Docker를 사용한 구축

AWS EC2
-------

물리적인 사내 리소스가 있는 경우 VPN 구축작업은 사실 굉장히 쉽다. 최근에는 공유기나 NAS에 기본 기능으로 탑재되어 있는 경우도 많기 때문에 곧바로 사용할 수도 있다. 그러나 물리적인 이슈가 생겼을 때 사무실로 이동해야하는 불편함이 있을것이고 모든 리소스가 클라우드에 존재하는데 사무실을 거쳐 접근해야 한다면 성능에서도 불리할 것이다.

사내 리소스에 접근할 때만 VPN을 통하기
-----------------------

VPN클라이언트를 동작시켰을 때 모든 트래픽을 VPN을 통하게 한다면 AWS 데이터 트래픽 비용을 감당하기 어려울 것이다. 사내 리소스에 접근할 때만 VPN을 통하도록 Split Tunnel 방식으로 구축했다.

Docker OpenVPN
--------------

사내 리소스들은 대부분 Docker 기반으로 동작하고 있다. 사용률이 높지 않아 EC2 하나에 여러 컨테이너를 동작시키고 있다. OpenVPN도 Docker 로 잘 만들어진 이미지가 있다. 우리가 사용한 이미지는 kylemanna/docker-openvpn 이고 아래 주소들에서 확인할 수 있다.

-   [https://hub.docker.com/r/kylemanna/openvpn/](https://hub.docker.com/r/kylemanna/openvpn/)
-   [https://github.com/kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)

이 이미지는 Config 파일 생성과 사용자 추가 삭제 등 관리에 필요한 커맨드들을 다수 제공하고 문서화가 잘 되어있는 장점이 있다.

설치법도 매우 간단해서 Medium 에는 5분만에 설치하는 글이 있을 정도다. ([https://medium.com/@gurayy/set-up-a-vpn-server-with-docker-in-5-minutes-a66184882c45](https://medium.com/@gurayy/set-up-a-vpn-server-with-docker-in-5-minutes-a66184882c45))

설치
--

아래 설치법은 프로젝트의 README 를 따라 거의 그대로 옮긴 것이다.

### 볼륨 만들기

아래 만들 설정파일과 인증서 파일들을 저장할 볼륨을 만든다.

```
OVPN_DATA="ovpn-data-test"

docker volume create --name $OVPN_DATA
```

### ovpn\_genconfig 를 사용해 기본값으로 서버만들기

```
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_genconfig -u udp://VPN.SERVERNAME.COM
```

### 루트인증서 생성

실행 중 발급기관 이름(CN)과 비밀번호를 물어본다. 비밀번호는 앞으로 사용자 추가 / 삭제에도 사용되므로 메모해둬야 한다.

```
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn ovpn_initpki

init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /etc/openvpn/pki

Generating a 2048 bit RSA private key
............................................................................+++
....+++
writing new private key to '/etc/openvpn/pki/private/ca.key.XX'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
...
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/etc/openvpn/pki/ca.crt

Generating DH parameters, 2048 bit long safe prime, generator 2
This is going to take a long time
... 

Enter pass phrase for /etc/openvpn/pki/private/ca.key:
Check that the request matches the signature
...
Enter pass phrase for /etc/openvpn/pki/private/ca.key:

An updated CRL has been created.
CRL file: /etc/openvpn/pki/crl.pem
```

### 클라이언트 개인용 인증서 만들기

`CLIENTNAME` 을 CN 으로 하는 10년짜리 인증서가 만들어진다.

```
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass

Generating a 2048 bit RSA private key
......................................+++++
.....................................................................................................+++++
writing new private key to '/etc/openvpn/pki/private/CLIENTNAME.key.XXXXOGlPiN'
-----
Using configuration from /usr/share/easy-rsa/openssl-easyrsa.cnf
Enter pass phrase for /etc/openvpn/pki/private/ca.key:
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'CLIENTNAME'
Certificate is to be certified until Nov 14 14:02:04 2028 GMT (3650 days)

Write out database with 1 new entries
Data Base Updated
```

### 참고 – 사용자 제거

```
# crt 파일과 key는 보존
docker run --rm openvpn ovpn_revokeclient CLIENTNAME
# crt 파일과 key도 함께 제거
docker run --rm openvpn ovpn_revokeclient CLIENTNAME remove 
```

### 사용자에게 배포할 클라이언트 Config 파일 만들기

이 파일에 개인용 인증서가 포함되어 있어 중앙에서 배포할 때 편리하다.

```
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn
```

### 서버실행

AWS Security Group 에서 UDP 1194 포트를 열어야 함에 유의한다.

```
docker run -v $OVPN_DATA:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn
```

### 테스트

VPN 클라이언트를 사용해 클라이언트 Config 파일 설치 후, VPN 접속해본다.

VPN 접속 전후로 외부 아이피 주소가 달라지면 성공이다.

나는 주로 [ifconfig.co](http://ifconfig.co) 라는 서비스에서 외부 아이피 주소를 활용한다.

```
$ curl ifconig.co
0.0.0.0
```

설정 팁
----

구축을 위해 사용한 몇가지 설정을 소개한다. 이 설정들은 컨테이너 볼륨의 `/etc/openvpn/openvpn.conf` 수정내역에 대한 것이고, 이미지에는 vi가 설치되어있어 config 파일에 접근이 가능하다는 점을 알아두고 시작하면 좋다.

```
docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn vi /etc/openvpn/openvpn.conf
```

### Split Tunnel 사용

OpenVPN 에서 Split Tunnel은 특정 아이피 대역을 VPN Gateway 를 통하도록 한다. 설치때 사용한 `ovpn_genconfig` 은 기본값으로 클라이언트의 모든 트래픽이 VPN 네트워크를 통하게 한다. 따라서 이를 변경해줘야 한다. AWS 내의 내부 자원이므로 아이피 대역은 VPC의 아이피 대역을 적어준다.

```
# push "redirect-gateway def1" # => 모든 트래픽이 VPN 을 통하는 설정, 주석처리 / 삭제
push "route 10.20.0.0 255.255.0.0 vpn_gateway" # VPC 대역에 접근할 때만 VPN 을 통하도록 클라이언트 접속 설정에 PUSH
```

### 다중 클라이언트 접속

한 클라이언트 CN으로 동시에 접속가능하게 하려면 `duplicate-cn` 을 사용한다. 모바일과 PC에서 동시에 접속하려면 필요할 것이다.

```
duplicate-cn
```

### Route53의 Private DNS 설정

액세스 편의를 위해 내부 자원에 Private DNS 를 사용할 수 있다. Route53를 이용한 Private DNS는 VPC 대역 +2에 위치한다. 예를 들어 10.20.0.0, 255.255.0.0 이라면, 10.20.0.2 에 DNS 서버가 위치한다. DNS 설정은 `push dhcp-option DNS` 를 사용한다.

```
push "dhcp-option DNS 10.20.0.2" 
```

맺음말
---

VPN을 구축하면서 자체 보안 뿐 아니라 다른 회사와의 협업 부분에서도 개선된 면이 있다. 앱 특성상 제도권 금융회사들과 협업하는 경우가 있다. 이들 회사는 높은 수준의 보안을 요구하고 이에 따라 협업시 고정 아이피 주소를 교환하는 작업이 자주 일어난다. 서버 측이야 문제될 것이 없지만 클라이언트는 사정이 좀 다르다. 개발자의 장소에 구애받을 뿐 아니라 휴대폰 망을 사용하는지, 와이파이를 사용하는지에 따라서도 제한을 받게 된다. 최근 클라이언트 개발에 있어서도 협업할 부분이 있었는데 아직 출시 전인 기능을 함께 테스트하기 위해 모바일 클라이언트가 금융회사 내부 자원에 접근해야 할일을 VPN을 통해 간단히 처리할 수 있었다.

들이는 품에 비해 만족도가 매우 높은 작업이었다. 사내보안 때문에 Security Group 수정이 잦거나, 관리가 안되는 징조가 보이는 상황이라면 도입을 시도해 보는 것을 추천한다.

참고
--

-   [https://github.com/kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)
-   OpenVPN Cookbook [https://www.packtpub.com/mapt/book/networking\_and\_servers/9781786463128](https://www.packtpub.com/mapt/book/networking_and_servers/9781786463128)
