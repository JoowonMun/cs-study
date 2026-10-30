# s01. 터미널과 셸

## 오늘의 목표

1. 터미널과 셸이 무엇인지 설명할 수 있다
2. 절대 경로와 상대 경로를 구분할 수 있다
3. 터미널만으로 폴더와 파일을 만들고, 복사·이동·삭제할 수 있다
4. 환경변수 PATH가 무엇인지 설명할 수 있다

## 진행 방법

- 파트 1 → 2 → 3 순서로 읽고, 실습(총 4개)을 직접 해 본다.
- 질문에는 **답을 먼저 쓰고** 나서 접힌 정답을 연다.
- 마지막에 마무리 퀴즈를 푼다.
- 다 끝나면 튜터에게 **틀린 문제, 헷갈린 점, 질문**을 한 번에 보낸다.

---

## 파트 1. 터미널, 셸, 그리고 경로

### 1-1. 터미널과 셸

| 용어 | 뜻 | Jupyter 비유 |
|---|---|---|
| **터미널 (terminal)** | 글자를 입력받고 결과를 보여주는 **창**. 스스로 명령을 이해하지 못한다 | 브라우저에 뜬 노트북 화면 |
| **셸 (shell)** | 입력된 명령을 **해석해서 운영체제에 전달하는 프로그램** | 실제로 코드를 실행하는 커널 |

```
 나 ──키보드──▶ [ 터미널 ] ──글자 전달──▶ [ 셸 ] ──해석해서 요청──▶ [ 운영체제 ]
     ◀──화면 표시──        ◀──결과 출력──        ◀──결과 반환──
```

- 터미널은 **입력과 출력만** 맡는다. 키보드로 친 글자를 셸에 넘기고, 셸이 돌려준 글자를 화면에 띄운다.
- 셸은 **언어**다. 셸마다 문법이 다르다.
- Jupyter에서 커널을 Python에서 R로 바꿔도 화면은 그대로인 것처럼, **같은 터미널 창에서 셸만 바꿀 수 있다.**

| 셸 | 주로 쓰는 곳 |
|---|---|
| PowerShell | Windows 기본 |
| Bash | Linux, 서버, Git Bash |
| Zsh | macOS 기본 |

**터미널을 쓰는 이유:** ① 반복 작업을 자동화할 수 있다 ② 서버에는 GUI가 없다 ③ Git, Docker, pip 같은 개발 도구 대부분이 터미널 기반이다.

### 1-2. 파일 시스템과 경로

파일 시스템은 **트리 구조**다.

```
C:\                      ← 루트 (Windows는 드라이브마다 루트가 있음)
├── Users\
│   └── user\            ← 홈 디렉터리 (~)
└── cs-study\
    ├── 00-setup\
    ├── 01-python\
    │   └── notes\
    └── 02-c\
```

- **절대 경로:** 루트부터 전부 적은 경로. 어디서 실행해도 같은 곳을 가리킨다. 예: `C:\cs-study\01-python\notes`
- **상대 경로:** **현재 디렉터리** 기준의 경로. 예: `..\..\02-c`

| 기호 | 뜻 |
|---|---|
| `.` | 현재 폴더 |
| `..` | 부모 폴더 (한 칸 위) |
| `~` | 홈 디렉터리 |

### 1-3. 첫 명령어

| 명령어 | 하는 일 | 영어 |
|---|---|---|
| `pwd` | 현재 위치 출력 | print working directory |
| `ls` | 현재 폴더 내용 보기 | list |
| `cd 경로` | 폴더 이동 | change directory |

### 실습 1. 경로 이동 (PowerShell)

