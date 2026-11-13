## 문제

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 3 in the Stratos Datacenter:

1. Install the required SELinux packages.

2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

4. Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

보안 감사(Security Audit) 이후, xFusionCorp Industries의 보안팀은 SELinux를 활용하여 애플리케이션 및 서버 보안을 강화하기로 결정했습니다.

테스트를 시작하기 위해 Stratos 데이터센터의 애플리케이션 서버 3(App Server 3) 에 대해 다음 요구사항이 수립되었습니다.

작업 요구사항

1. SELinux 필수 패키지 설치
    * SELinux를 사용하는 데 필요한 패키지를 설치합니다.
2. SELinux 영구 비활성화
    * 현재는 SELinux를 영구적으로 비활성화합니다.
    * 필요한 설정 변경이 완료되면 추후 다시 활성화할 예정입니다.
3. 서버 재부팅 불필요
    * 오늘 밤 예정된 정기 유지보수 작업에서 서버를 재부팅할 계획이므로, 지금 재부팅할 필요는 없습니다.
4. 재부팅 후 SELinux 상태 확인 기준
    * 명령줄에서 확인되는 SELinux의 현재 상태는 고려하지 않아도 됩니다.
    * 서버 재부팅 후 최종 SELinux 상태가 disabled가 되도록 설정해야 합니다.

## 풀이

① 서버 접속 및 OS 확인

```bash
ssh banner@stapp03
sudo -i
cat /etc/os-release
```

패키지 관리 도구가 OS마다 다르므로 먼저 확인한다.

* Ubuntu/Debian: apt
* RHEL/Rocky/CentOS: yum, dnf

② SELinux 패키지 설치

패키지 이름을 모를 때는 검색하거나 설명을 확인할 수 있다.

```bash
dnf search selinux
dnf info selinux-policy
```

패키지 설치:

```bash
yum install -y selinux-policy selinux-policy-targeted policycoreutils
```

* selinux-policy: SELinux 기본 정책
* selinux-policy-targeted: Targeted 보안 정책
* policycoreutils: SELinux 관리 명령어 제공
* -y: 설치 확인 질문에 자동 동의

설치 확인:

```bash
rpm -q selinux-policy selinux-policy-targeted policycoreutils
```

rpm -q는 패키지 설치 여부와 버전을 확인하는 명령어다.

③ SELinux 영구 비활성화

```bash
vi /etc/selinux/config
```

다음 설정을 변경한다.

```bash
### 변경 전
SELINUX=enforcing
### 변경 후
SELINUX=disabled
```

설정 확인:

```bash
grep '^SELINUX=' /etc/selinux/config
```

결과:

```bash
SELINUX=disabled
```

grep은 특정 문자열을 검색하는 명령어이며, ^는 줄의 시작을 의미한다.

문제에서 재부팅하지 말라고 했으므로 여기서 작업을 종료한다.

## 추가로 알게 된 내용

SELinux란?

Linux에서 프로세스나 사용자의 파일 및 시스템 자원 접근을 보안 정책에 따라 제한하는 기능이다.

모드	설명
```bash
Enforcing	보안 정책 적용 및 위반 접근 차단
Permissive	차단하지 않고 위반 사항만 기록
Disabled	SELinux 비활성화
```

일시적 설정과 영구 설정

```bash
getenforce
```

현재 SELinux 상태 확인

```bash
setenforce 0
```

현재 SELinux를 Permissive 모드로 변경 (일시적)

```bash
vi /etc/selinux/config
```

SELinux 영구 설정 변경

setenforce 0은 SELinux를 완전히 비활성화하는 명령어가 아니다. 재부팅 후에도 비활성화하려면 영구 설정이 필요하다.

※ 최신 RHEL 계열에서는 완전 비활성화를 위해 커널 부팅 옵션 selinux=0이 권장될 수 있으므로 실제 운영 환경에서는 OS별 설정 방법을 확인해야 한다.

## 정리

이번 문제에서 중요한 것은 현재 SELinux 상태가 아니라 재부팅 후 적용될 설정이다.

또한 패키지 이름이나 명령어를 모를 때는 다음 방법으로 찾아볼 수 있다.

```bash
cat /etc/os-release  # OS 확인
dnf search 키워드      # 패키지 검색
dnf info 패키지명      # 패키지 설명 확인
rpm -q 패키지명        # 설치 여부 확인
man 명령어             # 명령어 매뉴얼 확인
명령어 --help          # 사용 가능한 옵션 확인
```

배운 점: 문제를 읽고 바로 명령어를 떠올리기보다, OS 확인 → 필요한 패키지 및 설정 방법 검색 → 실행 → 결과 검증 순서로 접근해야 한다.
