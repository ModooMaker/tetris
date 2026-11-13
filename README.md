# ModooMaker/tetris — 원작 Tetris 시뮬레이터 Fork

Eugene Tiurin의 [ytiurin/tetris](https://github.com/ytiurin/tetris)를 Fork한 저장소입니다. 원작은 1984년 Tetris의 분위기를 문자열 화면과 효과음으로 재현한 브라우저 게임입니다. 게임 엔진과 아래 기능을 이 조직의 독자적인 구현 성과로 소개하지 않습니다.

## 목적과 주요 기능

원작은 [DVK-2에서 실행한 게임 영상](https://www.youtube.com/watch?v=O0gAgQQHFcQ)을 참고해 텍스트 기반 게임을 브라우저로 옮겼습니다.

- 문자열 프레임으로 표현하는 게임판·점수·레벨
- 키보드·화면 버튼을 통한 회전·이동·낙하·일시정지
- Web Audio 효과음과 음소거
- localStorage의 개인 점수·이름 보관
- Service Worker 자산 캐시와 웹 앱 manifest
- 외부 점수 서버에 대한 순위 조회·등록 클라이언트

점수 서버 자체는 이 저장소에 없습니다. 외부 순위 기능과 오프라인 동작이 현재 브라우저에서 정상 작동하는지는 검증하지 않았습니다.

## 화면과 데모

다음 이미지는 원작 코드와 함께 포함된 게임 화면입니다.

![원작 게임 화면](public/screenshot.png)

이 Fork는 GitHub Pages가 비활성화되어 있습니다. [원작 플레이 페이지](https://ytiurin.github.io/tetris/)와 [원작 데모 GIF](public/demo.gif)는 원작 참고 자료이며 이 저장소의 배포 결과가 아닙니다.

## 기술 스택과 구조

| 파일 | 역할 |
|---|---|
| [src/tetris.js](src/tetris.js) | 게임 상태·프레임 생성·입력 처리 |
| [src/play.js](src/play.js) | 화면·컨트롤·점수 저장·외부 순위 요청 |
| [src/audio.js](src/audio.js) | Web Audio 효과음 |
| [gulpfile.js](gulpfile.js) | HTML/CSS/JS 압축 및 public 자산 생성 |
| [sw.js](sw.js), [manifest.json](manifest.json) | 브라우저 캐시·웹 앱 설정 |

JavaScript 엔진이 `nextFrame` 이벤트로 문자열을 전달하고 표시 코드가 이를 HTML로 바꾸는 구조입니다. 게임 상태와 브라우저 표시·입력·오디오를 나눕니다. 이 설계는 원작의 구현입니다.

## 설치와 실행

저장소에는 `index.html`과 생성된 `public/` 파일이 포함되어 있으므로, 확인만 할 때는 npm 설치나 재빌드 없이 정적 서버로 제공할 수 있습니다.

```bash
git clone https://github.com/ModooMaker/tetris.git
cd tetris
python3 -m http.server 5173 --bind 127.0.0.1
```

Python 3으로 서버를 실행한 뒤 `http://127.0.0.1:5173/`에서 엽니다. 기존 [package.json](package.json)에는 `http-server`를 사용하는 `npm start`도 있습니다. 소스를 다시 묶는 Gulp 기본 작업은 `index.html`과 생성 파일을 덮어쓰므로 단순 열람에는 필요하지 않습니다.

## 검증과 현재 한계

- `npm test`는 “no test specified”를 출력하고 실패하는 기본 스크립트입니다. 자동 게임 회귀 테스트나 CI workflow는 없습니다.
- 점수 클라이언트는 과거 외부 API 주소를 사용합니다. 서버의 현재 접근 가능성·저장·순위 응답을 보장할 수 없습니다.
- Service Worker와 Web Audio 구현은 있지만 현재 브라우저의 설치·오프라인·오디오 정책에 대한 검증은 별도 필요합니다.
- 개발 의존성에 `latest` 범위가 사용되고 잠금 파일이 없어 당시 빌드 환경을 고정하지 못합니다.
- 2026-11-13 문서 점검은 원작 관계·소스·빌드·캐시·라이선스 조사에 한정했으며 게임 플레이, 전체 빌드와 외부 점수 전송을 실행하지 않았습니다.

## 원작과 라이선스

- 원작: [Eugene Tiurin — ytiurin/tetris](https://github.com/ytiurin/tetris)
- 원작 참고 영상: [DVK-2 Tetris](https://www.youtube.com/watch?v=O0gAgQQHFcQ)
- [MIT License](LICENSE), Copyright (c) 2016 Eugene Tiurin

원작의 저작권과 소스·자산의 라이선스 표기를 유지해야 합니다.
