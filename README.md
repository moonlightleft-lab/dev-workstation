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