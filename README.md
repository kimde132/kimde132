# 👋 안녕하세요, 김동언입니다

현장의 문제를 기술로 해결하는 **웹 개발자**를 목표로 공부하고 있습니다.  
현재 **SSAFY Python 트랙**에서 웹 개발과 AI를 학습하고 있으며  
웹 서비스 개발과 인프라 구축에 관심을 가지고 있습니다.

---

# 🧑‍💻 About Me

- 🎓 기계공학 전공
- 📚 SSAFY 14기 Python Track (2025.07 ~ 2026.06)
- ⚙️ 실제 문제를 해결하는 서비스를 만드는 것을 좋아합니다

---

# 🛠 Tech Stack

### Frontend

- Vue.js
- React
- TypeScript
- HTML
- CSS

### Backend

- Python
- Django
- Java
- Spring Boot

### Database

- PostgreSQL

### Infra / DevOps

- Docker
- Docker Compose
- Jenkins
- Nginx

---

# 🚀 Projects

## 1️⃣ Robot-based Industrial Safety Management Platform

로봇이 수집한 데이터를 기반으로 산업 현장의 안전 상태를 실시간으로 모니터링하고  
이벤트 대응 및 원격 제어를 수행할 수 있는 **웹 기반 안전 관제 시스템**입니다.

기존의 수기 점검과 사후 보고 중심의 안전 관리 방식에서 벗어나  
**실시간 데이터 기반 자동화된 안전 관리 환경 구축**을 목표로 개발되었습니다.

---

### 해결하려는 문제

- 현장 안전 점검의 실시간성 부족
- 수기 보고 기반의 비효율적인 관리 방식
- 이벤트 발생 이후 대응 지연

---

### 시스템 구조

로봇 (ROS2)
→ MQTT / HTTP
→ Spring Boot Backend
→ PostgreSQL
→ Next.js Frontend

- 로봇 → 서버 : MQTT 기반 실시간 데이터 전송
- 서버 → 로봇 : 명령 생성 후 MQTT로 전달
- 영상 스트리밍 : MJPEG 기반 실시간 모니터링

---

### 주요 기능

#### 1. 로봇 데이터 수집

- 상태 / 위치 / heartbeat / 이벤트 / PPE 로그 수집
- MQTT 기반 비동기 메시지 처리

#### 2. 실시간 안전 관제

- 이벤트 센터에서 이벤트 조회 및 상태 변경
- 조치 이력 및 이미지 기반 증적 관리

#### 3. 로봇 제어

- 웹에서 명령 생성 → MQTT로 로봇 전달
- 원격 제어 및 상태 확인

#### 4. 모니터링 & 대시보드

- 로봇 상태 / 이벤트 / PPE 통계 시각화
- 10초 주기 실시간 데이터 갱신

#### 5. 영상 스트리밍

- MJPEG 기반 실시간 영상 모니터링
- Nginx + Backend 프록시 구조

#### 6. 알림 시스템

- 이벤트 발생 / 로봇 오프라인 / 명령 실패 알림
- 웹에서 즉시 확인 및 이동

---

### 데이터 흐름 (핵심)

로봇 → MQTT → Backend → DB 저장 → 알림 생성 → Front 표시

- 이벤트 발생 시 자동으로
  → DB 저장 → 알림 생성 → UI 반영까지 이어지는 구조

---

### 기술 스택

- Frontend : Next.js, React, TypeScript
- Backend : Spring Boot, Spring Security, JPA
- Database : PostgreSQL
- Infra : Docker, Docker Compose, Jenkins, Nginx, AWS EC2
- Communication : MQTT, REST API
- Streaming : MJPEG (WebRTC → MJPEG 전환)

---

### 트러블슈팅

#### 1. WebRTC → MJPEG 전환

- 문제 : WebRTC는 NAT/방화벽/ICE 문제로 운영 불안정
- 해결 : MJPEG 기반 단순 HTTP 스트리밍으로 변경
- 결과 : 배포 안정성 및 디버깅 용이성 향상

#### 2. 스트리밍 연결 끊김 문제

