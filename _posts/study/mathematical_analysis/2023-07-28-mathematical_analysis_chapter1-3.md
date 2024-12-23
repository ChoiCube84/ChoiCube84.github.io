---
layout: post
title: 해석학 1단원 실수체와 복소수체 - 3
subtitle: 체
date: '2023-07-28 23:59:00+0900'
background: /img/posts/2023/July/mathematical_analysis.jpg
katex: true
category: Study
tags:
  - mathematical_analysis
  - mathematical_analysis_game
---
# 해석학 공부하기

오늘도 해석학 공부를 시작하도록 하겠다.

우리는 일상에서 매일 매일 사칙연산을 사용한다. 수포자들 마저도 "어짜피 사는데는 사칙연산만 할 줄 알면 되지!" 라고 말하며 사칙연산의 중요성을 강조한다! 사칙연산을 일상에서 사용하는 예시로는 가게에서 물건을 살 때 덧셈과 곱셈을 통해 총 가격을 계산하고, 술 값을 정산할 때 나눗셈을 사용하기도 한다. 이런 연산들이 당연하게 느껴지지만, 사실 덧셈과 곱셈이 일정한 규칙을 가지고 일관되게 작동해야만 우리의 계산이 올바른 결과를 줄 수 있다.

이러한 사칙연산이 항상 잘 작동하도록 보장하는 구조가 바로 수학에서 말하는 `field` 이다. 오늘은 `field` 의 정의와 성질에 대해 먼저 알아보겠다. 그 후 `ordered set` 의 개념을 함께 활용하여 `ordered field` 의 정의와 성질에 대해 알아보겠다.

## 지난 내용 복습

지난 번에 배웠던 내용은 ordered set 과 least-upper-bound property 에 대한 내용이었다. 우리는 먼저 ordered set 을 정의하면서 기존의 최대값과 최솟값의 개념을 확장한 supermum 과 infimum 을 정의하였다. 그 다음 least-upper-bound property 에 대해 정의하면서 greatest-lower-bound property 를 가지는 것은 least-upper-bound property 를 가지는 것과 동일하다는 것을 증명하여, LUBP 만을 사용해도 된다는 것을 보였다.

## Fields

이 소단원에서는 field 가 무엇인지 정의할 것이다. field 라는 개념은 실수를 구성하는 또 다른 큰 부품중 하나로, 실수에서의 '사칙연산' 이 제대로 작동하게 하는 역할을 한다. 생각보다 내용이 많을 수 있으니 마음의 준비를 단단히 해두자!

### 1.12 Definition

`field` (체)는 아래의 "field axiom" 이라고 불리는 아래의 내용들을 만족하는 `addition` (덧셈)과 `multiplication` (곱셈)이라고 불리는 두 가지 연산이 정의된 어떤 집합 $F$이다.

1. Axioms for addition (덧셈 공리)

- (A1) 만약 $x \in F$이고 $y \in F$라면, 두 원소의 합인 $x+y$가 $F$에 존재한다.  

- (A2) 덧셈은 교환법칙이 성립한다: 모든 $x, y \in F$에 대하여 $x+y=y+x$ 이다.  

- (A3) 덧셈은 결합법칙이 성립한다: 모든 $x, y, z \in F$에 대하여 $(x+y)+z=x+(y+z)$ 이다.  

- (A4) $F$는 모든 $x \in F$에 대하여, $0+x=x$인 원소 $0$을 가진다.

- (A5) 모든 $x \in F$에 대하여 이에 대응하는 $-x \in F$가 존재하여 다음을 만족시킨다.

\\[x + (-x) = 0\\]

2. Axioms for multiplication (곱셈 공리)

- (M1) 만약 $x \in F$이고 $y \in F$라면, 두 원소의 곱인 $xy$가 $F$에 존재한다.  

- (M2) 곱셈은 교환법칙이 성립한다: 모든 $x, y \in F$에 대하여 $xy=yx$ 이다.  

- (M3) 곱셈은 결합법칙이 성립한다: 모든 $x, y, z \in F$에 대하여 $(xy)z=x(yz)$ 이다.  

