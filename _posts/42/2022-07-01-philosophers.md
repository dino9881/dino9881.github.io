--- 
layout: post
title: Philosopher 시작하기
subtitle: " Philosopher 시작하기"
categories: 42seoul
tags:
- 42
- TIL
- Minitalk
comments: true
published: true
---

## Philosopher 시작하기.

### 함수 정리

#### gettimeofday
<pre><code>
#include <sys/time.h>

int gettimeofday(struct timeval *tv, struct timezone *tz);

struct timeval {
    time_t      tv_sec;     /* 초 */
    suseconds_t tv_usec;    /* 마이크로초 */
};

struct timezone {
    int tz_minuteswest;     /* 그리니치 서쪽으로 몇 분 */
    int tz_dsttime;         /* DST 수정 종류 */
};

</code></pre>
##### Description
tv 나 tz 중 어느 쪽이라도 NULL 이면 해당 구조체를 설정 내지 반환하지 않는다. (tv 가 NULL일 경우 컴파일 경고가 난다.)
timezone 구조체 사용은 구식화되어 보통은 tz 인자를 NULL로 지정하는게 좋다. 

##### Return Value
성공시 0 을 반환하고 오류시 -1을 반환한다. 
***
#### pthread_create
<pre><code>

#include <pthread.h>

int  pthread_create(pthread_t  *  thread, pthread_attr_t *attr, void * (*start_routine)(void *), void * arg);

</code></pre>
##### Description 
새로운 쓰레드를 생성한다. 새로운 쓰레드는 start_routine 함수를 arg 인자로 실행시키면서 생성한다. 
생성된 쓰레드는 pthread_exit()를 호출하거나 또는 start_rutine 에서 return 할경우 제거된다. 
attr 아규먼트는 쓰레드와 관련된 특성을 지정하기 위한용도로 사용된다. 이에 대한 내용은 pthread_attr_init(3) 을 참고해야 한다.  
attr 을 NULL 로 할경우 기본 특성으로 지정된다. 리눅스에서의 쓰레드는 joinable 과 non real-time 스케쥴 정책을 기본특성으로 한다.

##### Return Value
성공할경우 쓰레드식별자인 thread 에 쓰레드 식별번호를 저장하고, 0을 리턴한다. 실패했을경우 에러코드 값을 리턴한다. 
쓰레드 생성을 위한 자원이 부족하거나, PTHREAD_THREADS_MAX를 초과해서 쓰레드 생성을 요청ㅇ할경우 에러가 발생한다. 
***
#### pthread_detach
<pre><code>

#include <pthread.h>

int pthread_detach(pthread_t th);

</code></pre>
##### Description  
쓰레드 식별자 th를 가지는 쓰레드를 메인쓰레드에서 분리 시킨다. th를 가지는 쓰레드가 종료되는 즉시 쓰레드의 모든 자원을 free 해 줄 것을 보증한다. 
detach시키지 않았다면 쓰레드가 종료한다고 하더라도 pthread_join을 호출하지 않는 한 자원을 되돌리지 않는다. 
pthread_detach()함수를 호출하는 외에도 pthread_create()시 pthread_attr_t에 detachstate를 지정해 줌으로써 detach상태로 생성할 수 도 있다. pthread_attr_t는 pthread_attr_init(3)함수를 이용해서 변경할 수 있다.

##### Return Value
성공하면 0, 실패하면 0이 아닌 값을 리턴한다. 
***
#### pthread_join
<pre><code>

#include <pthread.h>

int pthread_join(pthread_t th, void **thread_return);

</code></pre>
##### Description  
쓰레드 식별번호 th 를 가진 쓰레드가 종료되는것을 기다린다. thread_return 값이 NULL이 아니라면 , 리턴값이 저장된 영역이 전달되게 된다.  
join되기 위해서는 쓰레드가 joinable 상태로 작동하고 있어야 하며, 만약 기다리는 쓰레드가 detached 상태라면 기다릴 수 없다.  
joinable 쓰레드가 종료되면 , 메모리 자원의 해제가 자동으로 되지 않기때문에 phtread_join 함수를 호출해주어야 메모리 누수를 막을 수 있다.  

##### Return Value
성공할 경우 thread 에 식별 번호를 저장하고 0을 리턴한다. 실패했을경우 에러코드 값을 리턴한다. 
***
#### pthread_mutex_init
<pre><code>