VS Code에서 `` Ctrl + ` ``로 터미널을 연다. **각 명령을 실행하기 전에 결과를 먼저 예측한다.**

```powershell
pwd
ls
cd 00-setup
pwd
cd ..
pwd
cd ~
pwd
cd C:\cs-study\01-python\notes
pwd
cd ..\..\02-c
pwd
```

**Q1.** 마지막 `pwd`의 결과는?
<details>
<summary>정답 보기</summary>

`C:\cs-study\02-c`

</details>

<br>

**Q2.** `cd ..\..\02-c`에서 `..`가 두 번 필요한 이유는?
<details>
<summary>정답 보기</summary>

`notes`에서 `..` 한 번이면 `01-python`, 두 번이면 `cs-study`까지 올라간다. 거기서 `02-c`로 내려간다.

</details>

<br>

**Q3.** `C:\cs-study\01-python\notes`와 `..\..\02-c`는 각각 절대 경로인가, 상대 경로인가?
<details>
<summary>정답 보기</summary>

앞의 것은 절대 경로, 뒤의 것은 상대 경로.

</details>

### 실습 2. 셸이 다르면 문법이 다르다

VS Code 터미널 오른쪽 위 `+` 옆의 `∨`를 눌러 **Git Bash**를 선택하면 Bash 셸 터미널이 새로 열린다. PowerShell 터미널과 Git Bash 터미널에서 같은 명령을 실행해 본다.

```
ls -la
echo $SHELL
```

**Q1.** 두 셸의 결과는 어떻게 달랐는가? 왜 같은 명령인데 결과가 다른가?
<details>
<summary>정답 보기</summary>

- **Bash:** `ls -la`는 숨김 파일까지 자세히 보여주고, `echo $SHELL`은 `/usr/bin/bash` 같은 셸 경로를 출력한다.
- **PowerShell:** `ls -la`는 에러가 난다. PowerShell의 `ls`는 `Get-ChildItem`의 별명(alias)이라 `-la` 옵션을 모른다. `$SHELL`은 정의되지 않은 변수라 빈 줄이 출력된다.
- **이유:** 창(터미널)은 똑같이 생겼지만, 명령을 해석하는 프로그램(셸)이 달라서 결과가 다르다.

</details>

> **Bash란?** Bourne Again SHell. 예전 표준 셸인 Bourne shell(`sh`)을 개선해 1989년에 나온 셸이다. Linux 서버, Docker 컨테이너, GitHub Actions, 인터넷의 설치 가이드 대부분이 Bash 문법을 쓴다. 데이터 분석에서 Python과 R이 같은 일을 다른 문법으로 하듯, PowerShell(Windows)과 Bash(Linux)는 같은 일을 다른 문법으로 한다.
>
> | | PowerShell | Bash |
> |---|---|---|
> | 만든 곳 | Microsoft | GNU 프로젝트 (오픈소스) |
> | 주 무대 | Windows | Linux, Mac, 서버 |
> | 스크립트 파일 | `.ps1` | `.sh` |
>
> **Git Bash란?** Git 전용 언어가 아니라, Windows용 Git을 설치할 때 **함께 딸려 오는 Bash 셸**이다. 문법은 Linux의 Bash와 같다.
>
> **같은 `ls`, 다른 정체:** PowerShell의 `ls`는 `Get-ChildItem`의 별명이고, Bash의 `ls`는 별도의 프로그램이다. 이름만 같고 옵션은 다르다.

---

## 파트 2. 파일과 폴더 다루기

### 2-0. 앞으로 실습은 Bash로 한다

Linux 서버, Docker, CI에서 쓰는 셸이 Bash라서 앞으로 실습은 **Git Bash**로 한다. (s03에서 WSL을 설치한 뒤에는 WSL의 Bash를 쓴다) PowerShell 명령어는 참고용으로만 적어 둔다.

**VS Code 기본 터미널을 Git Bash로 바꾸기:** `Ctrl + Shift + P` → `Terminal: Select Default Profile` → `Git Bash` 선택. 이후 새로 여는 터미널은 Git Bash로 열린다.

**Bash에서는 경로를 다르게 쓴다.**

| | Windows (PowerShell) | Bash (Git Bash) |
|---|---|---|
| 구분자 | `\` (역슬래시) | `/` (슬래시) |
| 드라이브 | `C:\cs-study` | `/c/cs-study` |
| 홈 | `C:\Users\user` | `~` 또는 `/c/Users/user` |

Linux에는 드라이브(C:, D:)라는 개념이 없고, 모든 것이 `/` 하나의 루트 아래에 있다. Git Bash는 Windows 드라이브를 `/c`, `/d`처럼 보여준다.

### 2-1. 기본 명령어

| 하는 일 | Bash | 영어 | (참고) PowerShell |
|---|---|---|---|
| 폴더 만들기 | `mkdir 이름` | make directory | `mkdir 이름` |
| 빈 파일 만들기 | `touch 이름` | | `New-Item 이름` |
| 파일에 글 쓰기 | `echo "내용" > 파일` | | `echo "내용" > 파일` |
| 파일 내용 보기 | `cat 파일` | concatenate | `cat 파일` |
| 복사 | `cp 원본 대상` | copy | `cp 원본 대상` |
| 이동 / 이름 바꾸기 | `mv 원본 대상` | move | `mv 원본 대상` |
| 파일 삭제 | `rm 파일` | remove | `rm 파일` |
| 폴더 삭제 (안의 내용까지) | `rm -r 폴더` | recursive | `rm -Recurse 폴더` |
| 숨김 파일까지 자세히 보기 | `ls -la` | list all, long | `ls -Force` |
| 화면 지우기 | `clear` | | `clear` |

`-r`, `-la`처럼 명령어 뒤에 붙는 `-`로 시작하는 것을 **옵션(option, flag)**이라고 한다. 명령의 동작을 바꾼다. `ls -la`는 `ls -l -a`를 합쳐 쓴 것이다.

- `>`는 **리다이렉션**이다. 화면에 출력될 내용을 파일로 보낸다. 같은 파일이 이미 있으면 **덮어쓴다**. 이어서 쓰려면 `>>`를 쓴다.
- `mv`는 이동과 이름 바꾸기를 같은 명령으로 한다. 대상이 **같은 폴더 안의 새 이름**이면 이름 바꾸기가 된다.
- ⚠️ `rm`으로 지운 파일은 **휴지통으로 가지 않는다.** 되돌릴 수 없다.

### 2-2. 따옴표: 작은따옴표 vs 큰따옴표

`echo`로는 **어떤 종류의 파일이든** 만들 수 있다. `.txt`든 `.py`든 확장자는 그냥 이름의 일부일 뿐, 내용은 똑같은 텍스트다. 다만 내용에 특수 문자가 들어가면 **따옴표 규칙**을 알아야 한다.

```bash
echo "print("Hello, World!")" > hello.py
# bash: !": event not found
```

여기에는 두 가지 문제가 있다.

1. **큰따옴표 안에 큰따옴표:** 셸은 두 번째 `"`를 만나는 순간 문자열이 끝났다고 판단한다. 따옴표 짝이 의도와 다르게 맞춰진다.
2. **`!`는 Bash의 특수 문자:** 큰따옴표 안에서도 `!`는 "이전에 실행한 명령 불러오기(history expansion)"로 해석된다. `!"`를 "`"`로 시작하는 이전 명령"으로 읽으려다 실패한 것이다.

| 따옴표 | 안에 있는 특수 문자 | 예 |
|---|---|---|
| `'작은따옴표'` | **전부 글자 그대로** 취급 | `echo '$HOME'` → `$HOME` |
| `"큰따옴표"` | `$변수`, `!` 등을 **해석함** | `echo "$HOME"` → `/c/Users/user` |

**규칙: 내용을 글자 그대로 쓰고 싶으면 작은따옴표를 쓴다.**

```bash
echo 'print("Hello, World!")' > hello.py
```

### 2-3. 생산성 팁

| 키 | 기능 |
|---|---|
| `Tab` | 파일·폴더 이름 자동 완성 (앞 몇 글자만 치고 누르기) |
| `↑` / `↓` | 이전에 실행한 명령 불러오기 |
| `Ctrl + C` | 실행 중인 명령 강제 중단 |

### 실습 3. 파일과 폴더 다루기 (Git Bash)

**각 명령을 실행하기 전에 결과를 예측한다.** 폴더 이름은 `Tab` 자동 완성으로 입력해 본다.

```bash
cd /c/cs-study/00-setup/labs
pwd
ls -la
mkdir playground
cd playground
touch hello.txt
echo "hello terminal" > hello.txt
cat hello.txt
echo "second line" >> hello.txt
cat hello.txt
mkdir backup
cp hello.txt backup/
ls backup
mv hello.txt hi.txt
ls
echo 'print("Hello, World!")' > hello.py
cat hello.py
python hello.py
echo "$HOME"
echo '$HOME'
rm hi.txt
ls
cd ..
rm -r playground
ls
```

**Q1.** `ls -la`를 했을 때 `.gitkeep`이 보였는가? 그냥 `ls`에서는?
<details>
<summary>정답 보기</summary>

`ls -la`에서만 보인다. 이름이 `.`으로 시작하는 파일은 숨김 파일이라, `-a`(all) 옵션이 있어야 보인다.

</details>

<br>

**Q2.** `cp hello.txt backup/`를 실행한 직후, `hello.txt`는 모두 몇 개 있는가?
<details>
<summary>정답 보기</summary>

2개. `playground/hello.txt`와 `playground/backup/hello.txt`. 복사는 원본을 남긴다.

</details>

<br>

**Q3.** `mv hello.txt hi.txt`는 무엇을 했는가?
<details>
<summary>정답 보기</summary>

같은 폴더 안에서 이름을 `hi.txt`로 바꿨다. 이동과 이름 바꾸기는 같은 명령이다.

</details>

<br>

**Q4.** `echo "second line"`을 쓸 때 `>>` 대신 `>`를 썼다면 파일 내용은 어떻게 됐을까?
<details>
<summary>정답 보기</summary>

기존 내용이 지워지고 `second line`만 남는다.

</details>

<br>

**Q5.** `hello.py`를 만들 때 큰따옴표가 아니라 작은따옴표로 감싼 이유는? `echo "$HOME"`과 `echo '$HOME'`의 결과는 어떻게 달랐는가?
<details>
<summary>정답 보기</summary>

- 내용에 큰따옴표와 `!`가 들어 있어서, 큰따옴표로 감싸면 셸이 따옴표 짝을 잘못 맞추고 `!`를 특수 문자로 해석한다. 작은따옴표 안의 내용은 **전부 글자 그대로** 쓰인다.
- `echo "$HOME"`은 변수를 해석해서 `/c/Users/user`를 출력하고, `echo '$HOME'`은 글자 그대로 `$HOME`을 출력한다.

</details>

<br>

**Q6.** 폴더를 지울 때 `-r`이 필요한 이유는?
<details>
<summary>정답 보기</summary>

`rm`은 원래 **파일 하나만** 지운다. 폴더를 지우려면 안의 내용을 먼저 비워야 하는데, 안에 또 폴더가 있으면 그 안으로 들어가 같은 작업을 반복해야 한다. 이렇게 안쪽으로 파고들며 반복하는 것이 **재귀(recursive)**다. `-r`을 붙이면 하위 폴더와 그 안의 파일까지 **전부** 지운다.

```
playground/          ← 이걸 지우려면
├── hi.txt           ← 1) 파일은 지운다
└── backup/          ← 2) 폴더면 안으로 들어가 똑같이 반복
    └── hello.txt    ←    파일 지우고, 빈 backup/ 지우고
                     ← 3) 빈 playground/를 마지막으로 지운다
