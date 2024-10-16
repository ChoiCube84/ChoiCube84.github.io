---
layout: post
subtitle: 순서 집합
date: '2023-07-27 23:59:00+0900'
background: /img/posts/2023/July/mathematical_analysis.jpg
katex: true
category: Study
tags:
  - mathematical_analysis
  - mathematical_analysis_game
title: 해석학 1단원 실수체와 복소수체 - 2
---
# 해석학 공부하기

오늘도 해석학 공부를 시작하도록 하겠다.

우리는 일상에서 크고 작은 것들에 대한 비교를 자주 한다. 키를 서로 비교한다던가, 시험 성적을 비교한다거나, ~~아니면 남은 복무일 수를 비교한다거나~~. 오늘은 이러한 '크고 작음'의 개념을 수학적으로 정의한 순서 집합 (`ordered set`) 을 정의해보도록 하겠다. 그리고 `least-upper-bound property` 라고 불리는 성질에 대해서도 알아보도록 하겠다.

## 지난 내용 복습

지난 번에 배웠던 내용은 실수의 필요성에 대한 내용이었다. 유리수 만으로는 $\sqrt{2}$ 같이 쉽게 생각해볼 수 있는 수도 나타낼 수 없기 때문에 실수가 필요하다는 이야기를 했었고, 집합들의 상태, 포함 관계 등을 나타내는 기호들을 배웠다.

## Ordered Sets

이 소단원에서는 ordered set 이 무엇인지 정의할 것이다. 이 ordered set 이라는 개념은 실수를 구성하는 큰 부품중 하나로, 실수에서의 '크고 작음' 의 개념이 존재하게 하는 역할을 한다. 지금부터는 해석학이나 심화된 미분적분학을 공부한 적이 없다면 생소한 용어들이 나오기 시작할테니 기대해도 좋다. 

### 1.5 Definition

$S$ 를 어떤 집합이라고 하자. 어떤 `order`는 $S$ 에서 정의 되는 관계로, $<$ 기호로 표현되며 다음과 같은 두 가지 성질을 만족한다.

1. 만약 $x\in S$ 이고 $y\in S$ 라면 이 세 가지 문장중 딱 하나만 참이다.  

	$$\begin{align*}
		x<y, \qquad x=y, \qquad y<x
	\end{align*}$$

2. 만약 $x,y,z \in S$ 이고, $x<y$ 이면서 $y<z$ 라면, $x<z$ 이다.

$x < y$ 대신 $y > x$ 라고 쓰기도 한다. $x \leq y$ 라고 쓰는 것은, $x < y$ 이거나 $x = y$ 임을 나타내며, 둘 중에 정확히 어떤 것이 참인지를 표현하지는 않는다. 달리 말하자면, $x \leq y$ 는 $x > y$ 의 부정문이다.

#### 해설

이 내용은 아마 대부분의 사람에게 익숙한 내용일 것이다. 부등호와 등호는 초, 중학교 때 배우고 오는 개념이기 떄문이다. 지체하지 않고 바로 다음 정의로 넘어가도록 하겠다.

### 1.6 Definition

`ordered set`은 `order`가 정의된 집합이다.

#### 해설

`ordered set`이라는 용어는 단어가 주는 느낌 그대로 `order`가 정의된 집합을 의미한다. 책에 나온 예시로 설명하면, 임의의 유리수 $r$ 과 $s$ 에 대하여 $s-r$ 이 양의 유리수를 의미하도록 $r<s$ 를 정의하면, 유리수 $\mathbb{Q}$ 는 `ordered set`이다.

### 1.7 Definition

$S$ 가 ordered set 이고, $E\subset S$ 라고 하자. 어떤 $\beta\in S$ 가 존재하여 모든 $x\in E$ 에 대하여  $x\leq\beta$ 를 만족시키면, $E$ 가 `bounded above`라고 하며, $\beta$ 를 $E$ 의 `upper bound`라고 한다. `lower bound`는 위의 정의에서 $\leq$ 기호를 $\geq$ 기호로 바꾸면 정의된다.

