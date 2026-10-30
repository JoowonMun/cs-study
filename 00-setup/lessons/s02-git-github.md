# s02. Git과 GitHub

## 오늘의 목표

1. Git과 GitHub가 각각 무엇이고 왜 쓰는지 설명할 수 있다
2. 작업 디렉터리, 스테이징 영역, 저장소의 차이를 설명할 수 있다
3. `status` / `add` / `commit` / `log` / `diff`로 변경 사항을 기록할 수 있다
4. `.gitignore`로 올리면 안 되는 파일을 제외할 수 있다
5. **이 저장소를 첫 커밋하고 GitHub 공개 저장소에 올린다**

## 진행 방법

- 준비 → 파트 1 → 2 → 3 → 4 순서로 읽고, 실습(총 5개)을 직접 해 본다. 실습은 **Git Bash**에서 한다.
- 질문에는 **답을 먼저 쓰고** 나서 접힌 정답을 연다.
- 마지막에 마무리 퀴즈를 풀고, `notes/s02-git-github.md`에 내 말로 정리한다.
- 다 끝나면 튜터에게 **틀린 문제, 헷갈린 점, 질문**을 한 번에 보낸다.

## 준비

- [ ] GitHub 계정 만들기: <https://github.com> → Sign up
- [ ] GitHub 이메일 비공개 설정: GitHub 오른쪽 위 프로필 사진 → **Settings** → **Emails** → **Keep my email addresses private** 체크
  - 체크하면 그 아래에 `숫자+아이디@users.noreply.github.com` 형태의 주소가 보인다. **실습 1에서 쓰니 복사해 둔다.**
  - 이유: 커밋에는 작성자 이메일이 기록되고, 공개 저장소에서는 누구나 볼 수 있다. 진짜 이메일 대신 이 주소를 쓰면 스팸을 막을 수 있다.

---

## 파트 1. Git이란

### 1-1. 버전 관리가 필요한 이유

```
보고서.pptx
보고서_수정.pptx
보고서_최종.pptx
보고서_최종_진짜최종.pptx
보고서_최종_진짜최종_팀장님피드백반영.pptx
```

이렇게 파일을 복사해 가며 버전을 관리하면 ① 무엇이 바뀌었는지 알 수 없고 ② 예전 버전으로 돌아가기 어렵고 ③ 여럿이 동시에 고치면 엉킨다.

**Git**은 이 문제를 푸는 **버전 관리 시스템(VCS, Version Control System)**이다. 파일을 복사하는 대신, 원하는 시점마다 프로젝트 전체의 **스냅샷(사진)**을 찍어 기록한다. 이 스냅샷 하나를 **커밋(commit)**이라고 한다.

```
커밋 1 ──▶ 커밋 2 ──▶ 커밋 3 ──▶ 커밋 4
"폴더 구조"  "s01 자료"  "따옴표 추가"  "s02 자료"
```

각 커밋에는 **누가, 언제, 무엇을, 왜** 바꿨는지가 남는다. 언제든 과거 커밋으로 돌아가 볼 수 있다.

### 1-2. Git vs GitHub

| | Git | GitHub |
|---|---|---|
| 정체 | **프로그램** (내 컴퓨터에 설치) | **웹 서비스** (인터넷의 저장소 보관소) |
| 하는 일 | 변경 기록을 만들고 관리 | Git 저장소를 인터넷에 올려 백업·공유·협업 |
| 인터넷 | 없어도 됨 | 필요 |
| 비유 | 사진을 찍는 **카메라** | 사진을 올리는 **클라우드 앨범** |

Git 없이 GitHub는 쓸 수 없지만, GitHub 없이 Git은 혼자서도 쓸 수 있다. GitHub와 비슷한 서비스로 GitLab, Bitbucket이 있다.

### 1-3. 세 가지 영역

Git을 이해하는 핵심이다. 파일은 세 영역을 거쳐 기록된다.

```
 [작업 디렉터리]  ──git add──▶  [스테이징 영역]  ──git commit──▶  [저장소]
  지금 내가 고치는         다음 커밋에 넣을            커밋(스냅샷)이
  실제 파일들             변경 사항을 모아 둔 곳         쌓이는 곳 (.git 폴더)
```

| 영역 | 영어 | 비유 (온라인 쇼핑) |
|---|---|---|
| 작업 디렉터리 | working directory | 상품을 구경하는 매장 |
| 스테이징 영역 | staging area (index) | **장바구니** |
| 저장소 | repository | **결제 완료** 내역 |

