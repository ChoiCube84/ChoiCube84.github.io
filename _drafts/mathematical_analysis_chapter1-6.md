---
layout: post
title: 해석학 1단원 실수체와 복소수체 - 6
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

오늘도 해석학 공부를 시작하겠다. 실수체의 필요성을 논하는 것 부터 시작하여, 실수체를 실제로 정의하고 이를 기반으로 복소수체와 유클리드 공간을 정의하며 이들의 성질을 알아보았고, 실수를 실제로 '구성하는' 방법까지 모두 알아보았다. 이제 우리는 배웠던 내용들과 연관하여 여러 가지 연습문제들을 풀어볼 것이다.

1단원의 연습 문제는 총 20문제가 있다. 각 문제들의 주제는 다음과 같다.

1. Exercise 1~5: (WIP)

2. Exercise 6~7: 어떤 수에 '실수' 를 제곱하는 것을 정의하고, 이를 활용하여 '로그 값' 이 존재함에 대해 논한다.

3. Exercise 8~14: 복소수체와 관련된 다양한 성질과 부등식들에 대해 살펴본다.

4. Exercise 15~19: 유클리드 공간과 관련된 다양한 성질과 부등식들에 대해 살펴본다.

5. Exercise 20: 실수를 구성하는 과정에서 정의했던 'cut' 에 대해 자세히 탐구해본다.

바로 문제 풀이를 시작하겠다.

## 문제 풀이

### Exercise 1

만약 $r$ 이 유리수이고 $(r \neq 0)$ $x$ 가 무리수라면, $r+x$ 와 $rx$ 가 무리수임을 증명하라.

#### 풀이

Assume $r+x$ is rational. Then, $p=r+x$ is rational, $x=p-r$ is rational as well. But $x$ is irrational. So $r+x$ is irrational. Assume $rx$ is rational. Then, $q=rx$ is rational, $x=q/r$ is rational as well. (Note that $r \neq 0$.) But $x$ is irrational. So $rx$ is rational. $\blacksquare$

#### 설명

(WIP)

### Exercise 2

제곱하여 $12$ 가 되는 유리수가 없음을 증명하라.

#### 풀이

Assume there is a rational number $p$ whose square is $12$. Then, $p^2=12$. Since $p$ is a rational number, we can express $p=m/n$ for integers $m$ and $n$ that are coprime. Then, $p^2=m^2/n^2=12$, $m^2=12n^2$. Since $m^2$ is a even number, $m$ is also an even number, so we can express $m=2k$ for an integer $k$. Then, $4k^2=12n^2$, $k^2=3n^2$. Since $k^2$ can be divided by $3$, $k$ also can be divided by $3$. So we can express $k=3l$ for an integer $l$. Now, $9l^2=3n^2$, $n^2=3l^2$. Again, we can see that $n$ can be divided by $3$, but since $k$ can be divided by $3$, $m=2k$ can be divided by $3$. This contradicts our assumption thaat $m$ and $n$ are coprime. So tehre's no rational number whose square is $12$. $\blacksquare$

#### 설명

(WIP)

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

#### 설명

(WIP)

### Exercise 4

$E$ 를 어떤 순서 집합의 '공집합이 아닌 부분집합' 이라고 하자; $\alpha$ 를 $E$ 의 lower bound, $\beta$ 를 $E$ 의 upper bound 라고 가정하자. $\alpha \le \beta$ 임을 증명하라.

#### 풀이

Assume $\alpha > \beta$. Since $E$ is nonempty, there exist an element in $E$. Let $x$ denote that element. Since $\beta$ is an upper bound of $E$, $x \le \beta < \alpha$, so $x < \alpha$. However, this contradicts the fact that $\alpha$ is a lower bound of $E$, which means that $\alpha \le x$. Therefore, $\alpha \le \beta$. $\blacksquare$

#### 설명

(WIP)

### Exercise 5

Let $A$ be a nonempty set of real numbers which is bounded below. Let $-A$ be the set of all numbers $-x$, where $x \in A$. Prove that $\inf A = -\sup(-A)$.

#### 풀이

Let $\alpha := \inf A$. Then, $\alpha \le x$ for every $x \in A$. This implies $-x \le -\alpha$ foro every $x \in A$, according to the Proposition 1.18 (c). Since $-A$ contains every $-x$ for every $x \in A$, $y \le -\alpha$ for every $y \in -A$. (WIP: Now we need to prove that any real number $\gamma$ smaller than $-\alpha$ cannot be an upper bound of $-A$, but me in the past forgor to show it 💀)

#### 설명

(WIP)

### Exercise 6

Fix $b>1$.

