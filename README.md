# Cowart

Cowart는 Codex에서 쓰는 로컬 무한 캔버스 플러그인입니다. tldraw 기반 캔버스를 열어 이미지 기획, 주석, AI 이미지 생성, 주석 기반 수정 작업을 한 화면에서 처리합니다.

캔버스 데이터는 플러그인 저장소가 아니라 현재 작업 중인 프로젝트의 `canvas/` 폴더에 저장됩니다.

영문 README: [README.en.md](README.en.md)

## 주요 기능

- Codex에서 로컬 tldraw 캔버스 열기
- 프로젝트별 캔버스 페이지와 이미지 asset 저장
- AI image holder를 선택해 이미지 생성 결과 삽입
- Cowart 주석 스크린샷을 기준으로 수정본 생성
- 원본과 수정본을 캔버스에 나란히 배치
- MCP 도구로 선택 상태 읽기, 이미지 삽입, 캔버스 저장

## 설치

### Codex로 설치

Codex에 아래처럼 요청하면 됩니다.

```text
git@github.com:bear2u/Cowart.git 에서 Cowart Codex 플러그인을 설치해줘.
~/plugins/cowart 에 clone 하고, npm install / npm run build 후
personal marketplace에 등록해서 cowart@personal 로 설치해줘.
```

### 수동 설치

```bash
mkdir -p ~/plugins
git clone git@github.com:bear2u/Cowart.git ~/plugins/cowart
cd ~/plugins/cowart
npm install
npm run build
```

`~/.agents/plugins/marketplace.json`에 Cowart 항목이 없다면 추가합니다.

```json
{
  "name": "personal",
  "interface": {
    "displayName": "Personal"
  },
  "plugins": [
    {
      "name": "cowart",
      "source": {
        "source": "local",
        "path": "./plugins/cowart"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

등록 후 설치합니다.

```bash
codex plugin marketplace add ~
codex plugin add cowart@personal
```

설치 후 새 Codex 대화를 열면 Cowart skill과 MCP 도구가 로드됩니다.

## 사용법

### 캔버스 열기

Codex에 이렇게 말합니다.

```text
Open the Cowart canvas for this project.
```

기본 주소:

```text
http://127.0.0.1:43217/
```

프로젝트에는 아래 파일들이 생성됩니다.

```text
canvas/pages/<page-id>/cowart-canvas.json
canvas/pages/<page-id>/assets/
```

![Cowart 캔버스 열기](assets/open-canvas.png)

### 이미지 생성

1. Cowart 캔버스를 엽니다.
2. 캔버스에서 AI image holder를 만들고 선택합니다.
3. Codex에 생성할 이미지를 설명합니다.

```text
Generate a new image into the selected Cowart AI image holder.
```

Codex가 선택된 holder 비율에 맞춰 이미지를 생성하고 캔버스에 넣습니다.

![Cowart 이미지 생성](assets/generate-image.png)

### 주석 기준 수정본 만들기

1. Cowart 캔버스에서 원본 이미지 위에 주석을 답니다.
2. 주석이 보이는 화면을 캡처해서 Codex에 첨부합니다.
3. Codex에 이렇게 요청합니다.

```text
Use my Cowart annotation screenshot to generate a clean revised image beside the original.
```

Codex는 주석과 화살표를 해석해 수정본을 만들고, 원본 옆에 배치합니다. 원본과 주석은 삭제하지 않습니다.

![Cowart 주석 기반 수정](assets/annotation-edit.png)

## Skill

- `cowart:cowart-open-canvas`: 현재 프로젝트의 Cowart 캔버스 열기
- `cowart:cowart-image-gen`: 선택된 AI image holder에 생성 이미지 삽입
- `cowart:cowart-image-edit`: 주석 스크린샷 기준으로 수정 이미지 생성

## 개발

```bash
npm install
npm run dev
npm run build
```

프로젝트를 지정해 캔버스 서버를 직접 실행할 수도 있습니다.

```bash
./scripts/start-canvas.sh /path/to/project
```

환경 변수:

- `COWART_PORT`: 로컬 캔버스 포트, 기본값 `43217`
- `COWART_PROJECT_DIR`: 캔버스를 저장할 프로젝트 경로
- `COWART_CANVAS_DIR`: 캔버스 데이터 경로, 기본값 `$COWART_PROJECT_DIR/canvas`

## 저장소

GitHub: [bear2u/Cowart](https://github.com/bear2u/Cowart)

## 감사

Cowart의 캔버스 기능은 [tldraw](https://github.com/tldraw/tldraw)를 기반으로 합니다.