- (M4) $F$는 모든 $x \in F$에 대하여, $1x=x$인 원소 $1$을 가진다. 이때, $1 \neq 0$ 이다.

- (M5) $0$을 제외한 모든 $x \in F$에 대하여 이에 대응하는 $1/x \in F$가 존재하여 다음을 만족시킨다.

\\[x \cdot (1/x) = 1\\]

3. The distributive law (분배 법칙)

모든 $x,y,z \in F$에 대하여 다음이 성립한다.

\\[x(y+z)=xy+yz\\]

#### 해설

이것이 `field`의 정의이다. 이런저런 이야기가 한꺼번에 쏟아져 나와서 헷갈릴 수 있지만, 걱정하지 않아도 된다. 지금부터 내용을 하나씩 설명해주도록 하겠다.

우선 더하기와 곱하기 자체는 우리에게 매우 익숙한 개념이다. 초, 중학교를 거치면서 거의 필수적으로 배우는 개념이기 때문이다. `field` 는 이러한 덧셈과 곱셈이 사용될 수 있어야 하는 것이다. 그것의 `field`의 정의의 '일부'이다.

우선 A1과 M1을 살펴보자. 이 두 가지 조건이 말하는 내용은 간단하다. `field` $F$에서 어떤 원소 두 개를 가져와서 덧셈이든 곱셈이든 연산을 하고 나면, 어떤 결과가 나와야 하는데, 그 결과가 $F$에 포함되어 있어야 한다는 의미이다. 좀 더 정확히 말하면, 그것이 덧셈과 곱셈이 정의되어야 하는 방식이다.

A2와 M2, A3와 M3도 우리에게 친숙한 개념이다. 두 수를 연산하는 과정에서 순서를 바꿔도 결과가 같아야 하고, 특정 연산을 두 번 반복해야 할 때 뭘 먼저하든 상관이 없게 정의되어야 한다는 뜻이다.

A4와 M4의 경우에는, 연산 결과 자기자신이 나오게하는 '0'과 '1'이 필요하다는 내용이다. 어떤 수에 0을 더하면 자기 자신이 나오고, 어떤 수에 1을 곱해도 자기 자신이 나온다는 건 대부분의 사람이 아는 내용일 것이다. 그런데 여기서 내 눈에 띈 부분이 하나 있었는데, 그건 **1이 0과 달라야 한다**는 부분이었다.

당연히 0과 1은 다른거 아니냐라고 생각할 수 있지만, 여기서의 0과 1은 우리가 아는 것과는 조금 다른 0과 1을 생각해야 한다. 기존에 늘 사용해오던 **정수 0과 1**이 아니라, **덧셈에서 0의 역할을 하는 무언가**와, **곱셈에서 1의 역할을 하는 무언가**로 생각을 해야한다.

애초에 우리가 하고있는 것은 `field`가 무엇인지를 정의하는 것이지, 유리수나 실수에서의 덧셈과 곱셈을 콕 집어 설명하고 있는 것이 아니다. `field`는 유리수체와 실수체 이외에도 여러 가지가 있기 때문에, 어떤 **'체가 아니면서 덧셈과 곱셈이 정의되는 집합'** 에서는 **덧셈에서 0의 역할을 하는 무언가**와, **곱셈에서 1의 역할을 하는 무언가**가 서로 일치하는 경우도 있다! 그렇게 때문에 굳이 $1 \neq 0$ 이어야 한다는 조건이 M4에 붇게되는 것이다.

마지막 A5와 M5는, 덧셈의 경우는 $F$에 포함된 모든 원소가, 곱셈의 경우에는 $0$을 제외한 $F$에 포함된 모든 원소가, 대응되는 '역원'이 $F$에 포함되어 있다는 것을 의미한다. 여기서 '역원'이라는 단어를 사용했는데, 쉽게 말하면 원래꺼랑 연산했을 때 '항등원'이 나오게 하는 것이다. 여기서 항등원은 A4와 M4에 대한 부분에서 이야기 했던 0과 1이다. 덧셈의 항등원은 연산결과 자기자신이 나오는 0, 그리고 곱셈의 항등원은 연산결과 자기자신이 나오는 1이다. 

