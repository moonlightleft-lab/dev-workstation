## 미션의 의미와 핵심 개념 정리

### 1. 미션의 의미: 재현 가능한 개발 워크스테이션 구성
- 코드를 작성하기에 앞서, **어디서든 동일하게 실행되는 개발 환경(워크스테이션)**을 직접 구성하고 검증하는 연습입니다.
- 터미널(CLI)로 파일과 권한을 관리하고, Docker로 실행 환경을 패키징하며, Git/GitHub로 작업 이력과 문서를 남김으로써 환경 차이("내 컴퓨터에서만 동작하는 문제")를 줄이고 재현성을 확보하는 것을 목표로 합니다.

### 2. 핵심 개념 요약
- **절대 경로 vs 상대 경로 및 파일 권한(`rwx`, `755`/`644`):**
  - **경로:** 현재 작업 위치 기준(`상대 경로`)과 최상위 기준(`절대 경로`)을 구분해 정확한 위치에서 명령과 빌드를 수행합니다.
  - **권한:** 소유자·그룹·기타 사용자별 읽기(`r`), 쓰기(`w`), 실행(`x`) 규칙을 관리해 불필요한 수정이나 실행 오류를 방지합니다.
- **Docker와 커스텀 이미지(`Dockerfile`):**
  - **이미지:** 컨테이너를 생성하기 위한 템플릿(설계도)이며, **컨테이너:** 이미지를 바탕으로 실행되는 격리된 프로세스입니다.
  - `Dockerfile`에 베이스 이미지(`FROM nginx:alpine`)와 웹 콘텐츠(`COPY`)를 정의해 일관된 웹 서버 환경을 빌드합니다.
- **포트 매핑(`-p 8080:80` 또는 `-p 8080:5000`):**
  - 격리된 컨테이너 내부 포트와 호스트 포트를 연결하여, 외부(브라우저, `curl` 등)에서 호스트 포트를 통해 컨테이너 서비스에 접속할 수 있도록 합니다.
- **바인드 마운트 vs Docker 볼륨(데이터 영속성):**
  - **바인드 마운트:** 호스트 디렉토리를 컨테이너 경로에 직접 연결해 호스트 측 변경 사항을 빠르게 반영·확인합니다.
  - **Docker 볼륨:** Docker가 관리하는 저장 공간을 연결해 컨테이너 중지·삭제 이후에도 데이터가 유지(영속성)되도록 합니다.
- **Git과 GitHub:**
  - **Git:** 로컬 환경에서 변경 이력을 추적하고 커밋(`add`, `commit`)으로 버전을 관리하는 도구입니다.
  - **GitHub:** 로컬 변경 이력과 문서를 원격 저장소에 공유·보관하고 제출할 수 있는 협업 플랫폼입니다.

### 3. 실행 로그와 트러블슈팅 기록의 목적
- 명령어와 실제 실행 결과를 문서에 함께 남김으로써, 수행 과정을 객관적으로 검증하고 동일한 절차로 재현할 수 있도록 합니다.
- 빌드 경로 오류(`COPY` 대상 경로 불일치 등)처럼 겪은 문제를 원인 가설·검증·조치 과정으로 기록해 환경 구조와 동작 원리에 대한 이해를 높입니다.

## 핵심 용어 정리

