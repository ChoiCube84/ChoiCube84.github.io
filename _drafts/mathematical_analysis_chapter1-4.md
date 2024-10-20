---
layout: post
title: 해석학 1단원 실수체와 복소수체 - 4
subtitle: '복소수체, 유클리드 공간'
date: '2023-08-04 23:59:00+0900'
background: /img/posts/2023/July/mathematical_analysis.jpg
katex: true
category: Study
tags:
  - mathematical_analysis
  - mathematical_analysis_game
---

# 해석학 공부하기

오늘도 해석학 공부를 시작하도록 하겠다. 1단원이 끝나가는게 점점 느껴지는 것 같다.

해석학이라는 과목이 왜 생겨났는지 알고있는가? 나도 정확하게는 잘 모르지만, 미분적분학이 등장하면서 극한의 개념이 나오게 되고, 이러한 추상적이고 애매한 개념을 제대로 정립하기 위해 생겨난 학문이 해석학이라고 들었다.

그런 만큼, 해석학이라는 과목은 정의와 탄탄한 논리를 기반으로 진행되는 학문이다. 해석학스러운 엄밀함이 없다면, 상식적으로도 말도 안되는 결과도 맞다고 우길 수 있게 되기 때문이다.

우리 세상에도 그런 일들이 종종 있지 않은가? 종종 당연하다고 생각했던 일들이 당연하지 않았다는 사실을 깨닫게 되기도 하고, 말도 안되는 일이 진실로 드러나는 일도 있다.

이런 혼란스러운 세상을 헤쳐나가기 위해서는 확실한 주관과 논리를 가지고 생각하고 행동해야 한다고 생각한다. 해석학을 공부하는 것은 사고방식에 있어, 이런 것들을 크게 도와줄 수 있을 것이라고 생각한다.

잡담은 여기까지 하고, 오늘은 복소수체에 대한 내용들을 배워보도록 하겠다. 그리고 유클리드 공간에 대해서도 알아보겠다.

## 지난 내용 복습

(WIP)

## The Complex Field

### 1.24 Definition

`complex number` (복소수) 는 실수들의 `ordered pair` (정렬된 쌍) $(a, b)$ 이다. "Ordered" 의 의미는 $a \neq b$ 일 때 $(a, b)$ 와 $(b, a)$ 를 서로 구분되는 것 (다른 것) 으로 간주한다는 의미이다.

$x = (a,b)$ 와 $y = (c,d)$ 를 두 복소수라고 하자. 우리는 $a = c$ 이고 $b = d$ 일 때만 $x = y$ 라고 쓴다. 동시에, $x = y$로 쓰는 것은 $a = c$ 이고 $b = d$ 임을 의미한다. (이 정의는 불필요하지 않다. 정수 두개로 표현되는 유리수에서 같다는 개념이 어떻게 정의되는지 생각해보라.)

우리는 이렇게 정의한다.

- $x + y = (a + c, b + d)$

- $xy = (ac - bd, ad + bc)$

#### 해설

우리는 방금 막 복소수를 정의하였다. 복소수는 실수 두 개의 쌍으로 정의되며, 순서가 바뀌면 $a$와 $b$가 같지 않은 이상 서로 다른 수가 된다. 이 정의를 보고 이상하다고 생각하는 사람들이 있을 수가 있다. 항상 보던 $i$는 어디로 간 것이며, $i=\sqrt{-1}$ 과같은 식도 보이지 않는다고 질문할 수 있다. 물론 이 내용은 뒤에서 나올 내용이니 걱정하지 않아도 좋다.

두 복소수가 같다는 것은 두 쌍의 왼쪽과 오른쪽이 각각 같아야 함을 의미한다. 거꾸로, 두 쌍의 왼쪽과 오른쪽이 각각 같으면 두 복소수가 같다고 할 수 있는 것이다. 유리수와는 조금 다른 경우인데, 유리수의 경우는 복소수와 비슷하게 두 개의 정수쌍으로 나타나게 되지만, $1/2=2/4$ 인 것처럼 반드시 왼쪽 요소들과 오른쪽 요소들이 각각 같아야 하는 것은 아니다. 하지만 복소수는 $(1, 2) \neq (2, 4)$인 것이다.

그 다음은 복소수에서의 덧셈과 곱셈을 정의하였는데, 덧셈의 정의는 직관적으로 이해할 수 있지만, 곱셈의 정의가 조금 이상하게 되어있는 것을 알 수 있다. 복소수에 대해 이미 알고 있던 사람들이라면 왜 곱셈이 저렇게 정의되는지 바로 이해할 수 있겠지만, 복소수가 무엇인지 모르는 사람들은 이상하게 보일 수 있다. 이는 뒤에서 이해가 될 것이다.