마지막으로, The distributive law 부분은 분배법칙에 대한 이야기이다. 식을 보면 $x$가 $y+z$에 곱해져 있는데, 이 곱해지는 $x$가 $y$와 $z$에 각각 '분배' 되어 $xy+xz$가 되는 것을 확인할 수 있다. 이 또한 `field`의 덧셈과 곱셈이 정의되어야 하는 방식이다.

이걸로 `field`의 정의에 대한 설명을 완료하였다. 요약하자면, `field`는 덧셈과 곱셈이 합쳐서 총 11개의 공리를 따르도록 정의가 된 집합을 의미하는 것이다.

### 1.13 Remarks

- (a) 임의의 field 에서

\\[x+(-y), x\cdot\left(\frac{1}{y}\right), (x+y)+z, (xy)z, xx, xxx, x+x, x+x+x, \cdots\\]

와 같은 것들을 아래의

\\[x-y, \frac{x}{y}, x+y+z, xyz, x^2, x^3, 2x, 3x, \cdots\\]

와 같이 쓸 수 있다.

- (b) 덧셈과 곱셈이 흔히 가지는 의미대로 정의된다면, 모든 유리수의 집합인 $\mathbb{Q}$ 은 field 의 공리들을 만족한다. 따라서, $\mathbb{Q}$ 는 field 이다.

- (c) 우리의 목적이 field (나 다른 대수적 구조들) 을 자세하게 공부하는 것은 아니지만, $\mathbb{Q}$ 에서 성립하는 친숙한 성질들이 field 의 공리들의 결과라는 것은 증명할 만한 가치가 있다. 한 번 하고 나면, 똑같은 증명을 실수들과 복소수들에 대해서 또 할 필요가 없을 것이다.

#### 해설

이 Remark 에서는 우선 식의 기호를 표기하는 댜앙한 방식을 설명하고 있다. 새로이 등장한 표기 방법이 더 보기 편하고 익숙할 것이기 때문에 앞으로 계속 사용할 것이다.

그 다음에는 $\mathbb{Q}$ 가 field임을 설명하는 부분인데, 이를 정의를 이용하여 직접 보이지는 않고 된다고만 하면서 넘어간다. 간단하게 증명하고 넘어가겠다.

<details>
<summary>증명</summary>

<br>
  
증명에 앞서 두 유리수를 $p = \frac{a}{b}$, $q = \frac{c}{d}$, $r = \frac{e}{f}$ 로 표기하겠다. (이 때 $b$, $d$, $f$ 는 $0$ 이 아니다.) <br> <br> 
  
- (A1), (M1): 두 유리수를 더하거나 곱한 건 당연히 유리수가 된다. $p + q = \frac{a}{b} + \frac{c}{d} = \frac{ad+bc}{bd}$ 이고, $\frac{ad+bc}{bd}$ 는 당연히 유리수이다. $ad+bc$ 가 정수이고 $bd$ 가 $0$ 이 아닌 정수이기 때문이다. 마찬가지로, $pq = \frac{a}{b}\frac{c}{d} = \frac{ac}{bd}$ 도 당연히 유리수이다. $ac$ 가 정수이고, $bd$ 가 $0$ 이 아닌 정수이기 때문이다. <br> <br>
  
- (A2), (M2): $p + q = \frac{a}{b} + \frac{c}{d} = \frac{ad+bc}{bd} = \frac{bc+ad}{db} = \frac{c}{d} + \frac{a}{b} = q + p$ 이고, $pq = \frac{a}{b} \cdot \frac{c}{d} = \frac{ac}{bd} = \frac{ca}{db} = qp$ 이다. 따라서 $\mathbb{Q}$ 에서는 덧셈과 곱셈 모두 교환 법칙이 성립한다. <br> <br>

