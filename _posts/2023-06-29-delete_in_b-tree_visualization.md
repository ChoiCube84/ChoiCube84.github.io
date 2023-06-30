---
layout: post
title: "B-Tree에 대하여 - 5"
subtitle: "B-Tree에서 키를 빼는 법: 그림을 통해 알아보기"
date: 2023-06-29 23:58:00+0900
background: '/img/posts/2023/June/B-Tree.png'
katex: true
category: Project
---

# B-Tree 프로젝트

오늘은 지난번에 이어서 B-Tree에서 키값을 제거하는 Delete에 대해서 이야기 하도록 하겠다. 지난번에 하지 못했던 그림을 통한 B-Tree에서 Delete가 진행되는 과정에 대한 설명을 진행할 것이다.

## B-Tree의 Delete 과정 간단 복습

### 1. 리프 노드에서의 삭제

1. 삭제할 키값을 Search 한다.

2. 그 값이 리프 노드에 있을 경우, 그 노드에서 해당 키값을 제거한다.

3. 만약 언더플로우가 발생하면, 트리를 리밸런싱한다.

### 2. 내부 노드에서의 삭제

1. 왼쪽 subtree의 가장 큰 요소나 오른쪽 subtree의 가장 작은 요소 중 하나를 새로운 separator로 선택하고, 선택된 그 요소를 원래 있던 리프노드에서 제거한 뒤, 기존의 separator를 대체한다.

2. 1번에서 기존의 separator를 대체하던 중 리프 노드에서 새로운 separator가 삭제되었다. 이 때 해당 리프 노드가 가져야 할 최소 요소의 개수보다 적은 요소를 가지게 되면, 그 리프 노드에서 부터 리밸런싱을 시작한다.

## 예시

지금부터 order가 3인 이 B-Tree로 예를 들어 그림을 통해 B-Tree에서 삭제가 일어나는 과정을 설명해보겠다.

<img src="/img/posts/2023/June/29/b-tree_1.png" width="100%" height="100%" alt="26개의 원소를 가진 B Tree">

### 1. B-Tree에서 4, 19, 22, 25 제거

우선 이 B-Tree에서 4와 19, 22, 25를 제거해보겠다. 이 세 키값들은 모두 리프 노드에 있으며, 각각의 키값을 제거하더라도 각각의 리프노드에서 가져야할 최소 개수의 키값은 가지고 있게 된다. 그러므로, 삭제 이후 리밸런싱을 진행하지 않아도 된다.

<img src="/img/posts/2023/June/29/b-tree_2.png" width="100%" height="100%" alt="22개의 원소를 가진 B Tree">

### 2. B-Tree에서 5 제거

이번에는 이 트리에서 5를 제거해보겠다. 우선 5는 리프 노드에 있으므로 바로 제거한다.

<img src="/img/posts/2023/June/29/b-tree_3.png" width="100%" height="100%" alt="리프 노드에서 5가 제거된 B Tree">

이 때 방금 5가 제거된 리프 노드는 가져야 할 최소 키값의 개수인 1개보다 적은 0개의 키값을 가지고 있다. 따라서 이 트리에서는 리밸런싱을 진행해야 한다.

현재 이 트리에서 리밸런싱을 진행하려는 리프 노드의 형제 노드들 중 오른쪽에 있는 노드에 충분한 개수의 키값이 있다는 것을 확인할 수 있다. 따라서, 왼쪽 방향으로 '회전'을 시킬 수 있다.

<img src="/img/posts/2023/June/29/b-tree_4.png" width="100%" height="100%" alt="왼쪽으로 회전">

<img src="/img/posts/2023/June/29/b-tree_5.png" width="100%" height="100%" alt="B-Tree에서 5 제거 완료">

### 3. B-Tree에서 23 제거

이번에는 이 B-Tree에서 23을 제거해보겠다. 우선 23는 리프 노드에 있으므로 바로 제거한다.

<img src="/img/posts/2023/June/29/b-tree_6.png" width="100%" height="100%" alt="리프 노드에서 23이 제거된 B Tree">

이 때 방금 23이 제거된 리프 노드는 가져야 할 최소 키값의 개수인 1개보다 적은 0개의 키값을 가지고 있다. 따라서 이 트리에서는 리밸런싱을 진행해야 한다.

현재 이 트리에서 리밸런싱을 진행하려는 리프 노드의 양쪽 형제 노드 모두 회전을 진행하게 되면 최소로 가져야할 개수보다 적어지는 상황이다. 따라서 다음과 같은 과정을 따라 리밸런싱을 진행해야 한다.

1. separator인 21을 왼쪽 형제 노드의 끝에 삽입한다.

    <img src="/img/posts/2023/June/29/b-tree_7.png" width="100%" height="100%" alt="리프 노드에서 separator인 21이 삽입된 B Tree">

