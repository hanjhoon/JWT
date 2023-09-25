# JWT
> [JWT 공식 홈페이지](https://jwt.io/introduction)


## WEB Session 방식
+ session을 사용하여 로그인을 구현하는 방식

  +  Session에 로그인 정보를 저장하고 그 정보를 이용한다.

  +  로그인을 하면 Session Id가 생성되어 Front End에 쿠키로 전달되고, 그 난수가 Session에 저장된다.

  + 이후 클라이언트의 요청이 있을 때마다 withCredentials 설정을 통해 함께 전달된 쿠키를 Session에 저장된 정보와 확인한다.

  +  이후 확인이 완료되면 DB에서 해당 유저 정보를 가져와 Request의 user에 값을 넣어준다.

 

## JWT(JASON Web Token)란?
+ JSON으로 만들어진 웹기반 토큰이다.

+ OAuth과 개념에 대해서 헷갈리는 경우가 있는데 OAuth는 Token을 이용한 인증방식을 말하는 것이고 JWT는 Token이다.

+ 정보가 담긴 데이터( JSON 객체 )를 암호화하여, HTTP 헤더에 추가시킨다.

+ 권한을 부여하기 위해 필요한 데이터가 JWT안에 모두 담겨있다.

+ 구성

  + 헤더(header): 토큰 종류와 해시 알고리즘 정보
  + 페이로드(payload): 토큰의 내용물이 인코딩된 부분
  + 시그니처(signature): 일련의 문자열, 시그니처를 통해 토큰이 변조되었는지 여부를 확인  

![image](https://github.com/hanjhoon/JWT/assets/121271030/d9dd478c-a352-4084-9d26-43bbe436a9b0)

### Refresh Token?
 
+ Access Token(JWT)를 통한 인증 방식의 문제는 만일 제 3자에게 탈취당할 경우 보안에 취약하다는 점이다.

+ 유효기간이 짧은 Token의 경우 그만큼 사용자는 로그인을 자주 해서 새롭게 Token을 발급받아야 하므로 불편하다. 그러나 유효기간을 늘리자면, 토큰을 탈취당했을 때 보안에 더 취약해지게 된다. 

+ 이 때, “그러면 유효기간을 짧게 하면서  좋은 방법이 있지는 않을까?”라는 질문의 답이 바로 "Refresh Token" 이다. 

+ Refresh Token은 Access Token과 똑같은 형태의 JWT다. 처음에 로그인을 완료했을 때 Access Token과 동시에 발급되는 Refresh Token은 긴 유효기간을 가지면서, Access Token이 만료됐을 때 새로 발급해주는 열쇠가 된다.

  + Refresh Token의 유효기간은 2주, Access Token의 유효기간은 1시간이라 하면, 사용자는 API 요청을 하다가 1시간이 지나게 되면, 가지고 있는 Access Token은 만료된다. 그러면 Refresh Token의 유효기간 전까지는 Access Token을 새롭게 발급받을 수 있다. 

  + Access Token은 탈취당하면 정보가 유출되는건 동일하다. 다만 짧은 유효기간 안에만 사용이 가능하기에 더 안전하다. 

  + Refresh Token의 유효기간이 만료됐다면, 사용자는 새로 로그인해야 한다. Refresh Token도 탈취될 가능성이 있기 때문에 적절한 유효기간 설정이 필요하다.



## JWT 토큰을 사용한 request 프로세스
+ JSON 객체에 요구사항을 작성

+ 어떤 암호화 방식을 사용해서 문자열로 인코딩

+ HTTP header에 추가함으로써 사용자 인증을 요청

+ 서버에서는 header에 추가된 token을 디코딩하여 사용자를 인증

![image](https://github.com/hanjhoon/JWT/assets/121271030/d260cdf1-666c-42bc-a810-737d11ea0276)


## JWT 관리
### 1. LocalStorage
  +  WebStorage의 하나인 localStorage에 저장하는 방식. 가장 흔히 사용하는 방법이다.
    
  + 장점: 웹 페이지를 새로고침 해도 살아있다.
  + 단점: XSS 공격에 취약하다.
  + sessionStorage는 localStorage와 똑같다. 브라우저 종료되었을 때 사라지냐, 사라지지 않느냐의 차이.

### 2. Cookie
+ 쿠키에 그냥 저장하는 방식은 안전하지 않다.

+ 하지만 httpOnly, sameSite, secure설정을 한다면.

  + HttpOnly: javascript에서 document.cookie로 접근하는 것을 막는다
  + SameSite: SameSite=strict설정을하게되면, CSRF 공격으로부터 안전해진다.
  + Secure: 오직 HTTPS에서만 쿠키에 접근할 수 있다.
  + 장점: HttpOnly, SameSite, Secure 설정을 한다면 안전하다.
  + 단점: 해당 설정을 지원하지 않는 브라우저 스펙이 있다. 만약 해당 설정을 하지 못한다면 XSS, CSRF공격에 취약하다.
  + 상대가 사파리 혹은 인터넷익스플로러라면 브라우저 지원을 꼼꼼이 살펴봐야 한다.

### 3. In Memory
+ in memory 변수에 저장하는 방법

+ 장점: 안전
+ 단점: 사용자가 탭을 새로 생성한다면 사라진다. 새로운 탭을 열어 다시 로그인을 해야합니다. 로그아웃도 탭마다 해줘야 한다.

### Redis
+ inMemory 캐시 스토어로 많이 사용되는 Redis에 저장하는 방법. 안전하지만 클라이언트 사이드에서는 이용할 수 없다.


## NestJS에서 jsonwebtoken 모듈 사용하기
+ nest에서 JWT를 사용하기 위해서 jsonwebtoken이라는 모듈을 사용할 수 있다.

+ JWT를 쉽게 사용하기 위해 사용하는 모듈이며 아래의 예제를 보면 JWT를 통해 로그인을 구현할 수 있다.

## 로그인 시 토큰 관리