- (A3), (M3): $(p + q) + r = \frac{ad+bc}{bd} + \frac{e}{f} = \frac{adf + bcf + bde}{bdf} = \frac{a}{b} + \frac{cf + de}{df} = p + (q + r)$ 이고, $(pq)r = \frac{ac}{bd} \cdot \frac{e}{f} = \frac{ace}{bdf} = \frac{a}{b} \cdot \frac{ce}{df} = p(qr)$ 이다. 따라서 $\mathbb{Q}$ 에서는 덧셈과 곱셈 모두 결합 법칙이 성립한다.  <br> <br>
  
- (A4), (M4): $\frac{0}{1} + p = \frac{0}{1} + \frac{a}{b} = \frac{0}{b} + \frac{a}{b} = \frac{0+a}{b} = \frac{a}{b} = p$ 이고, $\frac{1}{1} \cdot p = \frac{1}{1} \cdot \frac{a}{b} = \frac{1 \cdot a}{1 \cdot b} = \frac{a}{b} = p$ 이다. $\frac{0}{1}$ 과 $\frac{1}{1}$ 은 모두 유리수 이므로, $\mathbb{Q}$ 는 덧셈과 곱셈에 대해서 모두 항등원을 갖는다. <br> <br>
  
- (A5), (M5): $-p = \frac{-a}{b}$ 라고 두면, $p + (-p) = \frac{a}{b} + \frac{-a}{b} = \frac{0}{b} = 0$ 이고, $p$ 가 $0$ 이 아닐 때 $\frac{1}{p} = \frac{b}{a}$ 라고 두면, $p \cdot \frac{1}{p} = \frac{a}{b} \cdot \frac{b}{a} = \frac{ab}{ab} = 1$ 이다. 따라서 $\mathbb{Q}$ 는 모든 원소에 대해 덧셈과 곱셈의 역원을 가진다. (물론 0의 곱셈에서의 역원은 빼고.) <br> <br>
  
- The distributive law (분배 법칙): $x(y+z) = \frac{a}{b}(\frac{c}{d}+\frac{e}{f}) = \frac{a}{b} \cdot \frac{cf + de}{df} = \frac{a(cf + de)}{bdf} = \frac{acf + ade}{bdf} = \frac{acf}{bdf} + \frac{ade}{bdf} = \frac{a}{b} \cdot \frac{cf}{df} + \frac{a}{b} \cdot \frac{de}{df} = \frac{a}{b} \cdot \frac{c}{d} + \frac{a}{b} \cdot \frac{e}{f} = xy + xz$ 이므로, $\mathbb{Q}$ 의 덧셈과 곱셈은 분배 법칙을 만족한다. $\blacksquare$
  
</details>

마지막 부분은 $\mathbb{Q}$의 친숙한 성질들이 field 의 공리의 결과임을 증명할만한 가치가 있다고 언급하면서, 한 번 해두면 실수와 복소수에 대해서 다시 할 필요는 없을거라고 말한다. 언급된 '친숙한 성질'들에 대한 증명은 이어지는 Proposition 1.14 부터 다루기 시작할 것이다.

### 1.14 Proposition

덧셈의 공리는 아래의 문장들을 암시한다.

- (a) $x+y=x+z$ 이면, $y=z$ 이다.

- (b) $x+y=x$ 이면, $y=0$ 이다.

- (c) $x+y=0$ 이면, $y=-x$ 이다.

- (d) $(-x)+x=0$ 이다.

#### 해설

덧셈의 공리들을 이용하여 식들을 이리저리 잘 조작하면 저런 결과들을 얻어낼 수 있다는 것을 의미한다. 우선 (a) 먼저 증명하고 나머지 명제들이 (a)를 이용하여 쉽게 증명될 수 있다는 것을 보이겠다.

<details>
<summary>증명</summary>

<br>
  
1번 문장 증명: <br> <br> 

1. $y = 0 + y$ 이다. (A4)
2. $0 + y = (-x + x) + y$ 이다. (A5)
3. $(-x + x) + y = -x + (x + y)$ 이다. (A3)
4. $-x + (x + y) = -x + (x + z)$ 이다. (문장의 전제 조건)
5. $-x + (x + z) = (-x + x) + z$ 이다. (A3)
6. $(-x + x) + z = 0 + z$ 이다. (A5)
7. $0 + z = z$ 이다. (A4)