#### 해설

슬슬 낯선 단어가 나오기 시작한다. 우선 이 정의에서는 $E$ 의 `upper bound`와 `lower bound`가 무엇인지를 정의하는데, bound 라는 단어는 경계라는 의미를 가지고 있으며, `upper bound`는 말 그대로 위에 있는 경계, `lower bound`는 아래에 있는 경계를 의미한다. $E$ 의 어떠한 원소도 `upper bound`보다 클 수 없으며, `lower bound`보다 작을 수 없다.

### 1.8 Definition

$S$ 가 어떤 ordered set 이라고 하고, $E\subset S$ 이며, $E$ 가 bounded above 되어 있다고 하자. 다음 성질들을 만족하는 $\alpha\in S$ 가 존재한다고 할 때, 

1. $\alpha$ 는 $E$ 의 upper bound 이다.
2. $\gamma < \alpha$ 이 경우 $\gamma$ 는 $E$ 의 upper bound 가 아니다.

$\alpha$ 를 $E$ 의 `least upper bound` 또는 `supremum` 이라고 부르며, 다음과 같이 쓴다.  
\\[\alpha=\sup E\\]

$E$ 의 `greatest lower bound` 또는 `infimum` 도 같은 방식으로 정의된다.  
\\[\alpha=\inf E\\]

가 의미하는 바는 $\alpha$ 는 $E$ 의 lower bound 이며, $\beta>\alpha$ 이 때 $\beta$ 가 $E$ 의 lower bound 인 것은 없다는 것이다.

#### 해설

처음 보는 단어들이 나오기 시작해서 어렵게 느껴질 수 있지만 같이 의미를 천천히 뜯어나가다 보다보면 그렇게까지 어려운 개념이 아니라는 것을 알 수 있을 것이다.

`least upper bound`라는 용어를 뜯어보면, '가장 작은 위에 있는 경계'라는 말이된다. 이게 무슨 말이냐 하면, 위에 있는 경계는 경계인데, 그 경계 들중 가장 작은거라고 생각하면 된다. 

최댓값과 최솟값의 개념을 확장한 것으로 이해하면 쉬운데, 최댓값과 최솟값은 그 값이 해당 집합에 포함되어 있어야 하지만, `least upper bound` 와 `greatest lower bound`는 해당 집합에 포함되지 않을 수 있는 것이다.

다음에 나오는 1.9 예시에서는 1.8에서의 정의를 활용하여 몇 가지 집합에서 이 개념들이 어떻게 적용되는지를 설명한다.. 그리고 앞으로 `least upper bound` 와 `greatest lower bound` 보다 `supremum` 과 `infinum` 이라는 용어를 사용하는 쪽을 지향하겠다.

### 1.9 Examples

1. $A$ 를 $p^2<2$ 를 만족하는 모든 유리수 $p$ 를 모아둔 집합, $B$ 를 $p^2>2$ 를 만족하는 모든 유리수 $p$ 를 모아둔 집합이라고 하자. '유리수 집합'의 부분집합으로써의 집합 $A$ 와 $B$ 를 바라보자. 집합 $A$ 는 bounded above 되어있다. 실제로, $A$ 의 upper bound 들은 정확히 $B$ 의 원소이다. $B$ 에는 가장 작은 원소가 없으므로, $A$ 는 $\mathbb Q$ 에서 least upper bound 를 가지지 않는다.

	비슷하게, $B$ 는 bounded below 되어있다. $B$ 의 모든 lower bound 의 집합은 $A$ 와 모든 $r \le 0$ 인 $r \in \mathbb Q$ 로 구성되어 있다. $A$ 에는 가장 큰 원소가 없으므로, $B$ 는 $\mathbb Q$ 에서 greatest lower bound 를 가지지 않는다.

