---
layout: post
title:  " 캐시 일관성(Cache Coherence)"
categories: Technology
tags: Technology
author: goodGid
---
* content
{:toc}

## 캐시 일관성이란?

* 캐시 일관성(영어: Cache Coherence)이란 **공유 메모리 시스템**에서 **각 클라이언트(혹은 프로세서)**가 가진 **로컬 캐시 간**의 **일관성**을 의미한다.

* 각 클라이언트가 자신 만의 로컬 캐시를 가지고 다른 여러 클라이언트와 메모리를 공유하고 있을 때, 캐시의 갱신으로 인한 **데이터 불일치 문제**가 발생한다. 











* 예를 들어 변수 X에 대해서 두 클라이언트가 변수 X를 공유하고 있고 그 값이 0이라고 하자. 

* 이때 클라이언트 1(그림의 윗쪽)이 X에 1을 대입하였고 클라이언트 2(그림의 아래쪽)가 변수 X를 읽어들이게 되면 클라이언트 2는 클라이언트 1에 의해 수정된 값인 1을 받아들이는 것이 아니라 현재 자신의 로컬 캐시에 있는 0을 읽어들이게 된다. 

* 따라서 캐시 1, 2는 같은 X라는 변수에 대해 다른 값을 가지게 되므로 데이터 불일치 문제가 발생한다. 

![](/assets/img/os/cache_coherence_1.png)

* 또다른 예를 들면 만약 코어 0이 100번지 해당하는 데이터를 읽었다고 할때 100번지 데이터는 코어 0의 전용 캐시에 있게 된다. 

* 코어 1 역시 100번지 데이터를 읽는다고 하자. 역시 100번지는 코어 1에 전용 캐시에 있게 된다.

* 두 코어 모두 읽기만 했으므로 두코어의 전용 캐시에 있는 100번지 데이터는 서로 같다. 

* 그런데 코어 0에서 값을 Write한다면 문제가 발생된다. 

* 대부분 캐시는 **라이트 백 쓰기 정책**을 사용하므로 100번지에 쓴 값은 바로 메모리에 갱신되지 않고 코어 0에 최신값으로 갱신된다.

* 이 때 코어 1에서 100번지 데이터가 다시 필요하면 코어 1은 최신 값이 아닌 자신의 캐시에서 값을 읽어오기 때문에 이전에 값을 읽어온다. 

* 따라서 하드웨어 차원에서 항상 최신내용을 읽을 수 있게 캐시 일관성(Cache Coherence)을 지원해야한다.

* **캐시 일관성**을 **유지**한다고 하는 것은 이러한 **데이터 불일치 현상**을 없애는 것을 의미한다.

* 캐시 일관성을 유지하기 위해서는 다른 프로세서가 갱신한 캐시 값을 **곧바로** 혹은 **지연**하여 다른 프로세서에서 사용할 수 있도록 해주어야 한다. 

* 캐시 일관성을 유지하기 위한 다양한 프로토콜이 존재한다.

---

## 캐시 일관성 프로토콜

* 디렉토리와 스누핑은 연구가 활발히 진행되고 있다.

### 디렉토리 프로토콜

* 디렉토리 기반 일관성 구조는 캐시 블록의 공유 상태, 노드 등을 기록하는 저장 공간인 디렉터리를 이용하여 관리하는 구조이다.

* **데이터의 복사본**이 **존재하는 위치**에 대한 정보를 수집하고 유지한다. 

* 디렉터리 기반 구조는 어떤 노드에서 해당 캐시 블록의 복사본을 가지고 있는지를 알고 있기 때문에 **특정 노드**에만 **요청**을 하게 된다. 

* 따라서 **브로드캐스트**가 **불필요**하게 되어 **대역폭**이 상대적으로 작아도 된다.

* 주기억장치에는 여러 개의 지역 캐시들의 내용에 대한 **전역 상태 정보**가 담긴 디렉토리가 저장된다. 

* 또한 주기억장치 제어기의 일부로 존재하는 **중앙 제어기**는 프로세서가 가진 복사본의 라인 정보를 가지고 있다. 

* **중앙 제어기**는 **모든 지역 캐시 제어기**의 **동작**을 **제어**하고 **보고**를 받아 **캐시 일관성**을 **유지**한다.