2. 23이 제거된 리프 노드에 있는 모든 원소들을 왼쪽 노드에 삽입한다. 이 때 이 리프 노드에는 이미 어떠한 원소도 존재하지 않으므로 변화가 일어나지는 않는다.

3. 부모 노드의 separator와 함께 텅 빈 그 리프 노드를 제거한다. 이 때, 부모 노드의 키값이 하나 감소하게 되고, 최소 필요 개수보다 적어지지는 않으므로 과정이 종료된다.

    <img src="/img/posts/2023/June/29/b-tree_8.png" width="100%" height="100%" alt="B-Tree에서 23 제거 완료">

### 4. B-Tree에서 20, 24 제거

이번에는 이 B-Tree에서 20, 24을 순서대로 제거해보겠다. 우선 20는 리프 노드에 있으므로 바로 제거한다.

<img src="/img/posts/2023/June/29/b-tree_9.png" width="100%" height="100%" alt="리프 노드에서 20이 제거된 B Tree">

다음은 24를 제거해보겠다. 24는 리프 노드에 있으므로 바로 제거한다.

<img src="/img/posts/2023/June/29/b-tree_10.png" width="100%" height="100%" alt="리프 노드에서 24가 제거된 B Tree">

이 때 방금 24가 제거된 내부 노드는 가져야 할 최소 키값의 개수인 1개보다 적은 0개의 키값을 가지고 있다. 따라서 이 트리에서는 리밸런싱을 진행해야 한다.

리밸런싱을 진행하려는 노드는 내부 노드이므로, 왼쪽 subtree의 가장 큰 요소인 21을 새로운 separator로 선택하고, 선택된 그 요소를 원래 있던 리프노드에서 제거한 뒤, 기존의 separator였던 24을 대체한다.

<img src="/img/posts/2023/June/29/b-tree_11.png" width="100%" height="100%" alt="왼쪽 subtree에서 가장 큰 키값 선택">

<img src="/img/posts/2023/June/29/b-tree_12.png" width="100%" height="100%" alt="separator 대체 완료">

이 때, separator를 대체하는 과정에서 한 리프 노드가 가져야 할 최소 개수보다 적은 수의 키값을 가지게 되는 것을 확인할 수 있다. 따라서, 리밸런싱을 진행해주어야 한다.

현재 이 트리에서 리밸런싱을 진행하려는 리프 노드의 양쪽 형제 노드 모두 회전을 진행하게 되면 최소로 가져야할 개수보다 적어지는 상황이다. 따라서 다음과 같은 과정을 따라 리밸런싱을 진행해야 한다.

1. separator인 21을 리프 노드의 끝에 삽입한다.

    <img src="/img/posts/2023/June/29/b-tree_13.png" width="100%" height="100%" alt="리프 노드에서 separator인 21이 삽입된 B Tree">

2. 오른쪽 형제 리프 노드에 있는 모든 원소들을 왼쪽 노드에 삽입한다.

    <img src="/img/posts/2023/June/29/b-tree_14.png" width="100%" height="100%" alt="형제 리프 노드에서 26을 가져옴">

3. 부모 노드의 separator와 함께 오른쪽 리프 노드를 제거한다. 이 때, 부모 노드의 키값이 하나 감소하게 되고, 최소 필요 개수보다 적어지므로 다시 리밸런싱이 일어나야 한다.

    <img src="/img/posts/2023/June/29/b-tree_15.png" width="100%" height="100%" alt="B-Tree에서 24 제거 후 리밸런싱 재필요">

이제 이 부모 노드의 왼쪽 형제 노드에 separator인 18을 삽입한다.

<img src="/img/posts/2023/June/29/b-tree_16.png" width="100%" height="100%" alt="separator 18 삽입">

이제 텅 빈 오른쪽 노드를 왼쪽 노드와 병합한다. 그리고 separator와 합병된 노드를 삭제한다.

<img src="/img/posts/2023/June/29/b-tree_17.png" width="100%" height="100%" alt="노드 병합 후 separator 및 오른쪽 노드 삭제">

이 때, 병합된 노드가 가질 수 있는 최대 키값 개수보다 더 많은 개수의 키값을 가지고 있으므로, 노드 분할을 진행하고, 15를 승격시키면 삭제 작업이 마무리 된다.

<img src="/img/posts/2023/June/29/b-tree_18.png" width="100%" height="100%" alt="24 삭제 작업 완료">

## 마무리

이로써 B-Tree에 대한 이론적인 설명을 모두 마치도록 하겠다! 내일은 이제까지 작성한 코드들을 리뷰하는 시간을 가져보도록 하겠다. 

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://en.wikipedia.org/wiki/B-tree>  