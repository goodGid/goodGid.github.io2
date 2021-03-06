---
layout: post
title:  " CPU와 쓰레드 "
categories: OS
author: goodGid
---
* content
{:toc}


* CPU와 쓰레드 개념에 대해 알아보자.










* Facebook에 다음과 같은 [질문](https://www.facebook.com/groups/codingeverybody/permalink/2367609543279568/?comment_id=2367620266611829&reply_comment_id=2368777433162779&notif_id=1536100067643730&notif_t=group_comment)을 올렸다.

```
Q1.
CPU의 코어가 많은거랑
쓰레드가 많은거랑 차이가 무엇일까요?

Q2.
( 코어 1개 당 쓰레드 1개 ) x 10 개랑 
코어 1개 당 쓰레드 10개

두개의 차이 알려주시면 감사하겠습니다 ㅠㅠ
```


## 하이퍼쓰레딩

* 코어는 물리적인 개념이다.

* 하이퍼쓰레딩은 물리적 코어의 유휴동작을 활용하여 연산을 할 수 있도록한 인텔의 기술이다.

* 한 코어 내에 여러 연산 유닛들이 있는데, 모든 CPU 연산이 이걸 다 쓰지는 않는다. 

* 이에 착안한게 **하이퍼쓰레딩**으로, 코어마다 안쓰는 유닛을 다른 쓰레드 계산할 때 가져다 쓰는 것이다.

* 하나의 코어가 마치 두개인것처럼 동작을 하기는 하지만, 실제로는 코어의 동작 상태에 따라서 2개의 성능을 다하진 못하다.

---

```
즉, 1Core 일때 두개의 쓰레드가 동작하는 것과
1Core 하이퍼쓰레딩일때 두개의 쓰레드가 동작하는 것은 다르게 동작하고,
속도도 하이퍼쓰레딩이 더 빠릅니다.

그렇다 하더라도 결국 
1Core Hyperthreading보다는 당연히 2Core의 동작 속도가 더 빠릅니다.
```

---

```
두개의 코어일때는 각각 동작합니다.

1. 1코어 1스레드에서 2스레드 : 두개 스레드가 번갈아가며 실행한다.

2. 1코어 2하이퍼스레드에서 2스레드 : 각 스레드에서 실행하는 명령이 독립적이면 동시 실행 
    (예를들어 한쪽에서 실수연산을 하는동안 다른쪽이 정수연산을 하면 거의 코어가 2개인것 처럼 작동한다.
              똑같은 연산을 해야하면 순차적으로 실행된다.)

3. 2코어에서 2스레드 : 두개 스레드가 완전 별개로 실행.
```

---

```
코어는 물리적 단위고 쓰레드는 논리적 단위입니다.
하이퍼쓰레딩이 대표적이죠, 실제로는 1개의 물리적 코어지만 논리적으로 2개의 쓰레드가 있는 것처럼 동작하도록 한겁니다.
당연히 1코어에 1쓰레드를 가진게 10개면 
물리적으로 10개의 코어가 있어 10코어에 맞는 정직한 성능이 나오지만, 

1코어 10쓰레드는 물리적으로 1개의 코어라는 한계가 있죠. 
당연히 물리적으로 많은게 더 좋습니다
```

---

```
현대 CPU는 슈퍼스칼라, 파이프라인, 비순차실행유닛 등을 통해 명령어를 잘게 쪼개고 
서로 영향을 주지 않는 명령어를 사이사이에 넣어서 실행시간을 단축합니다. 

이러한 기술의 연장선에서 나온 것이 SMT(하이퍼쓰레딩)기술이고 
1코어 안에 있는 여러 연산장치중에 서로 영향을 주지않는 명령어를 같이 실행시키는 기술입니다. 
따라서 1물리적 코어가 여러개의 논리프로세서(쓰레드)로 컴퓨터에서 보여지는거죠.
10코어일경우 각각의 코어는 들어오는 명령을 다른 코어에 관계없이 계속 처리합니다
```

---

```
1코어 10쓰레드 - 코어 1개 돌아가는걸 운영체제에서 논리적으로 분할해서 병렬처리하는것처럼 사용.
(CPU에서 하이퍼쓰레딩을 지원 할 경우 단순히 논리적으로만 병렬처리 되는 것 이외에도 성능이 향상되긴 함) -> IO에 의한 락 발생 등 병렬처리가 필요 할 경우를 위해 사용

10코어 10쓰레드 - 실질적으로 연산하는 코어의 수 자체가 많아지므로 논리적으로 병렬처리를 가능하게 하는 것 외에 거대한 연산을 할 경우 성능자체가 향상됨
```

---

```
A1. 열개의 일이 있을때
열명이 하나씩 일을 하는 거랑
한명이 열개의 일을 하는 차이.

A2. 열개의 일이 있을때
열명이 나누어서 하는 거랑
한명이 나누어서 하는 차이.

==> 코어를 "사람" 쓰레드를 "일"로 생각하면 편하겠다.
```
---

```
뻥튀기를 생각하면 됨
뻥튀기 열개를 먹을 것인지
뻥튀기 한개를 10 조각 내서 먹을 것인지에 대한 문제임
뻥튀기 하나를 10조각 내서 먹으면 흘리지 않고 깔끔하게 먹을 가능성이 높지만 그 반대는 흘릴 가능성이 높음
```


---

## Review

* 다른 블로그처럼 정리하는 것보다 이렇게 Q/A 식으로 개념을 정립하는게 더 편하고 쉽게 이해됐다. <small>정리하기 귀찮아서 하는 말 같지만 진짜다 ㅎㅎ</small>










