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

브라우저에서 방송 화면을 직접 구성하고,  
실시간 송출(RTMP), 녹화, 채팅 연동, 숏폼 생성까지 처리하는  
**웹 기반 방송 스튜디오 플랫폼**입니다.

기존 OBS와 같은 별도 프로그램 없이  
브라우저만으로 방송 제작 → 송출 → 콘텐츠 재활용까지 연결하는  
통합 워크플로를 목표로 개발되었습니다.

### 해결하려는 문제

- 라이브 방송과 영상 편집이 분리된 비효율적인 워크플로
- 크리에이터 1인이 기획, 방송, 편집을 모두 수행해야 하는 높은 작업 부담
- 방송 이후 콘텐츠 재활용 과정에 많은 시간과 비용이 드는 문제

### 주요 기능

#### 1. 브라우저 기반 방송 스튜디오

- 캔버스 기반으로 카메라, 화면공유, 배너 등을 배치
- 씬(Scene) 및 레이아웃 구성 기능 제공
- 웹에서 방송 화면을 편집하고 미리보기 가능
- 표시용 캔버스와 송출용 캔버스를 분리하여 UI 렌더링과 실제 방송 출력을 독립적으로 관리

#### 2. 실시간 스트리밍 및 송출

- LiveKit(WebRTC) 기반 실시간 스트리밍
- RTMP를 통해 YouTube 등 외부 플랫폼으로 송출
- 방송 시작/종료 및 상태 제어 UI 제공

#### 3. 협업 기반 스튜디오 환경

- WebSocket(STOMP) 기반 실시간 상태 동기화
- 다수 사용자 협업 편집 지원
- 편집 충돌 방지를 위한 락(Lock) 구조 적용

#### 4. 녹화 및 라이브러리 관리

- 방송 녹화 및 저장
- 녹화본 라이브러리 조회 및 다운로드
- 방송 이후 콘텐츠 재활용 가능

#### 5. AI 기반 숏폼 생성

- 녹화 영상 기반 숏폼 생성 기능 제공
- 댓글 수 분석 및 마커/하이라이트 관련 기능 제공
- 음성 인식 기반 전사 및 자막 생성
- Whisper 사용 여부는 추가 확인 필요

### 시스템 구조 (핵심)

Frontend (Next.js)  
→ API Gateway  
→ Core Service / Media Service  
→ LiveKit (WebRTC)  
→ RTMP (YouTube 등 외부 플랫폼)

- 브라우저에서 구성한 송출용 캔버스를 MediaStream으로 변환
- LiveKit을 통해 실시간 방송 스트림으로 전달
- LiveKit Egress를 통해 RTMP 송출
- 녹화 및 후처리 후 라이브러리 및 AI 서비스로 연계

### 기술 스택

- Frontend : Next.js, React, TypeScript, Zustand, Canvas / Konva
- Backend : Spring Boot, Spring Security, JPA
- Realtime : WebSocket(STOMP)
- Streaming : WebRTC (LiveKit), RTMP
- AI : 음성 인식 기반 전사, 숏폼 생성 파이프라인
- Video Processing : FFmpeg

### 역할

- 프론트엔드 개발
- 캔버스 기반 방송 스튜디오 UI 구현
- 씬(Scene) 및 레이아웃 편집 기능 개발
- LiveKit 기반 실시간 스트리밍 연결 UI 구현
- WebSocket 기반 협업 상태 동기화 UI 처리
- API 연동 및 상태 관리 구현

### 트러블슈팅

#### 1. 반응형 캔버스 좌표 문제

- 문제  
  브라우저 크기 변경 시 요소 위치와 크기가 어긋나는 문제 발생

- 원인  
  화면 크기는 변하지만 소스 배치 정보는 별도 좌표 체계로 관리되어  
  실제 렌더링 좌표와 불일치가 발생

- 해결
  - 소스 위치와 크기를 정규화 좌표로 저장
  - 화면 크기에 맞게 픽셀 좌표로 변환
  - 레이아웃 변경 시 동일한 기준으로 재계산

- 결과  
  다양한 해상도에서도 비교적 일관된 방송 화면 유지

### 핵심 구현 포인트

- 브라우저에서 단순 미리보기 화면만 그린 것이 아니라, 표시용 캔버스와 송출용 캔버스를 분리해 UI와 실제 방송 출력 경로를 독립적으로 관리
- 브라우저에서 구성한 송출용 캔버스를 MediaStream으로 변환해 LiveKit을 통해 실제 방송 스트림으로 전달
- WebRTC → RTMP 송출까지 이어지는 방송 파이프라인 구현
- 실시간 협업 환경에서 상태 동기화 및 편집 충돌 제어
- 녹화 → 라이브러리 → 숏폼 생성으로 이어지는 콘텐츠 재활용 흐름 구현

### 성과

- OBS와 유사한 방향의 웹 기반 방송 스튜디오 구조 구현
- 실시간 송출 및 녹화를 포함한 통합 방송 워크플로 구축
- 방송 이후 콘텐츠 재활용까지 고려한 서비스 구현

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
