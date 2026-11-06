# s03. 개발 도구: VS Code, Python 가상환경, WSL, 마크다운

## 오늘의 목표

1. VS Code의 핵심 기능과 단축키를 쓸 수 있다
2. 가상환경이 왜 필요한지 설명하고, uv로 가상환경을 만들어 패키지를 설치할 수 있다
3. WSL을 설치하고 Linux 명령어를 실행할 수 있다
4. 마크다운 문법으로 문서를 쓸 수 있다

## 진행 방법

- 시작 전에 → 파트 1 → 2 → 3 → 4 순서로 읽고, 실습(총 5개)을 직접 해 본다. 실습은 **Git Bash**에서 한다. (파트 3의 WSL 실습만 Ubuntu에서 한다)
- 질문에는 **답을 먼저 쓰고** 나서 접힌 정답을 연다.
- 마지막에 마무리 퀴즈를 푼다.
- 다 끝나면 튜터에게 **틀린 문제, 헷갈린 점, 질문**을 한 번에 보낸다.

---

## 시작 전에: s02 마무리와 git pull

GitHub 웹에서 `README.md`를 직접 고치셨죠? 그 변경은 **원격 저장소에만** 있고, 내 컴퓨터(로컬)에는 아직 없다. 지금 상태를 그림으로 보면 이렇다.

```
GitHub (origin/main):  abb9246 ──▶ e673b06 "Update README.md"
내 컴퓨터 (main):       abb9246
                                  ↑ 로컬이 커밋 1개 뒤처져 있다 (behind 1)
```

이 상태에서 로컬에서 커밋하고 push하면, 두 저장소의 기록이 갈라져서 **push가 거부된다.** 그래서 작업을 시작하기 전에 **`git pull`로 원격의 변경을 먼저 받아 오는 습관**을 들인다.

| 명령어 | 하는 일 |
|---|---|
| `git fetch` | 원격 저장소의 새 커밋 정보를 **받아 오기만** 한다. 내 파일은 그대로다 |
| `git pull` | `fetch` + 내 파일에 **합치기(merge)**까지 한다 |

### 실습 1. 원격 변경 받아 오고 s02 마무리하기

```bash
cd /c/cs-study
git status
git pull
git log --oneline
cat README.md
git status
git add .
git commit -m "00-setup/s02: Git 강의 자료 수정"
git push
```

**Q1.** `git pull` 전에 `git status`의 맨 위 두 줄은 무엇이라고 했는가?
<details>
<summary>정답 보기</summary>

`On branch main`과 `Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.` 로컬이 원격보다 커밋 1개 뒤처져 있다는 뜻이다.

</details>

<br>

**Q2.** `git pull` 결과에 나온 `Fast-forward`는 무슨 뜻일까?
<details>
<summary>정답 보기</summary>

로컬에는 새 커밋이 없고 원격에만 새 커밋이 있어서, 로컬의 `main`을 원격 쪽으로 **앞으로 쭉 당기기만 하면** 되는 경우다. 합칠 것이 없으니 충돌도 생기지 않는다. (양쪽에 모두 새 커밋이 있으면 진짜로 합치는 merge가 일어난다. 08단계에서 다룬다)

</details>

<br>

**Q3.** 커밋하지 않은 수정(`s02-git-github.md`, `CLAUDE.md`)이 있었는데도 `git pull`이 성공한 이유는?
<details>
<summary>정답 보기</summary>

원격에서 바뀐 파일(`README.md`)과 로컬에서 수정 중인 파일이 **겹치지 않았기** 때문이다. 같은 파일이었다면 Git이 "로컬 변경이 덮어써질 수 있다"며 pull을 거부한다. 그때는 먼저 커밋하고 pull한다.

</details>

---

## 파트 1. VS Code 제대로 쓰기

### 1-1. VS Code는 무엇인가

