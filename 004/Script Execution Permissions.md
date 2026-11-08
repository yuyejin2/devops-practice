## 문제

In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named xfusioncorp.sh. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 3 within the Stratos Datacenter.

Your task is to grant executable permissions to the /tmp/xfusioncorp.sh script on App Server 3. Additionally, ensure that all users have the capability to execute it (any user must be able to run it directly, for example /tmp/xfusioncorp.sh).

xFusionCorp Industries의 시스템 관리자 팀은 백업 프로세스를 자동화하기 위해 xfusioncorp.sh라는 새 Bash 스크립트를 개발했습니다. 이 스크립트는 필요한 모든 서버에 배포되었지만, Stratos 데이터센터의 App Server 3에서는 실행 권한이 없습니다.

당신의 작업은 App Server 3에 있는 /tmp/xfusioncorp.sh 스크립트에 실행 권한을 부여하는 것입니다. 또한 모든 사용자가 이 스크립트를 직접 실행할 수 있도록 하세요. 예를 들어, 어떤 사용자든 /tmp/xfusioncorp.sh를 입력해 실행할 수 있어야 합니다.

## 풀이

```bash
thor@jump-host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (10.244.244.151)' can't be established.
ED25519 key fingerprint is SHA256:rdI2ozoMm52XH1n50hsZvkZmUlx3lXUHpytH9f/CGk8.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03' (ED25519) to the list of known hosts.
banner@stapp03's password: 
Permission denied, please try again.
banner@stapp03's password: 
Last failed login: Sun Nov  8 07:30:59 UTC 2026 from 10.244.247.253 on ssh:notty
There was 1 failed login attempt since the last successful login.
[banner@stapp03 ~]$ 
[banner@stapp03 ~]$ ls -ltr /tmp/xfusioncorp.sh
---------- 1 root root 40 Nov  8 07:28 /tmp/xfusioncorp.sh
[banner@stapp03 ~]$ sudo chmod a+rx /tmp/xfusioncorp.sh

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
Sorry, try again.
[sudo] password for banner: 
[banner@stapp03 ~]$ ls -ltr /tmp/xfusioncorp.sh
-r-xr-xr-x 1 root root 40 Nov  8 07:28 /tmp/xfusioncorp.sh
[banner@stapp03 ~]$ /tmp/xfusioncorp.sh
Welcome To KodeKloud
```

- ssh banner@stapp03으로 App Server 3에 접속했다.
- ls -l /tmp/xfusioncorp.sh로 확인하니 파일 소유자는 root였고, 권한은 000(----------)이었다.
- sudo chmod a+rx /tmp/xfusioncorp.sh로 소유자, 그룹, 기타 사용자 모두에게 읽기(r)와 실행(x) 권한을 추가했다.
- 스크립트를 경로로 직접 실행하면 운영체제가 먼저 실행 권한을 확인한다. 그다음 Bash가 실행한 사용자의 권한으로 스크립트 파일을 열어 명령을 읽기 때문에 읽기 권한도 필요하다.
- 초기 권한이 000인 상태에서 a+x만 적용하면 111이 된다. 이 경우 실행 권한 검사는 통과하지만 Bash가 파일을 읽는 단계에서 실패한다.
- ls -l에서 변경된 권한이 555(-r-xr-xr-x)인지 확인했다. banner 계정으로 /tmp/xfusioncorp.sh를 직접 실행하니 Welcome To KodeKloud가 출력됐다.
