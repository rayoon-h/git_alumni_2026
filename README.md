# 걸스인텍 × IWE 분기별 회고 사이트

16명이 동시에 기록을 남기고 실시간으로 함께 보는 분기별 회고 웹사이트입니다.
GitHub Pages + Firebase Realtime Database 조합으로 **완전 무료**로 운영됩니다.

---

## 🚀 빠른 배포 가이드 (10분이면 끝나요)

### 1단계 — Firebase 프로젝트 만들기

1. https://console.firebase.google.com 접속 → Google 계정으로 로그인
2. **프로젝트 추가** → 이름 자유롭게 (예: `gitkor-iwe-retro`) → 다음 → 다음 → 만들기
3. 좌측 메뉴 **빌드 → Realtime Database** → **데이터베이스 만들기**
   - 위치: `asia-southeast1` (싱가포르) 또는 가까운 곳
   - **테스트 모드로 시작** 선택 → 사용 설정
4. 데이터베이스가 만들어지면 상단 **규칙(Rules)** 탭으로 이동해 아래로 교체 후 **게시(Publish)**:
   ```json
   {
     "rules": {
       "q2": { ".read": true, ".write": true },
       "q3": { ".read": true, ".write": true },
       "q4": { ".read": true, ".write": true }
     }
   }
   ```
   > 16명 비공개 그룹용이라 단순 공개 규칙이에요. 더 강하게 하고 싶으면 익명 인증을 켜고 `.read/.write`를 `auth != null` 로 바꾸세요.

### 2단계 — 웹 앱 등록하고 config 키 받기

1. Firebase 콘솔 좌측 상단 **⚙️ 톱니바퀴 → 프로젝트 설정**
2. **내 앱** 섹션에서 **`</>` (웹) 아이콘** 클릭
3. 앱 별명 입력 → **앱 등록**
4. 화면에 뜨는 `firebaseConfig` 객체를 복사해두세요. 아래 모양입니다:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSyXXXXX...",
     authDomain: "your-project.firebaseapp.com",
     databaseURL: "https://your-project-default-rtdb.asia-southeast1.firebasedatabase.app",
     projectId: "your-project",
     storageBucket: "your-project.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abcdef..."
   };
   ```
   > **⚠️ databaseURL이 화면에 안 보이면** Realtime Database 페이지로 가서 상단 URL을 복사해 직접 넣으세요.

### 3단계 — index.html에 config 붙여넣기

`index.html` 을 열어 `// ====== Firebase 설정 ======` 아래의 `firebaseConfig` 블록을 방금 복사한 값으로 **통째로 교체**합니다.

### 4단계 — GitHub에 올리기

1. GitHub에서 **새 저장소(Repository) 생성** (예: `quarterly-retro`) → Public
2. 이 폴더의 `index.html` 을 저장소에 업로드 (드래그 앤 드롭으로 업로드 가능)
3. 저장소 **Settings → Pages**
   - Source: **Deploy from a branch**
   - Branch: **main / (root)** → Save
4. 1~2분 후 `https://<your-username>.github.io/<repo-name>/` 주소가 활성화돼요.
5. 16명에게 이 주소만 공유하면 끝!

---

## 📋 사이트가 하는 일

| 기능 | 설명 |
|------|------|
| 1분기 탭 | 1/12 미팅에서 모은 11명의 회고를 카드로 깔끔하게 표시 |
| 2분기 탭 | 16명이 각자 폼에 기록 → 실시간으로 모두에게 동기화 |
| 트렌드 요약 | 입력된 키워드와 본문 단어 빈도로 이번 분기 핫 키워드 자동 추출 |
| 참여율 | 16명 중 몇 명이 제출했는지 한눈에 |
| 3·4분기 | 빈 탭으로 미리 준비. 같은 폼 구조 복제만 하면 사용 가능 |

---

## 🛠 자주 묻는 것

**Q. Firebase 무료로 정말 충분한가요?**
A. 네. 무료 Spark 요금제는 동시접속 100명, 월 10GB 다운로드까지 지원합니다. 16명 텍스트 기록은 평생 무료 한도 안에 있어요.

**Q. 누가 장난으로 데이터를 지우면요?**
A. Firebase 콘솔 **Realtime Database → 데이터** 탭에서 모든 입력이 보입니다. 필요하면 거기서 직접 복구 가능해요. 더 강한 보호가 필요하면 익명 인증으로 규칙을 바꾸세요.

**Q. 입력 후 새로고침하면 사라져요!**
A. Firebase config가 아직 `YOUR_API_KEY` 그대로일 거예요. 3단계를 다시 확인하세요. 화면 위쪽에 노란색 경고가 떠요.

**Q. 1분기 데이터를 수정하고 싶어요.**
A. `index.html` 의 `const q1Data = [...]` 배열을 직접 편집한 뒤 다시 푸시하면 됩니다.

**Q. 3·4분기 폼은 어떻게 만들죠?**
A. 2분기 패널 코드를 복사해 `panel-q3`, `panel-q4` 안에 붙여넣고, ID 와 Firebase 경로(`q2/entries` → `q3/entries`)만 바꿔주세요. 분기마다 별도로 작업하기 어려우시면 그때 다시 도와드릴게요.

---

## 💡 16명 운영 팁

- 미팅 시작 전 링크와 함께 "이름은 본명/별명 통일해주세요" 안내
- 화면 공유로 트렌드 요약 띄워두면 회고가 훨씬 풍성해져요
- 미팅 끝나고 캡처해서 노션/슬랙에 아카이브하면 연말 회고 때 자료 됩니다

만들 때 막히면 언제든 다시 말씀해주세요 🙌
