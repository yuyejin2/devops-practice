# 002-Temporary User Setup with Expiry

## 문제

As part of the temporary assignment to the Nautilus project, a developer named rose requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:
Create a user named rose on App Server 2 in Stratos Datacenter. Set the expiry date to 2026-12-07, ensuring the user is created in lowercase as per standard protocol.
Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

Nautilus 프로젝트의 임시 업무에 참여하는 개발자 rose에게 일정 기간 동안 접근 권한이 필요합니다. 원활한 접근 권한 관리를 위해 만료일이 설정된 임시 사용자 계정이 필요합니다. 수행할 작업은 다음과 같습니다.
Stratos 데이터센터의 App Server 2에 rose라는 사용자를 생성하세요. 만료일은 2026-12-07로 설정하고, 표준 규칙에 따라 사용자 이름은 소문자로 생성하세요.
참고: 페이지 오른쪽 상단의 Details of all Users and Servers 버튼을 클릭하면 인프라 세부 정보를 확인할 수 있습니다.

## 풀이

### 1. app server 2에 접속
```bash
thor@jump-host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (10.244.195.3)' can't be established.
ED25519 key fingerprint is SHA256:Q5Zhd5PMvNV5/yM5kcrkKyeVY2MPXprEnLk1d1iz/1E.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02' (ED25519) to the list of known hosts.
steve@stapp02's password: 
Permission denied, please try again.
steve@stapp02's password: 
Last failed login: Tue Nov  3 13:09:44 UTC 2026 from 10.244.97.151 on ssh:notty
There was 1 failed login attempt since the last successful login.
[steve@stapp02 ~]$
```

### 2. 사용자 생성 및 만료일 생성
```bash
[steve@stapp02 ~]$ useradd -e 20261207 rose
useradd: Permission denied.
useradd: cannot lock /etc/passwd; try again later.
[steve@stapp02 ~]$ sudo useradd -e 20261207 rose

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ id rose
uid=1001(rose) gid=1001(rose) groups=1001(rose)
[steve@stapp02 ~]$ getent passwd rose
rose:x:1001:1001::/home/rose:/bin/bash
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ sudo chage -l rose
Last password change                                    : Nov 03, 2026
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Apr 21, 57443
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ sudo usermod -e 2026-12-07 rose
[steve@stapp02 ~]$ id rose
uid=1001(rose) gid=1001(rose) groups=1001(rose)
[steve@stapp02 ~]$ sudo chage -l rose
Last password change                                    : Nov 03, 2026
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Dec 07, 2026
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
[steve@stapp02 ~]$ 
```