2. 만약 $\alpha x = \sup E$ 가 존재한다면, $\alpha x$ 는 $E$ 의 원소일 수도 있고, 아닐 수도 있다. 예를 들어, $E_1$ 를 $r < 0$ 인 $r \in \mathbb Q$ 들을 모아놓은 집합이라고 하자. 그리고 $E_2$ 를 $r \le 0$ 인 $r \in \mathbb Q$ 들을 모아놓은 집합이라고 하자. 그러면 아래의 식이 성립한다.

    \\[ \sup E_1 = \sup E_2 = 0. \\]

	그리고 우리는 $0 \notin E_1$ 이지만, $0 \in E_2$ 라는 사실을 확인할 수 있다.

3. $E$ 를 $n = 1, 2, 3, \cdots$ 에 대하여 $\frac{1}{n}$ 들을 모아놓은 집합이라고 하자. 그러면 $\sup E = 1$ 이고, 이는 $E$ 에 포함되어 있으며, $\inf E = 0$ 이고, 이는 $E$ 에 포함되어 있지 않다.

#### 해설

1. 이 예시는 Example 1.1 에서의 내용과 연결되는 내용이다. 쉽게 설명하면, $A$ 집합은 $\sqrt{2}$ 보다 작은 모든 유리수를 모아놓은 집합이고, $B$ 집합은 $\sqrt{2}$ 보다 큰 모든 유리수를 모아놓은 집합인 것인데, 각각 supremum 과 infimum 이 유리수 집합내에서는 존재하지 않는다. 그야 그 값이 있다면 $\sqrt{2}$ 여야 하는데, 이는 유리수가 아니기 때문이다.

2. 이 예시에서는 어떤 집합의 supremum 이나 infimum 이 해당 집합에 있을 수도, 없을 수도 있다는 것을 보여주고 있다. 실제로 $E_1$ 에는 supremum 인 $0$ 이 없고, $E_2$ 에는 있다는 것을 확인할 수 있다.

3. 이 예시에서는 supremum 의 집합 내 존재 여부와, infimum 의 집합 내 존재 여부가 서로 관련이 없다는 것을 보여주고 있다. 실제로 $E$ 에 supremum 인 $1$ 은 포함되어 있지만, infimum 인 $0$ 은 포함되어 있지 않다.

이 예시들을 살펴보며 알 수 있는 것은, supremum 과 infimum 은 존재하거나 존재하지 않을 수 있으며, 해당 집합에 포함될 수도 있고, 포함되지 않을 수도 있으며, 심지어 둘 중 한쪽만 포함될 수도 있다는 것이다.

이제 Definition 1.10 에서는 자세하게 알아본 supremum 과 infimum 을 기반으로 실수체가 가져야 하는 조건중 하나인 `least-upper-bound-property` 에 대해 알아볼 것이다.

### 1.10 Definition

어떤 ordered set $S$ 는 다음의 성질을 만족할 떄, `least-upper-bound property`를 만족한다고 한다:

만약 $E\subset S$ 이고, $E$ 가 공집합이 아니며, $E$ 가 bounded above 되어있으면, $\sup E$ 는 $S$ 에 존재한다.

#### 해설

이 부분의 정의가 무슨 말을 하는건지 조금 헷갈릴 수 있다. $S$ 가 `least-upper-boundproperty`를 가지고 있다는 것은, $S$ 의 부분집합인 $E$ 가 특별한 성질을 가지게 된다는 것을 의미한다.

그 특별한 성질이 무엇인가 하면, $S$ 에서 가져온 부분집합 $E$ 가 운좋게도 공집합이 아니면서 bounded above 되어있다면, 그 부분집합 $E$ 의 least upper bound 가 존재하고, 그것이 $S$ 에 포함된다는 것이다.

만약 기껏 $E$ 를 뽑긴헀는데, 공집합이거나 bounded above 되어있지 않은 경우는 전제부터 어긋나있기 때문에 아예 고려하지 않는다.

