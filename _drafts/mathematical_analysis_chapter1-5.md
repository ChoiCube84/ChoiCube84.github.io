---
layout: post
title: 해석학 1단원 실수체와 복소수체 - 5
subtitle: '부록: 실수체 구성하기'
date: '2023-08-04 23:59:00+0900'
background: /img/posts/2023/July/mathematical_analysis.jpg
katex: true
category: Study
tags:
  - mathematical_analysis
  - mathematical_analysis_game
---

# 해석학 공부하기

오늘도 해석학 공부를 시작하겠다. (WIP)

## 지난 내용 복습

(WIP)

## Appendix

Theorem 1.19 는 $\mathbb{Q}$ 를 이용하여 $\mathbb{R}$ 를 구성함으로써 증명될 것이다. 우리는 구성하는 과정을 여러 단계로 나눌 것이다.

### Step 1

$\mathbb{R}$ 의 원소들은 `cut` 이라고 불리는 $\mathbb{Q}$ 의 특정 부분 집합들이 될 것이다. `cut` 이란, 정의에 따라 다음 3가지 성질을 만족하는 아무 집합 $\alpha \subset \mathbb{Q}$ 을 의미한다.

- (I) $\alpha$ 는 공집합이 아니고, $a \neq \mathbb{Q}$ 이다.

- (II) 만약 $p \in \alpha$, $q \in \mathbb{Q}$, 그리고 $q < p$ 이면, $q \in \alpha$ 이다.

- (III) 만약 $p \in \alpha$ 이면, 어떤 $r \in \alpha$ 에 대하여 $p < r$ 이다.

$p$, $q$, $r$, ... 과 같은 문자들은 항상 유리수를 나타낼 것이며, $\alpha$, $\beta$, $\gamma$, ... 와 같은 문자들은 `cut` 들을 나타낼 것이다.

(III) 은 단지 $\alpha$ 에 '가장 큰 원소' 가 없음을 의미한다. (II) 는 다음 두가지 사실들이 자유롭게 사용될 수 있다는 점을 암시한다:

- 만약 $p \in \alpha$ 이고 $q \notin \alpha$ 이면, $p < q$ 이다.

- 만약 $r \notin \alpha$ 이고 $r < s$ 이면 $s \notin \alpha$ 이다.

#### 해설

(WIP)

### Step 2

"$\alpha < \beta$" 가 다음 내용을 의미하도록 정의하자: $\alpha$ 는 $\beta$ 의 진부분 집합이다.

이것이 Definition 1.5 의 조건들을 만족하는지 살펴보자.

만약 $\alpha < \beta$ 이고 $\beta < \gamma$ 이면 $\alpha < \gamma$ 임은 분명하다. (어떤 집합의 진부분 집합의 진부분 집합은, 그 어떤 집합의 진부분 집합이다.) 또한, 아무 쌍 $\alpha$, $\beta$ 에 대하여 다음 세 가지 관계 $\alpha < \beta$, $\alpha = \beta$, $\beta < \alpha$ 중 많아봐야 한 개만 성립함은 분명하다. 적어도 하나가 성립함을 보이기 위해서, 처음 2가지 관계가 성립하지 않는다고 가정하자. 그렇다면 $\alpha$ 는 $\beta$ 의 부분 집합이 아니다. 따라서, 어떤 $p \in \alpha$ 가 $p \notin \beta$ 를 만족한다. 만약 $q \in \beta$ 이면, $q < p$ 임을 알 수 있고 ($p \notin \beta$ 이기에), 그러므로 (II) 에 의해 $q \in \alpha$ 이다. 그러므로 $\beta \subset \alpha$ 이다. $\beta \neq \alpha$ 이므로, $\beta < \alpha$ 이다. 그러므로 $\mathbb{R}$ 은 이제 ordered set 이다.

#### 해설

(WIP)

### Step 3

(WIP)

#### 해설

(WIP)

### Step 4

(WIP)

#### 해설

(WIP)

### Step 5

(WIP)

#### 해설

(WIP)

### Step 6

(WIP)

#### 해설

(WIP)

### Step 7

(WIP)

#### 해설

(WIP)

### Step 8

(WIP)

#### 해설

(WIP)

### Step 9

(WIP)

#### 해설

(WIP)

## 마무리

오늘 배운 내용들을 간단하게 요약해보겠다.

1. 

오늘의 해석학 공부는 여기까지!
