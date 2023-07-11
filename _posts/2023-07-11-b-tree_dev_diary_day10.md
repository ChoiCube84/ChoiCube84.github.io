---
layout: post
title: "B-Tree 프로젝트 개발일지 10일차"
subtitle: "BNode 클래스"
date: 2023-07-11 22:24:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BKeyList::replaceKeyByIndex` 함수 이름 수정 (새 이름: `BKeyList::setKeyByIndex`):

    기존 코드:
    ```cpp
    // TODO: Reconsider using this function
	void replaceKeyByIndex(const T& newKey, size_t index) {
		keys[index] = newKey;
	}
    ```

    새 코드:
    ```cpp
    // TODO: Reconsider using this function
	void setKeyByIndex(const T& newKey, size_t index) {
		keys[index] = newKey;
	}
    ```

2. `BNode::leftRotation`, `BNode::rightRotation`, `BNode::mergeNode` 함수 추가 및 일부 구현:

    각 상황에 맞는 리밸런싱 동작을 담당하는 함수들을 추가하였다. `BNode::leftRotation`, `BNode::rightRotation` 함수는 구현을 완료하였다.

    새 코드:
    ```cpp
    void leftRotation(void) {
		size_t separatorIndex = childIndex;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		insert(oldSeparator);

		BNode* rightSibling = getRightSibling();
		T newSeparator = rightSibling->keys->getKeyByIndex(0);
		rightSibling->remove(newSeparator);

		//TODO: Reconsider using this function
		parent->keys->setKeyByIndex(newSeparator, separatorIndex);
	}

	void rightRotation(void) {
		size_t separatorIndex = childIndex - 1;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		insert(oldSeparator);

		BNode* leftSibling = getLeftSibling();
		T newSeparator = leftSibling->keys->getKeyByIndex(leftSibling->keys->getCurrentSize() - 1);
		leftSibling->remove(newSeparator);

		//TODO: Reconsider using this function
		parent->keys->setKeyByIndex(newSeparator, separatorIndex);
	}

	void mergeNode(void) {
		std::cout << "Not implemented yet" << std::endl;
	}
    ```

3. `BNode::rebalance` 함수 수정:

    `BNode::rebalance` 함수에서 각 상황에 맞추어 리밸런싱을 위한 각 함수들을 호출하도록 수정하였다.

    기존 코드:
    ```cpp
    void rebalance(void) {
		BNode* leftSibling = getLeftSibling();
		BNode* rightSibling = getRightSibling();

		if (leftSibling != nullptr && leftSibling->isExceedingNode()) {
			// TODO: Implement right rotation
		}
		else if (rightSibling != nullptr && rightSibling->isExceedingNode()) {
			// TODO: Implement left rotation
		}
		else {
			// TODO: Implement merging
		}
	}
    ```

    새 코드:
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
			mergeNode();
		}
	}
    ```
## 마무리

드디어 리밸런싱 함수도 거의 완성되어가고 있다. 삭제 과정 자체가 매우 복잡해서 테스트를 하기가 계속 애매했는데, 드디어 최종장이다. `BNode::mergeNode` 함수가 구현되는대로, 테스트를 바로 진행해 볼 수 있게 될 것이다. 솔직히 코드를 짜면서도 스스로에게 별로 자신이 없었어서, 조금 두렵긴 하다. 그러나 테스트를 안해보면 코드를 고칠수도 없다. 곧 결전의 날이 다가오리라 생각된다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  