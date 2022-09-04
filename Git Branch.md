# Git Branch

## Git Branch 란?

- **독립적으로 개발할 수 있도록 도와주는 것**
- **장점**
  - 독립공간을 형성하기 때문에 기존의 코드를 손상시키지 않고 안전하게 작업을 진행할 수 있다.
  - 매우 빠릿하고 가벼워서 생성하고 사용하기 쉽다.



- **master branch**
  - 상용하는 브랜치
  - 수정, 추가, 삭제 사항이 있으면 브랜치를 생성해서 독립적으로 작업을 수행 후 마스터 브랜치와 합친다.



- **branch 관련 명령어**
  - `git branch`: 브랜치 목록확인
  - `git branch 브랜치이름`: 새로운 브랜치 생성
  - `git branch -d 브랜치이름`: master와 병합되고 난 뒤의 특정 브랜치 삭제
  - `git branch -D 브랜치이름`: master와 병합되지 않아도 강제로 특정브랜치 삭제
  - `git switch 브랜치이름`: 다른 브랜치로 이동
  - `git switch -c 브랜치이름`: 브랜치를 새로 생성과 동시에 이동
  - `git log --oneline`: 로그기록을 한줄로 보여준다.
  - `git log --oneline --all`: 현재 위치한 브랜치 뿐 아니라 다른 곳의 모든 로그기록을 한번에 보여준다.
  - `git log --online --all --graph`: 그래프로도 갈라진 브랜치들을 보여준다.
  - `git merge 병합할 브랜치 이름`: 메인 브랜치와 다른 브랜치를 합친다.
    - merge 하기 전에 메인브랜치가 되는 브랜치로 이동 후에 명령어를 실행해야함



- **HEAD의 위치**
  - 가장 최근에 만든 커밋 즉, master브랜치의 최근위치
  - branch는  생성 당시의 모습으로 이동한다.



- **주의사항**
  - 브랜치의 위치를 이동하기 전에 add 되지 않은 파일이 있어서 버전관리 하에 있지 않으면 파일 관리상 꼬일 수 있으므로 주의!



- **merge(병합)**

  - `git merge 병합할 브랜치 이름`
  - merge가 된 브랜치는 그 순간 역할이 끝나기 때문에 삭제를 해주면 된다.

  1. fast_forward: merge 과정 없이 그저 최신 커밋으로 이동

     - branch 생성 이후 master의 변화가 없을 때 master와 branch의 병합은  HEAD가 branch에 맞춰 앞으로 나아가는 상황을 말한다.

  2. 3-way merge (merge commit)

     - branch 생성 이후 master가 앞으로 나아간 경우 branch의 가장 최신상태, master의 최신상태, master와 branch의 분기점. 이 3가지 상태를 하나로 병합하면서 새로운  커밋으로 만들어 내는 상황을 말한다.

  3. merge conflict

     - merge하는 두 브랜치에서 같은 파일의 같은 부분을 동시에 수정하고 merge 하면 gi은 해당 부분을 자동으로 merge 해주지 못함

     - 반면 동일 파일이더라도 서로 다른 부분을 수정했다면 conflict 없이 자동으로 merge commit 된다.

     - 충돌 발생시 충돌이 발생한 부분을 수정해주고 commit하면 vim에디터가 뜬다.

     - 충돌 이후의 commit 명령어는 다음과 같다

       ```bash
       $ git commit
       ```

     - vim에서는 저장을 하고 나가야한다.

       - `esc`  > `:`입력 >  `wq`입력 (write quit)