* 중앙 병목(central bottleneck) 및 캐시 제어기들과 중앙 제어기 간의 통신 오버헤드를 가지는 단점이 존재한다.

* 하지만 다중 버스(multiple buses)나 복잡한 상호연결망을 포함하는 **대규모 시스템**에 **효과적**이다.

* 이 때문에 64개 이상의 프로세서를 가지는 **대규모 시스템**에서는 **디렉터리 기반**의 **캐시 일관성 프로토콜**을 사용하는 경우가 많다.




---


### 스누핑 프로토콜

* 스누핑(snooping)은 **주소 버스**를 **항상 감시**하여 캐시 상의 메모리에 대한 접근이 있는지를 감시하는 구조이다. 

* 다른 캐시에서 쓰기가 발생하면 **캐시 컨트롤러**에 의해서 자신의 캐시 위에 있는 **복사본**을 **무효화**시킨다.

* **캐시 일관성 유지**에 대한 **책임**을 다중 프로세서 내의 **모든 캐시 제어기들**에게 분산한다. 

* 캐시는 자신이 가진 라인이 다른 캐시와 공유되는지를 파악하고 있으며, 공유 캐시 라인이 갱신되었을 때, 각 캐시 제어기는 **Broadcasting**된 캐시 갱신 소식을 받아 해당 캐시를 무효화한다. 

* 공유 버스가 Broadcasting과 Snooping에 유리하므로 스누핑 프로토콜은 버스 기반 다중 프로세서에 적합하다. 

* write-invalidate, write-update, write-broadcast 등으로 스누피 방식을 구현한다.

* 이 방법은 모든 캐시 라인의 상태를 Modified, Exclusive, Shared, Invalid로 표현하기에 **MESI 프로토콜**이라고도 한다.

* MESI 프로토콜
    - 캐시 메모리의 일관성을 유지하기 위해서 **별도의 플래그(flag)**를 할당한 후 플래그의 상태를 통해 데이터의 유효성 여부를 판단하는 프로토콜이다. 
    - 데이터 캐시는 태그 당 **두 개의 상태 비트**를 **포함**한다.
    - 멀티프로세서 시스템에서 캐시 메모리의 일관성을 유지하기 위해 메모리가 가질 수 있는 **4가지 상태**를 정의한다.
        - Modified(수정) 상태 : 데이터가 수정된 상태
        - Exclusive(배타) 상태 : 유일한 복사복이며, 주기억장치의 내용과 동일한 상태
        - Shared(공유) 상태 : 데이터가 두 개 이상의 프로세서 캐쉬에 적재되어 있는 상태
        - Invalid(무효) 상태 : 데이터가 다른 프로세스에 의해 수정되어 무효화된 상태

<br>

* 스누핑의 경우 각 노드의 대역폭이 충분히 크다면 좋은 성능을 기대할 수 있다. 

* 그러나 **성능 확장성(Scalability)**이 좋지 않다.

* 왜냐하면 메모리 요청(Request)에 대해 다른 모든 노드에 브로드캐스트 해야 하기 때문이며 노드의 수가 증가하면 더 많은 브로드캐스트가 발생하게 되고 이 때문에 버스의 대역폭도 더 늘어나야만 한다.

---

## 캐시 관련 용어

* 캐시에 원하는 데이터가 있다면 **Cache Hit**

* 캐시에 원하는 데이터가 없다면 **Cache Miss** 

* 만약 Miss가 발생되었다면 해당 데이터를 찾아와야하는 비용이 생기는데 이를 **Cache Penalty**라고 한다. 

* 캐시가 적중해도 원하는 데이터를 가져오는 비용을 **Hit Latency**라고 표현한다.

---

## 참고

* [캐시 일관성](https://www.ibm.com/support/knowledgecenter/ko/ssw_aix_71/com.ibm.aix.performance/cache_coherency.htm)

* [Cache 란?](http://cesl.tistory.com/entry/Cache-%EC%A0%95%EB%A6%AC)

* [캐시 일관성](https://ko.wikipedia.org/wiki/%EC%BA%90%EC%8B%9C_%EC%9D%BC%EA%B4%80%EC%84%B1)

* [MESI 프로토콜](https://ko.wikipedia.org/wiki/MESI_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)