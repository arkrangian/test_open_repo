# 작업 환경 구성 — uv · Python · FastAPI · React

`Repo Ver1`의 환경 구성 내용을 새 작업에서도 따라 할 수 있도록 정리했습니다. 이 폴더에는 설치·초기 구성 안내만 있습니다. 명령은 필요한 단계만 직접 실행하세요.

## 1. 이미 설치되어 있는지 확인

터미널에서 각각 실행합니다.

```text
uv --version
node --version
npm --version
```

버전이 나오면 해당 도구를 다시 설치할 필요가 없습니다. Python은 아래에서 uv로 준비할 수 있습니다. 기존 Python을 확인하려면 Windows는 `python --version`, Linux는 `python3 --version`을 사용합니다.

## 2. uv 설치 — 없을 때만

Windows PowerShell:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Linux/macOS 터미널:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

설치 후 터미널을 새로 열어 `uv --version`을 확인합니다. 시험에서는 허용된 설치·자료 접근 방식을 따릅니다. [공식 uv 설치 안내](https://docs.astral.sh/uv/getting-started/installation/)

## 3. Python 준비

```text
uv python install 3.12
```

이 안내의 기본 버전은 Python 3.12입니다. 실제 과제나 기존 프로젝트가 다른 버전을 지정하면 그 조건을 우선합니다. [uv의 Python 관리](https://docs.astral.sh/uv/guides/install-python/)

## 4. Node.js와 npm 준비 — 없을 때만

[Node.js 공식 다운로드](https://nodejs.org/en/download)에서 현재 운영체제에 맞는 호환 LTS 설치 방법을 선택합니다. Windows 연습에서는 설치 프로그램을 사용할 수 있고, Linux 시험 환경에서는 해당 환경의 설치 방법과 권한을 확인합니다.

설치 후 새 터미널에서 확인합니다.

```text
node --version
npm --version
```

React는 별도 런타임 설치 대상이 아니라 npm으로 설치하는 프로젝트 라이브러리입니다. 아래 Vite 7 계열 구성은 Node 20.19+ 또는 22.12+ 계열 이상을 요구합니다. 준비된 Node가 호환되면 그대로 사용합니다. [Vite 공식 안내](https://vite.dev/guide/)

## 5. 빈 작업 폴더에서 Python 환경 만들기

Windows PowerShell에서 이 폴더를 작업 루트로 쓰려면:

```powershell
cd 'D:\Project\Hynics_Hackathon\Repo Ver2'
```

시험에서는 제공된 작업 폴더로 이동합니다. 그곳에 기존 프로젝트가 있으면 아래 초기화 명령을 실행하지 말고 마지막의 복원 방법을 확인하세요.

작업 루트에서 실행:

```text
uv init backend --bare --python 3.12 --vcs none
cd backend
uv python pin 3.12
uv add fastapi uvicorn
uv run python --version
uv run python -c "import fastapi, uvicorn; print('FastAPI / Uvicorn OK')"
cd ..
```

`uv add`가 프로젝트 의존성과 가상환경을 관리합니다. `uv run`을 사용하면 가상환경 활성화 명령을 따로 입력하지 않아도 됩니다. 이 단계는 패키지 설치까지이며 FastAPI 서버 코드는 생성하지 않습니다. [uv 프로젝트 관리](https://docs.astral.sh/uv/guides/projects/)

## 6. React 프로젝트 만들기

다시 작업 루트에서 실행합니다. `backend` 안에서 실행하지 않습니다.

```text
npm create vite@7 frontend -- --template react
cd frontend
npm install
npm run dev -- --host 0.0.0.0 --port 5173 --strictPort
```

React + JavaScript 기본 프로젝트가 생성됩니다. 로컬에서는 `http://localhost:5173`을 열고, 시험에서는 플랫폼의 5173 포트 미리보기 주소를 사용합니다. `0.0.0.0`은 수신 주소이며 브라우저에 입력할 주소가 아닙니다. 종료는 해당 터미널에서 **Ctrl+C**입니다.

이 단계의 React 기본 화면은 FastAPI와 아직 연결되지 않았습니다. [Vite 시작 안내](https://vite.dev/guide/), [서버 설정](https://vite.dev/config/server-options.html)

## 이미 구성된 프로젝트를 가져온 경우

`backend/pyproject.toml`과 `uv.lock`, `frontend/package.json`과 `package-lock.json`이 있는 프로젝트에만 적용합니다. 현재 문서만 있는 Repo Ver2에는 아직 이 파일들이 없습니다.

작업 루트에서:

```text
cd backend
uv sync --locked
cd ../frontend
npm ci
cd ..
```

처음 만드는 `uv init`·`npm create`와 기존 환경을 복원하는 `uv sync`·`npm ci`를 구분하세요. `.venv`·`node_modules`는 복사하지 않고 각 환경에서 다시 설치합니다. `npm ci`는 기존 node_modules를 다시 구성하며 lockfile이 필요합니다. [npm ci 안내](https://docs.npmjs.com/cli/v11/commands/npm-ci/)

## 확인할 결과

- uv·Node·npm 버전이 출력됩니다.
- backend에서 Python 버전과 `FastAPI / Uvicorn OK`가 출력됩니다.
- frontend 개발 서버의 기본 React 화면이 열립니다.

이 문서는 Repo Ver1의 안내를 분리·정리한 것입니다. Repo Ver2에서 설치나 프로젝트 생성 명령을 대신 실행하지 않았습니다. Skill·업무 예제·테스트·설계·제출 양식은 Repo Ver1에 그대로 있습니다.
