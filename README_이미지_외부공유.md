# 이미지·HTML 외부 공유 (GitHub 기준)

로컬 이미지를 서버에 올려 외부에서 접근 가능하게 하려면 **GitHub 저장소 + GitHub Pages** 조합이 가장 단순하다. 아래는 **하나하나 따라 할 수 있는** 단계 정리다. 유료·대안은 뒤쪽 섹션에 있다.

---

## 1. GitHub만 사용 (무료) — 요약

- **GitHub Pages**를 켜서, 프로토타입 폴더(HTML + 이미지)를 통째로 올리면 된다.
- 접속 주소 예: `https://<내계정>.github.io/<저장소이름>/`
- 이미지도 같은 저장소에 두면  
  `https://<내계정>.github.io/<저장소이름>/chzzk-silver.png` 처럼 외부에서 링크 가능.
- **비용**: 없음. 다만 **공개 저장소**면 코드·이미지가 모두 공개된다.
- 유료까지 고려할 때는: GitHub는 “소스/이미지 보관”용으로 두고, 배포는 **GitHub Pages(무료)** 또는 **Cloudflare Pages / Vercel / Netlify**(무료 티어)에 저장소 연동해서 쓰면 된다. 이미지 전용으로 더 안정된 URL이 필요하면 **Cloudflare R2**, **AWS S3 + CloudFront** 같은 유료 스토리지+CDN을 붙여 확장할 수 있다.

자세한 내용은 아래 단계를 그대로 따라 하면 된다.

---

## 2. 실제로 쓰는 순서 (GitHub Pages — 단계별)

### 2.1 준비물