여기서 한 가지 의문이 들게 된다. `least-upper-bound property`라는게 있다면, `greatest-lower-bound property`는 없는가? 그것에 대한 이야기가 다음에 오는 Theorem 1.10이다.

### 1.11 Theorem

$S$ 가 least-upper-bound property 를 지닌 ordered set 이라고 가정하자. $B\subset S$ 이고, $B$ 가 공집합이 아니며, $B$ 가 bounded below 되어있다고 하자. $L$ 이 $B$ 의 lower bound 들을 모두 모아놓은 집합이라고 하자. 그렇다면,  
\\[\alpha = \sup L\\]

이 $S$ 에 존재하며, $\alpha = \inf B$ 이다. 달리 말하면, $\inf B$ 는 $S$ 에 존재한다.

#### 해설

이 Theorem 이 말하고자 하는 것은 `least-upper-bound property`를 가진다는 것은, 동시에 `greatest-lower-bound property`를 가진다는 것을 의미한다는 것이다. 그걸 동시에 가진다는게 말이나 되나 싶을 수 있지만, 놀랍게도 이게 된다. 아래는 그 증명이다.

<details>
<summary>증명</summary>

<br>
  
우선 $L$ 에 대하여 차근차근 뜯어보면서 이걸 증명해보자. 일단 $L$ 은 $B$ 의 lower bound 들의 집합이라고 했는데, $E\subset S$ 이고, lower bound 들은 정의 1.7에 따라서, 모두 $S$ 에 포함되어 있다. 또한, $B$ 가 bounded below 되어있다고 했으니, 하나라도 lower bound 가 존재한다는 것을 의미하고, 그것은 $L$ 이 공집합이 아니라는 것을 말해준다. 또한 $B$ 에서 아무 원소 $b$ 를 잡고, $L$ 에서 아무 원소 $l$ 을 잡아도, lower bound 의 정의에 의해서 항상 $b\geq l$ 이다. 이 말을 다시 살펴보면, $L$ 의 모든 원소 $l$ 은 어떤 $b\in B\subset S$ (따라서 $b\in S$)에 대하여 $l\leq b$ 이므로, $L$ 은 bounded above 되어 있다.  <br> <br>

우리가 방금 얻어낸 중요한 정보는 총 3개이다.  <br>

1. $L\subset S$ 이다. <br>

2. $L$ 은 empty set 이 아니다. <br>

3. $L$ 은 bounded above 되어있다. <br> <br>

어디서 많이 본 조건들이 아닌가? 그렇다. 정의 1.10에서 우리는 이 조건들은 본적이 있고, $S$ 는 least-upper-bound-property (지금부터는 줄여서 LUBP 로도 부르겠다.) 를 지닌 ordered set 이므로, 정의에 따라 $sup L$ 은 $S$ 에 존재한다. 우리는 방금 $\alpha = \sup L \in S$ 임을 보인것이다! <br> <br>

이제 남은 과제는 $\alpha = \sup L = \inf B$ 임을 보이는 것이다. 그렇다면 이 Theorem 을 증명하는데 성공하게 된다. 말로 풀어서 말하면, $L$ 의 supremum 이 $B$ 의 infimum 이라는 것을 증명하기만 하면 모두 끝난다는 것이다. <br> <br>

여기서 우리는 귀류법을 사용할 것이다. 우선, 결론을 부정하여 $\alpha = \sup L \neq \inf B$ 라고 해보자. 정의 1.8에 따라서, $\alpha$ 가 $B$ 의 lower bound 가 아니 '거나' (OR), $\beta > \alpha$ 일 때 $\beta$ 가 $B$ 의 lower bound 인게 있다는 의미이다. <br> <br>

우선 첫 번째 조건을 살펴보자. $\alpha$ 가 $B$ 의 lower bound 가 아니라는 부분인데, 우리는 이 조건이 틀렸다는 것을 보여야 한다. $L$ 은 $B$ 의 모든 lower bound 들을 모아둔 집합이기 때문에 $\alpha \in L$ 라는 것만 보일 수 있다면 이 조건은 쉽게 틀렸다는 것을 보일 수 있다.
그렇다면 어떻게 $\alpha \in L$ 임을 보일 수 있을까? <br> <br>