- 파일을 고쳐도 자동으로 기록되지 않는다. `git add`로 장바구니에 담고, `git commit`으로 결제해야 기록된다.
- 장바구니가 있는 이유: 여러 파일을 고쳤어도 **관련된 것끼리만 골라서** 커밋할 수 있다. 예를 들어 "s01 자료 수정"과 "README 오타 수정"을 커밋 두 개로 나눌 수 있다.
- 저장소의 실체는 프로젝트 폴더 안의 숨김 폴더 **`.git`**이다. 이 폴더를 지우면 기록이 전부 사라진다. (직접 건드리지 않는다)

### 1-4. 기본 명령어

| 명령어 | 하는 일 |
|---|---|
| `git init` | 현재 폴더를 Git 저장소로 만든다 (`.git` 폴더 생성) |
| `git status` | 각 파일이 어느 영역에 있는지 보여준다. **가장 자주 쓴다** |
| `git add 파일` | 파일의 변경 사항을 스테이징 영역에 담는다 |
| `git add .` | 현재 폴더 아래의 모든 변경 사항을 담는다 |
| `git commit -m "메시지"` | 스테이징된 내용을 커밋으로 기록한다 |
| `git log` | 커밋 기록을 본다 (`--oneline`을 붙이면 한 줄씩) |
| `git diff` | 작업 디렉터리에서 **아직 add하지 않은** 변경 내용을 본다 |
| `git diff --staged` | **add한(스테이징된)** 변경 내용을 본다 |

> 💡 `git log`처럼 출력이 길면 화면이 멈춘 것처럼 보인다. 스크롤 모드라서 그렇다. **`q`를 누르면 빠져나온다.**

### 실습 1. Git 사용자 설정

커밋에 기록될 이름과 이메일을 설정한다. 컴퓨터마다 한 번만 하면 된다. 이메일에는 **준비 단계에서 복사한 noreply 주소**를 넣는다.

```bash
git config --global user.name "GitHub 아이디"
git config --global user.email "숫자+아이디@users.noreply.github.com"
git config --global --list
```

**Q1.** `--global`은 무슨 뜻일까?
<details>
<summary>정답 보기</summary>

이 컴퓨터의 **모든 Git 저장소**에 적용되는 설정이라는 뜻이다. 설정은 홈 디렉터리의 `~/.gitconfig` 파일에 저장된다. (`cat ~/.gitconfig`로 확인해 볼 수 있다) `--global` 없이 저장소 안에서 설정하면 그 저장소에만 적용된다.

</details>

### 실습 2. 연습용 저장소에서 세 영역 익히기

`cs-study`를 건드리기 전에, 홈 디렉터리에 연습용 저장소를 만들어 마음껏 실험한다. **각 명령 뒤에 `git status`의 결과가 어떻게 바뀌는지** 주의해서 본다.

```bash
cd ~
mkdir git-playground
cd git-playground
git init
ls -la
git status

echo "first line" > memo.txt
git status
git add memo.txt
git status
git commit -m "memo.txt 추가"
git status
git log

echo "second line" >> memo.txt
git status
git diff
git add memo.txt
git diff
git diff --staged
git commit -m "memo.txt에 두 번째 줄 추가"
git log --oneline
```

> ⚠️ `warning: ... LF will be replaced by CRLF` 경고가 나와도 괜찮다. 줄바꿈 문자가 Windows(CRLF, `\r\n`)와 Linux(LF, `\n`)에서 달라서, Git이 알아서 변환해 준다는 안내다.

**Q1.** `git init` 뒤에 `ls -la`를 했을 때 새로 생긴 것은?
<details>
<summary>정답 보기</summary>

`.git` 폴더. 이것이 저장소의 실체다. 커밋 기록이 전부 이 안에 저장된다.

</details>

<br>

**Q2.** `memo.txt`를 처음 만든 직후, `git add` 직후, `git commit` 직후에 `git status`는 각각 어떻게 말했는가?
<details>
<summary>정답 보기</summary>

| 시점 | `git status` | 의미 |
|---|---|---|
| 만든 직후 | `Untracked files: memo.txt` | Git이 아직 추적하지 않는 새 파일 |
| `git add` 직후 | `Changes to be committed: new file: memo.txt` | 스테이징 영역(장바구니)에 담김 |
| `git commit` 직후 | `nothing to commit, working tree clean` | 모든 변경이 기록되어 깨끗한 상태 |

</details>

<br>

