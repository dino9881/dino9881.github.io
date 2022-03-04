---
layout: post
title: Born2beroot 진행하기
subtitle: "Born2beroot"
categories: 42seoul
tags:
- 42
- Born2beroot
- TIL
comments: true
published: true
---
# Born2beroot 진행하기  

## 기본설정을 하자  
  

  
### sudo 

    apt install sudo  

- sudo를 설치한다.  

    visudo  

- sudoer 파일에 접근할 수 있다.  
  
  

> sudo 명령은 해당 명령을 root의 권한으로 실행하고 싶을때 사용하는 명령이므로 보안상 상당한 위험을 가진다. 따라서 sudoer 라는 설정 파일을 통해서 sudo명령을 사용할 수 있는 계정의 범위를 한정한다.  
  


    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"


> sudo 에서 제공하는 보안 기능 중 하나인 Secure Path는 명령을 실행할 때 사용하는 가상 쉘의 Path 정보를 설정한다.  
  

> 새로운 쉘에서 명령을 찾을 경로를 나열한 환경변수가 secure_path이다.  

> 이 기능은 트로이목마 해킹 공격에 대한 일차적인 방어 기능을 제공한다.  


>> sudo로 실행할수 있는 명령어들을 시스템에서 제공하는 디렉토리에서만 찾게 함으로써 해킹 공격자가 심어놓은 프로그램들을 관리자 권한으로 실행할 수 없도록 막는다.  


    Defaults authfail_message="authfail" #권한 획득 실패 시 출력 메세지 설정
    Defaults badpass_message="badpass" #비밀번호 불일치 시 출력 메세지 설정
    Defaults log_input #sudo명령어 실행 시 입력된 명령어 log 저장
    Defaults log_output #sudo명령어 실행 시 출력 log 저장
    Defaults requiretty #sudo TTY모드 활성화 
    Defaults iolog_dir="/var/log/sudo/" #sudo log 저장 디렉토리 설정
    Defaults passwd_tries=3 #비밀번호 시도 횟수 지정
> Ctrl + x , Y 로 저장

### 그룹 설정  
  

    groupadd user42
- user42 그룹을 생성한다.  
  
  
    usermod -aG sudo,user42 <사용자이름>
- 사용자를 해당 그룹에 추가한다.  
  

    usermod -g user42 <사용자이름>
- user42그룹을 primary group이 되도록 설정한다.  
  

### Vim 설치  
  

    sudo apt-get install vim  
  


### AppArmor   
  

#### AppArmor란?  
AppArmor(Application Armor)는 시스템 관리자가 프로그램 프로필 별로 프로그램의 역량을 제한할 수 있게 해주는 리눅스 커널 보안 모듈이다. 프로필들은 /etc/apparmor.d 디렉토리에 일반 텍스트 파일로 저장된다. MAC(Mandatory Access control)로 정의된 정책을 활용해 사용자와 프로세스의 행동을 제어 함으로써 DAC(Discretionary Access Control) 자원 접근 조절을 지원한다. 
#### AppArmor 설치하기  


    sudo apt install apparmor #AppArmor 설치
    sudo apt install apparmor-utils #AppArmor utils 설치
    aa-enabled #활성화 여부 확인 가능 
  

### UFW


#### UFW란?
UFW(Uncomplicated FireWall)는 리눅스 환경에서 작동되는 방화벽 관리 프로그램이다. 
> 방화벽 은 미리 정의된 보안 규칙에 기반한, 들어오고 나가는 네트워크 트래픽을 모니터링하고 제어하는 네트워크 보안 시스템이다. 
  

#### UFW를 설치하자
    apt-get install ufw # ufw 설치
    sudo ufw enable # ufw실행시키기
    sudo ufw allow 4242 # 4242포트 열기 
    sudo ufw status vebos # ufw 상태 확인 
    sudo ufw default deny # 기본 deny 설정 



## 요구사항을 설정하자  

### SSH

#### SSH란?
- SSH(Secure SHell)은 네트워크 상의 다른 컴퓨터에 로그인하거나, 명령을 실행하고, 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용프로그램 또는 프로토콜을 사용한다. 암호화 기법을 사용하여 보안성을 크게 높인것이 특징이다.  
- 사용자와 서버 모두 각각의 키를 보유하여(비대칭 키) 연결 상대를 인증하고 데이터를 주고 받게 된다. 사용자는 공개 키를 서버에 전송하고, 서버는 공개 키를 바탕으로 만들어진 랜덤한 값을 생성하여 사용자에게 다시 보내준다. 그 후 사용자는 개인키를 바탕으로 값을 풀어내고, 이 과정을 통해서 서버는 사용자의 접속을 허용한다.  
- 접속후 정보를 주고 받을 때에는 서버와 사용자 모두가 사용하는 한 개의 키(대칭 키)를 생성하여 복호화와 암호화를 진행한다. 이 때 사용하는 키는 접속할 때마다 생성되고, 정보의 교환이 완료되면 폐기된다. 

#### SSH설정을 해보자
    apt install openssh-server
- open ssh 를 설치한다. (debian 설치과정에서 미리 설치가능하다.)
    systemctl status ssh
- 사용포트를 확인할 수 있다. 
    sudo vim /etc/ssh/sshd_config
- ssh설정을 변경할 수 있다. 22번 포트를 4242번으로 변경해주자.  
    PermitRootLogin no 
- 루트 로그인을 차단한다.  
    sudo systemctl restart ssh
- 재시작하여 설정을 적용한다.