이 등식들을 쭉 옆으로 이어보면 $y=z$라는 결론에 도달하여 이 문장은 증명된다. $\blacksquare$ <br> <br>

이제 (b)의 증명은 (a)에서 $z$ 를 $0$ 으로 설정하면 되고, (c)의 증명은 $z$ 를 $-x$ 로 설정하면 된다. (d)의 증명은 (c)에서 $x$ 를 $-x$ 로 설정하면 된다.
  
</details>

<br>

### 1.15 Proposition

곱셈의 공리는 아래의 문장들을 암시한다.

- (a) $x \neq 0$ 이고, $xy=xz$ 이면, $y=z$이다.

- (b) $x \neq 0$ 이고, $xy=x$ 이면, $y=1$이다.

- (c) $x \neq 0$ 이고, $xy=1$ 이면, $y=1/x$이다.

- (d) $x \neq 0$ 이면, $1/(1/x)=x$ 이다.

#### 해설

덧셈과 마찬가지로 곱셈의 공리들을 이용하여 식들을 이리저리 잘 조작하면 저런 결과들을 얻어낼 수 있다는 것을 의미한다. 한 가지 눈에 띄는 부분은, 4개의 문장에 모두 $x \neq 0$ 이라는 전제 조건이 붙는데, 이는 증명 과정에서 (M5)의 조건을 자세히 살펴보면 이해할 수 있을 것이다. 이 명제들의 증명은 연습문제 풀이에서 다루게 될 것이다.

### 1.16 Proposition

field의 공리는 모든 $x,y,z \in F$ 에 대하여 아래 문장들을 암시한다.

- (a) $0x=0$ 이다.

- (b) $x \neq 0$ 이고, $y \neq 0$ 이면, $xy \neq 0$이다.

- (c) $(-x)y=-(xy)=x(-y)$ 이다.

- (d)$(-x)(-y)=xy$ 이다.

#### 해설

눈치챈 사람도 있겠지만, 자세히 보면 특정 연산의 공리라고 표현하지 않고 **field의 공리**라고 적혀있다. 그렇다. 이 Proposition은 덧셈과 곱셈이 모두 따라야 하는 공리인 분배 법칙을 기반으로 얻어지는 성질인 것이다. 각 명제들의 증명은 다음과 같다.
 
<details>
<summary>증명</summary>

<br>
  
우선 (a) 명제를 증명할 것이다. $0x + 0x = (0+0)x = 0x$ 를 만족하는데, Proposition 1.14 (b)에 따르면 $0x=0$ 을 만족해야 함을 알 수 있다. Proposition 1.14 (b) 에서 $x$ 와 $y$ 를 모두 $0x$ 로 두면 되는 것이다. <br> <br>
  
이제 (a) 명제의 내용을 기반으로 (b)를 증명할 수 있는데, 우리는 귀류법을 이용할 것이다. (b) 명제의 결론을 부정하여, $x \neq 0$ 이고 $y \neq 0$ 임에도 불구하고 $xy = 0$ 이라고 가정하자. 그러면, $1=\left(\frac{1}{y}\right)\left(\frac{1}{x}\right)xy=\left(\frac{1}{y}\right)\left(\frac{1}{x}\right)0$ 인데, $\left(\frac{1}{y}\right)\left(\frac{1}{x}\right)$ 는 $F$ 에 속하므로 (a)에 따라 $1=\left(\frac{1}{y}\right)\left(\frac{1}{x}\right)0=0$ 이다. 당연히 $1=0$ 일 수 없으므로 모순이 발생하고, (b)가 참임을 알 수 있다. <br> <br>
  