### 1.25 Theorem

위와 같은 덧셈과 곱셈의 정의는 모든 복소수들의 '집합' (set) 을 '체' (field) 가 되게 해준다. (이 때, $(0, 0)$ 과 $(1, 0)$ 을 $0$과 $1$로 둔다.)

#### 해설

정의 1.24와 같이 복소수를 정의하게 되면, 그런 복소수들을 모두 모아놓은 집합은 field를 형성한다는 것이 이 정리에서 말하고자 하는 내용이다. 증명은 다음과 같다.

<details>
<summary>증명</summary>

복소수들의 집합이 field를 구성한다는 것을 증명하기 위해서는, field의 정의가 무엇이었는지 다시 떠올려볼 필요가 있다. 간단하게 말하자면, '덧셈 연산' 과 '곱셈 연산' 이 정의된 어떤 집합의 각각 5개의 덧셈에 대한 공리와 곱셈에 대한 공리, 그리고 1개의 '분배 법칙' 을 만족할 때 우리는 이 집합을 field 라고 부를 수 있는 것이었다.
  
현시점에서 복소수를 모아놓은 집합에는 '덧셈 연산' 과 '곱셈 연산' 이 이미 정의되어 있으니 우리는 이 연산들이 총 11개의 공리들을 따르는지 확인하는 방식으로 이 복소수 '집합'이 복소수 '체'를 형성함을 증명할 것이다.
  
덧셈 공리:
 
$x = (a,b)$, $y = (c,d)$, $z = (e,f)$ 라고 두자.
  
(A1) $x+y=(a+c,b+d)$ 이고, $a+c$ 와 $b+d$는 모두 실수이기 때문에 $(a+c,b+d)$ 는 복소수가 되고, 당연히 복소수 집합에 포함된다. 따라서 이 공리는 성립한다.
  
(A2) $x+y=(a+c,b+d)=(c+a,d+b)=y+x$ 이므로, 이 공리는 성립한다.

(A3) $(x+y)+z=()$
 
(WIP)
 
</details>

### 1.26 Theorem

모든 실수 $a$ 와 $b$에 대하여 다음 식이 성립한다.

\\[(a, b) + (b, 0) = (a + b, 0), \qquad (a, 0)(b, 0) = (ab, 0)\\]

정리 1.26 은 $(a, 0)$ 형태의 복소수들은 대응하는 실수 $a$와 같은 연산 성질을 가진다는 것을 보여준다. 그러므로 우리는 $(a, 0)$와 $a$가 같다고 볼 수 있다. 이 동일성은 `complex field` (복소수체) 가 `real field` (실수체) 를 `subfield` 로 가진다는 것을 알 수 있다.

어떤 독자들은 우리가 정체불명의 $-1$의 제곱근에 대한 언급없이 복소수들을 정이ㅡ했다는 것을 눈치챘을 것이다. 이제 우리는 $(a, b)$와 같은 표기법이 더 익숙한 표기법인 $a + bi$ 와 동일하다는 것을 보일 것이다.

#### 해설

이 정리가 말하고자 하는 것은, 정의 1.24 에서처럼 연산을 정의한 상태로 실수에 대하여 연산을 적용하였을 때, 우리가 아는 실수끼리의 연산과의 결과가 나온다는 것을 이야기한다.

(WIP)

### 1.27 Definition

$i = (0,1)$.

#### 해설

이 정의는 우리가 $i$ 라는 기호를 $(0,1)$ 에 해당하는 복소수를 나타내는 문자로 사용할 것임을 약속하는 정의이다. 복소수에 대해 배운적인 있다면, 바로 그 $i$ 를 떠올리면 된다.

(WIP)

### 1.28 Theorem

$i^2=-1$.

#### 증명

$i^2=(0,1)(0,1)=(-1,0)=-1$.

#### 해설

이 정리는 정의 1.27 에서의 $i$ 를 두 번 곱했을 때 $-1$ 이 나온다는 것을 의미한다. 복소수에 대해 배운 적이 있는 사람들이라면, 제곱해서 $-1$ 이 되는 수로 $i$ 를 먼저 정의한 뒤, 그 이후에 일반적인 복소수의 형태를 정의하는 방식으로 배웠을 것이다.

