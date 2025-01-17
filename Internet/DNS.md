# DNS
## DNS란?
우리가 인터넷 브라우저를 사용할 때 도메인 이름을 입력하여 웹사이트에 접근할 수 있다. 이는 DNS(Domain Name System)이라는 시스템이 존재하기 때문에 가능한 일이다.
DNS는 인터넷의 핵심 인프라로서, 네트워크 트래픽이 올바른 IP 주소 목적지까지 찾아가도록 돕는다.

## DNS 작동 원리
DNS란, 도메인 이름을 IP 주소로 변환하여 주는 시스템을 말한다. DNS는 인터넷에서 사용되는 주소록과 같은 존재이다. DNS 시스템에서는 도메인 이름과 IP가 서로 매핑되어 저장되어 있다.
사용자가 특정 도메인을 입력하면, 그에 매치되는 IP 주소를 찾아서 접속할 수 있도록 해준다.
<br><br>
모든 장치는 IP 주소를 가지고 있다. 우리가 인터넷을 사용할 때 어떤 웹사이트에 접속을 하기 위해서는 이 IP 주소를 이용해야 한다. 하지만 모두가 IP 주소 대신에 URL 이라는 영문으로 이루어진 웹사이트 주소를 입력한다.
`www.example.com`와 같은 주소를 예시로 들 수 있다. 이를 `192.0.0.0`와 같은 IP 주소로 변환시켜 주는 것이 DNS 이다.

<p align="center">
  <img 
    src="https://ic.nordcdn.com/v1/https://nordvpn.com/wp-content/uploads/blog-infographic-how-dns-works-ko.svg"
    width="400"
    />
</p>

## DNS 서버 종류
DNS 서버는 역할에 따라 네 가지로 나눠진다. 각각의 서버가 맡은 기능을 수행하여 전체 도메인 네임 시스템이 동작할 수 있게 한다.

- **순환 DNS 서버(DNS Recurrsive Server/DNS resolver)** <br>
요청 받은 도메인에 매치되는 IP 주소를 찾기 위해 계층적으로 DNS 쿼리를 수행한다. DNS 쿼리의 첫 단계에 해당한다. DNS 캐시를 저장하는 곳이기도 하다.
- **루트 DNS 서버(Root Name Server)** <br>
순환 DNS 서버에서 처음으로 DNS 쿼리 요청을 보내는 서버이다. 해당 도메인의 확장자(`.com`, `.net`, `.org`)에 따른 TLD 네임 서버의 주소를 DNS Resolver에 응답해준다.
- **최상위 도메인 DNS 서버(TLD Name Server)** <br>
최상위 도메인(`.com`, `.net`, `.org`)에 대한 DNS 정보를 관리한다. 루트 DNS 서버와 권한 네임 서버의 중간 단계이다.
- **권한 네임 서버(Authoritative Name Server)** <br>도메인의 IP 주소를 응답해주는 최종 단계 DNS 서버다. 여기서 얻은 IP 주소가 순환 DNS 서버를 거쳐 브라우저까지 전달된다.

## DNS 역할군에 따른 작동원리
1. 사용자가 브라우저 주소창에 URL을 입력한다.
2. 브라우저가 입력된 도메인의 IP 주소를 알아내기 위해 운영체제를 거쳐 DNS Resolver에 요청을 보낸다.
3. DNS Resolver는 로컬 DNS 캐시에 이전 방문한 도메인 정보가 있는지 확인한다. 만약 캐시 기록이 있다면, 추가적인 DNS 조회가 필요하지 않는다. 이 단계에서 바로 IP 주소를 응답해줄 수 있다.
4. 캐시에 정보가 없는 경우, DNS Resolver는 루트 DNS 서버에서 최상위 도메인(TLD) DNS 서버의 IP 주소 정보를 알아낸다.
5. DNS Resolver는 최상위 도메인(TLD) DNS 서버에 연결하여 권한 네임 서버의 IP 주소를 요청하여 받아낸다.
6. 최종적으로 DNS Resolver는 권한 네임 서버에서 도메인의 최종 IP 주소를 마침내 알아낸다. 이렇게 받아온 IP 주소는 로컬 DNS 캐시에 저장된다. 이후로는 같은 요청이 있다면 DNS Resolver에서 바로 응답이 가능하다.
7. 받아온 최종 IP 주소를 다시 거꾸로 운영체제를 거쳐 브라우저로 전달한다.

사용자는 위의 과정을 다 거쳤다면 해당 IP 서버에 연결할 수 있게된다.

## DNS 캐싱
DNS 캐싱이란 DNS 서버 안에 이전에 처리한 DNS 쿼리 결과값을 한시적으로 저장하는 것이다. 이를 통해 불필요한 네트워크 트래픽을 줄이고 동일한 쿼리에 한해서 응답 시간을 단축할 수 있다. 결과적으로 웹 사이트 로딩 속도가 빨라진다.
<br><br>
순환 DNS 서버가 DNS 캐시를 저장하는 역할을 한다. DNS Resolver 단계에서 이미 이전에 처리했던 쿼리와 동일한 쿼리를 받았다면 즉시 캐시에서 응답을 반환해 줄 수 있다. 즉, 캐시에 없는 내용만 다른 DNS 서버에 질의하면 되므로 응답 시간을 단축하고 트래픽도 절약할 수 있다.
<br><br>
캐시된 내용은 TTL(Time To Live) 설정 값에 따라 유지된다. TTL이 지난 캐시 내용은 서버에서 자동으로 삭제된다. 하지만 때로는 502 bad gateway와 같은 에러가 있을 때, 캐시를 수동 플러싱 해야 할 때도 있다. DNS 캐시 수동 삭제를 하고 나면 캐시가 초기화 된 후 다시 쌓이기 시작할 것이다.
<br><br>
한편, DNS 스푸핑 중 가장 많이 쓰이는 수법인 DNS 캐시 포이즈닝에 대해 주의해야 한다. 이는 공격자가 캐시에 잘못된 IP 주소를 주입하는 방식이다. 그런 다음 악성 웹사이트로 유도하여 멀웨어를 다운로드 받게 하거나 개인정보를 중간에서 탈취한다. 카드 번호와 같은 금융 정보를 탈취하여 금전적인 손실을 초래하기도 한다.

## 개인 DNS
개인 DNS(Private DNS)란, 인터넷 서비스 공급 업체(ISP)가 운영하는 공용 DNS 서비스와 달리 사용자가 만들고 관리하는 DNS이다. 인터넷 서비스 공급 업체가 제공하는 DNS 서버를 사용 시 접속 기록이 수년간 업체에 남아있는 경우가 많다. 하지만 개인 DNS를 사용하면 사용자의 온라인 활동을 인터넷 서비스 공급 업체가 추적할 수 없다. 따라서 개인 DNS를 사용하면 온라인 상의 개인 정보를 보호할 수 있다.

### 참고문헌 및 이미지 출처
<p>
  <a href="https://nordvpn.com/ko/blog/dns-explained/">
    NordVPN
  </a>
</p>