- **GitHub 계정**. 없으면 [github.com](https://github.com) 에서 가입.
- **로컬에 Git 설치 여부 확인**. 터미널에서 `git --version` 입력 후 버전이 나오면 OK.
- **올릴 폴더**: `WORK_COLLECTION_nonproduct` 안의 다음 내용.
  - HTML 파일들 (예: `비상품_보증서_치지직_실버버튼.html`, `비상품_보증서_해피빈_기부증서.html` 등 전부)
  - 이미지 파일 (예: `chzzk-silver.png`)
  - (선택) `비상품_보증서_UI_프로토타입.html` — 목차용
  - (선택) `README_이미지_외부공유.md` — 공유용 설명

**중요**: HTML에서 이미지 경로가 `chzzk-silver.png` 처럼 **상대 경로**로 되어 있으면, 저장소 루트에 같은 이름으로 이미지를 두기만 하면 된다. 수정할 필요 없다.

---

### 2.2 1단계: GitHub에 새 저장소 만들기

1. GitHub에 로그인한 뒤 우측 상단 **+** → **New repository** 클릭.
2. **Repository name**에 저장소 이름 입력.  
   예: `collection-nonproduct-cert` (영문·숫자·하이픈만 권장).
3. **Description**은 선택. 예: `비상품 보증서 UI 프로토타입`.
4. **Public** 선택 (무료로 Pages 쓰려면 공개가 보통 필요).
5. **Add a README file** 체크는 **하지 않는다**.  
   “빈 저장소”로 두고, 아래에서 로컬 폴더를 통째로 올릴 예정이기 때문.
6. **Create repository** 클릭.
7. 생성된 페이지에 나오는 **빈 저장소 안내**(예: “quick setup”, “create a new repository on the command line”)는 무시해도 된다. 아래 2.3에서 로컬에서 올린다.

---

### 2.3 2단계: 로컬에서 올릴 폴더 준비

로컬에서 **저장소로 쓸 폴더 하나**를 정한다. 두 가지 방법이 있다.

**방법 A — 기존 폴더를 그대로 저장소로 쓰기 (이미 Git으로 관리 중이 아닐 때)**

1. `WORK_COLLECTION_nonproduct` 폴더로 이동한다.  
   터미널 예시 (경로는 본인 환경에 맞게 바꾼다):
   ```bash
   cd "/Users/user/Library/CloudStorage/WORKS드라이브-jungseob.lee@navercorp.com/내 드라이브/Clark's works drive_from 2026/00. AI/WORK_COLLECTION/WORK_COLLECTION_nonproduct"
   ```
2. 이 폴더가 아직 Git 저장소가 아니면 초기화한다:
   ```bash
   git init
   ```
3. 올릴 파일만 추가한다 (전부 올리려면):
   ```bash
   git add *.html chzzk-silver.png
   ```
   또는 이미지가 여러 개면:
   ```bash
   git add *.html *.png
   ```
   README도 올리려면:
   ```bash
   git add README_이미지_외부공유.md
   ```
4. 첫 커밋을 만든다:
   ```bash
   git commit -m "비상품 보증서 프로토타입 + 이미지"
   ```
5. 브랜치 이름을 `main`으로 맞춘다 (GitHub 기본값):
   ```bash
   git branch -M main
   ```
6. GitHub에서 만든 저장소를 원격(remote)으로 추가한다.  
   `<username>`은 본인 GitHub 아이디, `<repo-name>`은 2.2에서 만든 저장소 이름으로 바꾼다:
   ```bash
   git remote add origin https://github.com/<username>/<repo-name>.git
   ```
   예: `git remote add origin https://github.com/jungseoblee/collection-nonproduct-cert.git`
7. 푸시한다:
   ```bash
   git push -u origin main
   ```
   브랜치를 처음 푸시할 때만 `-u origin main`을 쓰면 되고, 이후에는 `git push`만 해도 된다.

**방법 B — 새 폴더를 만들어 복사한 뒤 올리기**

1. 빈 폴더 하나를 만든다. 예: `collection-nonproduct-cert`.
2. `WORK_COLLECTION_nonproduct` 안에서 **올리고 싶은 파일만** 복사한다.  
   예: 모든 `*.html`, `chzzk-silver.png`, (선택) `README_이미지_외부공유.md`.
3. 그 빈 폴더로 이동한 뒤:
   ```bash
   cd /경로/collection-nonproduct-cert
   git init
   git add .
   git commit -m "비상품 보증서 프로토타입 + 이미지"
   git branch -M main
   git remote add origin https://github.com/<username>/<repo-name>.git
   git push -u origin main
   ```
   `<username>`, `<repo-name>`은 위와 같이 본인 값으로 바꾼다.

**한글/공백 경로**: 터미널에서 경로에 한글이나 공백이 있으면 경로 전체를 따옴표로 감싼다.  
예: `cd "/Users/user/.../WORK_COLLECTION_nonproduct"`

---

### 2.4 3단계: GitHub Pages 켜기

1. GitHub 웹에서 해당 **저장소 페이지**로 간다.
2. 상단 메뉴에서 **Settings** 클릭.
3. 왼쪽 사이드바에서 **Pages** 클릭.  
   (Code and automation → Pages)
4. **Build and deployment** 섹션에서:
   - **Source**: **Deploy from a branch** 를 선택한다.
   - **Branch**: `main` (또는 푸시한 브랜치) 선택.
   - **Folder**: **/ (root)** 선택.
5. **Save** 클릭.
6. 상단에 노란색 안내가 뜨면 “GitHub Pages is currently building…” 같은 문구가 보일 수 있다.  
   **몇 분(보통 1~3분)** 기다린다.

---

### 2.5 4단계: 배포 확인 및 URL 정리

1. 같은 **Settings → Pages** 화면을 새로고침한다.
2. 상단에 **Your site is live at …** 로 시작하는 초록색 박스가 뜨면 성공이다.  
   주소 형식: `https://<username>.github.io/<repo-name>/`
3. 브라우저에서 해당 주소로 접속해 본다.
   - **목차(인덱스) 페이지**를 올렸다면:  
     `https://<username>.github.io/<repo-name>/비상품_보증서_UI_프로토타입.html`  
     또는 `.../` 만 열어서 나오는 페이지를 확인.
   - **실버버튼 페이지 직접 열기**:  
     `https://<username>.github.io/<repo-name>/비상품_보증서_치지직_실버버튼.html`
4. **이미지 외부 공유용 URL** (다른 문서·채팅에 붙여넣을 때):
   - 이미지: `https://<username>.github.io/<repo-name>/chzzk-silver.png`
   - HTML은 그대로 상대 경로 `chzzk-silver.png` 를 쓰면 되므로, 위 저장소 구조만 맞추면 수정할 필요 없다.

---

### 2.6 5단계: 이후 수정 반영하기

로컬에서 HTML·이미지를 수정한 뒤 다시 배포하려면:

1. 해당 폴더에서:
   ```bash
   git add .
   git commit -m "수정 내용 요약"
   git push
   ```
2. GitHub Pages는 **자동으로 다시 빌드**한다. 1~2분 뒤 새 내용이 반영된다.  
   (Settings → Pages 에서 “last deployed” 시간으로 확인 가능)

---

## 3. 이미지만 저장소에 두고 Raw URL로 쓰는 방법 (비권장)

- 이미지만 넣은 저장소를 만들고, 파일의 **Raw** 버튼으로 나오는 URL을 복사해 다른 HTML의 `img src`에 넣는 방식이다.
- 예: `https://raw.githubusercontent.com/<username>/<repo>/main/chzzk-silver.png`
- **단점**: GitHub는 raw URL을 CDN 용도로 권장하지 않으며, 요청 제한·캐시 정책이 바뀔 수 있고, 외부 페이지에서 불러올 때 **CORS 제한**이 걸릴 수 있다.
- **정리**: 가끔 내부에서만 쓸 이미지에만 쓰고, **외부 공유용에는 2번처럼 Pages로 HTML과 이미지를 같이 배포**하는 쪽이 낫다.

---

## 4. GitHub + 무료/유료 호스팅 (추가 옵션)

- **GitHub**: 소스·이미지 **보관**용.
- **호스팅**: 같은 저장소를 연동해 자동 배포하는 서비스를 쓰면, 동일한 구조로 다른 URL을 쓸 수 있다.

| 서비스 | 설명 | 비용 |
|--------|------|------|
| **GitHub Pages** | 2번과 동일. 저장소 푸시만 하면 `username.github.io/repo` 에 배포. | 무료(공개 repo) |
| **Cloudflare Pages** | GitHub 연동 후, 빌드 없이 정적 사이트 배포. 커스텀 도메인 가능. | 무료 티어 넉넉함 |
| **Vercel** | GitHub 연동 후 정적 사이트 즉시 배포. | 무료 티어 있음 |
| **Netlify** | 동일. 드래그 앤 드롭 배포도 가능. | 무료 티어 있음 |

**권장**:  
- “GitHub만 쓴다” → **GitHub Pages**만 켜도 충분하다.  
- 나중에 도메인 연결·트래픽·보안을 더 쓰고 싶다면 **Cloudflare Pages**나 **Vercel**을 GitHub 연동해서 쓰면 된다.

### 4.1 Cloudflare Pages로 배포하는 흐름 (요약)

1. [pages.cloudflare.com](https://pages.cloudflare.com) 에서 **Create a project** → **Connect to Git**.
2. GitHub 권한 허용 후, **저장소 선택** (위에서 만든 `collection-nonproduct-cert` 등).
3. **Build settings**:
   - Framework preset: **None**.
   - Build command: 비움.
   - Build output directory: **/** 또는 `/` (루트를 그대로 배포).
4. **Save and Deploy** 후, 나오는 URL 예: `https://<random>.pages.dev` 또는 연결한 커스텀 도메인.  
   이미지·HTML 경로는 GitHub와 동일하게 상대 경로로 동작한다.

### 4.2 Vercel / Netlify

- **Vercel**: [vercel.com](https://vercel.com) → **Add New** → **Project** → GitHub 저장소 선택 → Import. 정적 사이트면 빌드 설정 없이 루트 배포.
- **Netlify**: [netlify.com](https://netlify.com) → **Add new site** → **Import an existing project** → GitHub 연결 → 저장소 선택 → Publish. 마찬가지로 루트를 배포하면 된다.

---

## 5. 이미지만 별도로 안정 URL 쓰고 싶을 때 (유료 확장)

- **GitHub Pages**: 이미지만 넣은 저장소에 Pages를 켜서 이미지 URL을 쓰는 방법도 있다 (2번과 같은 방식, 폴더에 이미지만 넣어도 됨).
- **더 안정적인 URL이 필요하면**:
  - **Cloudflare R2**, **AWS S3 + CloudFront** 등에 이미지를 올리고 **공개 URL**을 부여하는 방식.
  - 이때는 HTML은 그대로 GitHub Pages(또는 Cloudflare Pages 등)에 두고, `img src`만 R2/S3(또는 CloudFront) URL로 바꾸면 된다.
  - 설정·과금은 각 서비스 문서를 참고하면 된다.

---

## 6. 자주 걸리는 문제 (트러블슈팅)

| 현상 | 확인·조치 |
|------|------------|
| **404 / There isn’t a GitHub Pages site here** | Pages를 방금 켰다면 1~3분 기다리기. Source가 **Deploy from a branch**, Branch·Folder 설정이 맞는지 확인. |
| **이미지가 안 나온다** | HTML과 이미지가 **같은 저장소·같은 브랜치**에 있고, 파일명(대소문자 포함)이 `chzzk-silver.png` 등과 일치하는지 확인. 상대 경로 `chzzk-silver.png` 로 두었는지 확인. |
| **한글 파일명이 깨진다** | 브라우저 주소창에 한글 경로가 인코딩되어 보일 수 있음. 링크할 때는 전체 URL을 그대로 쓰면 된다. |
| **push 시 인증 실패** | GitHub에서 비밀번호 대신 **Personal Access Token** 사용. Settings → Developer settings → Personal access tokens 에서 생성 후, 비밀번호 자리에 토큰 입력. |
| **공개가 부담된다** | 비공개 저장소로 두면 GitHub Pages는 **유료 플랜**에서만 사용 가능. 무료로 쓰려면 공개 저장소가 필요하다. |

---

## 7. 한 줄 요약

- **지금 바로 쓰려면**: 새 GitHub 저장소 만들고 → `WORK_COLLECTION_nonproduct` 내용(HTML + `chzzk-silver.png` 등) 푸시 → **Settings → Pages** 에서 Source를 해당 브랜치·**/ (root)** 로 설정 → 몇 분 후 `https://<username>.github.io/<repo-name>/` 로 접속. 이미지 URL은 `https://<username>.github.io/<repo-name>/chzzk-silver.png` 형태로 외부 공유 가능.
- 자세한 단계는 **2.1~2.6**을 순서대로 따라 하면 된다.

---

*작성: 2026-03-08, 보강: 2026-03-08*