**코드 에디터**다. 메모장처럼 글자를 편집하는 프로그램인데, 코드를 쓰기 편한 기능(문법 색칠, 자동 완성, 터미널, Git, 디버거)이 붙어 있다. 기능 대부분은 **확장(extension)**으로 추가한다.

> 참고: PyCharm, IntelliJ처럼 기능이 처음부터 다 들어 있는 무거운 도구는 **IDE(통합 개발 환경)**라고 부른다. VS Code는 가벼운 에디터에 확장을 붙여 IDE처럼 쓰는 방식이다.

### 1-2. 꼭 외울 단축키

| 단축키 | 기능 |
|---|---|
| `Ctrl + Shift + P` | **명령 팔레트.** VS Code의 모든 기능을 이름으로 검색해서 실행한다. 단축키가 기억나지 않으면 여기서 찾는다 |
| `Ctrl + P` | 파일 이름으로 빠르게 열기 |
| `` Ctrl + ` `` | 터미널 열기/닫기 |
| `Ctrl + B` | 왼쪽 사이드바 열기/닫기 |
| `Ctrl + /` | 선택한 줄 주석 처리/해제 |
| `Alt + ↑` / `Alt + ↓` | 현재 줄을 위/아래로 이동 |
| `Shift + Alt + ↓` | 현재 줄을 아래로 복사 |
| `Ctrl + D` | 같은 단어를 하나씩 추가 선택 (여러 곳 동시 수정) |
| `Alt + 클릭` | 커서를 여러 개 만들기 |
| `Ctrl + Shift + F` | 전체 파일에서 검색 |

### 1-3. 추천 확장

왼쪽 사이드바의 블록 모양 아이콘(`Ctrl + Shift + X`)에서 검색해 설치한다.

| 확장 | 만든 곳 | 용도 |
|---|---|---|
| **Python** | Microsoft | Python 실행, 자동 완성, 가상환경 선택 |
| **WSL** | Microsoft | VS Code에서 WSL(Linux) 환경 쓰기 (파트 3) |
| Jupyter | Microsoft | `.ipynb` 노트북 열기 (필요하면) |

> 확장을 고를 때는 **만든 곳(publisher)과 다운로드 수**를 확인한다. 이름만 비슷한 가짜 확장이 악성 코드를 심는 사례도 있다.

### 실습 2. VS Code 단축키 손에 익히기

1. Git Bash에서 `cd /c/cs-study` 후 **`code .`**을 실행해 본다. (이미 열려 있으면 같은 창이 앞으로 온다)
2. `Ctrl + P` → `s01`을 입력해 `s01-terminal-shell.md`를 연다.
3. `Ctrl + Shift + F`로 `PATH`를 검색해 본다.
4. `00-setup/labs/`에 `shortcuts.py` 파일을 새로 만들고, 아래 내용을 붙여 넣는다.

```python
apple = 1
apple_price = 1000
print(apple)
print(apple_price)
```

5. 첫 줄의 `apple`에 커서를 두고(드래그하지 않고) `Ctrl + D`를 여러 번 눌러 본다. 그대로 `banana`라고 입력하면 선택된 곳이 동시에 바뀐다. `Ctrl + Z`로 되돌린 뒤, 이번에는 `apple`을 **마우스로 드래그해서 선택한 다음** `Ctrl + D`를 여러 번 눌러 본다.
6. `print` 줄에서 `Alt + ↑`, `Shift + Alt + ↓`, `Ctrl + /`를 눌러 본다.
7. 다 해 봤으면 파일을 지운다. (`rm 00-setup/labs/shortcuts.py`)

**Q1.** `code .`의 `.`은 무엇이고, 이 명령은 어떻게 실행될 수 있는가?
<details>
<summary>정답 보기</summary>

`.`은 현재 폴더다. "현재 폴더를 VS Code로 열어라"라는 뜻이다. `code`라는 실행 파일이 있는 폴더(`.../Microsoft VS Code/bin`)가 **PATH에 등록되어 있어서** 어디서든 실행할 수 있다. (`which code`로 확인해 보자)

</details>

<br>

**Q2.** 5번의 두 가지 방법에서 `apple_price`의 `apple`도 선택되었는가?
<details>
<summary>정답 보기</summary>

시작 방법에 따라 다르다.

- **커서만 두고** `Ctrl + D`로 시작하면 `apple`이라는 **단어 전체**가 일치하는 것만 찾는다. `apple_price`는 선택되지 않는다.
- **드래그로 선택하고** 시작하면 그 **글자**를 찾기 때문에 `apple_price` 안의 `apple`까지 잡힌다.

변수 이름을 정확히 바꾸고 싶다면 변수 위에서 `F2`(이름 바꾸기)를 쓴다. Python 확장이 진짜 그 변수만 골라서 바꿔 준다.

</details>

---

## 파트 2. Python 가상환경과 uv

### 2-1. 이 컴퓨터에는 Python이 몇 개 있을까

```bash
which python
uv python list --only-installed
```

이 컴퓨터에는 Anaconda의 Python 3.13, 따로 설치된 3.14·3.11, uv가 받아 둔 3.12·3.14 등 **Python이 여러 개** 있다. 터미널에 `python`이라고 치면 그중 **PATH에서 가장 먼저 찾은 것**(지금은 Anaconda)이 실행된다. s01에서 배운 PATH 그대로다.

### 2-2. 가상환경이 필요한 이유

프로젝트 A는 `pandas 1.5`, 프로젝트 B는 `pandas 2.2`가 필요하다고 하자. 패키지를 한 곳에 설치하면 둘 중 하나는 깨진다.

**가상환경(virtual environment)**은 **프로젝트마다 따로 쓰는 독립된 Python과 패키지 보관함**이다.

```
전역 Python (Anaconda)       ← 여기에 이것저것 설치하면 꼬인다
cs-study/.venv/              ← cs-study 전용 Python + 패키지
회사프로젝트/.venv/           ← 회사 프로젝트 전용 Python + 패키지
```

통계 비유: R에서 프로젝트마다 `renv`로 패키지 버전을 고정하는 것과 같다. 분석 결과를 **재현**하려면 같은 버전이 필요하듯, 코드를 다른 컴퓨터나 서버에서 똑같이 돌리려면 패키지 버전을 고정해야 한다.

### 2-3. 도구 정리: pip, venv, conda, uv

| 도구 | 하는 일 | 특징 |
|---|---|---|
| `pip` | 패키지 설치 | Python 기본 도구 |
| `venv` | 가상환경 생성 | Python 기본 도구 |
| `conda` | 패키지 설치 + 가상환경 + Python 버전 관리 | 데이터 과학 쪽에서 많이 쓴다. Anaconda가 이것이다 |
| **`uv`** | 위의 일을 **전부** + 버전 고정 | 매우 빠르다. 요즘 Python 프로젝트의 표준으로 자리 잡는 중 |

이 저장소에서는 **uv**를 쓴다. 이 컴퓨터에는 이미 설치되어 있다. (`uv --version`) Anaconda는 지우지 않아도 된다. 다만 앞으로 이 저장소의 실습은 `.venv`에서 한다.

### 2-4. uv 핵심 명령어

| 명령어 | 하는 일 |
|---|---|
| `uv init --bare` | 현재 폴더를 Python 프로젝트로 만든다 (`pyproject.toml` 생성) |
| `uv add 패키지` | 패키지를 설치하고 `pyproject.toml`과 `uv.lock`에 기록한다. `.venv`가 없으면 자동으로 만든다 |
| `uv remove 패키지` | 패키지를 지운다 |
| `uv run 명령` | `.venv` 환경에서 명령을 실행한다 (활성화하지 않아도 된다) |
| `uv sync` | `uv.lock`에 적힌 그대로 `.venv`를 맞춘다 (다른 컴퓨터에서 처음 받았을 때) |

만들어지는 파일:

| 파일 | 역할 | Git에 올리는가 |
|---|---|---|
| `pyproject.toml` | 프로젝트 설정과 **필요한 패키지 목록** (사람이 읽는 파일) | ✅ |
| `uv.lock` | 실제 설치된 **정확한 버전** 기록 (재현용) | ✅ |
| `.venv/` | 실제 Python과 패키지가 설치된 폴더 | ❌ (`.gitignore`에 있다) |

`.venv`는 용량이 크고 컴퓨터마다 다르다. 대신 `pyproject.toml`과 `uv.lock`만 올려 두면 어느 컴퓨터에서든 `uv sync` 한 번으로 똑같이 만들 수 있다.

### 2-5. 가상환경 활성화

`uv run` 대신 가상환경을 **활성화(activate)**해서 쓸 수도 있다. 활성화하면 PATH 맨 앞에 `.venv`의 폴더가 끼워져서, `python`을 치면 `.venv`의 Python이 먼저 잡힌다.

| 환경 | 활성화 명령 |
|---|---|
| Git Bash (Windows) | `source .venv/Scripts/activate` |
| Linux, Mac, WSL | `source .venv/bin/activate` |
| 비활성화 (공통) | `deactivate` |

### 실습 3. cs-study에 가상환경 만들기

```bash
cd /c/cs-study
which python
uv init --bare
cat pyproject.toml
uv add pytest
ls -la
cat pyproject.toml
git status