**Q3.** 두 번째 줄을 추가한 뒤, `git add` 전후로 `git diff`의 결과는 어떻게 달랐는가? 왜 그런가?
<details>
<summary>정답 보기</summary>

`add` 전에는 `+second line`이 보였지만, `add` 후에는 아무것도 나오지 않았다. `git diff`는 **아직 스테이징하지 않은** 변경만 보여주기 때문이다. 스테이징한 변경은 `git diff --staged`로 봐야 보인다.

</details>

<br>

**Q4.** `git log --oneline`에 나온 `a1b2c3d` 같은 문자열은 무엇일까?
<details>
<summary>정답 보기</summary>

**커밋 해시(commit hash)**. 커밋마다 붙는 고유한 ID다. 원래는 40자리인데 앞 7자리만 보여준 것이다. 이 ID로 특정 커밋을 가리킬 수 있다.

</details>

실습이 끝나면 연습용 저장소를 지운다.

```bash
cd ~
rm -rf git-playground
```

> `.git` 폴더 안의 일부 파일은 읽기 전용이라 `rm -r`만으로는 확인을 묻는다. 그래서 `-f`(force)를 함께 쓴다. 지우기 전에 **경로가 `git-playground`인지 꼭 확인한다.**

---

## 파트 2. .gitignore와 .gitkeep

### 2-1. .gitignore

`.gitignore`는 **Git이 무시할 파일 목록**이다. 여기에 적힌 파일은 `git status`에도 나오지 않고, `git add .`을 해도 담기지 않는다.

이 저장소의 `.gitignore`에 들어 있는 것들:

| 패턴 | 제외하는 이유 |
|---|---|
| `.env` | API 키, 비밀번호 같은 **비밀 정보** |
| `private/` | 공개하지 않을 개인 메모 |
| `__pycache__/`, `*.class`, `*.o` | 실행할 때 자동으로 생기는 파일. 소스 코드만 있으면 다시 만들 수 있다 |
| `.venv/` | 가상환경. 용량이 크고, 설치 목록만 있으면 다시 만들 수 있다 |

패턴 규칙: `*`는 "아무 글자나", 끝의 `/`는 "폴더"를 뜻한다. `*.o`는 `.o`로 끝나는 모든 파일이다.

> ⚠️ **한 번 커밋된 파일은 나중에 지워도 기록에 남는다.** 비밀 정보는 처음부터 `.gitignore`에 넣어 둬야 한다. 공개 저장소에 API 키를 올리면, 몇 분 안에 자동 수집 봇이 찾아내 악용하는 일도 흔하다.

### 2-2. .gitkeep 다시 보기

**Git은 폴더가 아니라 파일만 추적한다.** 빈 폴더는 커밋되지 않는다. 그래서 빈 폴더를 유지하려고 아무 내용 없는 `.gitkeep` 파일을 넣어 둔다. Git 공식 기능이 아니라 관례다.

### 실습 3. .gitignore 확인하기

```bash
cd /c/cs-study
git status
mkdir private
echo "회사 관련 메모" > private/memo.txt
echo "API_KEY=abc123" > .env
git status
git check-ignore -v .env
git check-ignore -v private/memo.txt
rm .env
```

**Q1.** `private/`와 `.env`를 만든 뒤에도 `git status`에 나오지 않는 이유는?
<details>
<summary>정답 보기</summary>

`.gitignore`에 적혀 있어서 Git이 무시하기 때문이다. `git check-ignore -v`는 **`.gitignore`의 몇 번째 줄 규칙 때문에** 무시되는지 알려준다.

</details>

<br>

**Q2.** `git status`에 `00-setup/`처럼 폴더 이름만 나오고 안의 파일은 안 보이는 이유는?
<details>
<summary>정답 보기</summary>

폴더 전체가 아직 한 번도 추적된 적 없는(untracked) 상태라서, Git이 폴더 단위로 묶어서 보여준 것이다. `git status -u`를 하면 파일 하나하나를 다 보여준다.

</details>

`private/` 폴더는 지우지 않고 남겨 둔다. 앞으로 공개하면 안 되는 메모는 여기에 쓴다.

---

## 파트 3. 첫 커밋

### 3-1. 좋은 커밋 메시지

커밋 메시지는 **나중의 나와 동료가 읽는 기록**이다. "수정", "asdf" 같은 메시지는 나중에 아무 도움이 안 된다.

- **무엇을, 왜** 했는지 한 줄로 쓴다. (50자 안팎)
- 이 저장소에서는 `단계/세션: 내용` 형식을 권장한다.