```

`-r` 없이 `rm playground`를 하면 `Is a directory` 에러로 거부된다. 일부러 `-r`을 붙여야만 폴더가 통째로 지워지니, **실수로 폴더 전체를 날리는 사고를 막는 안전장치**다. 참고로 `rmdir 폴더`는 **빈 폴더만** 지운다.

⚠️ 인터넷에서 보이는 `rm -rf`는 "확인 없이 강제로(force) 전부 삭제"다. 경로를 잘못 쓰면 큰 사고가 나니, 실행 전에 경로를 꼭 다시 확인한다.

</details>

<br>

**Q7.** `rm`으로 지운 `hi.txt`는 휴지통에 있는가?
<details>
<summary>정답 보기</summary>

없다. 터미널의 `rm`은 휴지통을 거치지 않고 바로 삭제한다.

</details>

---

## 파트 3. 환경변수와 PATH

### 3-1. 환경변수

**환경변수(environment variable)**는 운영체제가 들고 있는 "이름 = 값" 쌍이다. 컴퓨터에서 실행되는 **모든 프로그램이 읽을 수 있는 공용 설정값**이라고 보면 된다.

| 환경변수 | 값의 예 | 의미 |
|---|---|---|
| `USERNAME` | `user` | 현재 사용자 이름 |
| `HOME` | `/c/Users/user` | 홈 디렉터리 |
| `PATH` | `/mingw64/bin:/usr/bin:...` | 실행 파일을 찾을 폴더 목록 |

나중에 LLM API 키 같은 **비밀 정보를 코드에 직접 쓰지 않고 환경변수에 넣어 두는 것**도 같은 원리다. (09단계에서 다룬다)

| 하는 일 | Bash | (참고) PowerShell |
|---|---|---|
| 환경변수 읽기 | `echo $이름` | `$env:이름` |
| 환경변수 설정 (현재 터미널에서만) | `export 이름=값` | `$env:이름 = "값"` |
| 실행 파일 위치 찾기 | `which 명령어` | `Get-Command 명령어` |

Bash에서 `export 이름=값`을 쓸 때 **`=` 양옆에 공백을 넣으면 안 된다.** Python 습관대로 `export A = 1`이라고 쓰면 에러가 난다.

### 3-2. PATH

터미널에 `git`이라고 치면, 셸은 `git`이라는 실행 파일을 찾아서 실행한다. 그런데 컴퓨터에 있는 파일 전체를 뒤지지는 않는다. **PATH에 적힌 폴더들만 앞에서부터 순서대로** 찾아본다.

```
나: git
셸: PATH 목록 확인
    1) /c/Users/user/bin/git ? → 없음
    2) /mingw64/bin/git ? → 있음! → 실행
