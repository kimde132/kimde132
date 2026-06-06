# 👋 안녕하세요, 김동언입니다

웹 개발을 기반으로 AI와 인프라를 연결해
실제 문제를 해결하는 서비스를 만들고 있습니다.

현재 **SSAFY 14기 Python Track**에서 웹 개발과 AI를 학습하고 있으며,
사용자가 실제로 사용할 수 있는 서비스와 안정적으로 동작하는 시스템을 만드는 데 관심이 있습니다.

---

# 🧑‍💻 About Me

* 🎓 기계공학 전공
* 📚 SSAFY 14기 Python Track (2025.07 ~ 2026.06)
* 🧩 웹 개발을 기반으로 AI, 인프라, 하드웨어 연동까지 확장해보고 있습니다
* ⚙️ 단순 기능 구현보다 실제 문제를 서비스 흐름으로 해결하는 개발을 지향합니다

---

# 🛠 Tech Stack

### Language

* Python
* Java
* JavaScript
* TypeScript

### Frontend

* Vue.js
* React
* Next.js
* HTML
* CSS
* Canvas API
* Konva.js

### Backend

* Django
* Django REST Framework
* Spring Boot
* REST API
* MQTT

### AI / Vision

* YOLOX
* MediaPipe Pose
* VLM
* PyTorch
* KoFinBERT

### Database

* PostgreSQL
* Django ORM

### Infra / DevOps

* Docker
* Docker Compose
* Jenkins
* Nginx
* AWS EC2

### Monitoring

* Prometheus
* Grafana

---

# 🚀 Projects

## 1️⃣ AURA

* GitHub : https://github.com/kimde132/AURA

AI 비전 기반으로 작업자의 행동을 분석하여
재고 변화 가능성이 높은 구역만 자동 스캔하는
**지능형 재고 자율 운영 시스템**입니다.

기존 전수 재고 조사 방식은 모든 선반을 직접 확인해야 하므로
많은 시간과 인력이 필요하고, 재고 불일치에 즉각 대응하기 어렵다는 한계가 있었습니다.

AURA는 고정 카메라 영상에서 작업자의 행동을 분석하고,
실제 재고 변화가 발생했을 가능성이 높은 Section만 선별하여
이동형 스캐너가 해당 구역을 자동 촬영하도록 설계되었습니다.

---

### 해결하려는 문제

* 모든 선반을 직접 확인해야 하는 전수 재고 조사의 비효율
* 재고 조사 주기가 길어 재고 불일치를 즉시 확인하기 어려운 문제
* 작업자의 단순 통행과 실제 재고 작업 행동을 구분해야 하는 문제
* AI 추론 정확도와 실시간 처리 성능을 동시에 고려해야 하는 문제

---

### 시스템 구조

Camera
→ AI Server
→ DCA
→ Jetson + Plotter
→ SSS Analysis
→ Web Dashboard

* Camera : 작업자 행동 영상 수집
* AI Server : 작업자 감지, 행동 분석, 후보 Section 추론
* DCA : AI 이벤트 및 후보 구역 전달
* Jetson + Plotter : 후보 구역 촬영
* SSS Analysis : 촬영 이미지 기반 재고 분석
* Web Dashboard : 재고 상태 및 이벤트 확인

---

### 주요 기능

#### 1. 행동 기반 AI Pipeline

* YOLOX 기반 작업자 감지
* MediaPipe Pose 기반 관절 좌표 추출
* ROI 진입 / 접근 / 이탈 상태 관리
* 이벤트 기반으로 분석 구간 생성

#### 2. Section 후보 추론

* 손목 위치 기반 점수 계산
* 팔 방향 기반 점수 계산
* 작업자와 선반 간 거리 반영
* 행동 지속 시간 및 반복 접근 횟수 반영
* Score Gap 및 Top-N 기준으로 후보 Section 선별

#### 3. VLM 기반 후보 검증