uv run python --version
uv run python -c "import sys; print(sys.executable)"
python --version

source .venv/Scripts/activate
which python
echo $PATH | tr ':' '\n' | head -3
python --version
deactivate
which python
```

마지막으로 VS Code에서 `Ctrl + Shift + P` → `Python: Select Interpreter` → `.venv`가 들어간 항목을 고른다. 이제 VS Code에서 Python 파일을 실행하면 이 가상환경이 쓰인다.

**Q1.** `uv add pytest` 뒤에 새로 생긴 파일과 폴더는? 그중 `git status`에 나오지 않는 것은?
<details>
<summary>정답 보기</summary>

`.venv/`, `uv.lock`이 새로 생기고 `pyproject.toml`의 `dependencies`에 `pytest`가 추가된다. `.venv/`는 `.gitignore`에 있어서 `git status`에 나오지 않는다.

</details>

<br>

**Q2.** 활성화 전후로 `which python`의 결과는 어떻게 달랐는가? 왜 그런가?
<details>
<summary>정답 보기</summary>

- 활성화 전: `/c/Users/user/anaconda3/python`
- 활성화 후: `/c/cs-study/.venv/Scripts/python`

`activate`가 PATH 맨 앞에 `.venv/Scripts`를 끼워 넣었기 때문이다. 셸은 PATH를 앞에서부터 찾으니 `.venv`의 Python이 먼저 잡힌다. `deactivate`하면 PATH가 원래대로 돌아간다. (s01의 PATH 개념 그대로다)

</details>

<br>

**Q3.** `python --version`과 `uv run python --version`의 결과가 다른 이유는?
<details>
<summary>정답 보기</summary>

그냥 `python`은 PATH에서 처음 찾은 Anaconda의 Python(3.13)을 실행하고, `uv run python`은 `.venv`의 Python(3.14)을 실행하기 때문이다. `uv run`은 활성화하지 않아도 항상 프로젝트의 가상환경을 쓴다.

</details>

<br>

**Q4.** 다른 컴퓨터에서 이 저장소를 `git clone`으로 받았다. `.venv`가 없는데 어떻게 같은 환경을 만드는가?
<details>
<summary>정답 보기</summary>

`uv sync`를 실행한다. `uv.lock`에 적힌 정확한 버전대로 `.venv`를 새로 만들어 준다.

</details>

실습이 끝나면 커밋한다.

```bash
git add pyproject.toml uv.lock
git commit -m "00-setup/s03: uv 프로젝트 설정과 pytest 추가"
git push
```

---

## 파트 3. WSL로 Linux 써 보기

### 3-1. WSL이란

**WSL(Windows Subsystem for Linux)**은 Windows 안에서 **진짜 Linux**를 돌리는 기능이다. 가상 머신처럼 무겁지 않고, Windows 파일에도 바로 접근할 수 있다.

| | Git Bash | WSL (Ubuntu) |
|---|---|---|
| 정체 | Windows 위에서 Bash를 **흉내** | 진짜 Linux 커널 위의 Bash |
| 쓸 수 있는 프로그램 | 기본 명령어 일부 | Linux 프로그램 전부 (`apt`로 설치) |
| 쓰는 곳 | 지금까지의 실습 | C 컴파일(02단계), Linux 실습(09단계), Docker |

**Ubuntu**는 Linux 배포판(distribution) 중 하나다. Linux 커널에 여러 프로그램을 묶어 배포한 것을 배포판이라고 한다. Ubuntu, Debian, Fedora 등이 있고, 서버와 입문용으로 Ubuntu를 가장 많이 쓴다.

### 3-2. Linux 폴더 구조

Linux에는 드라이브(C:, D:)가 없고, 모든 것이 루트(`/`) 아래에 있다.

| 경로 | 용도 | Windows로 치면 |
|---|---|---|
| `/` | 루트 | `C:\` |
| `/home/사용자명` | 홈 디렉터리 (`~`) | `C:\Users\user` |
| `/usr/bin` | 프로그램 실행 파일 | `C:\Program Files` |
| `/etc` | 시스템 설정 파일 | (레지스트리 비슷) |
| `/tmp` | 임시 파일 | `C:\Users\user\AppData\Local\Temp` |
| `/mnt/c` | WSL에서 보는 **Windows의 C 드라이브** | `C:\` |

### 3-3. 관리자 권한: sudo

Linux에서 프로그램 설치처럼 시스템을 바꾸는 작업은 **관리자(root) 권한**이 필요하다. 명령 앞에 `sudo`(superuser do)를 붙이면 그 명령만 관리자 권한으로 실행된다. 실행할 때 비밀번호를 묻는다.

```bash
sudo apt update          # 설치 가능한 프로그램 목록 갱신
sudo apt install 이름     # 프로그램 설치
```

`apt`는 Ubuntu의 **패키지 관리자**다. Python의 `pip`/`uv`처럼, 운영체제 프로그램을 설치하고 관리한다.

### 실습 4. WSL 설치하고 Linux 맛보기

> ⚠️ **회사 PC라면** 보안 정책 때문에 WSL 설치가 막혀 있거나 관리자 권한이 없을 수 있다. 설치가 안 되면 무리하지 말고 건너뛴다. Git Bash로도 01~08단계는 충분히 진행할 수 있고, WSL은 나중에 필요할 때 다시 시도하면 된다.

**1) 설치** (지금 이 컴퓨터에는 WSL이 설치되어 있지 않다)

1. 시작 메뉴에서 `PowerShell` 검색 → **관리자 권한으로 실행**
2. 아래 명령 실행

```powershell
wsl --install
```

3. 완료 메시지가 나오면 **컴퓨터를 재부팅**한다.
4. 재부팅 후 Ubuntu 창이 자동으로 뜬다. (안 뜨면 시작 메뉴에서 `Ubuntu` 실행)
5. 사용자 이름과 비밀번호를 만든다.
   - 사용자 이름은 **영어 소문자**로 쓴다.
   - ⚠️ 비밀번호를 칠 때 **화면에 아무것도 표시되지 않는다.** 고장이 아니라 보안을 위한 Linux의 기본 동작이다. 그냥 치고 Enter를 누른다.
   - 이 비밀번호는 `sudo`를 쓸 때마다 필요하니 기억해 둔다.

**2) VS Code에서 WSL 터미널 열기**

VS Code 터미널 오른쪽 위 `+` 옆의 `∨` → **Ubuntu (WSL)** 선택

**3) Linux 맛보기**

```bash
pwd
whoami
cat /etc/os-release
ls /
ls /mnt/c
cd /mnt/c/cs-study
ls
cd ~
sudo apt update
which python3
python3 --version
which gcc
```

> WSL은 Windows와 **별개의 컴퓨터**처럼 동작한다. Git 사용자 설정이나 설치한 프로그램도 따로다. 그래서 지금은 **Git 작업은 Git Bash에서** 하고, WSL은 Linux 연습용으로 쓴다.

**Q1.** `pwd`의 결과가 `/c/Users/user`가 아니라 `/home/사용자명`인 이유는?
<details>
<summary>정답 보기</summary>

WSL은 Windows와 분리된 진짜 Linux라서 자기만의 홈 디렉터리(`/home/사용자명`)를 가진다. Windows의 파일은 `/mnt/c` 아래에서 볼 수 있다.

</details>

<br>

**Q2.** `sudo apt update`에서 `sudo`를 빼면 어떻게 될까?
<details>
<summary>정답 보기</summary>

`Permission denied` 같은 권한 에러가 난다. 프로그램 목록을 갱신하는 것은 시스템을 바꾸는 작업이라 관리자 권한이 필요하다.

</details>

<br>

**Q3.** `which gcc`의 결과가 아무것도 나오지 않는다면 무슨 뜻인가?
<details>
<summary>정답 보기</summary>

PATH에 `gcc`(C 컴파일러)가 없다, 즉 아직 설치되지 않았다는 뜻이다. 02단계(C 언어)에서 `sudo apt install build-essential`로 설치한다.

</details>

---

## 파트 4. 마크다운

### 4-1. 마크다운이란

**마크다운(Markdown)**은 간단한 기호로 서식을 표현하는 문서 형식이다. 지금 읽고 있는 강의 자료, `README.md`, GitHub 이슈, Jupyter의 텍스트 셀, Notion, 그리고 AI 채팅 답변까지 거의 모두 마크다운이다. 개발자의 기본 글쓰기 도구라고 보면 된다.

### 4-2. 기본 문법

| 서식 | 문법 | 결과 |
|---|---|---|
| 제목 | `# 제목1`, `## 제목2`, `### 제목3` | 크기가 다른 제목 |
| 굵게 | `**굵게**` | **굵게** |
| 기울임 | `*기울임*` | *기울임* |
| 인라인 코드 | `` `code` `` | `code` |
| 링크 | `[보일 글자](https://github.com)` | [보일 글자](https://github.com) |
| 순서 없는 목록 | `- 항목` | • 항목 |
| 순서 있는 목록 | `1. 항목` | 1. 항목 |
| 체크박스 | `- [ ] 할 일` / `- [x] 한 일` | ☐ / ☑ |
| 인용 | `> 인용문` | 왼쪽에 세로줄이 있는 블록 |
| 구분선 | `---` | 가로줄 |

**코드 블록**은 백틱 3개로 감싸고, 첫 줄에 언어 이름을 쓰면 문법 색칠이 된다.

````markdown
```python
print("hello")
```
````

**표**는 `|`로 칸을 나누고, 둘째 줄에 `---`를 쓴다.

```markdown
| 명령어 | 하는 일 |
|---|---|
| pwd | 현재 위치 출력 |
```

> ⚠️ **줄바꿈 주의:** 마크다운에서는 Enter를 한 번 쳐도 줄이 바뀌지 않고 앞 줄에 이어 붙는다. 문단을 나누려면 **빈 줄**을 넣는다.

### 실습 5. 마크다운 연습 문서 쓰기

`00-setup/labs/markdown-practice.md`를 만들고, 오늘 배운 내용을 자유롭게 정리한다. **아래 문법을 모두 한 번 이상** 써 본다.

- [ ] 제목 (`#`, `##`)
- [ ] 체크박스
- [ ] 굵게
- [ ] 인라인 코드
- [ ] 코드 블록 (오늘 쓴 명령어 몇 줄)
- [ ] 표
- [ ] 링크 (오늘 강의 자료 링크: `[s03 강의 자료](../lessons/s03-dev-tools.md)`)

`Ctrl + K` → `V`로 미리보기를 옆에 띄워 놓고 쓰면 결과를 바로 확인할 수 있다. 다 쓰면 커밋하고 push한다. GitHub 웹에서도 같은 모양으로 보이는지 확인한다.

```bash
git add 00-setup/labs/markdown-practice.md
git commit -m "00-setup/s03: 마크다운 연습"
git push
```

**Q1.** 링크에 쓴 `../lessons/s03-dev-tools.md`는 절대 경로인가, 상대 경로인가? `..`은 어디를 가리키는가?
<details>
<summary>정답 보기</summary>

상대 경로다. 연습 문서가 있는 `00-setup/labs/` 폴더가 기준이고, `..`은 그 부모인 `00-setup/`을 가리킨다. 거기서 `lessons/s03-dev-tools.md`로 내려간다. s01의 경로 개념이 마크다운 링크에도 그대로 쓰인다.

</details>

---

## 마무리 퀴즈

**Q1.** VS Code에서 기능의 단축키가 기억나지 않을 때 쓰는 단축키는?
<details>
<summary>정답 보기</summary>

`Ctrl + Shift + P` (명령 팔레트). 기능 이름으로 검색해서 실행할 수 있다.

</details>

<br>

**Q2.** 가상환경을 쓰는 이유를 두 가지 이상 설명하라.
<details>
<summary>정답 보기</summary>

① 프로젝트마다 다른 패키지 버전을 써도 서로 충돌하지 않는다. ② 패키지 버전을 고정해 다른 컴퓨터나 서버에서도 똑같은 환경을 **재현**할 수 있다. ③ 전역 Python을 깨끗하게 유지할 수 있다.

</details>

<br>

**Q3.** `pyproject.toml`, `uv.lock`, `.venv/` 중 Git에 올리지 않는 것은? 그 이유는?
<details>
<summary>정답 보기</summary>

`.venv/`. 용량이 크고 운영체제마다 내용이 다르며, `uv.lock`만 있으면 `uv sync`로 언제든 다시 만들 수 있기 때문이다.

</details>

<br>

**Q4.** 가상환경을 활성화하면 `python`을 쳤을 때 `.venv`의 Python이 실행되는 원리는?
<details>
<summary>정답 보기</summary>

활성화 스크립트가 **PATH 맨 앞**에 `.venv`의 실행 파일 폴더를 끼워 넣는다. 셸은 PATH를 앞에서부터 찾으므로 `.venv`의 Python을 가장 먼저 찾는다.

</details>

<br>

**Q5.** WSL에서 Windows의 `C:\cs-study` 폴더로 가려면 어떤 경로로 `cd`해야 하는가?
<details>
<summary>정답 보기</summary>

`cd /mnt/c/cs-study`

</details>

<br>

**Q6.** GitHub 웹에서 파일을 고친 뒤, 로컬에서 작업을 시작하기 전에 해야 할 일은?
<details>
<summary>정답 보기</summary>

`git pull`로 원격의 변경을 먼저 받아 온다. 그러지 않고 로컬에서 커밋하면 두 기록이 갈라져 push가 거부된다.

</details>

<br>

**Q7.** 마크다운에서 아래처럼 썼더니 미리보기에서 한 줄로 붙어 나왔다. 왜 그런가?

```markdown
첫 번째 줄
두 번째 줄
```
<details>
<summary>정답 보기</summary>

마크다운에서는 Enter 한 번은 줄바꿈으로 치지 않고, 같은 문단으로 이어 붙인다. 문단을 나누려면 두 줄 사이에 **빈 줄**을 넣는다. (줄 끝에 공백 두 칸을 넣어도 줄이 바뀌지만, 눈에 보이지 않아 실수하기 쉽다)

</details>
