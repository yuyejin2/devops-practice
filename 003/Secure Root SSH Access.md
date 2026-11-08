## 문제

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.

Your task is to disable direct SSH root login on all app servers (stapp01, stapp02 and stapp03) within the Stratos Datacenter. Make sure the change is applied to the running SSH service on each server.

보안 감사 이후, xFusionCorp Industries의 보안팀은 root 계정으로 직접 SSH 로그인하는 것을 제한하는 조치를 포함해 새로운 보안 프로토콜을 도입했습니다.

여러분의 임무는 Stratos 데이터센터의 모든 앱 서버(stapp01, stapp02, stapp03)에서 root 계정으로 직접 SSH 로그인하는 것을 비활성화하는 것입니다. 각 서버에서 실행 중인 SSH 서비스에 이 변경 사항이 적용되도록 하세요.

## 풀이

```bash
thor@jump-host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (10.244.97.160)' can't be established.
ED25519 key fingerprint is SHA256:J3QSotautXda91mLWHVu/QoQZw6hp5YC5MCoOlWVsN8.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01' (ED25519) to the list of known hosts.
tony@stapp01's password: 
[tony@stapp01 ~]$ 
[tony@stapp01 ~]$ sudo vi /etc/ssh/sshd_config

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ 
[tony@stapp01 ~]$ sudo sshd -t && sudo systemctl restart sshd[tony@stapp01 ~]$ sudo systemctl is-active sshd
active
[tony@stapp01 ~]$ sudo sshd -T | grep '^permitrootlogin '
permitrootlogin no
[tony@stapp01 ~]$ 
[tony@stapp01 ~]$ 
[tony@stapp01 ~]$ ssh steve@stapp02
The authenticity of host 'stapp02 (10.244.13.82)' can't be established.
ED25519 key fingerprint is SHA256:q0iTFsoQCORB9sJBhSjPZuTzUXB2cMDmDGB+xjlgBXo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02' (ED25519) to the list of known hosts.
steve@stapp02's password: 
[steve@stapp02 ~]$ sudo vi /etc/ssh/sshd_config

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[steve@stapp02 ~]$ sudo sshd -t && sudo systemctl restart sshd
[steve@stapp02 ~]$ sudo sshd -T | grep '^permitrootlogin '
permitrootlogin no
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ ssh banner@stapp03
The authenticity of host 'stapp03 (10.244.234.249)' can't be established.
ED25519 key fingerprint is SHA256:KxbgcLvLF2fG7EQpxWNJsZ4CYYyL1/sCLum8hbvLMaI.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03' (ED25519) to the list of known hosts.
banner@stapp03's password: 
[banner@stapp03 ~]$ sudo vi /etc/ssh/sshd_config

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ sudo ssh -t && sudo systemctl restart sshd
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
           [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]
           [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
           [-i identity_file] [-J [user@]host[:port]] [-L address]
           [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
           [-Q query_option] [-R address] [-S ctl_path] [-W host:port]
           [-w local_tun[:remote_tun]] destination [command]
[banner@stapp03 ~]$ sudo sshd -t && sudo systemctl restart sshd
[banner@stapp03 ~]$ sudo sshd -T | grep '^permitrootlogin '
permitrootlogin no
```

- ssh는 다른 서버에 접속할 때 쓰는 프로그램이고, sshd는 서버에서 SSH 접속 요청을 받는 프로그램이다.
- SSH 서버 설정은 서버마다 따로 있으므로 stapp01, stapp02, stapp03에서 각각 수정해야 한다.
- sudo vi /etc/ssh/sshd_config는 현재 서버의 SSH 설정 파일을 관리자 권한으로 연다.
- PermitRootLogin no는 root 계정으로 직접 SSH 로그인하는 것을 차단한다. 일반 계정으로 접속한 뒤 권한이 있으면 sudo를 사용할 수 있다.
- sudo sshd -t && sudo systemctl restart sshd로 설정을 검사한 뒤 서비스를 재시작한다.
- sudo systemctl is-active sshd에서 active가 나오면 SSH 서비스가 실행 중이다.
- sudo sshd -T | grep '^permitrootlogin '에서 permitrootlogin no가 나오면 설정 파일에서 읽은 값이 맞다. 대문자 -T는 설정값을 출력하고, | grep은 그중 필요한 줄만 골라준다.
- 제출 전 세 서버 각각에서 재시작 성공, active, permitrootlogin no를 확인한다.