* MediaPipe 기반 후보 추론 결과를 VLM으로 보완
* 작업자의 실제 상호작용 여부를 영상 기반으로 검증
* 후보 Section과 VLM 결과를 병합하는 Candidate Merge 구조 설계

#### 4. 실시간 처리 최적화

* realtime-skip 구조로 지연 프레임 제거
* YOLO / Pose FPS 분리
* VLM 추론을 비동기 Queue로 분리
* Prompt 경량화 및 Top-N 제한으로 VLM 처리 부담 감소

#### 5. 평가 및 모니터링

* prediction_log와 final_event를 분리한 평가 구조 설계
* event-level 평가 자동화
* Prometheus 기반 지표 수집
* Grafana 기반 처리 시간 및 FPS 모니터링

---

### 데이터 흐름

Camera Frame
→ YOLOX Person Detection
→ MediaPipe Pose Estimation
→ Section Scoring
→ VLM Candidate Verification
→ Final Candidate Section
→ DCA Event
→ Plotter Scan Request

---

### 기술 스택

* AI : YOLOX, MediaPipe Pose, VLM, PyTorch
* Pipeline : Python
* API / Integration : FastAPI
* Monitoring : Prometheus, Grafana
* Device Integration : DCA, Jetson, Plotter

---

### 트러블슈팅

#### 1. VLM Latency 문제

* 문제
  VLM 추론 시간이 길어 실시간 파이프라인이 지연되는 문제가 발생했습니다.

* 원인
  VLM 추론이 실시간 처리 흐름에 직접 포함되어 있었고,
  후보가 많아질수록 응답 대기 시간이 증가했습니다.

* 해결

  * Prompt 경량화
  * 불필요한 후보 요청 제거
  * VLM 요청을 비동기 Queue로 분리
  * Top-N 후보 제한 적용

* 결과
  VLM 후보 검증 구조는 유지하면서
  실시간 파이프라인이 VLM 응답에 의해 차단되지 않도록 개선했습니다.

#### 2. 행동 기반 이벤트 설계

* 문제
  사람 감지만으로는 단순 통행자와 실제 재고 작업자를 구분하기 어려웠습니다.

* 해결

  * ROI 진입 / 접근 / 이탈 상태 정의
  * 손 뻗기 또는 일정 시간 이상 체류 시 접근 이벤트로 확정
  * 이탈 후 이벤트 구간 Clip 생성
  * 해당 Clip을 VLM 후보 검증에 활용

* 결과
  단순 감지가 아닌 상태 기반 이벤트 구조를 통해
  이벤트 중복과 노이즈를 줄이고 Section 후보 추론 신뢰도를 높였습니다.

---

### 역할

* AI Pipeline 설계 및 구현
* YOLOX 기반 작업자 감지 구조 구현
* MediaPipe Pose 기반 행동 분석 및 Section Scoring 구현
* VLM 기반 후보 검증 및 Candidate Merge 구조 설계
* realtime-skip, FPS 분리, VLM 비동기 Queue 구조 적용
* prediction_log / final_event 기반 평가 구조 설계
* Prometheus / Grafana 기반 모니터링 구축

---

### 성과

* SSAFY 자율 프로젝트 우수상 수상
* 행동 기반 AI Pipeline 구축
* Section 후보 추론 및 VLM 검증 구조 구현
* 실시간 처리와 VLM 검증을 분리한 비동기 구조 설계
* 기업에서 프로토타입 활용을 위해 산출물 인수

---

## 2️⃣ Robot-based Industrial Safety Management Platform

* GitHub : https://github.com/kimde132/safety_dog

4족 보행 로봇이 수집한 데이터를 기반으로
산업 현장의 안전 상태를 실시간으로 모니터링하고,
이벤트 대응 및 원격 제어를 수행할 수 있는 **웹 기반 안전 관제 시스템**입니다.

기존의 수기 점검과 사후 보고 중심의 안전 관리 방식에서 벗어나
**실시간 데이터 기반 자동화된 안전 관리 환경 구축**을 목표로 개발되었습니다.

---

