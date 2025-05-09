  
## 1. IP란?  
---  
IP란 Internet Protocol의 약자로 네트워크상에서 정보를 송수신 하는 통신에 대한 규약을 의미한다.  
  
어떠한 데이터를 주고 받을 때 컴퓨터는 서로 사용하는 운영체제가 다르고 프로그램은 구현되는 언어가 다르다. 그래서 이들이 서로 데이터 통신을 하기 위한 공통된 규약이 필요하다.  
  
IP는 이러한 이유 때문에 필요하며 IP 자체는 프로토콜이고 IP주소는 각각의 기기들이 가지고 있는 고유한 주소라고 생각하면 된다.  
  
IP주소는 IPv4와 IPv6 이렇게 두가지로 나뉜다.  
  
## 2. IPv4 vs IPv6  
---  
### 2-1.) IPv4  
IPv4는 32비트로 구성된 주소이며 총 43억개의 고유한 IP주소 생성이 가능하다. IP주소는 . 을 구분자로 4개의 0 부터 255 사이의 숫자로 만들어진다.  
ex) 127.0.0.1  
  
### 2-2.) IPv6  
IPv6는 128비트로 구성된 주소이며 : 을 구분자로 구분된 8개의 4자리 16진수로 이루어져 있다.  
ex) 2001:0db8:85a3:0000:0000:8a2e:0370:7334  
## 3. IPv4의 문제  
---  
현재 대중적으로 많이 사용되고 있는 IP형태는 IPv4이다. 하지만 위에서도 말했듯이 IPv4는 총 43억개의 주소밖에 생성하지 못하는 문제가 있다. 전세계적으로 43억대의 컴퓨터 또는 핸드폰 기기만 IP를 가질 수 있다는 의미이다.  
  
이를 해결하기 위해서 거의 무한대급으로 사용 가능한 IPv6가 등장했지만 상용화가 되기에는 아직 무리가 있고 기존 사용중인 IPv4를 IPv6로 마이그레이션 하는 과정도 비용 문제가 많이 들 것이다.  
  
하지만 우리는 살아가면서 IP가 부족하다는 말도 못 들어봤고 문제없이 사용중이다.  
  
그것이 바로 사설 IP 때문이다.  
## 4. 공인IP / 사설IP  
---  
IP는 크게 공인IP와 사설IP로 나뉜다.  
### 4-1.) 공인IP (Public IP)  
공인 IP는 KT, SKT, LG와 같은 인터넷을 제공하는 ISP 업체에 의해서 제공받게 되며, 인터넷 상의 다른 장치와 통신이 가능하다.  
  
### 4-2.) 사설IP (Private IP)  
내부 네트워크에서만 사용되는 IP 주소로, 라우터(공유기)가 각 기기에 자동으로 할당한다. 이 주소는 외부 네트워크에서는 사용되지 않으며, NAT(Network Address Translation)를 통해 공인 IP와 연결된다.  
  
사설 IP는 네트워크가 다르다면 중복해서 사용이 가능하고, 외부에서 직접 접근이 불가능하다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/c953afca-aa11-4d24-b09f-4a857471ceba-IP(1).png)  