여기서는 $i$ 라는 기호를 먼저 도입하는 대신, 어떤 복소수를 실수 두 개의 `ordered pair` 로 정의한 뒤, 연산 규칙을 정의하고, 그 다음에 $i$ 라는 기호를 도입했다. 이 정리는 `ordered pair` 로 정의된 복소수가 우리가 아는 그 복소수가 맞다는 걸 보여주는 좋은 사례이다.

증명은 연산 규칙을 그대로 사용한 것 뿐이니 차근히 읽어보면 바로 이해할 수 있을 것이다.

### 1.29 Theorem

만약 $a$ 와 $b$ 가 실수라면, $(a,b) = a + bi$ 이다. 

#### 증명

$a + bi = (a, 0) + (b, 0)(0, 1) = (a, 0) + (0, b) = (a, b).$

#### 해설

이 정리는 복소수가 $\text{(실수)} + (\text{실수} * i)$ 의 형태로 표현될 수 있다는 것을 의미한다. 복소수에 대해 이미 배운 사람들에게는 이러한 표현이 `ordered pair` 형태보다 더 익숙할 것이다.

(WIP)

### 1.30 Definition

만약 $a$, $b$ 가 실수이고 $z = a + bi$ 라면, 복소수 $\overline{z}=a-bi$ 는 $z$의 `conjugate` (켤레복소수) 라고 부른다. 수 $a$와 $b$는 각각 $z$의 `real part` (실수부) 와 `imaginary part` (허수부) 라고 부른다.

우리는 때때로 다음과 같이 쓰기도 한다.

$$a = \operatorname{Re}(z), \qquad b = \operatorname{Im}(z)$$

#### 해설

이 정의에서는 어떤 복소수의 허수 부분만 부호를 바꾼 값인 켤레 복소수의 개념을 정의하면서, 복소수의 실수부와 허수부를 표현하기 위한 기호를 정의한다. 앞으로 켤레 복소수는 다양한 연산들을 간편하게 진행하는데 도움을 줄 것이다.

### 1.31 Theorem

만약 $z$ 와 $w$ 가 복소수라면, 다음 내용들이 성립한다.

1. $\overline{z + w} = \overline{z} + \overline{w}$
2. $\overline{zw} = \overline{z} \cdot \overline{w}$
3. $z + \overline{z} = 2\operatorname{Re}(z), \ z - \overline{z} = 2i\operatorname{Im}(z)$
4. $z\overline{z}$ 는 실수이고 양수이다. ($z = 0$ 인 경우를 제외하고)

#### 증명

(a), (b), 그리고 (c)는 자명하다. (d)를 증명하기 위해서, 

#### 해설

이 정리에서는 Definition 1.30 에서 정의했던 켤레 복소수의 다양한 성질들을 

### 1.32 Definition

만약 $z$ 가 복소수라면, 그것의 `absolute value` (절댓값) $\vert z \vert$ 는 음수가 아닌 $z\overline{z}$ 의 제곱근이며 그것은 $\vert z \vert =(z\overline{z})^{1/2}$ 이다.

#### 해설

(작성중)

### 1.33 Theorem

$z$ 와 $w$ 가 복소수라고 하자. 그러면 다음 내용들이 성립한다.

1. $z = 0$ 이 아닌 한은 $\vert z \vert > 0$ 이다. 그리고 $\vert 0 \vert =0$ 이다.
2. $\vert \overline{z} \vert = \vert z \vert$
3. $\vert zw \vert = \vert z \vert \vert w \vert$
4. $\vert \operatorname{Re}z \vert \leq \vert z \vert$
5. $\vert z + w \vert \leq \vert z \vert + \vert w \vert$

#### 해설

(작성중)

### 1.34 Notation

만약 $x_1, x_2, \cdots, x_n$ 이 복소수라면, 우리는 다음과 같이 표기한다.

$$x_1 + x_2 + \cdots + x_n = \sum_{j=1}^{n}{x_j}$$

우리는 이 구역을 `Schwarz inequality` (슈바르츠 부등식)으로 알려진 중요한 부등식 하나를 다룬 뒤 마칠 것이다.

#### 해설

(작성중)

### 1.35 Theorem

만약 $a_1, \cdots, a_n$ 와 $b_1, \cdots, b_n$ 이 복소수들이라면, 다음 부등식이 성립한다.

$$\left|\sum_{j=1}^{n}{a_j \overline{b_j}}\right| \leq \sum_{j=1}^{n}{|a_j|^2} \sum_{j=1}^{n}{|b_j|^2}$$