### 해결하려는 문제

* 사람이 직접 위험 구역을 순찰해야 하는 문제
* 고정형 CCTV만으로는 확인하기 어려운 현장 사각지대
* 수기 보고 기반의 비효율적인 안전 관리 방식
* 이벤트 발생 이후 대응 지연

---

### 시스템 구조

Robot
→ MQTT
→ Spring Boot Backend
→ PostgreSQL
→ Next.js Frontend

* Robot → Server : MQTT 기반 실시간 데이터 전송
* Server → Robot : 명령 생성 후 MQTT로 전달
* Server → Frontend : REST API 기반 데이터 조회
* 영상 스트리밍 : MJPEG 기반 실시간 모니터링

---

### 주요 기능

#### 1. 로봇 데이터 수집

* 상태 / 위치 / heartbeat / 이벤트 / PPE 로그 수집
* MQTT 기반 비동기 메시지 처리

#### 2. 실시간 안전 관제

* 이벤트 센터에서 이벤트 조회 및 상태 변경
* 조치 이력 및 이미지 기반 증적 관리

#### 3. 로봇 제어

* 웹에서 명령 생성 → MQTT로 로봇 전달
* 원격 제어 및 상태 확인

#### 4. 모니터링 & 대시보드

* 로봇 상태 / 이벤트 / PPE 통계 시각화
* 주기적 데이터 갱신을 통한 관제 화면 구성

#### 5. 영상 스트리밍

* MJPEG 기반 실시간 영상 모니터링
* Nginx + Backend Proxy 구조

#### 6. 알림 시스템

* 이벤트 발생 / 로봇 오프라인 / 명령 실패 알림
* 웹에서 즉시 확인 및 상세 화면 이동

---

### 데이터 흐름

Robot
→ MQTT
→ Backend
→ DB 저장
→ 알림 생성
→ Frontend 반영

이벤트 발생 시
DB 저장 → 알림 생성 → UI 반영까지 이어지는 구조로 구현했습니다.

---

### 기술 스택

* Frontend : Next.js, React, TypeScript
* Backend : Spring Boot, Spring Security, JPA
* Database : PostgreSQL
* Infra : Docker, Docker Compose, Jenkins, Nginx, AWS EC2
* Communication : MQTT, REST API
* Streaming : MJPEG

---

### 트러블슈팅

#### 1. WebRTC → MJPEG 전환

* 문제
  WebRTC는 낮은 지연 시간 측면에서 장점이 있었지만,
  NAT, 터널링, Docker 네트워크 환경에서 연결 안정성 문제가 발생했습니다.

* 해결
  HTTP 기반으로 구조가 단순하고 브라우저 호환성이 높은
  MJPEG 스트리밍 방식으로 전환했습니다.

* 결과
  제한된 개발 및 시연 환경에서 더 안정적으로
  로봇 카메라 영상을 웹에서 확인할 수 있었습니다.

#### 2. 스트리밍 연결 끊김 문제

* 원인
  Nginx proxy buffering 및 timeout 설정으로 인해
  장시간 스트리밍 연결이 불안정했습니다.

* 해결

  * Nginx buffering off
  * timeout 설정 조정
  * Backend Proxy 구조 정리

* 결과
  장시간 스트리밍 연결 안정성을 개선했습니다.

---

### 역할

* Web 파트 단독 담당
* Frontend / Backend / Infra 전반 구현
* 관제 Dashboard, Robot Monitoring, Event Center, Notification 화면 구현
* Spring Boot 기반 API 및 이벤트 처리 로직 구현
* MQTT 기반 로봇-서버 통신 연동
* MJPEG 스트리밍 구조 구현
* Docker Compose 기반 실행 환경 구성
* Jenkins, Nginx 기반 배포 흐름 구성

---

### 성과

* SSAFY 특화 프로젝트 우수상 수상
* 실제 로봇과 연결되는 Web 관제 시스템 구현
* 하드웨어 이벤트를 웹 관제 화면과 알림으로 연결
* 기능 구현뿐 아니라 시스템 연결 안정성의 중요성을 경험

