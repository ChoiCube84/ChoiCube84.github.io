---
layout: post
title: 해석학 2단원 기초적인 위상수학 - 1
subtitle: '유한 집합, 가산 집합, 그리고 비가산 집합'
date: '2023-08-04 23:59:00+0900'
background: /img/posts/2023/July/mathematical_analysis.jpg
katex: true
category: Study
tags:
  - mathematical_analysis
  - mathematical_analysis_game
---

# 해석학 공부하기

(WIP)

## 지난 내용 복습

(WIP)

## 2단원 공부 시작

오늘부터 다룰 내용은 2단원의 내용으로, 단원의 이름은 `Basic Topology` 이다. 이 단원은 총 6개의 소단원으로 구성되어 있다.

1. Finite, Countable, and Uncountable Sets: 집합의 '크기' 에는 `finite`, `countable`, 그리고 `uncountable` 이 있음을 이야기하며, 이 세 가지 크기의 집합들의 성질에 대해 이야기한다.

2. Metric Spaces: `metric` 이 무엇인지 정의하며, `metric` 이 정의되는 집합인 `metric space` 와 관련된 여러 가지 개념들을 정의한다.

3. Compact Sets: `compact set` 의 개념을 정의하고, 이 집합이 가지는 성질들을 알아본다.

4. Perfect Sets: 2번째 소단원에서 정의된 `perfect set` 의 성질을 알아보고, 대표적인 예시인 `Cantor set` 에 대해 알아본다.

5. Connected Sets: `connected set` 의 개념을 정의하고, 관련된 성질을 알아본다.

6. Exercises: 배운 개념을 활용하여 풀어볼 연습문제들이다.

오늘은 첫 번째 소단원인 개의 `Finite, Countable, and Uncountable Sets` 소단원을 다루어볼 것이다. 

## Finite, Countable, and Uncountable Sets

### 2.1 Definition

두 집합 $A$ 와 $B$ 를 생각해보자. 이 집합들의 원소들이 무엇이던간에, $A$의 각 원소 $x$ 가 어떤 특정한 방식으로 $B$ 의 원소인 $f(x)$ 와 관련이 있다고 한다면, $f$ 를 $A$ 에서 $B$ 로의 `function (함수)`  (또는 $A$ 에서 $B$ 로의 `mapping (매핑)`) 이라고 한다. 

이 때, 집합 $A$ 는 $f$ 의 `domain (정의역)` 이라고 불리고 (우리는 이 때 또한 $f$ 가 $A$ 에서 정의되어 있다고 한다), 그리고 원소들인 $f(x)$ 들은 $f$ 의 `values (값들)` 라고 불린다. $f$ 의 모든 `values` 들의 집합을 `range (치역)` 라고 부른다.

#### 해설

(WIP)

### 2.2 Definition

$A$ 와 $B$ 를 두 개의 집합, 그리고 $f$ 를 $A$ 에서 $B$ 로의 `mapping` 이라고 하자.
만약 $E \subset A$ 이고, $f(E)$ 를 모든 $x \in E$ 에 대한 $f(x)$ 원소들의 집합이라고 정의한다면, 우리는 $f(E)$ 를 $f$ 에 의한 $E$ 의 `image (상)` 라고 한다. 이러한 방식의 표기법에선, $f(A)$ 는 $f$ 의 `range` 이다. 

It is clear that $f(A) \subset B$. If $f(A) = B$, we say that $f$ maps A onto B. (Note that, according to this usage, onto is more specific than into.)
If $E \subset B$, $f^{-1}(E)$ denotes the set of all $x \in A$ such that $f(x) \in E$. We call $f^{-1}(E)$ the inverse image of $E$ under $f$ If $y \in B$, $f^{-1}(y)$ is the set of all $x \in A$ such that $f(x) = y$. If, for each $y \in B$, $f^{-1}(y)$ consists of at most one element of $A$, then $f$ is said to be a 1-1 (one-to-one) mapping of $A$ into $B$. This may also be expressed as follows: $f$ is a 1-1 mapping of $A$ into $B$ provided that $f(x_1) \ne f(x_2)$ whenever $x_1 \ne x_2$ , $x_1 \in A$, $x_2 \in A$. (The notation $x_1 \ne x_2$ means that $x_1$ and $x_2$ are distinct elements; otherwise we write $x_1 = x_2$.)

#### 해설

(WIP)

### 2.3 Definition

(WIP)

#### 해설

(WIP)

### 2.4 Definition

(WIP)

#### 해설

(WIP)

### 2.5 Example

(WIP)

#### 해설

(WIP)

### 2.6 Remark

(WIP)

#### 해설

(WIP)

### 2.7 Definition

(WIP)

#### 해설

(WIP)

### 2.8 Theorem

(WIP)

#### 해설

(WIP)

### 2.9 Definition

(WIP)

#### 해설

(WIP)

### 2.10 Examples

(WIP)

#### 해설

(WIP)

### 2.11 Remarks

(WIP)

#### 해설

(WIP)

### 2.12 Theorem

(WIP)

#### 해설

(WIP)

### Corollary of 2.12

(WIP)

#### 해설

(WIP)

### 2.13 Theorem

(WIP)

#### 해설

(WIP)

### Corollary of 2.13

(WIP)

#### 해설

(WIP)

### 2.14 Theorem

(WIP)

#### 해설

(WIP)

## 마무리

오늘 배운 내용들을 간단하게 요약해보겠다.

1. 

오늘의 공부는 여기까지!
