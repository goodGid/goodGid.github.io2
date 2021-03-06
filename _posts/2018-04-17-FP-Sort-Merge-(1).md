---
layout: post
title:  " 정렬/합병 [ Part 1 ] "
categories: 파일처리
author: goodGid
---
* content
{:toc}


# 정렬/합병의 개요

* 내부 정렬
    - 데이터가 적어서 메인 메모리 내에 모두 저장시켜 정렬가능할 때

* 외부 정렬
    - 데이터가 많아서 `메인 메모리`의 용량을 `초과`하여 보조 저장 장치에 저장된 파일을 정렬할 때
    - `레코드 판독`, `기록`에 `걸리는 시간`이 중요
    - 정렬/합병
        - `런(run)` : `하나의 파일`을 `여러 서브파일`로 나누어 `내부 정렬 기법`을 사용하여 정렬시킨 파일
        - `런`을 `합병`하여 원하는 `하나의 정렬된 파일`을 만듬


![](/assets/img/file_processing/sort_merge_1_1.png)



---

# 정렬 단계

* 런 생성방법
    - 내부 정렬
    - 대체 선택
    - 자연 선택
    - 런의 수를 줄이기 위해 `길이`는 `길게` <br> 그래야 합병할 때 `런`의 `수`가 `적어` 빠른 시간내 처리 가능

---

# [1] 내부 정렬

* 런 생성 방법
    - 파일을 n개 레코드씩 분할
    - 분할된 레코드들을 `내부 정렬 기법`으로 정렬
    - 길이를 고정

* 특징
    - 제일 `마지막 런`을 `제외`하고 모두 길이가 같다.


![](/assets/img/file_processing/sort_merge_1_2.png)



---

# [2] 대체 선택

* 런 생성 방법


![](/assets/img/file_processing/sort_merge_1_3.png)



* 특징
    - 내부 정렬과 달리 입력 파일의 일부 정렬된 레코드들의 순서를 이용 
    - 내부 정렬을 이용한 경우보다 `런의 길이`가 `길다`.
        - 런의 개수가 줄어(↓) --> 합병 과정 비용 줄임(↓)
    - 런의 평균 예상 길이 : 2m
    - 내림차순일 때 최악
    - 오름차순일 때 런 1개 생성
    - 내부에 비해 `런`의 `수`는 `1/2배`, `런`의 `길이`는 `2배`

---

# [3] 자연 선택

* 런 생성 방법
    - 대체 선택과는 달리 동결된 레코드들을 버퍼에 유지하지 않고 <br> 보조 저장 장치에 `예비 장소`를 설정하여 별도 저장
    - 하나의 런은 예비장소가 꽉 차고 버퍼에 들어올 데이터가 없거나 입력 파일이 공백이 될 때까지 계속 생성

* 특징
    - `대체 선택`보다 `런`을 `길게`하여 런 수를 `줄임(↓)` <br> --> `합병과정 비용`을 `줄임(↓)`


---

# 런 생성 방법의 비교

* 내부 정렬
    - 마지막 럭을 제외하고 모든 런들의 길이가 같음
    - 런의 길이를 예측할 수 있으므로 합병 알고리즘이 간단

* 대체 선택
    - 내부 정렬보다 평균적으로 훨씬 긴 런을 생성
    - 런의 길이가 길수록 합병 비용이 적음
    - 런의 길이가 일정치 않아 정렬/합병 알고리즘이 복잡

* 자연 선택
    - 앞의 두 방법보다 더 긴 런을 생성할 수 있음
    - 예비 장소로의 입출력이 문제가 됨
    - 긴 런 생성에 따른 효율화가 예비 장소 전송 비용보다 이익이 될 수도 있음

---

# 파일 정렬/합병 방법의 차이점

* 차이점을 나타내는 매개 변수
    - 적용하는 내부 정렬 방식
    - 내부정렬을 위해 할당된 메인 메모리의 크기
    - 정렬된 런들의 보조 저장 장치에서의 저장 분포
    - 정렬/합병 단계에서 동시에 처리할 수 있는 런의 수

* 정렬/합병 기법의 성능
    - 매개 변수에 따라 런의 수와 합병의 `패스(pass)수` 결정
        - `패스 수` : 정렬/합병 기법이 몇 번의 반복적 수행을 거쳐서 최종 파일에 출력하느냐를 나타내는 것

    - 레코드 판독/기록 횟수 성능에 영향을 미치는 요인
        - 가능한 `런의 길이`를 `길게` 만들어 `런의 수`를 `최소화`
        - `동시`에 `합병`할 수 있는 `런의 수`를 `늘리면` `합병`의 `패스 수`는 `감소`