---

## 3️⃣ OneTake Studio

* GitHub : https://github.com/kimde132/onetakestudio

브라우저에서 방송 화면을 직접 구성하고,
실시간 송출, 녹화, 채팅 연동, 숏폼 생성까지 처리하는
**웹 기반 방송 스튜디오 플랫폼**입니다.

기존 OBS와 같은 별도 프로그램 없이
브라우저만으로 방송 제작 → 송출 → 콘텐츠 재활용까지 연결하는
통합 워크플로를 목표로 개발되었습니다.

---

### 해결하려는 문제

* 라이브 방송과 영상 편집이 분리된 비효율적인 워크플로
* 크리에이터 1인이 기획, 방송, 편집을 모두 수행해야 하는 높은 작업 부담
* 방송 이후 콘텐츠 재활용 과정에 많은 시간과 비용이 드는 문제
* 별도 프로그램 설치와 복잡한 스트리밍 설정의 진입 장벽

---

### 주요 기능

#### 1. 브라우저 기반 방송 스튜디오

* 캔버스 기반으로 카메라, 화면공유, 배너 등을 배치
* Scene 및 레이아웃 구성 기능 제공
* 웹에서 방송 화면을 편집하고 미리보기 가능
* 표시용 캔버스와 송출용 캔버스를 분리하여 UI 렌더링과 실제 방송 출력을 독립적으로 관리

#### 2. 실시간 스트리밍 및 송출

* LiveKit 기반 실시간 스트리밍
* RTMP를 통해 YouTube 등 외부 플랫폼으로 송출
* 방송 시작/종료 및 상태 제어 UI 제공

#### 3. 협업 기반 스튜디오 환경

* 실시간 상태 동기화 기반 협업 편집 지원
* 다수 사용자 편집 상황을 고려한 UI 제공
* 편집 충돌 방지를 위한 Lock 구조 적용

#### 4. 녹화 및 라이브러리 관리

* 방송 녹화 및 저장
* 녹화본 라이브러리 조회 및 다운로드
* 방송 이후 콘텐츠 재활용 가능

#### 5. AI 기반 숏폼 생성

* 녹화 영상 기반 숏폼 생성 기능 제공
* 주요 구간 추출 및 콘텐츠 재활용 흐름 구현
* 생성된 콘텐츠를 라이브러리에서 확인할 수 있는 구조 제공

---

### 시스템 구조

Frontend
→ API Gateway
→ Core Service / Media Service
→ LiveKit
→ RTMP

* 브라우저에서 구성한 송출용 캔버스를 MediaStream으로 변환
* LiveKit을 통해 실시간 방송 스트림으로 전달
* RTMP를 통해 외부 플랫폼으로 송출
* 녹화 및 후처리 후 라이브러리와 AI 기능으로 연계

---

### 기술 스택

* Frontend : Next.js, React, TypeScript
* Canvas : Canvas API, Konva.js
* State Management : Zustand
* Streaming : LiveKit, WebRTC, RTMP
* Realtime : WebSocket
* Video Processing : FFmpeg

---

### 역할

* 프론트엔드 개발
* 캔버스 기반 방송 스튜디오 UI 구현
* Scene 및 레이아웃 편집 기능 개발
* LiveKit 기반 실시간 스트리밍 연결 UI 구현
* 협업 상태 동기화 UI 처리
* 라이브러리 및 에셋 관리 화면 구현
* API 연동 및 상태 관리 구현

---

### 트러블슈팅

#### 1. 반응형 캔버스 좌표 문제

* 문제
  브라우저 크기 변경 시 요소 위치와 크기가 어긋나는 문제가 발생했습니다.

* 원인
  화면 크기는 변하지만 소스 배치 정보는 별도 좌표 체계로 관리되어
  실제 렌더링 좌표와 불일치가 발생했습니다.