#include <pthread.h>

int pthread_mutex_init(pthread_mutex_t * mutex, const pthread_mutex_attr *attr);

</code></pre>
##### Description  
쓰레드가 공유하는 데이터 영역을 보호하기 위해서 사용되는 도구인 뮤텍스 객체를 초기화 하는 함수이다.  
첫번째 인자인 mutex는 초기화 시킬 뮤텍스 객체이다. attr인자를 통해 뮤텍스의 특징을 정의할 수 있다.  
뮤텍스는 "fast". "recursive", "error checking"의 종류를 가지며 기본적으로 "fast"rk tkdydehlsek. 

##### Return Value
항상 0 을 반환한다. 
***

#### pthread_mutex_init
<pre><code>

#include <pthread.h>

int pthread_mutex_destroy(pthread_mutex_t *mutex);

</code></pre>
##### Description  
mutex가 가르키는 뮤텍스 객체를 파기한다. 

##### Return Value
성공시 0을 반환, 실패할 경우 -1을 반환한다. 
***
#### pthread_mutex_lock, pthread_mutex_unlock
<pre><code>

#include <pthread.h>

int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);

</code></pre>
##### Description  
뮤텍스는 쓰레드간 공유하는 데이터 영역을 보호하기 위해 임계 영역을 만들고 임계영역내에 단 하나의 쓰레드만이 진입가능 하도록 하는 방식을 사용한다.  
lock은 임계영역에 진입하기 위한 요청, unlock은 임계영역을 빠져나오면서 다른 쓰레드에게 임계영역을 되돌려주기 위해서 사용한다. lock을 시도 했는데, 다른 쓰레드가 이미 있다면 해당 쓰레드가 unlock을 해서 나올때까지 기다린다.  
mutex는 fast와 recursive의 2가지 종류가 지원된다. 이것은 lock을 얻은 쓰레드가 다시 lock를 얻을 수 있도록 할 것인지를 결정하기 위해서 사용한다. 기본적으로 mutex의 종류는 fast 상태로 시작된다.    

##### Return Value
성공시 0을 반환, 실패할 경우 -1을 반환한다. 

### 개념정리

#### 프로세스(process)
프로세스 는 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램을 말한다.  
하드 디스크 등에 저장되어 있는 실행코드인 프로그램(Program)을 구동하여 메모리 상에서 실행되고 있는것을 지칭한다.  
프로그램이 실행되면 프로세스가 생성되어 준비 리스트의 끝에 들어가고, 준비 리스트 상의 다른 프로세스들이 CPU를 할당 받아 준비 리스트를 떠나면, 점차 준비 리스트의 앞으로 나가게 되고, 실행된다.  
- 디스패치(dispatch)
  준비 리스트의 맨 앞에 있던 프로세스가 CPU를 점유하게 되는것을 디스패치라고 한다. 
- 보류 (block)
  실행 상태의 프로세스가 허가된 시간을 다 쓰기 전에 입출력 동작을 필요로 하는 경우 프로세스는 CPU를 반납하고 보류 상태로 넘어간다. 
- 깨움 (wakeup)
  입출력 작업 종료 등 기다리던 사건이 일어났을 때 보류 상태에서 준비 상태로 넘어가는 과정을 말한다. 
- 시간제한 (timeout)
  운영체제는 프로세스가 프로세서를 계속 독점해서 사용하지 못하게 하기 위해 clock interrupt를 두어서 프로세스가 일정 시간동안만 프로세서를 점유할 수 있게 한다.  
#### 쓰레드(thread)
쓰레드는 프로세스 내에서 실행되는 흐름의 단위를 말한다. 일반적으로 한 프로그램은 하나의 쓰레드를 가지고 있지만, 프로그램 환경에 따라 둘 이상의 쓰레드를 동시에 실행할 수 있다.(멀티쓰레드) 쓰레드는 프로세스 내의 메모리를 공유해 사용할 수 있다. 
#### 뮤텍스(mutex)
뮤텍스는 상호 배제(mutual exclusion)의 약자이다.  
서로 다른 두 프로세스, 혹은 쓰레드 등의 처리 단위가 동시에 같이 접근해서는 안 되는 공유 영역인 임계구역(Critical Section)에 동시에 접근하지 못하게 하는  기법을 뜻한다. 

