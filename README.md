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

## JWT 토큰을 사용한 request 프로세스
+ JSON 객체에 요구사항을 작성

+ 어떤 암호화 방식을 사용해서 문자열로 인코딩

+ HTTP header에 추가함으로써 사용자 인증을 요청

+ 서버에서는 header에 추가된 token을 디코딩하여 사용자를 인증

![image](https://github.com/hanjhoon/JWT/assets/121271030/f7479239-2a2b-4cc0-973d-35ced2f9ae64)


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
nest에서 JWT를 사용하기 위해서 jsonwebtoken이라는 모듈을 사용할 수 있다.

JWT를 쉽게 사용하기 위해 사용하는 모듈이며 아래의 예제를 보면 JWT를 통해 로그인을 구현할 수 있다.

## 로그인 시 토큰 관리