#### 해설

(작성중)

## Euclidean Spaces

### 1.36 Definitions

모든 양의 정수 $k$ 에 대하여, 실수이면서 $\mathbf{x}$ 의 `coordinates` 라고 불리는 $x_1, \cdots, x_k$ 로 구성된

$$\textbf{x} = (x_1, x_2, \cdots, x_k)$$

와 같은 ordered $k$-tuples 의 집합을 $\mathbb{R}^k$ 라고 하자.

$\mathbb{R}^k$ 의 원소들은 특히 $k > 1$ 일 때 `points` (점들) 이나 `vector` (벡터) 라고 불린다. 우리는 이러한 벡터들을 **굵은 글씨**로 표기한다.

만약 $\textbf{y} = (y_1, \cdots, y_k)$ 이고 $\alpha$ 가 실수라면, 

$$
\begin{align*}
    \textbf{x} + \textbf{y} &= (x_1 + y_1, \cdots, x_k + y_k) \\
    \alpha \textbf{x} &= (\alpha x_1, \cdots, \alpha x_k)
\end{align*}
$$

이고, $\textbf{x} + \textbf{y} \in \mathbb{R}^k$ 이며 $\alpha \textbf{x} \in \mathbb{R}^k$ 이다. 이것은 벡터들의 `addition` (덧셈) 을 정의하며, 또한 '실수 (스칼라) 에 의한' 벡터의 `multiplication` (곱셈) 을 정의한다. 이 두 연산은 `commutative law` (교환 법칙), `associative law` (결합 법칙), 그리고 `distributive law` (분배 법칙) 을 만족시키며 (실수에서의 비슷한 법칙들을 고려해보면, 증명은 사소하다.) $\mathbb{R}^k$ 를 `vector space over the real field` 가 되도록 한다. $\mathbb{R}^k$ 의 `zero element` (때때로 `origin` 이나 `null vector` 로 불리는) 는 $\textbf{0}$ 으로, 모든 `coordinate` 들이 $0$인 것이다.

우리는 또한 $\textbf{x}$ 와 $\mathbf{y}$ 의 `inner product (내적)` (혹은 `scalar product (스칼라 곱)`) 라고 불리는 것을 다음과 같이 정의할 것이다.

\\[ \textbf{x} \cdot \textbf{y} = \sum_{i=1}^k x_i y_i \\]

그리고 $\textbf{x}$ 의 `norm` (노름) 을 다음과 같이 정의할 것이다.

\\[ \vert \textbf{x} \vert = (\textbf{x} \cdot \textbf{x})^{1/2} = \left(\sum_{i=1}^k x_i^2\right)^{1/2} \\]

이렇게 정의된 구조 (위에서 정의된 `inner product` 와 `norm` 이 포함된 `vector space` $\mathbb{R}^k$) 는 `euclidean k-space` 라고 불린다.

#### 해설

(작성중)

### 1.37 Theorem

$\mathbf x, \mathbf y, \mathbf z \in \mathbb R^k$ 와 실수 $\alpha$ 에 대하여 다음이 성립한다.

1. $|\mathbf x| \ge 0$
2. $|\mathbf x| = 0$ 이면 $\mathbf x = \mathbf 0$ 이고, 그 역도 성립한다.
3. $|\alpha \mathbf x| = |\alpha| |\mathbf x|$
4. $|\mathbf x \cdot \mathbf y| \le |\mathbf x| |\mathbf y|$
5. $|\mathbf x + \mathbf y| \le |\mathbf x| + |\mathbf y|$
6. $|\mathbf x - \mathbf z| \le |\mathbf x - \mathbf y| + |\mathbf y - \mathbf z|$

#### 해설

(작성중)

### 1.38 Remarks

정리 1.37의 1, 2, 6번 부분은 $\mathbb R^k$ 를 `metric space` (거리 공간) 으로 볼 수 있게 해준다. 

$\mathbb R^1$ 은 주로 'the line' (직선) 이나 'the real line' (실선) 으로 불린다. 비슷하게, $\mathbb R^2$ 은 'the plane' (평면) 이나 'the complex plane' (복소평면) 으로 불린다. 이 두 가지 경우에, `norm` (노름) 은 해당하는 실수나 복소수의 절댓값이다.

#### 해설

(WIP)

## 마무리

오늘 배운 내용들을 간단하게 요약해보겠다.

1. 

오늘의 해석학 공부는 여기까지!
