---
layout: post
title: Minitalk(1)
subtitle: "Minitalk 시작하기"
categories: 42seoul
tags:
- 42
- TIL
- Minitalk
comments: true
published: true
---

# Minitalk 을 시작해보자

## 목표

- UNIX signal을 이용한 소규모 데이터 교환 프로그램을 작성하자.   

## 요구사항

### 사용가능 함수

- write  
- signal  
- sigemptyset  
- sigaddset  
- sigaction  
- kill  
- getpid  
- malloc  
- free  
- pause  
- sleep  
- usleep 
- exit  

### Mandatory  
- 클라이언트와 서버가 서로 통신하는 프로그램을 작성할 것.  
- 서버가 먼저 실행되어야 하며, 실행 후 반드시 PID를 표시할것.  
- 클라이언트는 서버 PID와 전송할 문자열을 매개변수로 받을것.  
- 클라이언트는 전송할 문자열을 서버로 통신해야 한다. 서버는 문자열이 수신되면 해당 문자열을 표시해야 한다.  
- 서버와 클라이언트의 통신은 UNIX signal 을 이용하여야한다.  
- 서버는 문자열을 매우 빠른 속도로 표시할 수 있어야 한다.  
- 서버가 재시작할 필요없이 여러 클라이언트로부터 문자열을 연속으로 수신할 수 있어야 한다.  
- SIGUSR1, SIGUSR2 두 신호만 사용할 수 있다.  

### BONUS 
- 소규모 수신 확인 시스템을 추가할것.  
- 유니코드 문자도 지원하도록 할 것.  

## 시작 전 공부 

### UNIX siganl?

유닉스, 유닉스 계열 운영 체제에 쓰이는 제한된 형태의 프로세스 간 통신이다.  
프로세스나 동일 프로세스 내의 특정 스레드로 전달되는 비동기식 통보로 이루어진다.  
터미널에서 Ctrl-c 를 누르면 (SIGInT) 신호를 보내 프로세스를 종료한다.  
이외에도 여러 키 조합으로 특정 신호를 내보낼 수 있다.   

### 함수를 파악하자
<pre><code>
    void (*signal(int signum, void (*handler)(int)))(int)
</code></pre>
- signum = 시그널 번호
- handler = 시그널 처리 방법
	SIG_DFL -> 기존 방법을 따른다.  
	SIG_IGN -> 시그널을 무시한다.  
	함수 이름 -> 시그널이 발생하면 지정된 함수를 호출한다.  
<pre><code>
    int sigemptyset(sigset_t *set)
</code></pre>
- sigset_t 집합에서 모든 시그널을 제거하는 함수  
<pre><code>
    int sigaddset(sigset_t *set, int signum)
</code></pre>
- set = 기존 sigset_t  
- signum = 기존 set 에 추가할 signal  
<pre><code>
    int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact)  
</code></pre>
- signum = 시그널 번호  
- act = 새롭게 지정할 행동  
- sigaction  = 기존 행동  
<pre><code>
    struct sigaction {
    		void (*sa_handler)(int);
    		void (*sa_sigaction)(int, siginfo_t *, void *);// sa_falgs 가 SA_SIGINFO 일때 sa_handler 대신 동작하는 핸들러 함수 sa_handler 보다 다양한 인수를 받을 수 있는것이 특징이다. 
    		sigset_t sa_mask; // 시그널을 처리하는 동안 블록화할 시그널 집합의 마스크  
    		int sa_flags; 
    	}
</code></pre>

<pre><code>
    int kill(pid_t pid, int sig)
</code></pre>
- pid = PID  
- sig = 시그널 번호  
프로세스에 시그널을 전송하는 함수  
<pre><code>
    pid_t getpid(void)  
</code></pre>
실행중인 프로세스 ID를 반환하는 함수  
<pre><code>
    int pause(void)  
</code></pre>
항상 -1 을 반환하며 errno에는 ERESTARTNOHAND로 설정 시그널을 수신할 때까지 대기 상태로 빠진다.  
<pre><code>
    sleep(int sec)  
    usleep(int microsec)
</code></pre>

일정 시간동안 코드의 실행을 늦출 수 있는 함수

