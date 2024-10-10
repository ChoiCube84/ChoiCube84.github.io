---
layout: post
title: 해석학 1단원 실수체와 복소수체 - 5
subtitle: 1단원 연습문제 풀이
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

## 문제 풀이

### Exercise 1

만약 $r$ 이 유리수이고 $(r \neq 0)$ $x$ 가 무리수라면, $r+x$ 와 $rx$ 가 무리수임을 증명하라.

#### 풀이

Assume $r+x$ is rational. Then, $p=r+x$ is rational, $x=p-r$ is rational as well. But $x$ is irrational. So $r+x$ is irrational. Assume $rx$ is rational. Then, $q=rx$ is rational, $x=q/r$ is rational as well. (Note that $r \neq 0$.) But $x$ is irrational. So $rx$ is rational. $\blacksquare$

### Exercise 2

제곱하여 $12$ 가 되는 유리수가 없음을 증명하라.

#### 풀이

Assume there is a rational number $p$ whose square is $12$. Then, $p^2=12$. Since $p$ is a rational number, we can express $p=m/n$ for integers $m$ and $n$ that are coprime. Then, $p^2=m^2/n^2=12$, $m^2=12n^2$. Since $m^2$ is a even number, $m$ is also an even number, so we can express $m=2k$ for an integer $k$. Then, $4k^2=12n^2$, $k^2=3n^2$. Since $k^2$ can be divided by $3$, $k$ also can be divided by $3$. So we can express $k=3l$ for an integer $l$. Now, $9l^2=3n^2$, $n^2=3l^2$. Again, we can see that $n$ can be divided by $3$, but since $k$ can be divided by $3$, $m=2k$ can be divided by $3$. This contradicts our assumption thaat $m$ and $n$ are coprime. So tehre's no rational number whose square is $12$. $\blacksquare$

### Exercise 3

Proposition 1.15를 증명하라.

Proposition 1.15: 곱셈에 대한 공리들은 다음 명제들을 함의한다.

- (a) 만약 $x \neq 0$ 이고 $xy = xz$ 이면 $y = z$ 이다.

- (b) 만약 $x \neq 0$ 이고 $xy = x$ 이면 $y = 1$ 이다.

- (c) 만약 $x \neq 0$ 이고 $xy = 1$ 이면 $y = 1/x$ 이다.

- (d) 만약 $x \neq 0$ 이면 $1/(1/x) = x$ 이다.

#### 풀이

- (a) $y = 1y = ((1/x) \cdot x) \cdot y = (1/x) \cdot (xy) = (1/x) \cdot (xz) \ ((1/x) \cdot x) \cdot z = 1z = z$

- (b) $y = 1y = ((1/x) \cdot x) \cdot y = (1/x) \cdot (xy) = (1/x) \cdot x = 1$

- (c) $y = 1y = ((1/x) \cdot x) \cdot y = (1/x) \cdot (xy) = (1/x) \cdot 1 = 1/x$

- (d) We will use the result of (c). Replace $x$ with $1/x$ ($1/x$ is defined because $x \neq 0$) and replace $y$ with $x$. Then, we get $x=1/(1/x)$.

$\blacksquare$

### Exercise 4

$E$ 를 어떤 순서 집합의 '공집합이 아닌 부분집합' 이라고 하자; $\alpha$ 를 $E$ 의 lower bound, $\beta$ 를 $E$ 의 upper bound 라고 가정하자. $\alpha \le \beta$ 임을 증명하라.

#### 풀이

Assume $\alpha > \beta$. Since $E$ is nonempty, there exist an element in $E$. Let $x$ denote that element. Since $\beta$ is an upper bound of $E$, $x \le \beta < \alpha$, so $x < \alpha$. However, this contradicts the fact that $\alpha$ is a lower bound of $E$, which means that $\alpha \le x$. Therefore, $\alpha \le \beta$. $\blacksquare$

### Exercise 5

Let $A$ be a nonempty set of real numbers which is bounded below. Let $-A$ be the set of all numbers $-x$, where $x \in A$. Prove that $\inf A = -\sup(-A)$.

#### 풀이

Let $\alpha := \inf A$. Then, $\alpha \le x$ for every $x \in A$. This implies $-x \le -\alpha$ foro every $x \in A$, according to the Proposition 1.18 (c). Since $-A$ contains every $-x$ for every $x \in A$, $y \le -\alpha$ for every $y \in -A$. (WIP: Now we need to prove that any real number $\gamma$ smaller than $-\alpha$ cannot be an upper bound of $-A$, but me in the past forgor to show it 💀)

### Exercise 6

Fix $b>1$.

- (a) If $m,n,p,q$ are integers, $n>0$, $q>0$, and $r=m/n=p/q$, prove that $(b^m)^{1/n} = (b^p)^{1/q}$. Hence it makes sense to define $b^r = (b^m)^{1/n}$.

- (b) Prove that $b^{r+s} = b^r b^s$ if $r$ and $s$ are rational.

- (c) If $x$ is real, define $B(x)$ to be the set of all numbers $b^t$, where $t$ is rational and $t \le x$. Prove that $b^r = \sup B(r)$ when $r$ is rational.

- (d) Prove that $b^{x+y} = b^x b^y$ for all real $x$ and $y$.

#### 풀이

- (a) 

- (b) 

- (c) 

- (d) 

### Exercise 7

(WIP)

#### 풀이

(WIP)

### Exercise 8

(WIP)

#### 풀이

(WIP)

### Exercise 9

(WIP)

#### 풀이

(WIP)

### Exercise 10

(WIP)

#### 풀이

(WIP)

### Exercise 11

(WIP)

#### 풀이

(WIP)

### Exercise 12

(WIP)

#### 풀이

(WIP)

### Exercise 13

(WIP)

#### 풀이

(WIP)

### Exercise 14

(WIP)

#### 풀이

(WIP)

### Exercise 15

(WIP)

#### 풀이

(WIP)

### Exercise 16

(WIP)

#### 풀이

(WIP)

### Exercise 17

(WIP)

#### 풀이

(WIP)

### Exercise 18

(WIP)

#### 풀이

(WIP)

### Exercise 19

(WIP)

#### 풀이

(WIP)

### Exercise 20

(WIP)

#### 풀이

(WIP)

## 마무리

오늘 배운 내용들을 간단하게 요약해보겠다.

1. 

오늘의 공부는 여기까지!