```
좋은 예:  00-setup/s02: Git 강의 자료와 노트 추가
좋은 예:  01-python/s03: 기본값 함정 실습 코드 작성
나쁜 예:  수정
나쁜 예:  asdf
```

### 3-2. 커밋하기 전에 확인할 것

공개 저장소에 올라가면 누구나 볼 수 있다. `git add` 전에 **`git status -u`로 파일 목록을 훑어보며** 아래를 확인한다.

- [ ] `.env`, API 키, 비밀번호가 없다
- [ ] 회사명, 고객사, 업무 내용이 없다
- [ ] 공개되어도 괜찮은 내용인가? (`CLAUDE.md`에 적힌 학습자 소개 포함)

### 실습 4. 이 저장소의 첫 커밋

```bash
cd /c/cs-study
git status -u
git add .
git status
git commit -m "00-setup/s01: 저장소 구조와 터미널 강의 자료"
git log
git status
```

**Q1.** `git add .`의 `.`은 무슨 뜻인가?
<details>
<summary>정답 보기</summary>

s01에서 배운 **현재 폴더**다. "현재 폴더 아래의 모든 변경 사항을 담아라"라는 뜻이다. `.gitignore`에 적힌 파일은 제외된다.

</details>

<br>

**Q2.** `git log`에 나온 `Author:`에는 무엇이 적혀 있는가?
<details>
<summary>정답 보기</summary>

실습 1에서 설정한 이름과 noreply 이메일. 이 정보가 GitHub에 그대로 공개된다.

</details>

---

## 파트 4. GitHub에 올리기

### 4-1. 원격 저장소

내 컴퓨터에 있는 저장소를 **로컬 저장소(local)**, GitHub에 있는 저장소를 **원격 저장소(remote)**라고 한다.

```
 [로컬 저장소]  ────git push────▶  [원격 저장소 (GitHub)]
   내 컴퓨터     ◀───git pull────    인터넷
```

| 명령어 | 하는 일 |
|---|---|
| `git remote add origin URL` | 원격 저장소를 `origin`이라는 이름으로 등록한다 |
| `git remote -v` | 등록된 원격 저장소 목록을 본다 |
| `git push -u origin main` | 로컬의 `main` 브랜치 커밋을 `origin`에 올린다 |
| `git push` | (`-u`로 한 번 연결한 뒤에는) 이것만 쳐도 된다 |
| `git pull` | 원격 저장소의 새 커밋을 내려받는다 (다른 컴퓨터에서 작업했을 때) |

- `origin`은 원격 저장소의 **별명**이다. 관례상 이 이름을 쓴다.
- `main`은 **브랜치** 이름이다. 브랜치는 08단계에서 자세히 다룬다. 지금은 "커밋이 쌓이는 줄의 이름" 정도로 이해하면 된다.
- `-u`(upstream)는 "앞으로 로컬 `main`은 `origin`의 `main`과 짝이다"라고 연결해 둔다. 그래서 다음부터는 `git push`만 쳐도 된다.

### 실습 5. GitHub 공개 저장소에 올리기

**1) GitHub에서 저장소 만들기**

GitHub 오른쪽 위 `+` → **New repository**

- Repository name: `cs-study`
- **Public** 선택
- ⚠️ **"Add a README file", ".gitignore", "license"는 모두 체크하지 않는다.** 로컬에 이미 있으므로, 여기서 만들면 두 저장소의 기록이 달라져 push가 거부된다.
- **Create repository** 클릭

**2) 로컬 저장소와 연결하고 올리기**

화면에 나오는 주소를 복사해서 아래 `아이디` 자리에 넣는다.

```bash
cd /c/cs-study
git remote add origin https://github.com/아이디/cs-study.git
git remote -v
git push -u origin main
```

처음 push하면 **GitHub 로그인 창이 브라우저로 뜬다.** (Git for Windows에 포함된 Git Credential Manager가 띄워 준다) 로그인하면 이후에는 자동으로 인증된다.

**3) 확인하기**

브라우저에서 `https://github.com/아이디/cs-study`를 열어 파일이 올라갔는지 확인한다. `README.md`가 첫 화면에 렌더링되어 보일 것이다.

**4) 노트와 일지까지 올려 보기**

마무리 퀴즈와 노트 정리를 끝낸 뒤, 두 번째 커밋을 직접 해 본다.

```bash
git status
git add .
git commit -m "00-setup/s02: Git 노트와 학습 일지"
git push
```

