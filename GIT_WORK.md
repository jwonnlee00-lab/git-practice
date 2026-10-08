# Git 협업 실습 기록 (GIT_WORK)

## 1. 저장소 및 실습 환경
- 저장소: https://github.com/jwonnlee00-lab/git-practice
- 원격 저장소 별칭: `origin`
- 로컬 복제 폴더 1: `C:\work\git-practice-minsu`
- 로컬 복제 폴더 2: `C:\work\git-practice-jiyun`
- 협업 방식: `main` + 작업 브랜치, GitHub PR, **Create a merge commit**
- 역할은 한 PC·한 계정에서 두 로컬 폴더로 구분함.

## 2. PR 세 개의 작업 및 검토
| PR | 작업 브랜치 → base | 변경 파일 | 작업 내용 | 상태 |
|---|---|---|---|---|
| [#1 민수 작업 메모 추가](https://github.com/jwonnlee00-lab/git-practice/pull/1) | `feature/minsu` → `main` | `minsu.md` | 파일 추가 후 같은 브랜치에 보완 커밋을 push하여 **기존 PR에 반영** | Merged |
| [#2 지윤 작업 메모 추가](https://github.com/jwonnlee00-lab/git-practice/pull/2) | `feature/jiyun` → `main` | `jiyun.md` | 작업 메모 추가 및 내부 제목 '지윤' → '지연' 수정 | Merged |
| [#3 협업 점검표 추가](https://github.com/jwonnlee00-lab/git-practice/pull/3) | `feature/checklist` → `main` | `checklist.md` | 최신 main에서 새 브랜치를 만들고 병합·동기화 점검표 추가 | Merged |

PR #1의 작업 커밋: `22eaf93` → 보완 커밋 `e25d24b`. 기존 PR의 커밋 수가 2개로 늘어난 것을 확인했다.

PR #2의 작업 커밋: `47568a1` → 제목 수정 커밋 `32a4c42`.

PR #3의 점검표 커밋: `a4741f7`.

PR 세 개 모두 GitHub에서 병합 완료를 확인했다. 변경 파일은 각 PR의 **Files changed**에서 확인한다.

## 3. 주요 명령 및 실제 출력

### 민수 폴더 — 브랜치와 로컬 merge
실행 위치: `C:\work\git-practice-minsu`

```powershell
git status
git branch -a
git log --oneline --graph --all -12
git switch main
git merge feature/minsu
```

실제 출력(핵심 부분):
```text
On branch feature/minsu
Your branch is up to date with 'origin/feature/minsu'.
nothing to commit, working tree clean
* 47568a1 (origin/feature/jiyun) 지윤 작업 메모 추가
| * 22eaf93 (HEAD -> feature/minsu, origin/feature/minsu) 민수 작업 메모 추가
|/
* bdedbd0 (origin/main, origin/HEAD, main) Add initial README.md for Git practice
Switched to branch 'main'
Updating bdedbd0..22eaf93
Fast-forward
 minsu.md | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 minsu.md
```

**병합 방향:** `main`으로 전환한 뒤 `git merge feature/minsu`를 실행했으므로, 변경을 받는 쪽은 **로컬 main**이다. 이 작업 자체는 GitHub의 PR 병합이 아니다.

### 민수 폴더 — PR 생성 후 보완 커밋
```powershell
git switch feature/minsu
git diff
git add minsu.md
git commit -m "민수 작업 메모 보완"
git push origin feature/minsu
```

실제 출력(핵심 부분):
```text
[feature/minsu e25d24b] 민수 작업 메모 보완
 1 file changed, 2 insertions(+), 1 deletion(-)
To https://github.com/jwonnlee00-lab/git-practice.git
   22eaf93..e25d24b  feature/minsu -> feature/minsu
```

### 지윤 폴더 — 기존 변경 확인·커밋
실행 위치: `C:\work\git-practice-jiyun`

```powershell
git status
git diff jiyun.md
git add jiyun.md
git commit -m "지연 작업 메모 제목 수정"
git push origin feature/jiyun
```

실제 출력(핵심 부분):
```diff
-# 지윤 작업 메모
+# 지연 작업 메모
```
```text
[feature/jiyun 32a4c42] 지연 작업 메모 제목 수정
 1 file changed, 1 insertion(+), 1 deletion(-)
To https://github.com/jwonnlee00-lab/git-practice.git
   47568a1..32a4c42  feature/jiyun -> feature/jiyun
```

### 지윤 폴더 — 최신 main에서 후속 작업
```powershell
git switch main
git switch -c feature/checklist
git status
git add checklist.md
git commit -m "협업 점검표 추가"
git push -u origin feature/checklist
```

실제 출력(핵심 부분):
```text
On branch feature/checklist
Untracked files:
        checklist.md
[feature/checklist a4741f7] 협업 점검표 추가
 1 file changed, 6 insertions(+)
 create mode 100644 checklist.md
* [new branch]      feature/checklist -> feature/checklist
branch 'feature/checklist' set up to track 'origin/feature/checklist'.
```

### PR 병합 후 두 폴더의 main 동기화
각 폴더에서 수행한 명령:
```powershell
git switch main
git pull origin main
git status
dir *.md
```

세 번째 PR 병합 후 지윤 폴더에서 확인한 실제 출력:
```text
Fast-forward
 checklist.md | 6 ++++++
 1 file changed, 6 insertions(+)
 create mode 100644 checklist.md
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

두 폴더에서 확인한 최종 Markdown 파일:
```text
README.md
minsu.md
jiyun.md
checklist.md
```

### 로컬 작업 브랜치 정리
민수 폴더:
```powershell
git switch main
git branch -d feature/minsu
```
```text
Deleted branch feature/minsu (was e25d24b).
```

지윤 폴더:
```powershell
git switch main
git branch -d feature/jiyun
git branch -d feature/checklist
```
```text
Deleted branch feature/jiyun (was 32a4c42).
Deleted branch feature/checklist (was a4741f7).
```

`git branch -d`는 **로컬 작업 브랜치**를 삭제하며 GitHub의 원격 브랜치를 직접 삭제하지 않는다.

## 4. 확인 질문

**Q1. main에서 `git merge feature/minsu`를 실행하면 어느 브랜치가 변경을 받는가?**

현재 체크아웃한 `main`이 `feature/minsu`의 변경을 받는다. 실습에서는 fast-forward로 로컬 main이 민수 커밋까지 이동했다.

**Q2. GitHub에서 PR을 병합한 뒤에도 각 폴더에서 pull해야 하는 이유는?**

GitHub의 원격 `main`이 변경되어도 각 로컬 폴더의 `main`은 자동 갱신되지 않는다. 각 폴더에서 `git pull origin main`을 실행해야 병합된 파일과 커밋을 가져온다.

## 5. 추후 심화 실습
- 두 폴더에서 같은 파일의 같은 부분을 서로 다르게 수정하여 충돌을 만들고 해결할 예정.
- 이 문서는 대화에서 확인한 실제 명령·출력의 **핵심 부분을 발췌**한 기록이다. 초기 clone, 최초 브랜치 생성 등 전체 터미널 로그가 남아 있지 않은 부분은 재구성된 출력으로 꾸미지 않았다.
