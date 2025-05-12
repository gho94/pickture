# Pickture - 인스타그램 스타일 SNS 플랫폼

## 프로젝트 개요
사진 공유 및 소셜 네트워킹을 위한 SNS 앱(피드, 좋아요, 댓글, 팔로우 기능 제공)
- **프로젝트 기간:** 2024.12.18 ~ 2025.01.02
- **인원:** 3명
- **주요 역할:**  
  - 인스타그램 기능 및 UI 분석
  - 피드 관련 화면 및 기능 구현
  - 좋아요, 댓글 기능 구현
  - 사용자 프로필 및 팔로우, 팔로잉 기능 구현

---

## 주요 기능
- **사용자 인증**
  - 이메일/비밀번호, Google 소셜 로그인
  - 사용자 프로필 관리
- **사진 공유**
  - 갤러리/카메라 사진 업로드
  - 실시간 피드, 좋아요, 댓글
- **팔로우/팔로잉**
  - 사용자 간 팔로우/팔로잉
  - 팔로우 기반 피드 제공

---

## 기술 스택
- **Framework**: Flutter
- **상태관리**: Riverpod
- **주요 패키지**:
  - firebase_auth: 사용자 인증
  - cloud_firestore: 데이터베이스
  - go_router: 화면 간 라우팅 및 네비게이션 관리
  - image_picker: 이미지 선택
  - firebase_storage: 이미지 저장

---

## 프로젝트 구조

```
lib/
├── core/           # 앱 상수, 테마 및 스타일 가이드, 에러 처리
├── models/         # 데이터 모델
├── screens/        # 화면 UI (View)
├── widgets/        # 재사용 가능한 위젯
├── providers/      # 상태 관리 (ViewModel)
├── services/       # 비즈니스 로직
├── repositories/   # 데이터 접근 계층 (Firebase 등 외부 데이터와 통신)
├── utils/          # 유틸리티 함수 및 공통 기능
```

---

## 아키텍처

이 프로젝트는 MVVM (Model-View-ViewModel) 아키텍처 패턴을 따릅니다:

- **Model** 데이터 구조 정의 (models/)
- **View** UI 컴포넌트 (screens/, widgets/)
- **ViewModel** 비즈니스 로직 및 상태 관리 (providers/)

---

## 디자인 시스템
- **테마 시스템**: Material Design 3 기반의 커스텀 테마 적용
  - 앱 전체에 일관된 색상(Color) 팔레트 적용 (주색, 배경, 텍스트, 구분선 등)
  - 다양한 텍스트 스타일(타이포그래피) 계층 구조 제공 (제목, 본문, 라벨 등)
  - 버튼, 카드, 입력창 등 주요 컴포넌트의 스타일 일관성 유지
  - 라운드, 마진, 패딩 등 UI 요소의 공통 스타일(AppStyles)로 관리

---

## 주요 화면
- 로그인/회원가입: 사용자 인증
- 메인 피드: 사진 피드 및 상호작용
- 프로필: 사용자 정보 및 게시물 관리
- 채팅 화면: 실시간 1:1 채팅
- 알림: 사용자 활동 알림

---

## 트러블슈팅
- **관계형 vs NoSQL 데이터베이스 설계**
  - RDBMS라면 User, Post, Like, Comment 등 테이블을 조인하여 구현하지만, Firebase(NoSQL)는 컬렉션/서브컬렉션 구조로 설계해야 했음.
  - 피드, 좋아요, 댓글 등 관계형 데이터를 효율적으로 조회하기 위해 컬렉션 내에 리스트 필드로 중첩 구조화하여 단일 쿼리로 관련 데이터를 조회할 수 있도록 설계.
  - User, Post, Like, Comment 등에서 userID만 저장하고, 필요한 경우 Users 컬렉션을 한 번만 조회해 userMap을 만들어 캐싱 처리하여 중복 조회를 최소화함.
  - 데이터 구조 설계와 쿼리 최적화, 캐싱 전략을 통해 성능 저하와 읽기 비용을 절감함.

- **Firebase 데이터 조회 및 저장 이슈**
  - posts 컬렉션 하위에 userID별로 post 서브컬렉션을 두는 구조에서, 하위 필드가 없을 때 값을 읽지 못하는 문제 발생 → **createdAt 필드를 추가해 해결.**
  - post, like, comment 등에서 userID만 저장하므로, user 정보 업데이트 시 Users 컬렉션을 여러 번 조회해야 하는 비효율 발생 → **userID만 추출해 한 번에 userMap을 구성, Post 리스트를 순회하며 user 정보를 업데이트하는 방식으로 개선.**

---

## 프로젝트 회고
- Firebase 기반 NoSQL 데이터베이스 구조 설계의 어려움을 직접 경험하며, 관계형과의 차이를 체감함.
- 데이터 구조와 쿼리 효율화, 캐싱 전략 등 실무적인 문제 해결 경험을 쌓음.
- 더 직관적이고 효율적인 구조로 설계할 수 있었을 것 같아 아쉬움이 남는 프로젝트였음.

---

## 프로젝트 캡쳐 이미지

<img src="https://github.com/user-attachments/assets/21fb6c4d-78f5-473f-8d2f-9e92db3c878c" width="20%">
<img src="https://github.com/user-attachments/assets/7a4a5b39-4c54-40ac-bb18-c3cd9287cd8f" width="20%">
<img src="https://github.com/user-attachments/assets/1d1db374-479d-4ad9-9575-29b0ba2328f5" width="20%">
<img src="https://github.com/user-attachments/assets/e662a4e1-3fcd-4bd6-957c-4af17f5ebbb2" width="20%">
<img src="https://github.com/user-attachments/assets/84431003-cb81-4304-93b4-6db815345e66" width="20%">
<img src="https://github.com/user-attachments/assets/1c5ff03c-24ed-4947-b00c-62bbdba6e083" width="20%">
<img src="https://github.com/user-attachments/assets/c76c4437-cef6-44da-b61c-91fec40a9396" width="20%">
<img src="https://github.com/user-attachments/assets/b8c48ff9-835c-44cb-8720-59171137ccfc" width="20%">