### 1. 터미널 · 파일 · 권한 관련 용어
* **터미널(Terminal) / CLI**: 마우스 클릭 대신 글자(명령어)로 컴퓨터와 대화하는 창입니다. (예: PowerShell, bash)
* **절대 경로**: 최상위 기준(`C:\` 또는 `/`)부터 시작하는 전체 주소입니다. 어디서 명령하든 같은 위치를 가리킵니다. (예: `C:\test\집\dev-workstation`)
* **상대 경로**: 현재 내 위치를 기준으로 삼는 짧은 주소입니다. `.`은 현재 폴더, `..`은 한 단계 위 폴더를 뜻합니다. (예: `./README.md`)
* **파일 권한(`r`, `w`, `x`)**: 파일이나 폴더를 누가 어떻게 다룰 수 있는지 정한 규칙입니다. `r`(읽기, 4), `w`(쓰기, 2), `x`(실행, 1)를 더해서 `755`나 `644` 같은 숫자로 표현합니다.

### 2. Docker · 컨테이너 · 스토리지 관련 용어
* **Docker(도커)**: 앱과 그 앱이 돌아가는 데 필요한 환경(설정, 라이브러리 등)을 한데 묶어 어디서든 똑같이 실행되게 해주는 도구입니다.
* **이미지(Image)**: 컨테이너를 찍어내기 위한 설계도(틀)입니다. 한 번 만들어지면 기본 틀은 변하지 않습니다.
* **컨테이너(Container)**: 이미지를 바탕으로 실제로 실행 중인 프로그램 공간입니다. 껐다 켜거나 지울 수 있고, 같은 이미지로 여러 개를 띄울 수 있습니다.
* **Dockerfile**: 도커 이미지를 어떻게 만들지 적어 둔 레시피(설명서) 파일입니다. (`FROM`, `COPY` 같은 명령어로 구성)
* **OrbStack**: 컨테이너를 가볍고 빠르게 실행하게 해주는 도구입니다. `sudo` 권한이 제한된 환경에서도 별도 관리자 권한 없이 Docker 엔진을 쓰기 위해 활용됩니다.
* **포트 매핑(`-p 8080:5000`)**: 내 컴퓨터(호스트)의 문(`8080`)과 컨테이너 안의 문(`5000`)을 연결하는 것입니다. 그래야 브라우저에서 `localhost:8080`으로 접속해 컨테이너 안 앱을 볼 수 있습니다[cite: 1].
* **바인드 마운트(Bind Mount)**: 내 컴퓨터의 특정 폴더를 컨테이너 안 폴더와 직접 연결하는 방식입니다. 내 컴퓨터에서 파일을 수정하면 컨테이너에도 바로 반영됩니다[cite: 1].
* **볼륨(Volume)**: 도커가 직접 관리하는 데이터 저장 공간입니다. 컨테이너를 삭제(`docker rm`)해도 볼륨에 저장된 데이터는 지워지지 않고 남아 있습니다.

### 3. Git · GitHub · 협업 관련 용어
* **Git(깃)**: 내 컴퓨터에서 코드 변경 이력을 버전별로 저장하고 관리하는 프로그램입니다. ("언제, 누가, 무엇을 바꿨는지" 기록)
* **GitHub(깃허브)**: Git으로 관리하는 코드를 인터넷(클라우드)에 올려두고 공유·협업하는 웹사이트/플랫폼입니다.
* **저장소(Repository, Repo)**: 코드와 변경 이력, 문서, 이미지 등이 모여 있는 프로젝트 폴더 단위 공간입니다. 내 컴퓨터에 있으면 로컬 저장소, GitHub에 있으면 원격 저장소(`origin`)라고 부릅니다.
* **브랜치(Branch)**: 작업 갈래(줄기)입니다. 보통 대표 갈래를 `main`이라고 부릅니다.
* **`git add` / `commit` / `push`**: `add`는 이번에 저장할 파일 고르기, `commit`은 버전 스냅샷 찍기(내 컴퓨터에 기록), `push`는 GitHub 원격 저장소에 올리기입니다.
* **`git pull` / 병합(Merge)**: `pull`은 GitHub에 올라온 최신 내용을 내 컴퓨터로 가져오는 것이고, `merge`는 두 작업 내역을 하나로 합치는 과정입니다.
* **충돌(Conflict)**: 같은 파일의 같은 부분을 서로 다르게 수정해서 Git이 어느 쪽을 살릴지 자동으로 결정하지 못할 때 발생하는 상황입니다.
* **LF / CRLF**: 줄바꿈 문자를 표시하는 방식 차이입니다. Windows(`CRLF`)와 Linux/macOS(`LF`)가 달라서 Git에서 안내 경고가 뜨기도 합니다.



# 내 컴퓨터에 개발자용 '작업실' 꾸미기

## 1. 프로젝트 개요 및 미션 목표
- **미션 목표**: 개발의 출발점인 **터미널(CLI)**, **Docker(컨테이너)**, **Git/GitHub(버전 관리 및 협업)** 환경을 직접 구성하고, 코드가 *"내 컴퓨터에서만"* 동작하는 문제를 줄여 재현 가능한 개발 워크스테이션을 구축합니다.
- **핵심 구조 원칙**:
  - **이미지와 컨테이너의 분리**: 불변의 실행 템플릿(이미지)과 실제 실행 인스턴스(컨테이너)를 구분합니다.
  - **격리된 실행 환경**: 의존성과 프로세스를 격리해 환경 충돌을 최소화합니다.
  - **포트 및 스토리지 연결 방식**: 포트 매핑(`-p`)으로 외부 트래픽을 전달하고, 바인드 마운트/볼륨으로 실시간 코드 반영 및 데이터 영속성을 확보합니다.
- **확장성**: 본 경험은 향후 리눅스 트러블슈팅, CI/CD 파이프라인, 클라우드 배포 및 운영으로 이어집니다.
- **(서울캠퍼스 환경 안내)**: 시스템 보안 정책상 `sudo` 권한 사용이 제한될 수 있는 환경에서는 **OrbStack**을 활용해 별도의 `sudo` 권한 없이 Docker 엔진을 구동하고, 터미널에서 `docker run`, `docker ps`, `docker build` 등 기존과 동일한 `docker` 명령어를 사용할 수 있습니다.

---

## 2. 실행 환경
| 구분 | 항목 | 비고 |
| :--- | :--- | :--- |
| **OS / Shell / 터미널** | Windows (PowerShell) *(또는 Linux/macOS 환경 기재)* | `C:\test\집\dev-workstation` 기준 실습 |
| **컨테이너 런타임** | Docker Engine (OrbStack / Docker Desktop) | `docker --version`, `docker info` 확인 |
| **버전 관리** | Git | `git config --list` 확인 |
| **에디터 / 연동** | VS Code + GitHub | 저장소 원격 연동 |

> **재현성 안내**: 개인 PC 경로(`C:\test\집\dev-workstation` 등)는 각자 작업 디렉토리로 대체하여 실행할 수 있습니다. 민감정보(토큰, 비밀번호, 개인키, 인증 코드, 개인 식별 정보)는 로그 및 스크린샷에 포함하지 않았습니다.

---

## 3. 수행 항목 체크리스트
- [x] 터미널 기본 조작 및 작업 디렉토리 구성(`pwd`, `ls`, `cd`, `mkdir`, `cp`, `mv`, `rm`, 파일 읽기/빈 파일 생성)
- [ ] 파일(1개 이상) 및 디렉토리(1개 이상) 권한 확인/변경 전·후 비교
- [x] Docker 설치/구동 점검(`docker --version`, `docker info`)
- [ ] Docker 기본 운영 명령 수행(`docker images`, `docker ps`, `docker ps -a`, `docker logs`, `docker stats`)
- [ ] 컨테이너 실행 실습(`hello-world`, `ubuntu` 진입 및 명령 실행, 종료/유지 및 `attach`/`exec` 차이 관찰)
- [x] Dockerfile 작성 및 커스텀 웹 서버 이미지 빌드/실행
- [x] 포트 매핑 접속 확인(`localhost:8080` 브라우저 접속 화면 확보)[cite: 1]
- [x] 바인드 마운트 반영 확인(호스트 파일 변경 반영 확인)[cite: 1]
- [x] Docker 볼륨 생성·연결 및 컨테이너 삭제 전·후 데이터 영속성 검증
- [x] Git 사용자/기본 브랜치 설정(`git config --list`) 및 GitHub/VSCode 연동

---

## 4. 과제 목표 자가 점검(핵심 개념 정리)
1. **절대 경로 vs 상대 경로**:
   - **절대 경로**: 루트(`/`) 또는 드라이브 루트(`C:\`)부터 시작하는 전체 경로입니다(예: `/var/log/syslog`, `C:\test\집\dev-workstation`). 현재 위치와 무관하게 동일한 대상을 가리킵니다.
   - **상대 경로**: 현재 작업 디렉토리(`.`)나 상위 디렉토리(`..`)를 기준으로 표기하는 경로입니다(예: `./docker/Dockerfile`, `../app`). 현재 위치에 따라 해석이 달라집니다.
2. **파일 권한(`r`/`w`/`x`)과 `755`, `644` 표기**:
   - `r`(읽기, 4), `w`(쓰기, 2), `x`(실행, 1)의 합으로 **소유자(User) / 그룹(Group) / 기타(Others)** 권한을 표현합니다.
   - `755` (`rwxr-xr-x`): 소유자는 읽기·쓰기·실행(4+2+1=7), 그룹과 기타 사용자는 읽기·실행(4+1=5)이 가능합니다(실행 파일/디렉토리 등에 활용).
   - `644` (`rw-r--r--`): 소유자는 읽기·쓰기(4+2=6), 그룹과 기타 사용자는 읽기(4)만 가능합니다(일반 문서/설정 파일 등에 활용).
3. **기존 Dockerfile 기반 커스텀 이미지 제작**:
   - 베이스 이미지/예시 `Dockerfile` 위에 필요한 패키지, 설정, 웹 콘텐츠, 환경변수, 헬스체크 등을 추가해 재현 가능한 커스텀 이미지를 빌드할 수 있습니다.
4. **포트 매핑이 필요한 이유**:
   - 컨테이너는 자체 네트워크 네임스페이스를 가지므로 호스트 포트와 컨테이너 포트를 연결(`-p <host_port>:<container_port>`)해야 외부(브라우저 등)에서 호스트를 통해 컨테이너 서비스에 접근할 수 있습니다.
5. **Docker 볼륨(영속 데이터)**:
   - 컨테이너가 삭제되거나 재생성되어도 컨테이너 파일시스템과 분리된 Docker 관리 스토리지(`docker volume`)에 데이터가 유지되므로 상태/데이터 영속성을 보장할 수 있습니다.
6. **Git과 GitHub의 역할 차이**:
   - **Git**: 로컬 환경에서 소스코드 변경 이력을 추적·관리하는 분산 버전 관리 시스템(VCS)입니다.
   - **GitHub**: Git 저장소를 호스팅하고 Pull Request, 이슈, 코드 리뷰, 협업 및 원격 백업을 지원하는 클라우드 플랫폼입니다.

---

## 5. 검증 방법 및 수행 로그 / 증거

### 5.1 터미널 조작 로그
현재 위치 확인, 목록 확인(숨김 파일 포함), 이동, 생성, 복사, 이동/이름변경, 삭제, 파일 내용 확인, 빈 파일 생성 명령과 출력입니다.
```text
[여기에 터미널 기본 조작 명령어 및 출력 결과를 붙여넣으세요]
```

### 5.2 권한 실습 및 변경 전·후 비교
파일 1개, 디렉토리 1개에 대해 권한 확인 → 변경 → 변경 후 확인 결과입니다.
```text
[여기에 파일/디렉토리 권한 변경 전/후 명령어 및 출력 결과를 붙여넣으세요]
```

### 5.3 Docker 설치 및 기본 점검 (`docker --version`, `docker info`)
```text
[여기에 docker --version 및 docker info 명령어와 출력 결과를 붙여넣으세요]
```

### 5.4 Docker 기본 운영 명령 (`images`, `ps`, `ps -a`, `logs`, `stats`)
```text
[여기에 docker images, docker ps, docker ps -a, docker logs, docker stats 명령어와 출력 결과를 붙여넣으세요]
```

### 5.5 컨테이너 실행 실습 (`hello-world`, `ubuntu`, 종료/유지 및 `attach`/`exec` 비교)
- **`hello-world` 및 `ubuntu` 실행/내부 명령(`ls`, `echo` 등) 로그**:
```text
[여기에 hello-world 및 ubuntu 컨테이너 실행/내부 명령 출력 결과를 붙여넣으세요]
```
- **컨테이너 종료/유지 및 `attach` vs `exec` 관찰 요약**:
  - [메인 프로세스 종료 시 컨테이너 종료 여부, `-it` / `-d` 유지 방식, `attach`와 `exec`의 동작 차이를 정리하세요]

### 5.6 커스텀 이미지 제작 (`Dockerfile` 및 웹 소스)
- **선택한 베이스**: [예: `nginx:alpine` 또는 `ubuntu` / `python` 등 선택한 베이스 이미지/예시 기재]
- **커스텀 포인트 및 목적**: [정적 콘텐츠/설정 교체, 패키지 설치, 환경변수 설정 등 커스텀 항목별 목적 요약]
- **빌드/실행 명령 및 결과 로그**:
```text
[여기에 docker build 및 docker run 명령어와 출력 결과를 붙여넣으세요]
```
- **관련 파일 링크**: [Dockerfile](./docker/Dockerfile), [웹 소스 디렉토리](./docker/app/)

### 5.7 포트 매핑 접속 증거 및 바인드 마운트 반영 증거
- **포트 매핑 접속**: 호스트 포트와 컨테이너 포트를 연결해 브라우저에서 `localhost:8080` 접속을 확인했습니다[cite: 1].
- **바인드 마운트 반영**: 호스트 파일 변경 내용이 이미지 재빌드 없이 컨테이너에 반영되는 것을 확인했습니다[cite: 1].
- **바인드 마운트 실행 명령 및 호스트 변경 전/후 비교**:
```text
[여기에 바인드 마운트 실행 명령 및 호스트 파일 변경 전/후 비교 로그를 붙여넣으세요]
```
- **접속 및 바인드 마운트 스크린샷**:
  - 브라우저 주소창(`localhost:8080`)과 `"Hello, Bind Mount!"` 응답 화면을 캡처한 이미지를 저장소(예: `./assets/localhost-8080.png`)에 추가한 뒤 아래 링크를 활성화하세요[cite: 1].
  - `![포트 매핑 및 바인드 마운트 검증 화면](./assets/localhost-8080.png)`

### 5.8 Docker 볼륨 영속성 검증
볼륨 생성 → 컨테이너 연결 및 데이터 기록 → 컨테이너 삭제(`docker rm -f`) → 새 컨테이너 연결 후 데이터 유지 확인까지의 실행 로그입니다.
```powershell
PS C:\test\집\dev-workstation> docker volume create mission-vol
>> docker run -d --name vol-check -v mission-vol:/data ubuntu sleep infinity
>> docker exec vol-check bash -lc "echo 'persisted-data-test' > /data/test.txt && cat /data/test.txt"
mission-vol
edb3d07049ccf52ffd2716b5127c15d4dc6247a81cc23d07ab0442d47d33a8b6
persisted-data-test
PS C:\test\집\dev-workstation> docker rm -f vol-check
>> docker run -d --name vol-check-2 -v mission-vol:/data ubuntu sleep infinity
>> docker exec vol-check-2 bash -lc "cat /data/test.txt"
vol-check
68b61d0dd54a0ff25366567624832281a676d8325f0ad5b11a62b834575cedee
persisted-data-test
```

### 5.9 Git 설정 및 GitHub / VS Code 연동 증거
- **`git config --list` 출력 결과**:
```powershell
PS C:\test\집\dev-workstation> git config --global user.name "moon"
PS C:\test\집\dev-workstation> git config --global user.email "moon950605@gmail.com"
PS C:\test\집\dev-workstation> git config --global init.defaultBranch main
PS C:\test\집\dev-workstation> git config --list
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=schannel
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
credential.[https://dev.azure.com.usehttppath](https://dev.azure.com.usehttppath)=true
init.defaultbranch=master
user.name=moon
user.email=moon950605@gmail.com
init.defaultbranch=main
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=[https://github.com/moonlightleft-lab/dev-](https://github.com/moonlightleft-lab/dev-)
PS C:\test\집\dev-workstation> git init
Reinitialized existing Git repository in C:/test/집/dev-workstation/.git/
PS C:\test\집\dev-workstation> git add .
warning: in the working copy of 'docker/Dockerfile', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'docker/app/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'permission-test/sample.txt', LF will be replaced by CRLF the next time Git touches it
```
- **GitHub 및 VS Code 연동 증거**:
  - [VS Code GitHub 로그인/저장소 연동 스크린샷 경로를 첨부하세요, 예: `![VS Code GitHub 연동](./assets/vscode-github.png)`]

---

## 6. 트러블슈팅 (2건 이상)

### 사례 1: Windows 작업 공간에서 Git 스테이징 시 `LF will be replaced by CRLF` 경고 발생
- **문제**: `git add .` 실행 시 `docker/Dockerfile`, `docker/app/index.html`, `permission-test/sample.txt` 파일에서 `LF will be replaced by CRLF` 경고가 출력되었습니다.
- **원인 가설**: Windows 환경에서 Git 설정(`core.autocrlf=true`)이 활성화되어 있어 줄바꿈 문자(LF ↔ CRLF) 자동 변환을 안내하는 메시지일 것으로 추정했습니다.
- **확인**: 명령 종료 코드와 스테이징 대상 파일 포함 여부를 확인한 결과, 에러가 아닌 경고이며 스테이징 자체는 정상 진행됨을 확인했습니다.
- **해결/대안**: 스테이징 정상 동작을 확인하고 진행했으며, 팀 협업이나 쉘 스크립트/Dockerfile 줄바꿈 일관성이 중요한 경우에는 `.gitattributes`로 줄바꿈 규칙을 명시하는 방식을 활용할 수 있습니다.

### 사례 2: [본인이 겪은 두 번째 문제 제목을 입력하세요 — 예: 권한 제한/포트 충돌/원격 저장소 URL 확인 등]
- **문제**: [발생한 문제 현상 기재]
- **원인 가설**: [원인으로 추정한 가설 기재]
- **확인**: [진단 명령어 또는 상태 확인 결과 기재]
- **해결/대안**: [적용한 해결책 또는 대안 기재]

---

## 7. 보너스 과제 (선택)
- [ ] **보너스 1 (Docker Compose 기초)**: 단일 서비스 `docker-compose.yml` 작성 및 실행 (실행 명령을 문서화하는 이유 정리)
- [ ] **보너스 2 (Docker Compose 멀티 컨테이너)**: 웹 서버 + 보조 서비스(2개 이상) 구성 및 컨테이너 간 네트워크 통신 확인
- [ ] **보너스 3 (Compose 운영 명령어)**: `up`, `down`, `ps`, `logs` 루틴 실습
- [ ] **보너스 4 (환경 변수 활용)**: 환경 변수로 포트/모드 제어(설정과 코드의 분리)
- [ ] **보너스 5 (GitHub SSH 키 설정)**: SSH 키 등록 및 푸시 동작 확인

# 파일/폴더 생성·이동·삭제 명령과 그 출력(mkkir,mv,rm등) 실행 증거
PS C:\test\집> cd dev-workstation
PS C:\test\집\dev-workstation> mkdir test-dir


    디렉터리: C:\test\집\dev-workstation


Mode                 LastWriteTime         Length Name                    
----                 -------------         ------ ----                    
d-----      2026-11-20  오전 11:21                test-dir                


PS C:\test\집\dev-workstation> New-Item -ItemType File -Path ".\test-dir\sample.txt" -Value "hello"


    디렉터리: C:\test\집\dev-workstation\test-dir


Mode                 LastWriteTime         Length Name                    
----                 -------------         ------ ----                    
-a----      2026-11-20  오전 11:21              5 sample.txt              


PS C:\test\집\dev-workstation> dir .\test-dir


    디렉터리: C:\test\집\dev-workstation\test-dir


Mode                 LastWriteTime         Length Name                    
----                 -------------         ------ ----                    
-a----      2026-11-20  오전 11:21              5 sample.txt              


PS C:\test\집\dev-workstation> mkdir move-dir


    디렉터리: C:\test\집\dev-workstation


Mode                 LastWriteTime         Length Name                    
----                 -------------         ------ ----                    
d-----      2026-11-20  오전 11:22                move-dir                


PS C:\test\집\dev-workstation> mv .\test-dir\sample.txt .\move-dir\sample-moved.txt -Verbose
자세한 정보 표시: 대상 "항목: 
C:\test\집\dev-workstation\test-dir\sample.txt 대상: 
C:\test\집\dev-workstation\move-dir\sample-moved.txt"에서 "파일 이동" 
작업을 수행합니다.
PS C:\test\집\dev-workstation> dir .\move-dir


    디렉터리: C:\test\집\dev-workstation\move-dir


Mode                 LastWriteTime         Length Name                    
----                 -------------         ------ ----                    
-a----      2026-11-20  오전 11:21              5 sample-moved.txt        


PS C:\test\집\dev-workstation> rm .\move-dir\sample-moved.txt -Verbose
자세한 정보 표시: 대상 
"C:\test\집\dev-workstation\move-dir\sample-moved.txt"에서 "파일 제거" 
작업을 수행합니다.
PS C:\test\집\dev-workstation> rm .\test-dir, .\move-dir -Recurse -Force -Verbose
자세한 정보 표시: 대상 "C:\test\집\dev-workstation\test-dir"에서 "디렉터리
 제거" 작업을 수행합니다.
자세한 정보 표시: 대상 "C:\test\집\dev-workstation\move-dir"에서 "디렉터리
 제거" 작업을 수행합니다.
PS C:\test\집\dev-workstation> dir


    디렉터리: C:\test\집\dev-workstation


Mode                 LastWriteTime         Length Name                    
----                 -------------         ------ ----                    
d-----      2026-11-19   오전 9:51                app                     
d-----      2026-11-19  오전 11:40                docker                  
d-----      2026-11-19   오전 9:51                logs                    
d-----      2026-11-19  오전 10:00                permission-test         
d-----      2026-11-19   오전 9:54                practice                
d-----      2026-11-19   오전 9:51                screenshots             
-a----      2026-11-19   오후 3:36          16354 Hello Bind  mount스크린 
                                                  샷.png                  
-a----      2026-11-19   오후 3:36          16323 Hello Docker 스크린샷.pn
                                                  g                       
-a----      2026-11-19   오후 3:13              0 main.py                 
-a----      2026-11-19   오후 4:07          17459 README.md               


PS C:\test\집\dev-workstation> 


# chmod실행로그와 ls -l비교출력 증거
ADMIN@2026□□□□□□ UCRT64 /c/test/집 (practice)
$ cd /c/test/집/dev-workstation

ADMIN@2026□□□□□□ UCRT64 /c/test/집/dev-workstation (main)
$ ls -ld permission-test
drwxr-xr-x 1 ADMIN 197121 0 Nov 19 10:00 permission-test/

ADMIN@2026□□□□□□ UCRT64 /c/test/집/dev-workstation (main)
$ ls -l permission-test/sample.txt
-rw-r--r-- 1 ADMIN 197121 26 Nov 19 09:59 permission-test/sample.txt

ADMIN@2026□□□□□□ UCRT64 /c/test/집/dev-workstation (main)
$ chmod 755 permission-test/sample.txt

ADMIN@2026□□□□□□ UCRT64 /c/test/집/dev-workstation (main)
$ chmod 755 permission-test

ADMIN@2026□□□□□□ UCRT64 /c/test/집/dev-workstation (main)
$ ls -ld permission-test
drwxr-xr-x 1 ADMIN 197121 0 Nov 19 10:00 permission-test/

ADMIN@2026□□□□□□ UCRT64 /c/test/집/dev-workstation (main)
$ ls -l permission-test/sample.txt
-rw-r--r-- 1 ADMIN 197121 26 Nov 19 09:59 permission-test/sample.txt

ADMIN@2026□□□□□□ UCRT64 /c/test/집/dev-workstation (main)
$ 


# docker --version과 데몬상태(docker info등)실제 출력 증거
PS C:\test\집> cd dev-workstation
PS C:\test\집\dev-workstation> docker --version
Docker version 29.9.2, build a82e528
PS C:\test\집\dev-workstation> docker info
Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.152.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v5.6.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.98.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-desktop.exe
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.9
    Path:     C:\Program Files\Docker\cli-plugins\docker-dhi.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     C:\Program Files\Docker\cli-plugins\docker-extension.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-init.exe
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.2
    Path:     C:\Program Files\Docker\cli-plugins\docker-mcp.exe
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     C:\Program Files\Docker\cli-plugins\docker-model.exe
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.35
    Path:     C:\Program Files\Docker\cli-plugins\docker-offload.exe
  pass: Docker Pass Secrets Manager Plugin (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     C:\Program Files\Docker\cli-plugins\docker-pass.exe
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-sandbox.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.27.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-scout.exe

Server:
 Containers: 6
  Running: 2
  Paused: 0
  Stopped: 4
 Images: 7
 Server Version: 29.9.2
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: local
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Discovered Devices:
  cdi: docker.com/gpu=webgpu
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 nvidia runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b7a25261e029d68978bbf290df0a62fc819b3c51
 runc version: v1.5.2-0-g29dd3dc2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.18.58.1-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 16
 Total Memory: 3.756GiB
 Name: docker-desktop
 ID: 968b325e-3e0e-484f-8cf1-f33b151cb409
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables


 # docker run hello-world 실행결과 증거
 PS C:\test\집\dev-workstation> docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/


# docker images 및 docker ps -a 출력 증거
PS C:\test\집\dev-workstation> docker images
                                                      i Info →   U  In Use
IMAGE                  ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest     1e4c05b897a7       25.9kB         9.49kB    U   
my-custom-web:latest   4a63261d65ed       93.6MB         26.3MB    U   
my-first-web:latest    5550ccb9a3c3       93.6MB         26.4MB        
my-web-image:latest    6780f1560724       93.6MB         26.3MB    U   
nginx:alpine           0563c36f94a6       94.5MB         27.2MB    U   
python:3.12-slim       b18b261e1b49        180MB         45.5MB        
ubuntu:latest          d66b366dac61        163MB         45.9MB    U   
PS C:\test\집\dev-workstation> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS C:\test\집\dev-workstation> docker ps -a
CONTAINER ID   IMAGE           COMMAND                   CREATED         STATUS                      PORTS                  NAMES
6fd2351abbc7   hello-world     "/hello"                  2 minutes ago   Exited (0) 2 minutes ago                           sharp_heisenberg
68b61d0dd54a   ubuntu          "sleep infinity"          21 hours ago    Exited (255) 2 hours ago                           vol-check-2
e36c2c9f3df6   nginx:alpine    "/docker-entrypoint.…"   24 hours ago    Exited (255) 2 hours ago    0.0.0.0:8080->80/tcp   bind-mount-web
7116327869ae   ubuntu          "bash"                    24 hours ago    Exited (0) 24 hours ago                            jolly_hellman
64fe79c60849   hello-world     "/hello"                  24 hours ago    Exited (0) 24 hours ago                            crazy_visvesvaraya
e8fb7d1f9b02   hello-world     "/hello"                  44 hours ago    Exited (0) 44 hours ago                            adoring_hugle
8d7975a9e135   my-custom-web   "/docker-entrypoint.…"   2 weeks ago     Exited (255) 44 hours ago   0.0.0.0:8080->80/tcp   web-server
PS C:\test\집\dev-workstation> 


# docker build 실행 로그 증거
PS C:\test\집\dev-workstation> docker build -t my-nginx-app:latest -f .\docker\Dockerfile .\docker
[+] Building 1.0s (7/7) FINISHED                      docker:desktop-linux
 => [internal] load build definition from Dockerfile                  0.1s
 => => transferring dockerfile: 156B                                  0.1s
 => [internal] load metadata for docker.io/library/nginx:alpine       0.1s
 => [internal] load .dockerignore                                     0.0s
 => => transferring context: 2B                                       0.0s
 => [internal] load build context                                     0.1s
 => => transferring context: 419B                                     0.1s
 => CACHED [1/2] FROM docker.io/library/nginx:alpine@sha256:0563c36f  0.0s
 => => resolve docker.io/library/nginx:alpine@sha256:0563c36f94a6d3e  0.0s
 => [2/2] COPY app/index.html /usr/share/nginx/html/index.html        0.1s
 => exporting to image                                                0.4s
 => => exporting layers                                               0.1s
 => => exporting manifest sha256:7dfdea72f117e963784f4a46e9b7ddc64b6  0.0s
 => => exporting config sha256:453056a63dde42299b30822b3083f7f4efa72  0.0s
 => => exporting attestation manifest sha256:9a775d628a25966c610f9f9  0.0s
 => => exporting manifest list sha256:2cb2eb2f871de6b9c70862711ff0b5  0.0s
 => => naming to docker.io/library/my-nginx-app:latest                0.0s
 => => unpacking to docker.io/library/my-nginx-app:latest             0.1s
PS C:\test\집\dev-workstation> docker images
                                                      i Info →   U  In Use
IMAGE                  ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest     1e4c05b897a7       25.9kB         9.49kB    U   
my-custom-web:latest   4a63261d65ed       93.6MB         26.3MB    U   
my-first-web:latest    5550ccb9a3c3       93.6MB         26.4MB        
my-nginx-app:latest    2cb2eb2f871d       93.6MB         26.4MB        
my-web-image:latest    6780f1560724       93.6MB         26.3MB    U   
nginx:alpine           0563c36f94a6       94.5MB         27.2MB    U   
python:3.12-slim       b18b261e1b49        180MB         45.5MB        
ubuntu:latest          d66b366dac61        163MB         45.9MB    U   


# 호스트 포트 접속의 실제 응답 증거
PS C:\test\집\dev-workstation> docker run -d --name my-nginx-test -p 8080:80 my-nginx-app:latest
cc62a299b1ea1b1a251dfccc4548aaca7d610fcc70de2a46754f3d91f9e46a0a
PS C:\test\집\dev-workstation> docker ps
CONTAINER ID   IMAGE                 COMMAND                   CREATED         STATUS         PORTS                                     NAMES
cc62a299b1ea   my-nginx-app:latest   "/docker-entrypoint.…"   6 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-test
PS C:\test\집\dev-workstation> curl.exe -i http://localhost:8080
HTTP/1.1 200 OK
Server: nginx/1.31.9
Date: Fri, 20 Nov 2026 02:47:09 GMT
Content-Type: text/html
Content-Length: 349
Last-Modified: Thu, 19 Nov 2026 02:50:01 GMT
Connection: keep-alive
ETag: "6afe63d9-15d"
Accept-Ranges: bytes

<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bind Mount Test</title>
</head>
<body>
    <h1>Hello, Bind Mount!</h1>
    <p>Host file changes are reflected in the Docker container.</p>
    <p>No image rebuild is required.</p>
</body>
</html>
PS C:\test\집\dev-workstation> docker rm -f my-nginx-test
my-nginx-test


# 디렉토리 구조 설계 기준 및 폴더별 역할

- **루트 디렉토리 (`dev-workstation/`)**
  - **역할:** 프로젝트 전체 문서(`README.md`), 엔트리포인트 스크립트(`main.py`), 기능별 하위 폴더를 총괄하는 최상위 작업 공간입니다.
  - **규칙:** 문서와 스크립트는 루트에 두고, 빌드 자산·실습 폴더·로그·스크린샷 등은 목적별 하위 폴더로 분리합니다.
  - **예시:** `README.md`, `main.py`, `docker/`, `permission-test/`, `app/`, `logs/`, `practice/`, `screenshots/`

- **`docker/` (커스텀 이미지 빌드 디렉토리)**
  - **역할:** `Dockerfile`과 컨테이너 웹 서비스용 `app/` 하위 폴더를 관리합니다.
  - **규칙:** 빌드 정의(`Dockerfile`)와 앱 소스를 함께 두어 `docker build` 실행 시 빌드 컨텍스트를 명확히 분리합니다.
  - **예시:** `docker/Dockerfile`, `docker/app/`

- **`permission-test/` (파일·디렉토리 권한 실습 폴더)**
  - **역할:** 파일 및 하위 폴더의 권한 확인(`ls -l`)과 변경(`chmod`) 전·후를 검증하는 실습 공간입니다.
  - **규칙:** 일반 소스 코드나 빌드 파일과 섞이지 않도록 별도 테스트 폴더로 격리합니다.
  - **예시:** `permission-test/sample.txt`, `permission-test/test-folder/`

- **`app/`, `practice/`, `logs/`, `screenshots/` (기능·실습·증빙 관리 폴더)**
  - **역할:** 애플리케이션 관련 코드(`app/`), 추가 실습(`practice/`), 실행 기록(`logs/`), 화면 캡처 증빙(`screenshots/`)을 용도별로 구분해 보관합니다.
  - **규칙:** 성격이 다른 산출물(로그, 스크린샷, 연습 파일)을 루트에 방치하지 않고 각 목적에 맞는 폴더에 정리합니다.
  - **예시:** `app/`, `practice/`, `logs/`, `screenshots/`


  # ### 포트 매핑 및 볼륨 매핑 재현 가이드

#### 주요 옵션 설명
- `-d`: 컨테이너를 백그라운드 모드로 실행합니다.
- `--name <이름>`: 컨테이너에 이름을 부여해 조회·삭제를 쉽게 합니다.
- `-p <호스트포트>:<컨테이너포트>`: 호스트 포트와 컨테이너 내부 포트를 연결합니다 (예: `-p 8080:5000`, `-p 8080:80`).
- `-v <호스트경로>:<컨테이너경로>[:ro]`: 호스트 디렉토리나 볼륨을 컨테이너 경로에 연결해 데이터를 공유·보존합니다 (`:ro`는 읽기 전용).

#### 1) 포트 매핑 재현 절차 (`-p 8080:5000` / `-p 8080:80`)
1. 호스트 `8080` 포트와 컨테이너 `5000` 포트를 연결해 테스트 서버를 실행합니다.
   ```powershell
   docker run -d --name port-demo-5000 -p 8080:5000 python:3.12-slim python -m http.server 5000


# 포트 충돌 진단 및 포트 해제 절차

포트(예: `8080`, `5000`)가 이미 사용 중일 때는 아래 순서로 점유 주체를 확인하고 포트를 해제합니다.

#### 1) 포트 사용 프로세스 및 컨테이너 진단
- **Docker 컨테이너 확인 (공통)**
  ```powershell
  docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Ports}}\t{{.Status}}"


# 두번째 트러블슈팅 사례와 관련된 로그 증거
### 트러블슈팅 사례 #2: `docker build` 빌드 컨텍스트 경로 불일치로 인한 `COPY` 실패

- **문제:** 저장소 루트(`C:\test\집\dev-workstation`)에서 빌드 컨텍스트를 `.`으로 지정해 `docker build -t my-nginx-app:latest -f .\docker\Dockerfile .`을 실행했을 때 `COPY app/index.html /usr/share/nginx/html/index.html` 단계에서 `/app/index.html: not found` 오류가 발생했습니다.
- **가설:** `Dockerfile` 내부의 `COPY app/index.html` 경로는 `.\docker` 폴더를 빌드 컨텍스트 루트로 가정하고 작성되었는데, 빌드 컨텍스트를 루트(`.`)로 넘겨 실제 파일 경로(`docker/app/index.html`)와 어긋났을 가능성이 높다고 보았습니다.
- **검증:** `Dockerfile`이 `COPY app/index.html`을 참조하는지 확인하고, 빌드 컨텍스트를 `.\docker`로 지정해 `docker build -t my-nginx-app:latest -f .\docker\Dockerfile .\docker`를 실행해 빌드가 `(7/7) FINISHED`로 성공하는지 확인했습니다.
- **조치:** 빌드 컨텍스트를 `.\docker`로 지정해 이미지를 정상 빌드하고, 이후 빌드 시에도 `.\docker`를 컨텍스트로 사용하도록 명령을 고정했습니다.

#### 관련 실행 로그
```text
PS C:\test\집\dev-workstation> docker build -t my-nginx-app:latest -f .\docker\Dockerfile .
[+] Building 0.3s (6/6) FINISHED                      docker:desktop-linux
 => ERROR [2/2] COPY app/index.html /usr/share/nginx/html/index.html  0.0s
------
 > [2/2] COPY app/index.html /usr/share/nginx/html/index.html:
------
Dockerfile:3
--------------------
   1 |     FROM nginx:alpine
   2 |     
   3 | >>> COPY app/index.html /usr/share/nginx/html/index.html
   4 |     
   5 |     EXPOSE 80
--------------------
ERROR: failed to build: failed to solve: failed to compute cache key: failed to calculate checksum of ref zb60vgxkahhg3ory7ozem70ta::0q0zn0gqm5ns5l0zkowhunng2: "/app/index.html": not found

PS C:\test\집\dev-workstation> docker build -t my-nginx-app:latest -f .\docker\Dockerfile .\docker
[+] Building 1.0s (7/7) FINISHED                      docker:desktop-linux
 => [2/2] COPY app/index.html /usr/share/nginx/html/index.html        0.1s
 => exporting to image                                                0.4s
 => => naming to docker.io/library/my-nginx-app:latest                0.0s
