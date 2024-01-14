---
layout: post
title: "Rusty Surfers 프로젝트 개발일지 3일차"
subtitle: "부드러워진 레인 이동"
date: 2024-01-14 23:59:00+0900
background: '/img/posts/rusty_surfers/rusty_surfers_project_bg.png'
katex: true
category: Project
tags: [ rusty_surfers ]
---

# Rusty Surfers 프로젝트

오늘도 Rusty Surfers 프로젝트를 진행하며 새로 짜거나 수정한 코드들에 대해 정리해볼 것이다. Rusty Surfers 게임 코드들은 내 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 새로 추가된 코드와 기능

(WIP)

<details>
<summary>식 유도 과정</summary>

<br>

<h4>문자 정의</h4><br>

<h5>입력으로 주어지는 것</h5><br>

보드가 튀어오르는 최대 높이: $H$ <br>

보드가 점프한 뒤 되돌아오기까지 걸리는 프레임 수: $T$ <br><br>

<h5>알아내야 하는 것</h5><br>

보드가 점프를 시작할 때 초기 속도: $v_0$ <br>

중력 가속도: $g$ <br>

##### 식 전개에 필요한 것

시간에 따른 보드의 위치: $y(t) = \Sigma_{k=0}^t (v(k) \cdot 1)$

시간에 따른 보드의 $y$ 방향 속도: $v(t) = v_0 - gt$

##### 풀이

1. 보드가 되돌아오기까지의 변위는 $0$ 이므로, 다음 식이 성립한다.

    $$
    \begin{align*} 
        y(T) &= \Sigma_{t=0}^T (v(t) \cdot 1) = \Sigma_{t=0}^T v(t) \\
        &= \Sigma_{t=0}^T (v_0 - gt) = (T+1)v_0 - \frac{T(T+1)}{2}g = 0.
    \end{align*} 
    $$

2. $T+1 > 0$ 이므로 다음 식이 성립한다.

    $$
    \begin{align*}
        0 &= (T+1)v_0 - \frac{T(T+1)}{2}g \\
        0 &= v_0 - \frac{T}{2}g \\
        \frac{T}{2}g &= v_0 \\
        g &= \frac{2}{T} v_0
    \end{align*}
    $$

3. 보드가 최고점에 도달하는 조건은 $v(t) = 0$ 이며, 이 때 $t = \frac{T}{2}$ 이다. 따라서 다음 식이 성립한다.

    $$
    \begin{align*}
        y\left(\frac{T}{2}\right) &= \Sigma_{t=0}^{T/2} (v(t) \cdot 1) = \Sigma_{t=0}^{T/2} v(t) \\
        &= \Sigma_{t=0}^{T/2} (v_0 - gt) = \left(\frac{T}{2}+1\right)v_0 - \frac{\frac{T}{2}\left(\frac{T}{2}+1\right)}{2}g = H.
    \end{align*}
    $$

4. 2단계에서 얻은 결과를 3단계에서 얻은 식에 대입하면 다음과 같은 결과를 얻게 된다.

    $$
    \begin{align*}
        \left(\frac{T}{2}+1\right)v_0 - \frac{\frac{T}{2}\left(\frac{T}{2}+1\right)}{2}g &= H \\
        \left(\frac{T}{2}+1\right)v_0 - \frac{\frac{T}{2}\left(\frac{T}{2}+1\right)}{2}\left(\frac{2}{T} v_0\right) &= H \\
        \left(\frac{T}{2}+1\right)v_0 - \frac{\left(\frac{T}{2}+1\right)}{2} v_0 &= H \\
        \frac{\left(\frac{T}{2}+1\right)}{2} v_0 &= H \\
        \frac{T+2}{4} v_0 &= H \\
        v_0 &= \frac{4}{T+2} H
    \end{align*}
    $$

5. 최종적으로 다음 두 식을 얻게 된다.

$$v_0 = \frac{4}{T+2} H$$

$$g = \frac{2}{T} v_0$$


</details>

오늘 추가되거나 수정된 코드는 다음과 같다. 크게 수정된 부분들을 위주로 적어두었다.


### main.rs

```rust

```

### player.rs

```rust

```

## 마무리

(WIP)

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/rusty-surfers>  