* 해결

  * 소스 위치와 크기를 정규화 좌표로 저장
  * 화면 크기에 맞게 픽셀 좌표로 변환
  * 레이아웃 변경 시 동일한 기준으로 재계산

* 결과
  다양한 해상도에서도 비교적 일관된 방송 화면을 유지할 수 있었습니다.

---

### 핵심 구현 포인트

* 브라우저에서 단순 미리보기 화면만 그린 것이 아니라,
  표시용 캔버스와 송출용 캔버스를 분리해 UI와 실제 방송 출력 경로를 독립적으로 관리
* 브라우저에서 구성한 송출용 캔버스를 MediaStream으로 변환
* MediaStream을 LiveKit과 RTMP 송출 흐름에 연결
* 실시간 협업 환경에서 상태 동기화 및 편집 충돌 제어
* 녹화 → 라이브러리 → 숏폼 생성으로 이어지는 콘텐츠 재활용 흐름 구현

---

### 성과

* OBS와 유사한 방향의 웹 기반 방송 스튜디오 구조 구현
* 실시간 송출 및 녹화를 포함한 통합 방송 워크플로 구축
* 방송 이후 콘텐츠 재활용까지 고려한 서비스 구조 구현
* 브라우저 기반 미디어 파이프라인 설계 경험

---

## 4️⃣ PULSE

* GitHub : https://github.com/kimde132/PULSE

주식 시장의 감정을 데이터로 분석하여
투자 의사결정을 돕는 **감정 기반 주식 분석 서비스**입니다.

온라인 투자 커뮤니티, 뉴스, 유튜브 등 다양한 데이터를 분석하여
시장 심리를 정량화한 감정 점수를 제공합니다.

---

### 해결하려는 문제

* 개인 투자자의 뇌동매매
* 정보 과부하로 인한 의사결정 어려움
* 확증 편향에 따른 투자 판단 오류
* 뉴스, 커뮤니티, 영상 등 흩어진 정보의 맥락 파악 어려움

---

### 주요 기능

* 시장 감정 점수 대시보드
* 주가와 감정 점수의 상관관계 시각화
* 사용자 포트폴리오 분석
* 동일 업종 인기 종목 추천
* 뉴스, 유튜브, 커뮤니티 데이터 수집
* 종목별 감성 분석 결과 제공

---

### 데이터 소스

* 투자 커뮤니티 댓글
* 경제 뉴스 기사
* 유튜브 영상 콘텐츠

---

### 핵심 기술

* KoFinBERT 기반 금융 특화 감성 분석
* Django REST API
* Vue SPA 기반 대시보드
* Selenium 기반 데이터 크롤링
* Chart.js 기반 데이터 시각화

---

### 기술 스택

* Frontend : Vue.js, Chart.js
* Backend : Django, Django REST Framework
* AI : KoFinBERT
* Crawling : Selenium
* API : YouTube Data API, Naver News API

---

### 역할

* Frontend / Backend 개발
* 외부 데이터 수집 API 연동
* Selenium 기반 커뮤니티 데이터 크롤링
* KoFinBERT 감성 분석 결과 연동
* 종목 상세 및 포트폴리오 화면 구현
* 감정 점수 및 포트폴리오 비중 시각화

---

### 성과

* 뉴스, 유튜브, 커뮤니티 데이터를 통합 수집하는 데이터 파이프라인 구현
* KoFinBERT 기반 감성 분석 결과를 서비스 화면에 연동
* 수집 → 분석 → 시각화로 이어지는 전체 흐름 경험
* 비정형 데이터를 투자 인사이트로 변환하는 서비스 구조 구현

---

# 📚 Study

* 알고리즘 문제 풀이
* 운영체제 스터디
* 웹 개발 및 AI 기초 학습

---

# 🎯 Interests

* 🏃 러닝 (10km / 하프 마라톤)
* 🥃 위스키
* 📷 사진 촬영

---

# 📫 Contact

* GitHub : https://github.com/kimde132
* Email : [kimde1852@gmail.com](mailto:kimde1852@gmail.com)