이제 (c) 명제를 증명할 것이데, 우선 다음 등식을 살펴보자. $(-x)y+xy=(-x+x)y=0y=0$ 이 성립하는데, Proposition 1.14 (c) 의 결과를 이용하면 $(-x)y=-(xy)$ 가 성립함을 알 수 있다. ($x$ 자리에 $xy$ 를, $y$ 자리에 $(-x)y$ 를 넣으면 된다.) 이번에는 다음 등식을 살펴보자. $x(-y)+xy=x(-y+y)=x0=0x=0$ 가 성립하는데, 이번에도 Proposition 1.14 (c)의 결과를 이용하면 $x(-y)=-(xy)$ 가 성립함을 알 수 있다. ($x$ 자리에 $xy$ 를, $y$ 자리에 $x(-y)$ 를 넣으면 된다.) 얻어낸 2개의 결과를 합치면 $(-x)y=-(xy)=x(-y)$ 가 성립함을 확인할 수 있다. <br> <br>
  
마지막으로, (d) 명제의 증명은 (c) 와 Proposition 1.14 (d) 의 결과를 이용하여 진행할 것이다. 우선 (c) 의 결과에 따라 $(-x)(-y)=-[x(-y)]=-[-(xy)]$ 가 성립하고, Proposition 1.14 (d) 의 결과에 따라 $-[-(xy)]=xy$ 가 성립한다. ($x$ 자리에 $xy$ 를 넣으면 된다.) 이로써 Proposition 1.16의 4개의 명제의 증명이 모두 끝났다! $\blacksquare$
  
</details>

여기까지 어떤 field 가 만족하는 다양한 성질들을 알아보았다. Definition 1.17 에서는 우리가 이제까지 배운 ordered set의 개념과 field 의 개념을 이용하여, `ordered field` 에 대해 알아볼 것이다.
  
### 1.17 Definition

`ordered field`는 어떤 field $F$가 ordered set이기도 하면서 다음 조건을 만족하는 것이다.

1. $x,y,z \in F$ 이고 $y < z$ 이면, $x + y < x + z$ 이다.
2. $x,y \in F$ 이고 $x>0$, $y>0$ 이면, $xy > 0$ 이다.

만약 $x>0$ 이면 $x$가 `positive` 하다고 부르고, $x < 0$ 이면, `negative` 하다고 부른다.

예를 들면, $\mathbb{Q}$ 는 `ordered field` 이다.

모든 `ordered field` 에서는 부등식을 이용할 때 적용되는 모든 친숙한 규칙들을 적용할 수 있다: 양수 \[음수\] 로 곱하는 것은 부등호의 방향을 유지 \[역전\] 시키고, 어떤 제곱수도 음수가 아닌 것 등등. 이어지는 Proposition 1.18 에서는 이것들의 몇 가지를 보여준다.

#### 해설

드디어 실수을 만들기 위한 큰 발걸음을 내딛었다. 어제 배웠던 ordered set 의 개념과, 오늘 배운 field 의 개념을 2가지 조건을 끼워넣어 조합한 결과, `ordered field` 라는 새로운 개념을 정의하게 되었다.

이름이 암시하는 것 처럼, `ordered field` 는 order 가 부여된 field 이다. 물론 단순히 field인 동시에 ordered set 이기만 하면 되는 것은 아니고, 2가지 조건이 더 붙어야 한다. 그 두 조건을 자세히 살펴보면, 부등식의 양쪽에 같은 값을 더해도 관게가 유지되어야 한다는 성질 하나와, $0$ 보다 큰 두 원소의 곱셉결과도 $0$ 보다 더 커야한다는 조건이다.

여기서 한 가지 의문이 들 수 있는데, 왜 양변에 같은 수를 곱하는 경우에 대해서는 여기서 다루지 않은 것일까? 그것은 다음에 나올 Propoisition 1.18 에서 다루게 된다. 즉, 양변에 같은 수를 곱했을 때 일어나는 일은 `ordered field`의 '정의' 가 아니라, '성질' 이 되는 것이다.

### 1.18 Proposition

아래의 문장들은 모든 ordered field에서 참이다.

- (a) $x>0$ 이면 $-x<0$ 이고, 거꾸로도 성립한다. ($-x<0$ 이면 $x>0$ 이기도 하다.)

- (b) $x>0$ 이고 $y<z$ 이면, $xy<xz$ 이다.

- (c) $x<0$ 이고 $y<z$ 이면, $xy>xz$ 이다.