여기서 우리는 supremum 의 정의를 한 번 더 살펴보도록 하겠다. 정의 1.8에 따라 $\gamma<\alpha$ 라면, $\gamma$ 는 $L$ 의 upper bound 가 아니게 되고, 이는 $\gamma \notin B$ 임을 의미한다. 거꾸로 말하면, $\gamma \in B$ 라면 $\gamma$ 가 $L$ 의 upper bound 이고, 이는 $\alpha \leq \gamma$ 임을 의미한다. 따라서, $\alpha$ 는 $B$ 의 lower bound 이고, $\alpha\in L$ 이다. 이로써 우리는 첫 번째 조건이 거짓임을 보였다. <br> <br>

그러나 두 조건이 'or'로 연결되어 있기 때문에 우리는 두 번째 조건인 $\beta > \alpha$ 일 때 $\beta$ 가 $B$ 의 lower bound 인게 있다는 조건이 거짓임을 한 번 더 보여야 한다. 여기서 귀류법을 한 번 더 사용하도록 하겠다. <br> <br>

그런 $\beta$ 가 존재한다고 가정하자. $\beta$ 는 $B$ 의 lower bound 이기 때문에 $\beta \in L$ 이고, $\alpha = \sup L$ 이기 때문에, 정의에 따라 $\beta \leq \alpha$ 이다. 이는 우리가 처음에 가정했던 $\beta > \alpha$ 라는 것과 모순된다. 따라서 두 번째 조건도 거짓임을 밝혔다. <br> <br>

두 조건이 모두 거짓임으로, 다시 말해 $\alpha$ 가 $B$ 의 lower bound 가 '아닌 것'도 아니고, $\beta > \alpha$ 일 때 $\beta$ 가 $B$ 의 lower bound 인게 없다. 이는 우리의 가정과 모순되므로,  $\alpha = \sup L = \inf B$ 이다. $\alpha = \sup L \in S$ 이고, $\alpha = \sup L = \inf B$ 이므로, 당연히 $\inf B \in S$ 이다. $\blacksquare$

</details>

이걸로 Ordered Set 단원이 마무리 되었다. 이 단원에서는 order, ordered set, bounded above, bounded below, upper bound, lower bound, least upper bound (supremum), greatest lower bound (infimum), least-upper-bound-property 가 무엇인지 정의하였고, least-upper-bound-property 를 가진 ordered set 은 greatest-lower-bound-property 또한 가짐을 증명하였다.
  
## 마무리

오늘 배운 내용들을 간단하게 요약해보겠다.

1. ordered set 을 정의하면서 기존의 최대값과 최솟값의 개념을 확장한 supermum 과 infimum 을 정의하였다.

2. ordered set 이 least-upper-bound property 를 만족한다는 것을 해당 집합의 아무 부분집합 (공집합이 아닌) 을 뽑아도 원래 집합 안에 해당 집합의 supremum 이 존재하는 것으로 정의하였다.

3. greatest-lower-bound property 를 가지는 것은 least-upper-bound property 를 가지는 것과 동일하다는 것을 증명하여, LUBP 만을 사용해도 된다는 것을 보였다.

다음 글에서 다룰 Fields 소단원에서는 실수체가 만족해야 하는 구조인 `field` 에 대해 자세하게 알아보도록 할 것이다. 다음 글의 초반 부분이 오늘 배운 ordered set 과 바로 연결되지는 않기 때문에 새로운 정의와 개념이 갑자기 튀어나와 당황스러울 수 있지만 차분하게 설명을 읽고 우리가 아는 그 실수체에서의 성질들을 떠올려 본다면 충분히 이해할 수 있을 것이다.

오늘의 해석학 공부는 여기까지!