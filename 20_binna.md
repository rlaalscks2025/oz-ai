# 12월 26일 Git 학습 정리

## Git 브랜치

### 브랜치란?

브랜치는 **평행 세계**와 같다. 각 브랜치는 독립된 세계관을 가지고 있어서, 한쪽에서 작업해도 다른 쪽에 영향을 주지 않는다.

```
master (메인 세계)
  │
  ├── feature/login (로그인 개발 세계)
  │
  └── feature/signup (회원가입 개발 세계)
```

**Git이 알아서 상태를 바꿔준다!**
- `git switch feature/login` → 로그인 세계로 이동 (파일들이 자동으로 바뀜)
- `git switch master` → 메인 세계로 복귀

덕분에 **동시에 여러 가지 작업**을 진행할 수 있다. 로그인 기능 개발하다가 급한 버그 수정이 필요하면? master로 돌아가서 새 브랜치 만들고 수정하면 된다!

---

### 상황 1: 새 기능 개발 시작

로그인 기능을 개발할 때 master 브랜치에서 바로 작업하면 안 된다. 새 브랜치를 만들어야 한다.

```bash
# 현재 브랜치 확인
git branch

# 새 브랜치 만들고 이동
git switch -c feature/login
```

### 상황 2: 코드 작성 후 상태 확인

`login.py` 파일을 만들었다. 상태를 확인해보자.

```bash
git status
```
```
Untracked files:
    login.py
```

아직 Git이 추적 안 하는 상태다!

### 상황 3: 스테이징하기

커밋하려면 먼저 스테이징 영역에 올려야 한다.

```bash
git add login.py
git status
```
```
Changes to be committed:
    new file: login.py
```

이제 커밋 준비 완료!

### 상황 4: 앗, 실수했다! 스테이징 취소

`test.py`도 실수로 add 해버렸다. 취소하고 싶다.

```bash
git restore --staged test.py
```

스테이징만 취소되고 파일 내용은 그대로 유지된다.

### 상황 5: 파일 수정을 아예 되돌리고 싶다

`login.py`를 이상하게 수정해버렸다. 마지막 커밋 상태로 돌아가고 싶다.

```bash
git restore login.py
```

**주의:** 수정한 내용이 전부 사라진다!

### 상황 6: 커밋하기

이제 진짜 커밋!

```bash
git commit -m "feat: Add login feature"
```

### 상황 7: 커밋 히스토리 확인

내가 뭘 커밋했는지 보고 싶다.

```bash
git log --oneline
```
```
abc1234 feat: Add login feature
dfd5348 Initial commit
```

**브랜치 그래프로 보기:**
```bash
git log --graph --all
```

모든 브랜치의 커밋 히스토리를 그래프 형태로 볼 수 있다!

### 상황 8: GitHub에 올리기

**원격 저장소 연결하기:**
```bash
# GitHub 저장소 연결
git remote add origin https://github.com/유저명/저장소명.git

# 연결 확인
git remote -v
```
```
origin  https://github.com/qu3vipon/oz-ai-bootcamp.git (fetch)
origin  https://github.com/qu3vipon/oz-ai-bootcamp.git (push)
```

**푸시하기:**
```bash
git push origin main
```
```
Enumerating objects: 28, done.
Counting objects: 100% (28/28), done.
Delta compression using up to 8 threads
Compressing objects: 100% (23/23), done.
Writing objects: 100% (28/28), 2.25 KiB | 2.25 MiB/s, done.
Total 28 (delta 8), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (8/8), done.
To https://github.com/qu3vipon/oz-ai-bootcamp.git
 * [new branch]      main -> main
```

새 브랜치니까 `-u` 옵션 필요!

```bash
git push -u origin feature/login
```

다음부터는 `git push`만 치면 된다.

### 상황 9: 작업 끝! master로 돌아가기

```bash
git switch master
```

### 상황 10: 다른 브랜치에서 main으로 돌아왔을 때

feature/3way 브랜치에서 작업하다가 main으로 돌아왔다.

```bash
git switch main
```
```
Switched to branch 'main'
```

main에서 상태를 확인해보니 새 파일이 있다!

```bash
git status
```
```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    from_main.txt

nothing added to commit but untracked files present (use "git add" to track)
```

바로 add하고 commit!

```bash
git add from_main.txt
git commit -m "Add from_main.txt"
```
```
[main 941519e] Add from_main.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 from_main.txt
```

프롬프트에서 `main +`는 스테이징된 파일이 있다는 표시다!

---

### 상황 11: 브랜치 병합하기 (Merge)

UI 기능 개발이 완료됐다. 이제 main 브랜치에 합쳐야 한다.

```bash
# main 브랜치로 이동
git switch main

# feature/ui 브랜치를 main에 병합
git merge feature/ui
```
```
Updating 23d13e1..ab9c182
Fast-forward
hello_world.py | 1 +
1 file changed, 1 insertion(+)
```

병합 완료! `Fast-forward`는 충돌 없이 깔끔하게 병합됐다는 뜻이다.

### 3-way Merge란?

![merge gif](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZHZwcGI4cGlyOWhiZ3BoNzVxM3MwZWtranUzemw4anh2emR2bW1xaSZlcD12MV9naWZzX3NlYXJjaCZjdD1n/VePtB3roynxfLYicuV/giphy.gif)

두 브랜치가 각각 다른 커밋을 가지고 있을 때 발생하는 병합 방식이다.

```
c1 ── c2 ── c5 ────────── c6 (main)
             │            ↗
             └── c3 ── c4 (feature)
```

- `c1`, `c2`: 공통 조상
- `c5`: main에서만 진행된 커밋
- `c3`, `c4`: feature에서만 진행된 커밋
- `c6`: 두 브랜치를 합친 **머지 커밋**