```

- PATH에 있는 폴더 어디에도 없으면 에러가 난다.
  - Bash: `bash: xxx: command not found`
  - PowerShell: `'xxx' 용어가 cmdlet, 함수, 스크립트 파일 또는 실행할 수 있는 프로그램 이름으로 인식되지 않습니다`
- PATH는 폴더 경로들을 구분자로 이어 붙인 긴 문자열이다. 구분자는 Bash에서 `:`, Windows에서 `;`이다.
- 프로그램을 설치했는데 이런 에러가 나면 **대부분 PATH 문제**다. 설치 프로그램에 "Add to PATH" 체크박스가 있는 이유가 이것이다.
- 통계 비유: R이 패키지를 `.libPaths()`에 있는 폴더에서만 찾고, Python이 모듈을 `sys.path`에 있는 폴더에서만 찾는 것과 같은 원리다.

### 실습 4. 환경변수와 PATH 들여다보기 (Git Bash)

```bash
echo $USERNAME
echo $HOME
echo $PATH
echo $PATH | tr ':' '\n'
which git
which python
helloworld
export MY_STUDY="cs-study"
echo $MY_STUDY
```

> **`|` (파이프):** 왼쪽 명령의 출력을 오른쪽 명령의 입력으로 넘긴다. R의 `%>%`나 `|>`와 같은 발상이다.
> **`tr ':' '\n'`:** 입력에서 `:`를 줄바꿈(`\n`)으로 바꾼다(translate).

마지막 두 줄까지 실행한 뒤 **터미널을 닫고(휴지통 아이콘) 새로 연 다음** 다시 실행한다.

```bash
echo $MY_STUDY
```

**Q1.** `echo $PATH | tr ':' '\n'`은 무엇을 한 것인가?
<details>
<summary>정답 보기</summary>

PATH는 폴더 경로들을 `:`로 이어 붙인 긴 문자열이다. 이걸 파이프로 `tr`에 넘겨 `:`를 줄바꿈으로 바꿔서, 한 줄에 폴더 하나씩 보기 좋게 출력했다. (Python의 `"a:b".split(":")`과 비슷하다)

</details>

<br>

**Q2.** `which git`과 `which python`의 결과는 무엇을 의미하는가?
<details>
<summary>정답 보기</summary>

셸이 PATH를 뒤져서 찾아낸 실행 파일의 실제 위치. 그 경로의 폴더는 Q1에서 본 PATH 목록 안에 있다. `python`은 Anaconda가 설치한 위치(`/c/Users/user/anaconda3/python`)가 나온다.

</details>

<br>

**Q3.** `helloworld`를 실행하면 에러가 나는 이유는?
<details>
<summary>정답 보기</summary>

PATH에 있는 어떤 폴더에도 `helloworld`라는 실행 파일이 없기 때문이다.

</details>

<br>

**Q4.** 터미널을 새로 열었더니 `echo $MY_STUDY`가 빈 줄을 출력하는 이유는?
<details>
<summary>정답 보기</summary>

`export`로 설정한 환경변수는 **현재 터미널 세션에서만** 유지된다. 터미널을 닫으면 사라진다. 영구적으로 쓰려면 Bash 설정 파일(`~/.bashrc`)에 적어 둔다. (s03과 09단계에서 다룬다)

</details>

---

## 마무리 퀴즈

**Q1.** 터미널과 셸의 차이를 한 문장씩 설명하라.
<details>
<summary>정답 보기</summary>

터미널은 글자를 입력받고 출력을 보여주는 **창**이고, 셸은 입력된 명령을 **해석해서 운영체제에 전달하는 프로그램**이다.

</details>

<br>

**Q2.** Git Bash에서 현재 위치가 `/c/cs-study/02-c/labs`일 때, `cd ../../00-setup`을 실행하면 어디로 가는가?
<details>
<summary>정답 보기</summary>

`/c/cs-study/00-setup`. `labs` → `02-c` → `cs-study`로 두 칸 올라간 뒤 `00-setup`으로 내려간다.

</details>

<br>

**Q3.** `echo "a" > memo.txt`를 실행한 다음 `echo "b" > memo.txt`를 실행했다. memo.txt에는 무엇이 들어 있는가?
<details>
<summary>정답 보기</summary>

`b`만 들어 있다. `>`는 덮어쓰기다. 둘 다 남기려면 두 번째 명령에 `>>`를 써야 한다.

</details>

<br>

**Q4.** 같은 폴더 안에서 `report.txt`의 이름을 `final.txt`로 바꾸는 명령은?
<details>
<summary>정답 보기</summary>

`mv report.txt final.txt`

</details>

<br>

**Q5.** `echo "Price: $100"`을 실행하면 `Price: 00`이 출력된다. 왜 그런가? `Price: $100`을 그대로 출력하려면?
<details>
<summary>정답 보기</summary>

큰따옴표 안에서 `$1`이 변수로 해석되기 때문이다. (`$1`은 비어 있어서 사라지고 `00`만 남는다) 작은따옴표를 써서 `echo 'Price: $100'`이라고 하면 그대로 출력된다.

</details>

<br>

**Q6.** 새 프로그램을 설치했는데 터미널에서 `command not found` 에러가 났다. 가장 먼저 의심할 것은?
<details>
<summary>정답 보기</summary>

그 프로그램의 실행 파일이 있는 폴더가 **PATH에 등록되지 않았을** 가능성. 설치 후 터미널을 새로 열지 않아서 바뀐 PATH가 반영되지 않았을 수도 있다.

</details>

<br>

**Q7.** Linux 서버에 접속하면 PowerShell보다 Bash를 쓸 일이 많은 이유는?
<details>
<summary>정답 보기</summary>

Linux 서버, Docker 컨테이너, CI 스크립트 같은 서버 환경에서는 Bash가 사실상 표준 셸이기 때문이다.

</details>