- (d) $x \neq 0$ 이면 $x^2>0$ 이다. 특히, $1>0$ 이다. (여기서 $x^2$은 $xx$의 다른 표기법이다.)

- (e) $0<x<y$ 이면, $0<1/y<1/x$ 이다.

#### 해설

이 Proposition 에서는 ordered field가 지니는 특성에 대하여 이야기 한다. Definition 1.17 에서는 보이지 않았던 양변에 같은 수를 곱하는 경우에 대해서도 다루어준다. 각 명제의 증명은 다음과 같다.

<details>
<summary>증명</summary>

<br>
  
- (a) 만약 $x > 0$ 이면, $0 = -x + x > -x + 0 = -x$ 이므로, $0 > -x$, 즉 $-x < 0$ 가 성립한다. 만약 $x < 0$ 이라면, $0 = -x + x < -x + 0 = -x$ 이므로, $-x > 0$ 이다. 이로써 (a) 가 증명되었다. <br> <br>
  
- (b) $z > y$ 이므로, $z - y > y - y = 0$ 이 성립하는데, $x > 0$ 이므로 $x(z - y) > 0$ 이다. 따라서, $xz = x(z - y) + xy > 0 + xy = xy$ 이다. <br> <br>
  
- (c) (b) 에서와 마찬가지로 $z > y$ 이므로 $z - y > 0$ 인데, (a) 에 의하여 $-x > 0$ 이므로, $(-x)(z - y) > 0$ 이다. 따라서 $x(z - y) < 0$ 이고, $xz = x(z - y) + xy < 0 + xy = xy$ 이다. <br> <br>
  
- (d) $x > 0$ 이면, $x \cdot x = x^2 > 0$ 이 성립한다. $x < 0$ 이면, $-x > 0$ 이므로, Proposition 1.16 (d) 에 의하여 $(-x)(-x) = x \cdot x = x^2 > 0$ 이다. $1 > 0$ 의 증명은 간단하다. $1^2 = 1$ 이 성립하므로, $1 = 1^2 > 0$ 이다. <br> <br>
  
- (e) $y \cdot (1/y) = 1 > 0$ 인데, $y > 0$ 이므로, $(1/y) > 0$ 이다. (만약 $(1/y) \le 0$ 이었다면 $y > 0$ 이므로 $y \cdot (1/y) \le 0$ 이어야 하는데, 이는 (d) 의 결과와 모순된다.) 따라서, $1/y > 0$ 이다. 비슷하게 $1/x > 0$ 도 성립한다. 이제 $x < y$ 식의 양변에 $(1/x)(1/y)$ 를 곱하면, $1/y < 1/x$ 라는 결과를 얻게되고, 기존의 결과들과 합쳐 $0 < 1/y < 1/x$ 라는 식을 얻을 수 있게된다. $\blacksquare$

</details>

여기까지 `ordered field` 가 만족하는 다양한 성질들에 대해 알아보았다. 전반적으로 양수나 음수들을 곱했을 때 부호가 어떻게 변하는가에 대한 내용을 다루었고, 이것들은 모두 Definition 1.17 의 내용들만 이용하여 증명하였다. 오늘 다룰 내용은 여기까지고, 이제 배운 내용들을 정리해볼 것 이다.

## 마무리

오늘 배운 내용들을 간단하게 요약해보겠다.

1. field 를 정의하면서 'addition'과 'multiplication'이 따라야 하는 공리들이 무엇인지 알아보았다.

2. 이러한 공리들로부터 파생되는 다양한 성질들에 대해 알아보았다.

3. field 의 개념과 ordered set 의 개념을 활용하여 두 가지 제약조건을 추가로 넣으면서 ordered field 를 정의하였고, 정의된 ordered field 의 성질들을 알아보았다.

다음 글에서는 실수체를 정의해보고, 실수체에서 성립하는 다양한 성질들에 대해 알아보겠다. 또한, 실수체를 살짝 변형한 확장 실수계에 대해 알아보겠다.

오늘의 해석학 공부는 여기까지!
