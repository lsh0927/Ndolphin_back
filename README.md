# Back
<div align="center">

# 🐬 NDolphin
유저 매칭과 실시간 채팅을 위한 고가용성 메시징 서버

[//]: # (https://www.ndolophin.com/)

[2025/02/13] : 서버 비용문제로 인스턴스 종료하였습니다

[![Spring](https://img.shields.io/badge/Spring-6DB33F?style=flat-square&logo=spring&logoColor=white)](https://spring.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com/)
[![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Nginx](https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white)](https://www.nginx.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)](https://redis.io/)
[![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white)](https://www.rabbitmq.com/)

</div>

## ⚡️ 주요 기능

- `사용자 인증`: JWT와 SMTP, Kakao OAuth2.0을 통한 보안 강화
- `유저 매칭` : PostGis의 거리 계산 쿼리를 이용한 일정 범위 내 유저 추천
- `유저 필터링` : 한번 매칭된 유저 혹은 싫어요를 누른 유저를 추천에서 제외
- `실시간 채팅`: WebSocket(STOMP)과 RabbitMQ를 활용한 실시간 메시징
- `무중단 배포`: Blue/Green 배포 전략을 통한 서비스 안정성 확보
- `이미지 업로드`: AWS S3를 활용한 프로필 이미지 관리
- `채팅 메세지 관리`: 채팅 메세지는 NOSQL에 따로 저장

[//]: # (- `실시간 알림`: Redis를 활용한 효율적인 실시간 알림 처리)

## 🏗️ 시스템 아키텍처

<div align="center">
<img src="/src/main/resources/static/아키텍쳐설계도.png" alt="system-architecture">
</div>

## 🛠 Tech Stack

### Infrastructure
![AWS EC2](https://img.shields.io/badge/AWS_EC2-FF9900?style=for-the-badge&logo=amazonec2&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS_S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)

### Backend
![Java](https://img.shields.io/badge/Java_17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_3-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Spring Interceptor](https://img.shields.io/badge/Spring_Interceptor-6DB33F?style=for-the-badge&logo=spring&logoColor=white)
![SMTP](https://img.shields.io/badge/SMTP-005FF9?style=for-the-badge&logo=gmail&logoColor=white)
![Kakao OAuth](https://img.shields.io/badge/Kakao_OAuth-FFCD00?style=for-the-badge&logo=kakao&logoColor=black)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=json-web-tokens&logoColor=white)
![WebSocket](https://img.shields.io/badge/WebSocket-010101?style=for-the-badge&logo=socket.io&logoColor=white)

### Database
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)

## 📝 API Documentation
```http
http://{server-url}:8080/swagger-ui.html
http://{server-url}:8081/swagger-ui.html
```

## 💡 상세 기능

### 채팅
- 실시간 1:1 채팅
- 채팅방 목록 조회
- 페이징으로 메세지 조회
- 읽지 않은 메시지 카운트
- 이미지 전송 기능
- 채팅방 나가기

### 유저 매칭
- 프로필 추천 (거리 기반)
- 좋아요/싫어요 기능
- 매칭 시 채팅방 자동 생성

### 프로필
- 프로필 이미지 관리
- 기본 정보 설정 (나이, 성별, 주소)
- 관심사 설정
- 자기소개 관리


## 🔐 로그인 플로우

```mermaid
sequenceDiagram
    autonumber
    participant U as 사용자
    participant F as 프론트엔드
    participant B as 백엔드 서버
    participant DB as 데이터베이스/Redis
    participant K as 카카오 API

    alt 일반 로그인 (이메일/비밀번호)
        U->>F: 로그인 페이지 접속
        F->>U: 로그인 폼 제공
        U->>F: 이메일/비밀번호 입력
        F->>B: POST /api/v1/auth/sign-in
        B->>DB: 사용자 인증 요청
        DB->>B: 사용자 정보 반환
        B->>B: 비밀번호 검증(PasswordEncryptor)
        B->>B: JWT 토큰 생성(JwtTokenProvider)
        B->>DB: Refresh 토큰 저장(RedisService)
        B->>F: 응답: {accessToken, refreshToken, 사용자정보}
        F->>F: 토큰 저장(sessionStorage)
        F->>U: 로그인 성공 & 메인 페이지 리다이렉션
    else 카카오 소셜 로그인
        U->>F: 카카오 로그인 버튼 클릭
        F->>B: GET /api/v1/auth (로그인 페이지 요청)
        B->>U: 카카오 로그인 페이지로 리다이렉트
        U->>K: 카카오 로그인 정보 입력
        K->>U: 인증 승인 및 콜백 URL로 리다이렉트 (인증코드 포함)
        U->>F: 콜백 URL 접근 (code 파라미터 포함)
        F->>B: POST /api/v1/auth/kakao (code 전달)
        B->>K: 인증 코드로 액세스 토큰 요청
        K->>B: 카카오 액세스 토큰 발급
        B->>K: 사용자 정보 요청(KakaoApiClient)
        K->>B: 사용자 정보 반환
        B->>DB: 사용자 정보 조회/저장(OAuthLoginService)
        B->>B: JWT 토큰 생성(authTokensGenerator)
        B->>DB: Refresh 토큰 저장(RedisService)
        B->>F: 응답: {accessToken, refreshToken, 사용자정보}
        F->>F: 토큰 저장(sessionStorage)
        F->>U: 로그인 성공 & 메인/프로필 설정 페이지로 리다이렉트
    end

    Note right of F: 이후 API 요청 시 토큰 사용 프로세스
    F->>F: axios 인터셉터로 토큰 확인
    F->>B: API 요청 with Authorization 헤더
    B->>B: JWT 토큰 검증(JwtInterceptor)
    alt 토큰 유효
        B->>F: API 응답
    else 토큰 만료
        B->>F: 401 Unauthorized
        F->>F: 토큰 갱신 인터셉터 실행
        F->>B: POST /api/v1/auth/refresh (refreshToken 전송)
        B->>DB: Refresh 토큰 검증
        alt Refresh 토큰 유효
            B->>F: 새 Access Token 발급
            F->>F: 토큰 저장 및 원래 요청 재시도
            F->>B: 원래 API 요청(새 토큰)
            B->>F: API 응답
        else Refresh 토큰 만료
            B->>F: 401 Unauthorized
            F->>F: 로컬 스토리지 토큰 삭제
            F->>U: 로그인 페이지로 리다이렉트
        end
    end
```


## 유저(프로필) 매칭 플로우

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend
    participant Auth as AuthService
    participant Profile as ProfileService
    participant Location as LocationService
    participant Matching as MatchingService
    participant Chat as ChatService
    participant S3 as S3Storage
    
    %% 로그인 후 프로필 설정 시작
    User->>FE: 로그인
    FE->>Auth: 인증 요청
    Auth-->>FE: JWT 토큰 발급 및 프로필 상태 반환
    
    %% 프로필 정보 입력
    alt 프로필 미생성
        FE->>FE: 프로필 설정 페이지로 리다이렉트
        User->>FE: 기본 정보 입력 (성별, 생년월일, 닉네임, 자기소개)
        FE->>Profile: createProfile API 요청
        Profile-->>FE: 프로필 생성 결과 반환
        
        %% 프로필 이미지 업로드
        User->>FE: 프로필 이미지 업로드
        FE->>S3: 이미지 파일 업로드
        S3-->>FE: 이미지 URL 반환
        FE->>Profile: uploadProfileImage API 요청
        Profile-->>FE: 이미지 저장 결과 반환
        
        %% 위치 정보 설정
        User->>FE: 주소 검색 및 선택
        FE->>FE: 주소를 위도/경도로 변환
        FE->>Location: saveLocation API 요청
        Location-->>FE: 위치 저장 결과 반환
    end
    
    %% 매칭 시스템 진입
    FE->>FE: 매칭 페이지로 이동
    
    %% 주변 사용자 검색
    FE->>Matching: findProfiles API 요청
    Matching->>Location: 사용자 위치 기반 검색
    Location-->>Matching: 반경 내 프로필 목록 반환
    Matching->>Profile: 프로필 정보 및 이미지 조회
    Profile-->>Matching: 프로필 상세 정보 반환
    Matching-->>FE: 주변 사용자 프로필 목록 반환
    
    %% 사용자 스와이프 액션
    User->>FE: 프로필 좋아요/싫어요 선택
    
    alt 좋아요 선택
        FE->>Matching: like API 호출
        Matching->>Matching: 양방향 좋아요 확인
        
        alt 상대방도 좋아요한 경우 (매칭 성공)
            Matching->>Chat: 채팅방 생성
            Chat-->>Matching: 채팅방 ID 반환
            Matching-->>FE: 매칭 결과 및 채팅방 정보 반환
            FE-->>User: 매칭 성공 알림
        else 매칭 안됨
            Matching-->>FE: 좋아요 저장 결과 반환
        end
    else 싫어요 선택
        FE->>Matching: dislike API 호출
        Matching-->>FE: 싫어요 저장 결과 반환
    end
    
    %% 다음 프로필로 이동
    FE->>FE: 다음 사용자 프로필 표시
    FE-->>User: 새로운 프로필 표시
```

## 채팅 플로우

```mermaid
sequenceDiagram
    autonumber
    participant User1 as User 1
    participant User2 as User 2
    participant FE as Frontend
    participant BE as Backend
    participant WebSocket as WebSocket
    participant RabbitMQ as RabbitMQ
    participant Mongo as MongoDB
    participant Redis as Redis

    %% Matching and Chat Room Creation
    User1->>FE: Swipe right (Like)
    FE->>BE: POST /api/v1/swipes/like
    BE->>BE: Check if mutual match
    
    alt Mutual match found
        BE->>BE: Create chat room (ChatRoomService)
        BE->>RabbitMQ: Create exchange/queue bindings
        BE->>MongoDB: Save initial system message
        BE->>FE: Return match result with chatRoomId
        FE->>User1: Show match notification
        FE->>User2: Show match notification
    end

    %% Entering Chat Room
    User1->>FE: Open chat room
    FE->>WebSocket: Connect to WebSocket
    WebSocket->>BE: Establish STOMP connection
    BE->>BE: JWT Authentication (JwtAuthenticationInterceptor)
    BE->>Redis: Store online status
    
    FE->>WebSocket: Send STOMP frame to /pub/chat.enter
    WebSocket->>BE: Process chat room entry
    BE->>Redis: Update last entry time
    BE->>RabbitMQ: Subscribe to chat room topic
    BE->>FE: Send room entry confirmation

    %% Sending and Receiving Messages
    User1->>FE: Type and send message
    FE->>WebSocket: Send to /pub/chat.message
    WebSocket->>BE: Process message (ChatMessageService)
    BE->>MongoDB: Save message to ChatMessage collection
    BE->>RabbitMQ: Publish to exchange with routing key
    
    RabbitMQ->>WebSocket: Route message to subscribers
    WebSocket->>FE: Deliver message to connected clients
    FE->>User1: Display own message
    FE->>User2: Display received message
    
    %% File/Image Upload
    alt File Upload
        User1->>FE: Select file/image
        FE->>FE: Convert to base64
        FE->>WebSocket: Send to /pub/chat.message with FILE type
        WebSocket->>BE: Process file message
        BE->>BE: Extract file data
        BE->>S3: Upload file to S3
        BE->>MongoDB: Save message with file URL
        BE->>RabbitMQ: Publish file message
        RabbitMQ->>WebSocket: Route file message
        WebSocket->>FE: Deliver file message
        FE->>User1: Display sent file
        FE->>User2: Display received file
    end

    %% Message Loading and History
    User1->>FE: Scroll up in chat
    FE->>BE: GET /api/v1/chat-messages/chat-rooms/{id}?page=X
    BE->>MongoDB: Query paginated messages
    BE->>FE: Return message history
    FE->>User1: Display older messages

    %% Message Deletion
    User1->>FE: Delete message
    FE->>WebSocket: Send to /pub/chat.delete
    WebSocket->>BE: Process deletion request
    BE->>MongoDB: Update message type to DELETED
    BE->>RabbitMQ: Publish deletion event
    RabbitMQ->>WebSocket: Route deletion event
    WebSocket->>FE: Notify of deletion
    FE->>User1: Update UI to show "deleted message"
    FE->>User2: Update UI to show "deleted message"

    %% Leaving Chat Room
    User1->>FE: Click "Leave chat room"
    FE->>BE: DELETE /api/v1/chat-rooms/{id}/leave
    BE->>BE: Process leave request (ChatRoomServiceImpl)
    BE->>MongoDB: Add system message about leaving
    BE->>Redis: Clear chat room data
    BE->>MongoDB: Delete chat room participants
    BE->>RabbitMQ: Publish leave message
    RabbitMQ->>WebSocket: Route leave notification
    WebSocket->>FE: Notify remaining user
    FE->>User2: Show "User has left" message
    BE->>FE: Confirm leave success
    FE->>User1: Redirect to messages list
```
