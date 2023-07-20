---
layout: post
title: "B-Tree 프로젝트 개발일지 18일차"
subtitle: "Debugging"
date: 2023-07-20 23:44:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BNode::leftRotation`, `BNode::rightRotation` 함수 수정:

	리프 노드에서 리밸런싱이 진행 된 후 부모 노드가 부족한 노드가 되는 경우 부모 노드에서 리밸런싱이 이루어지는데, 왼쪽으로 회전시키는 함수와 오른쪽으로 회전시키는 함수에서 자식 노드를 이동시키는 코드를 추가하였다.

	기존 코드
	```cpp
	void leftRotation(void) {
		size_t separatorIndex = childIndex;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		keys->insert(oldSeparator);

		BNode* rightSibling = getRightSibling();
		T newSeparator = rightSibling->keys->getSmallestKey();
		rightSibling->keys->remove(newSeparator);
		
		parent->keys->remove(oldSeparator);
		parent->keys->insert(newSeparator);
	}

	void rightRotation(void) {
		size_t separatorIndex = childIndex - 1;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		keys->insert(oldSeparator);

		BNode* leftSibling = getLeftSibling();
		T newSeparator = leftSibling->keys->getLargestKey();
		leftSibling->keys->remove(newSeparator);

		parent->keys->remove(oldSeparator);
		parent->keys->insert(newSeparator);
	}
	```

	새 코드:
	```cpp
	void leftRotation(void) {
		size_t separatorIndex = childIndex;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		keys->insert(oldSeparator);

		BNode* rightSibling = getRightSibling();
		T newSeparator = rightSibling->keys->getSmallestKey();
		rightSibling->keys->remove(newSeparator);

		if (!isLeaf) {
			BNode* leftMostChildOfRightSibling = rightSibling->getLeftMostChild();
			setChildByIndex(leftMostChildOfRightSibling, getCurrentSize());
			for (size_t i = 0; i < rightSibling->getCurrentSize() + 1; i++) {
				BNode* childToShift = rightSibling->getChildByIndex(i + 1);
				rightSibling->setChildByIndex(childToShift, i);
			}
			rightSibling->setChildByIndex(nullptr, rightSibling->getCurrentSize()); // TODO: Check if this line is neccessary
		}
		
		parent->keys->remove(oldSeparator);
		parent->keys->insert(newSeparator);
	}

	void rightRotation(void) {
		size_t separatorIndex = childIndex - 1;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		keys->insert(oldSeparator);

		BNode* leftSibling = getLeftSibling();
		T newSeparator = leftSibling->keys->getLargestKey();
		leftSibling->keys->remove(newSeparator);

		if (!isLeaf) {
			BNode* rightMostChildOfLeftSibling = leftSibling->getRightMostChild();
			for (size_t i = getCurrentSize() - 1; i > 0; i--) {
				BNode* childToShift = leftSibling->getChildByIndex(i);
				leftSibling->setChildByIndex(childToShift, i + 1);
			}

			// Added these two lines because of range of size_t
			BNode* childToShift = leftSibling->getChildByIndex(0);
			leftSibling->setChildByIndex(childToShift, 1);

			setChildByIndex(rightMostChildOfLeftSibling, 0);
			leftSibling->setChildByIndex(nullptr, leftSibling->getCurrentSize()); // TODO: Check if this line is neccessary
		}

		parent->keys->remove(oldSeparator);
		parent->keys->insert(newSeparator);
	}
	```

2. `BNode::rebalance` 함수 수정:

    리프 노드가 아닌 경우의 노드에서 리밸런싱을 진행할 때의 조건을 따로 둘 필요가 없이 leftRotation 함수와 rightRotation 함수에서 경우에 따라 올바르게 처리하도록 코드를 짜는게 맞다고 생각하여 원래대로 코드를 돌렸다.

    기존 코드:
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
			mergeWithSiblingNode();
		}
	}
    ```

## 마무리

오늘은 안타깝게도 디버깅 결과 해결된 버그가 없었다. 함수에서 어떤 동작을 제대로 수행하지 않는지는 알아내어 다행히 이에 대한 코드는 수정할 수 있었으나, 여전히 마지막으로 발견된 버그가 해결된 것은 아니다.

원래대로라면 오늘 B-Tree 프로젝트를 완결짓고 싶었다. 계절학기 수업이 오늘로 종강이고, 내일 2박 3일로 여행을 다녀오기 때문에, 몇 가지 부가기능들이 완성되지는 않았지만 다른 방학에 진행하는 것으로 한 뒤 필수 기능은 완결을 짓고 다른 프로젝트로 넘어가고 싶었다.

하지만 오늘 버그를 해결하지 못했기 때문에 그러기는 어려울 것 같다. 또한, 여행 동안에는 개발일지는 물론이고 코딩도 하지 않을 계획이다. 모처럼의 여행에서까지 이런 일을 하고 싶지는 않기 때문이다.

잔디심기 기록이 깨지기는 하지만, 여행을 간 날까지 잔디심기가 되어있는건 말이 안된다고 생각한다. 말이 나온김에 내가 잔디심기를 하는 이유를 간단하게 말하자면, 잔디심기를 활동을 통해 조금씩이라도 프로그래밍 활동을 해야한다는 모종의 압력을 스스로에게 가하기 위함이였다. 여행 기간에도 잔디심기를 하는건 내가 처음 잔디심기를 시작한 근본적인 의도에 맞지 않는다. 기회가 되면 잔디심기에 대한 내 생각에 대한 내용은 따로 자세히 다루어보겠다.

여행을 다녀와서의 계획을 간단하게 정리하자면, 앞서 언급한 대로 부가기능은 배제하고 필수 기능을 완성한 뒤 이번 방학 기간의 B-Tree 프로젝트는 완결을 지으려고 한다. 하고 싶은 다양한 프로젝트와 공부가 있고, 솔직히 좀 지겹기 때문이다. 무슨 활동을 하게 될지 미리 이야기 하자면, 양자 컴퓨팅에 대한 공부와 이와는 별개로 게임을 하나 만들어보고 싶다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  