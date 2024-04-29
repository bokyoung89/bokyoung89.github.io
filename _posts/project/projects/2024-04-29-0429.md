---
published: true
title: "[프로젝트 요약] 대규모 트래픽에 대비한 예약구매 서비스"
categories:
- pre-order-service
toc: true
toc_label: "목록"
toc_icon: "bars"
toc_sticky: true

---

> 👩🏻‍💻 대규모 트래픽에 대비한 예약구매 서비스 프로젝트를 정리한 기록입니다.

## 프로젝트 목적
- 특정 시간에 상품을 구매하는 서비스로서 상품 등록, 주문, 결제 기능을 제공하며 대규모 트래픽 처리를 대비합니다.

## 🕒 개발 기간
- 총 4주(2024.01.24.~2024.02.22.)

## ⚒ 사용 기술
- Language : JDK 11
- Build Tool : Gradle
- Library&Framework : Spring Boot, Spring Cloud, JWT
- Database : MySQL, Redis
- ORM : Hibernate
- DevOps : Docker, Github
- Testing Tools : JUnit, nGrinder

## 🌟 주요 기능
* 예약 구매를 위한 상품 등록, 주문 생성, 결제 프로세스 기능
* Redis Cache를 활용한 실시간 재고관리 서비스
* JWT 토큰 생성 및 검증을 통한 로그인, 로그아웃 기능
* JavaMailSender와 Gmail의 SMTP 서버를 활용한 이메일 인증 기능
* 사용자의 뉴스피드 생성, 친구들의 포스트를 최신순으로 가져오는 기능
* 포스트, 댓글 CRUD, 좋아요, 팔로우 등 유저 활동 서비스

## 아키텍쳐
![architecture](https://github.com/bokyoung89/bokyoung89.github.io/assets/58727604/2d0f0d7d-5da2-4829-b77a-853aecc559d2)

## ER 다이어그램
![image](https://github.com/bokyoung89/bokyoung89.github.io/assets/58727604/6412a2a8-466d-4383-b20f-abd033a08e12)
![image](https://github.com/bokyoung89/bokyoung89.github.io/assets/58727604/2d06021c-e6e4-49e1-aee0-5a744f2057b5)

## 트러블 슈팅
### [마이크로서비스에서 인증 프로세스 구현](https://bokyoung89.github.io/pre-order-service/0215/)
**문제**
* 모놀리식 서비스를 마이크로서비스로 모듈화하면서 각 모듈이 가져야 할 기능 외에 모든 로직을 제거하였다. 각 모듈간의 관계를 분리하면서 user-service에서 담당했던 인증/인가를 사용할 수 없게 되었다. 해당 시스템은 로그인 이후 모든 API 호출에 대해서는 토큰을 검증하고 있다.

**AS-IS 기존 모놀리식 예약 구매 서비스의 인증 절차**
1. 클라이언트에서 api 요청이 들어오면 user-service의 JwtTokenFilter가 JWT 토큰의 유효성을 검사한다.
2. 토큰 정보가 유효한 경우, DB에서 얻은 사용자 정보를 가지고 인증 정보(authentication)를 설정한다.
3. 서비스는 전달받은 인증 정보(authentication)를 기반으로 요청을 처리한다.

**해결방안**
* 인증/인가를 계속 user-service에게 위임할 것인가를 고민했다. user-serivce는 회원가입, 로그인, 프로필 조회, 수정 기능만을 주기로 결정했기에 인증은 당연히 user-service의 역할이 되어선 안됐다. 그렇다면 마이크로서비스 구조에서 인증/인가를 구현하는 방법은 무엇일까?
내 프로젝트의 경우 모든 서비스의 요청은 인증 절차를 거쳐야 했기에 서비스 최전방에 위치한 API Gateway를 통해 인증 절차를 위임하는 것이 가장 적합하다고 판단하였다.
* Spring Cloud Gateway의 customfilter를 구현해 회원가입/로그인 이후에 발생하는 모든 API 호출은 인증을 거치도록 하였다. user-service는 회원가입도 로그인 시 토큰 발급 기능만 남겨두었다.

**TO-BE 마이크로서비스 예약 구매 서비스의 인증 절차**
1. 클라이언트에서 api 요청이 들어오면 API Gateway의 AuthorizationHeaderFilter가 JWT 토큰의 유효성을 검사한다.
2. 토큰 정보가 유효한 경우, 사용자 ID를 요청 헤더에 추가하여 서비스에 전달한다.
3. 서비스는 사용자 ID를 기반으로 요청을 처리한다.

**결과**
* 결과적으로 API Gateway에서 공통 인증 절차를 수행함으로써 뒷단의 서비스들은 인증 방식으로부터 완전히 독립되어 인증 서비스와의 의존성이 사라지게 되었고, 토큰 발급 / 인증의 역할 분리도 가능해졌다.