- 원인 : proxy buffering / timeout 설정
- 해결 :
  - Nginx buffering off
  - timeout 증가
- 결과 : 장시간 스트리밍 안정화

#### 3. 로봇 ↔ 서버 네트워크 문제

- 문제 : 로봇이 사설망 내부
- 해결 : SSH reverse tunnel 적용
- 결과 : 외부 서버에서 접근 가능

---

### 역할

- 웹 풀스택 개발 (Frontend + Backend)
- 실시간 데이터 처리 API 설계 및 구현
- MQTT 기반 로봇 통신 연동
- MJPEG 스트리밍 구조 구현
- CI/CD 구축 (GitLab → Jenkins → Docker Compose)
- Nginx 기반 리버스 프록시 및 HTTPS 구성

---

### 성과

- 실시간 안전 관제 시스템 구축
- 이벤트 → 알림 → 조치까지 자동화 흐름 구현
- 영상 + 데이터 통합 모니터링 환경 구축

---

## 2️⃣ OneTake Studio

브라우저에서 라이브 방송을 진행하면  
AI가 자동으로 하이라이트 영상과 숏폼 콘텐츠를 생성하는  
**AI 기반 라이브 콘텐츠 제작 플랫폼**입니다.

크리에이터가 방송 이후 직접 편집해야 하는 기존 워크플로의 문제를 해결하여  
**“방송 한 번으로 다양한 콘텐츠를 자동 생성”**하는 것을 목표로 개발되었습니다.

### 해결하려는 문제

- 라이브 방송과 영상 편집의 분리된 워크플로
- 크리에이터 1인이 기획·촬영·편집을 모두 수행해야 하는 구조
- 영상 편집에 많은 시간 소모

### 주요 기능

- WebRTC 기반 브라우저 라이브 방송
- 채팅 데이터 기반 하이라이트 자동 감지
- AI 기반 숏폼 영상 자동 생성
- Whisper 기반 음성 인식 및 자막 생성
- 팀 단위 협업 콘텐츠 관리
- 멀티 플랫폼 송출 (YouTube / Twitch 등)

### 기술 스택

- Frontend : React, Next.js, TypeScript
- Backend : Spring Boot
- AI : Whisper STT, Highlight Detection
- Streaming : WebRTC
- Video Processing : FFmpeg

### 역할

- 프론트엔드 개발
- 라이브 스트리밍 데이터 수집 로직 구현
- UI 기능 구현 및 서비스 화면 개발

---

## 3️⃣ PULSE (Emotion-based Stock Recommendation Service)

주식 시장의 감정을 데이터로 분석하여  
투자 의사결정을 돕는 **감정 기반 주식 분석 서비스**입니다.

온라인 투자 커뮤니티, 뉴스, 유튜브 등 다양한 데이터를 분석하여  
시장 심리를 정량화한 **감정 점수(Sentiment Score)** 를 제공합니다.

### 해결하려는 문제

- 개인 투자자의 뇌동매매
- 정보 과부하로 인한 의사결정 어려움
- 확증 편향에 따른 투자 판단 오류

### 주요 기능

- 시장 감정 점수 대시보드
- 주가와 감정 점수의 상관관계 시각화
- 사용자 포트폴리오 분석
- 동일 업종 인기 종목 추천
- 관리자 데이터 수집 관리 기능

### 데이터 소스

- 투자 커뮤니티 댓글
- 경제 뉴스 기사
- 유튜브 영상 콘텐츠

### 핵심 기술

- KoBERT 기반 감정 분석
- GPT 기반 뉴스 요약
- Django REST API
- Vue SPA 기반 대시보드
- Selenium 기반 데이터 크롤링

### 기술 스택

- Frontend : Vue.js
- Backend : Django, Django REST Framework
- AI : KoBERT, GPT

---

# 📚 Study

- 운영체제 스터디
- 알고리즘 문제 풀이

---

# 🎯 Interests

- 🏃 러닝 (10km / 하프 마라톤)
- 🥃 위스키
- 📷 사진 촬영

---

# 📫 Contact

- GitHub : https://github.com/kimde132
- Email : kimde1852@gmail.com