**Q1.** GitHub 웹에서 `00-setup/lessons` 폴더를 열면 `.gitkeep`이 있는가? `01-python/lessons`에는?
<details>
<summary>정답 보기</summary>

`00-setup/lessons`에는 강의 자료(`.md`)가 있어서 `.gitkeep`을 지워 뒀다. `01-python/lessons`는 아직 비어 있어서 `.gitkeep`이 있다. 이 파일 덕분에 빈 폴더도 GitHub에 보인다.

</details>

<br>

**Q2.** `private/memo.txt`는 GitHub에 올라갔는가?
<details>
<summary>정답 보기</summary>

올라가지 않았다. `.gitignore`에 `private/`가 있어서 `git add .`에서 제외됐다.

</details>

<br>

**Q3.** 두 번째에는 `git push -u origin main` 대신 `git push`만 쳐도 된 이유는?
<details>
<summary>정답 보기</summary>

처음 push할 때 `-u`로 로컬 `main`과 `origin/main`을 연결해 뒀기 때문이다.

</details>

---

## 마무리 퀴즈

**Q1.** Git과 GitHub의 차이를 한 문장으로 설명하라.
<details>
<summary>정답 보기</summary>

Git은 내 컴퓨터에서 변경 기록을 관리하는 **프로그램**이고, GitHub는 Git 저장소를 인터넷에 올려 백업·공유·협업하는 **웹 서비스**다.

</details>

<br>

**Q2.** 파일을 고친 뒤 커밋으로 기록되기까지 거치는 세 영역과, 영역 사이를 옮기는 명령어는?
<details>
<summary>정답 보기</summary>

작업 디렉터리 → (`git add`) → 스테이징 영역 → (`git commit`) → 저장소

</details>

<br>

**Q3.** `memo.txt`에 A를 쓰고 `git add memo.txt`를 했다. 그다음 B를 더 쓰고 **add 없이** `git commit`을 했다. 커밋에는 무엇이 들어가는가?
<details>
<summary>정답 보기</summary>

**A만** 들어간다. 커밋은 스테이징 영역에 담긴 내용만 기록한다. B는 작업 디렉터리에만 있는 상태로 남고, `git status`에 `modified`로 표시된다.

</details>

<br>

**Q4.** API 키가 든 `.env`를 실수로 커밋했다가, 다음 커밋에서 지웠다. 이제 안전한가?
<details>
<summary>정답 보기</summary>

**안전하지 않다.** 이전 커밋에 기록이 남아 있어서, 과거 커밋을 열어 보면 누구나 볼 수 있다. 해당 API 키를 **즉시 폐기하고 새로 발급**받아야 한다. 그래서 `.env`는 처음부터 `.gitignore`에 넣어 둔다.

</details>

<br>

**Q5.** 빈 폴더를 만들고 `git add .`을 했는데 `git status`에 아무것도 나오지 않는다. 왜 그런가?
<details>
<summary>정답 보기</summary>

Git은 폴더가 아니라 **파일만** 추적하기 때문이다. 빈 폴더를 유지하려면 안에 `.gitkeep` 같은 파일을 넣는다.

</details>

<br>

**Q6.** `git log`를 쳤더니 화면이 멈추고 아무 명령도 입력되지 않는다. 어떻게 해야 하는가?
<details>
<summary>정답 보기</summary>

`q`를 누른다. 출력이 길어서 스크롤 모드(pager)로 들어간 것이다. `↑` `↓`나 스페이스로 스크롤할 수 있다.

</details>

<br>

**Q7.** 아래 중 좋은 커밋 메시지는?
(a) `수정` (b) `01-python/s07: 은행 계좌 클래스 실습 작성` (c) `ㅁㄴㅇㄹ`
<details>
<summary>정답 보기</summary>

(b). 어느 단계의 어떤 작업인지, 무엇을 했는지 바로 알 수 있다.

</details>

---

## 노트 정리 가이드

`00-setup/notes/s02-git-github.md`에 **강의 자료를 보지 않고** 아래 질문에 내 말로 답해 본다. 막히는 부분만 강의 자료를 다시 보고 채운다.

1. Git과 GitHub는 각각 무엇인가?
2. 세 영역(작업 디렉터리, 스테이징 영역, 저장소)을 나만의 비유로 설명하면?
3. 오늘 배운 명령어 표 (명령어 | 하는 일)
4. `.gitignore`에 넣어야 하는 파일은 어떤 것들이고, 왜 처음부터 넣어야 하는가?
5. 오늘 헷갈렸던 것, 더 궁금한 것
