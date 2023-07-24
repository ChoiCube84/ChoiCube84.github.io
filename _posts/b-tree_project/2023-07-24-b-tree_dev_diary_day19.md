---
layout: post
title: "B-Tree 프로젝트 개발일지 19일차"
subtitle: "Debugging"
date: 2023-07-24 23:59:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

여행도 끝났으니 오늘 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BNode::leftRotation`, `BNode::rightRotation` 함수 수정:

	테스트를 진행하면서 얻어낸 데이터들을 기반으로 이 두 함수에 숨어있던 버그를 발견하였고, 이를 수정하였다.

	`BNode::leftRotation` 함수의 경우, rightSibling이 가지고 있는 새로운 separator를 너무 일찍 삭제하는 바람에 자식의 마지막 인덱스에 제대로 접근하지 못하는 문제점이 있었고 이를 수정하였다.

	`BNode::rightRotation` 함수의 경우, 함수가 실행된 노드에서의 요소로 작업해야 하는 코드에서 실수로 leftSibling에서 동작을 수행하도록 코드를 짠 부분이 있었다. 이 부분을 고쳤더니 정상적으로 작동하게 되었다.

	기존 코드
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
			BNode* childToShift = getChildByIndex(0);
			setChildByIndex(childToShift, 1);

			setChildByIndex(rightMostChildOfLeftSibling, 0);
			leftSibling->setChildByIndex(nullptr, leftSibling->getCurrentSize()); // TODO: Check if this line is neccessary
		}

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

		if (!isLeaf) {
			BNode* leftMostChildOfRightSibling = rightSibling->getLeftMostChild();
			setChildByIndex(leftMostChildOfRightSibling, getCurrentSize());
			
			for (size_t i = 0; i < rightSibling->getCurrentSize(); i++) {
				BNode* childToShift = rightSibling->getChildByIndex(i + 1);
				rightSibling->setChildByIndex(childToShift, i);
			}
			rightSibling->setChildByIndex(nullptr, rightSibling->getCurrentSize()); // TODO: Check if this line is neccessary
		}

		parent->keys->remove(oldSeparator);
		parent->keys->insert(newSeparator);

		rightSibling->keys->remove(newSeparator);
	}

	void rightRotation(void) {
		size_t separatorIndex = childIndex - 1;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);
		keys->insert(oldSeparator);

		BNode* leftSibling = getLeftSibling();
		T newSeparator = leftSibling->keys->getLargestKey();

		if (!isLeaf) {
			BNode* rightMostChildOfLeftSibling = leftSibling->getRightMostChild();
			for (size_t i = getCurrentSize() - 1; i > 0; i--) {
				BNode* childToShift = getChildByIndex(i);
				setChildByIndex(childToShift, i + 1);
			}

			// Added these two lines because of range of size_t
			BNode* childToShift = getChildByIndex(0);
			setChildByIndex(childToShift, 1);

			setChildByIndex(rightMostChildOfLeftSibling, 0);
			leftSibling->setChildByIndex(nullptr, leftSibling->getCurrentSize()); // TODO: Check if this line is neccessary
		}

		parent->keys->remove(oldSeparator);
		parent->keys->insert(newSeparator);

		leftSibling->keys->remove(newSeparator);
	}
	```

2. test.cpp 파일 수정:

    다양한 테스트 케이스와 상황을 두고 테스트를 진행해야 했기 때문에 여러 번의 수정이 있었다. 더 이상 필요하지 않은 함수들을 삭제하거나, 출력 여부를 인자로 받도록 코드를 수정하거나, 테스트 케이스를 변경하는 등의 수정이 발생하였다.
	
	B-Tree 구현에서 핵심적인 함수라기 보다는 테스트를 위한 주변 함수들이라고 판단되어 굳이 기존 코드와 새 코드를 이 글에 넣어두지는 않겠다. 궁금한 사람은 앞서 이야기한 내 깃헙 레포지토리의 커밋 기록을 참고하면 될 것이다.

## 마무리

오늘로 B-Tree의 프로젝트에서 꼭 필요한 기능은 모두 구현한 것 같다. 여러 번의 테스트를 거쳐도 무사히 통과한 것을 확인할 수 있었기 때문이다.

슬슬 프로젝트에도 마침표를 찍을 시간이 다가오고 있다. 가능하다면 내일 안에 이 프로젝트를 끝내고자 한다. 앞으로 진행하고 싶은 다양한 다른 활동들도 있고, 방학도 거의 끝나가기 때문이다.

내일 할 일은 현재 함수들이 복잡하게 되어있어 읽기 힘든 부분들이 많은데, 이런 부분들을 리팩토링 등을 진행하여 가독성을 높이는 작업을 진행할 것이다. 추가로 시간이 된다면 간단한 부가 기능을 추가할 예정이다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  