```bash
git merge feature/3way
```

Fast-forward와 달리, 새로운 머지 커밋이 생성된다!

### 상황 12: 필요 없는 브랜치 삭제

병합 완료된 브랜치는 삭제해도 된다.

```bash
# 브랜치 생성
git branch feature/conflict

# 브랜치 삭제
git branch -D feature/conflict
```

`-D`는 강제 삭제, `-d`는 병합된 브랜치만 삭제 가능.

### Git 설정: pull 시 rebase 사용

```bash
git config pull.rebase true
```

이 설정을 하면 `git pull` 할 때 merge 대신 rebase를 사용한다. 커밋 히스토리가 더 깔끔해진다!

### Fork & Pull Request (PR) 워크플로우

오픈소스 프로젝트에 기여하는 방법이다!

```
┌─────────────────┐    Pull Request (PR)    ┌─────────────────┐
│  Official       │ ◄───────────────────── │   내 계정        │
│  FastAPI        │                         │   FastAPI       │
└────────┬────────┘         fork           └────────▲────────┘
         │          ─────────────────────►          │
         │                                          │ push
         │ clone                                    │
         ▼                                          │
┌─────────────────┐                        ┌───────┴───────┐
│     로컬        │ ──────────────────────►│ commit:       │
│     (내 PC)     │       작업 후 push      │ bug fix       │
└─────────────────┘                        └───────────────┘
```

**흐름:**
1. **Fork**: 공식 저장소를 내 GitHub 계정으로 복사
2. **Clone**: 내 계정의 저장소를 로컬로 복제
3. **Commit**: 로컬에서 버그 수정/기능 추가
4. **Push**: 내 GitHub 저장소에 업로드
5. **Pull Request**: 공식 저장소에 "내 변경사항을 반영해주세요" 요청

**실제 명령어 흐름:**
```bash
# 1. Fork 후 내 저장소 Clone
git clone https://github.com/내아이디/oz-ai-bootcamp.git
cd oz-ai-bootcamp

# 2. 브랜치 만들고 작업
git switch -c feature/my-fix
# 코드 수정...
git add .
git commit -m "fix: 버그 수정"

# 3. 내 GitHub에 Push
git push -u origin feature/my-fix

# 4. GitHub에서 "Compare & pull request" 버튼 클릭하여 PR 생성
```

### Git Clone: 원격 저장소 복제하기

```bash
git clone https://github.com/유저명/저장소명.git
```

GitHub에 있는 저장소를 내 컴퓨터로 복제한다. 폴더째로 다운로드되고, 자동으로 원격 저장소(origin)도 연결된다!

**특정 폴더명으로 클론:**
```bash
git clone https://github.com/유저명/저장소명.git 내폴더명
```

### Git 저장소 삭제하기

**방법 1: .git 폴더 삭제 (Git 추적만 해제)**
```bash
# Windows
rmdir /s /q .git

# Mac/Linux
rm -rf .git
```

이렇게 하면 Git 히스토리만 삭제되고 파일은 그대로 남는다.

**방법 2: 폴더 전체 삭제**
```bash
# Windows
rmdir /s /q 폴더명

# Mac/Linux
rm -rf 폴더명
```

---

## 핵심 요약

| 하고 싶은 것 | 명령어 |
|-------------|--------|
| 브랜치 만들고 이동 | `git switch -c 브랜치명` |
| 상태 확인 | `git status` |
| 스테이징 | `git add 파일명` |
| 스테이징 취소 | `git restore --staged 파일명` |
| 수정 되돌리기 | `git restore 파일명` |
| 커밋 | `git commit -m "메시지"` |
| 히스토리 보기 | `git log --oneline` |
| 푸시 (새 브랜치) | `git push -u origin 브랜치명` |
| 브랜치 이동 | `git switch 브랜치명` |

---

## 오늘 배운 Git 명령어

### 1. git branch (브랜치 관리)

```bash
# 브랜치 목록 보기
git branch

# 새 브랜치 생성
git branch 브랜치명

# 브랜치 이동
git checkout 브랜치명
git switch 브랜치명

# 생성 + 이동 한번에
git checkout -b 브랜치명
git switch -c 브랜치명
```

---

### 2. git add (스테이징)

```bash
# 모든 파일 스테이징
git add .

# 특정 파일만 스테이징
git add 파일명
```

---

### 3. git status (상태 확인)

```bash
git status
```

- `Untracked files`: Git이 추적하지 않는 파일
- `Changes not staged`: 수정됐지만 add 안 된 파일
- `Changes to be committed`: 커밋 대기 중인 파일

---

### 4. git log (커밋 히스토리)

```bash
# 전체 로그 보기
git log

# 한 줄로 보기
git log --oneline
```

---

### 5. git restore (변경사항 되돌리기)

```bash
# 특정 파일 변경 취소 (스테이징 전)
git restore 파일명

# 모든 변경 취소
git restore .

# 스테이징된 파일 언스테이징 (변경은 유지)
git restore --staged 파일명
```

---

### 6. git commit (커밋)

```bash
git commit -m "커밋 메시지"
```

**좋은 커밋 메시지 예시:**
- `feat: 새 기능 추가`
- `fix: 버그 수정`
- `docs: 문서 수정`

---

## 오늘의 작업 흐름

```bash
# 1. 상태 확인
git status

# 2. 변경사항 스테이징
git add .

# 3. 커밋
git commit -m "커밋 메시지"

# 4. 푸시 (새 브랜치일 경우)
git push -u origin 브랜치명

# 5. 이후 푸시
git push
```