- (a) If $m,n,p,q$ are integers, $n>0$, $q>0$, and $r=m/n=p/q$, prove that $(b^m)^{1/n} = (b^p)^{1/q}$. Hence it makes sense to define $b^r = (b^m)^{1/n}$.

- (b) Prove that $b^{r+s} = b^r b^s$ if $r$ and $s$ are rational.

- (c) If $x$ is real, define $B(x)$ to be the set of all numbers $b^t$, where $t$ is rational and $t \le x$. Prove that $b^r = \sup B(r)$ when $r$ is rational.

- (d) Prove that $b^{x+y} = b^x b^y$ for all real $x$ and $y$.

#### 풀이

- (a) Since $m/n=p/q$, $mq=pn$. By multiplying $(b^m)^{1/n}$ $nq$ times, we get $((b^m)^{1/n})^nq=(b^m)^q=b^{mq}$. And if we multiply $(b^p)^{1/q}$ $nq$ times, we get $((b^p)^{1/q})^{nq}=((b^p)^{1/q})^{qn}=(b^p)^n=b^{pn}$. Since $mq=pn$, $b^{mq}=b^{pn}$, so both $(b^m)^{1/n}$ and $(b^p)^{1/q}$ are $nq$-th root of $b^{mq}$. According to the Theorem 1.21, there should be only one $nq$-th root of $b^{mq}$. Therefore, $(b^m)^{1/n}=(b^p)^{1/q}$, so it makes sense to define $b^r=(b^m)^{1/n}$.

- (b) Since $r$ and $s$ are rational, we can express r=m/n, s=p/q (n,q>0). Then, b^{r+s}=b^(m/n+p/q)=b^(mq/nq+pn/qn)=(b^(mq_pn))^{1/nq}=(b^{mq} b^{pn})^{1/nq}. According to the Corollary of Theorem 1.21, (b^{mq} b^{pn})^{1/nq}=b^{mq/nq}b^{pn/nq}=b^{m/n}b^{p/q}=b^r b^s. We proved that $b^{r+s}=b^r b^s$.

- (c) To show that b^r is the supremum of $B(r)$, we need to prove that: (i) b^r is an upper bound of B(r), (ii) If \gamma < b^r, then \gamma is not an upper bound of B(r). In B(r), every element is in form of b^t with t \le r. (WIP)

- (d) (WIP)

#### 설명

(WIP)

### Exercise 7

$b > 1$, $y > 0$ 라고 두고, $b^x=y$ 를 만족하는 '유일한' 실수 $x$ 존재함을 다음 개요를 완성하여 증명하라. (이 $x$ 는 $b$ 를 밑으로 하는 $y$ 의 *logarithm* (로그) 라고 불린다.)

- (a) 모든 양의 정수 $n$ 에 대하여, $b^n-1 \qe n(b-1)$ 을 만족한다.

- (b) 그러므로 $b-1 \ge n(b^{1/n} - 1)$ 이다.

- (c) 만약 $t > 1$ 이고 $n > (b-1)/(t-1)$ 이면, $b^{1/n} < t$ 이다.

- (d) 만약 $w$ 가 $b^w < y$ 를 만족한다면, 충분히 큰 $n$ 에 대해서 $b^{w+(1/n)} < y$ 가 성립한다. 이를 확인하기 위해서 (c)에서 $t$ 자리에 $y \cdot b^{-w}$ 를 넣어보아라.

- (e) 만약 $b^2 > y$ 이면, 충분히 큰 $n$ 에 대해서 $b^{w-(1/n)} > y$ 가 성립한다.

- (f) $b^w < y$ 를 만족하는 모든 $w$ 들의 집합을 $A$ 라 하고, $x = \sup A$ 가 $b^x = y$ 를 만족시킴을 보여라.

- (g) 이 $x$ 가 유일함을 증명하라.

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 8

어떤 order 도 복소수체를 ordered field 로 만들 수 없음을 증명하라. *힌트*: $-1$ 은 제곱수이다.

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 9

$z = a + bi$, $w = c + di$ 라고 하자. $a < c$ 이면 $z < w$ 라고 정의하고, $a = c$ 이지만 $b < d$ 인 경우에도 $z < w$ 라고 정의하자. 이렇게 하면 모든 복소수의 집합이 ordered set 이 된다는 것을 증명하라. (이런 종류의 순서 관계를 *dictionary order*, 혹은 *lexicographic order* 라고 부른다.) 이 ordered set 은 least-upper-bound property 를 가지는 가?

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 10

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 11

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 12

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 13

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 14

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 15

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 16

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 17

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 18

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 19

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

### Exercise 20

(WIP)

#### 풀이

(WIP)

#### 설명

(WIP)

## 마무리

오늘 배운 내용들을 간단하게 요약해보겠다.

1. 

오늘의 공부는 여기까지!
