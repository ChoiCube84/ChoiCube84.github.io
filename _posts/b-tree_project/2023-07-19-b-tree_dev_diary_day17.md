---
layout: post
title: "B-Tree 프로젝트 개발일지 17일차"
subtitle: "Debugging"
date: 2023-07-19 23:44:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BKeyList::isExistingKey` 함수 수정:

    기존의 키가 존재하는지 확인하는 이 함수에서는 매개변수로 들어온 key 값과, `findIndex` 함수로 찾아낸 인덱스에서의 값이 일치하는지 확인하는 방식으로 작동했다.
    
    그러나 `findIndex` 함수에서 반환하는 인덱스가 자식 노드를 찾아가는데에도 사용될 수 있어 범위를 넘어갈 수 있고, 이러한 문제를 해결하기 위해서 indexOfKey 값이 인덱스 범위를 벗어나지 않는지 먼저 검사하도록 수정하였다.

    기존 코드:
    ```cpp
    bool isExistingKey(const T& key) {
		size_t indexOfKey = findIndex(key);
		return (key == getKeyByIndex(indexOfKey));
	}
    ```

    새 코드:
    ```cpp
    bool isExistingKey(const T& key) {
		size_t indexOfKey = findIndex(key);

		// TODO: Revise this statement
		if (indexOfKey < currentSize) {
			return (key == getKeyByIndex(indexOfKey));
		}
		else {
			return false;
		}
	}
    ```

2. `BNode::rebalance` 함수 수정:

    기존의 리밸런싱을 진행하는 함수에서 리프 노드가 아닌 경우의 노드에서 리밸런싱을 진행할 때의 조건을 추가하였다.

    기존 코드:
    ```cpp
    void rebalance(void) {
		BNode* leftSibling = getLeftSibling();
		BNode* rightSibling = getRightSibling();

        if (leftSibling != nullptr && leftSibling->isExceedingNode()) {
			rightRotation();
		}
		else if (rightSibling != nullptr && rightSibling->isExceedingNode()) {
			leftRotation();
		}
		else {
			mergeWithSiblingNode();
		}
	}
    ```

    새 코드:
    ```cpp
    void rebalance(void) {
		BNode* leftSibling = getLeftSibling();
		BNode* rightSibling = getRightSibling();

		// TODO: Check if checking if it is leaf node is neccessary

		if (!isLeaf && isEmpty()) {
			mergeWithSiblingNode();
		}
		else if (leftSibling != nullptr && leftSibling->isExceedingNode()) {
			rightRotation();
		}
		else if (rightSibling != nullptr && rightSibling->isExceedingNode()) {
			leftRotation();
		}
		else {
			mergeWithSiblingNode();
		}
	}
    ```

3. `BNode::mergeTwoChildrenNode` 함수 수정:
    
    자식 노드들을 재배치 하는 과정에서 이미 옮겨진 노드가 또 옮겨지는 문제를 해결하기 위해 탐색 인덱스 순서를 역방향에서 정방향으로 바꾸었다.

    기존 코드:
    ```cpp
    for (size_t i = separatorIndex + 2; i < keys->getCurrentSize() + 2; i++) {
        BNode* childToShift = getChildByIndex(i);
        setChildByIndex(childToShift, i - 1);
    }
    ```

    새 코드:
    ```cpp
    for (size_t i = separatorIndex + 2; i < keys->getCurrentSize() + 2; i++) {
		BNode* childToShift = getChildByIndex(i);
		setChildByIndex(childToShift, i - 1);
	}
    ```

## 마무리

오늘은 여러 가지 테스트 케이스들을 돌려보면서 문제가 발생하는 테스트 케이스들에서의 동작을 추적하는 방식으로 디버깅을 진행하였다. 자동으로 여러 가지 테스트 케이스들을 생성하여 테스트를 해주는 코드를 만든 덕분에 문제가 되는 케이스들을 빠르게 캐치할 수 있었다.

현 시점에서 order 3에 한해서는 매우 잘 작동하는 것을 확인했다. 4에서도 잘 작동하는 것 같았다. 문제는 order가 5 이상인 경우 버그가 남아있는 것 같다. 내일은 이 부분에서 디버깅을 진행해볼 것이다.

코드의 상당수가 완성이 되었고, 이제 디버깅을 열심히 하는 일만 남은 것 같다. 그런데 이러한 디버깅이 생각보다 훨씬 피로한 작업인 것 같다. 계속 코드를 들여다봐야 해서 그런지는 몰라도, 집중력이 쉽게 바닥나는 것 같다. 조금 쉬엄쉬엄 해야될지도 모르겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  