# 001-Linux User Setup with Non-Interactive Shell

## 문제

To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here's your task:
Create a user named javed with a non-interactive shell on App Server 3.
Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

백업 에이전트 도구의 사양을 충족하기 위해, xFusionCorp Industries의 시스템 관리자 팀은 비대화형 셸을 사용하는 사용자를 생성해야 합니다. 작업은 다음과 같습니다.
App Server 3에 비대화형 셸을 사용하는 javed라는 사용자를 생성하세요.
참고: 페이지 오른쪽 상단의 Details of all Users and Servers 버튼을 클릭하면 인프라 세부 정보를 확인할 수 있습니다.

## 풀이
### 1. app server 3에 접속
```bash
thor@jump-host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (10.244.49.38)' can't be established.
ED25519 key fingerprint is SHA256:EnfMp5tl3A7NzXV9iFO/D0zKNmTriP6khXGNAm6KUiA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03' (ED25519) to the list of known hosts.
banner@stapp03's password: 
[banner@stapp03 ~]$ 
```

### 2. 사용자 생성
```bash
[banner@stapp03 ~]$ command -v nologin
/usr/sbin/nologin
[banner@stapp03 ~]$ sudo useradd -s /usr/sbin/nologin javed

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ 
```

### 3. 확인
```bash
[banner@stapp03 ~]$ id javed
uid=1001(javed) gid=1001(javed) groups=1001(javed)
[banner@stapp03 ~]$ getent passwd javed
javed:x:1001:1001::/home/javed:/usr/sbin/nologin
[banner@stapp03 ~]$ 
```

