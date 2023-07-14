---
layout: post
title: "B-Tree 프로젝트 개발일지 12일차"
subtitle: "BNode 클래스"
date: 2023-07-14 23:19:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BNode::mergeWithSiblingNode` 함수 구현 완료: `BNode::mergeWithSiblingNode` 함수가 수행해야 하는 기능들을 모두 구현하였다. 왼쪽 형제 노드나 오른쪽 형제 노드가 존재하는지 여부에 따라 각각 왼쪽 혹은 오른쪽 형제 노드와 merge를 진행한 뒤, 부모 노드가 리밸런싱이 필요한지 판단하여 리밸런싱을 진행하도록 구현하였다.

    기존 코드:
    ```cpp
    void mergeWithSiblingNode(void) {
		BNode* leftSibling = getLeftSibling();
		BNode* rightSibling = getRightSibling();

		if (leftSibling != nullptr) {
			// TODO: Merge with left sibling node
			size_t separatorIndex = childIndex;
			T separator = parent->keys->getKeyByIndex(separatorIndex);

			leftSibling->insert(separator);
			keys->mergeWithOtherBKeyList(leftSibling->keys);

			// TODO: Detach left sibling and shift the children
		}
		
		else {
			// TODO: Merge with right sibling node
		}
	}
    ```

    새 코드:
    ```cpp
    void mergeWithSiblingNode(void) {
		BNode* leftSibling = getLeftSibling();
		BNode* rightSibling = getRightSibling();

		if (leftSibling != nullptr) {
			size_t separatorIndex = childIndex - 1;
			T separator = parent->keys->getKeyByIndex(separatorIndex);

			parent->keys->remove(separator);
			leftSibling->insert(separator);

			keys->mergeWithOtherBKeyList(leftSibling->keys);

			for (size_t i = parent->keys->getCurrentSize() + 1; i > separatorIndex; i--) {
				BNode* childToShift = parent->getChildByIndex(i);
				parent->setChildByIndex(childToShift, i - 1);
			}

			delete leftSibling;
		}
		
		else {
			size_t separatorIndex = childIndex;
			T separator = parent->keys->getKeyByIndex(separatorIndex);

			parent->keys->remove(separator);
			insert(separator);

			keys->mergeWithOtherBKeyList(rightSibling->keys);

			for (size_t i = parent->keys->getCurrentSize() + 1; i > separatorIndex + 1; i--) {
				BNode* childToShift = parent->getChildByIndex(i);
				parent->setChildByIndex(childToShift, i - 1);
			}

			delete rightSibling;
		}

		if (parent->parent != nullptr && parent->isDeficientNode()) {
			parent->rebalance();
		}
	}
    ```

## 마무리

일단 BNode 클래스에서 제공해야 하는 기능들의 구현을 완료하였다. 이제 BNode에서 삭제 기능을 테스트하는 코드를 작성하여 코드에 문제점이 있는지 찾고, 수정하는 작업만이 남았다.

내 추측으로는 무조건 오류가 있으리라 생각한다. 삭제는 삽입 과정보다도 복잡하고 경우의 수가 많은데다가 그 삽입 연산에서 조차도 수많은 오류들과 예외가 있었다. 이번에도 그런 오류들과 싸워나가며 함수들을 리팩토링하고 논리적인 결함들을 찾아나가면서 코드들을 수정하게 될 것이다.

몇 가지 코드와 관련해서 마음이 걸리는 점이 있는데, 우선 BNode 클래스가 마치 루트 노드처럼 행동하는 것이 괜찮을지 의문스럽다. 함수들을 뜯어다보면 자식 혹은 부모노드들에서 동일한 이름의 함수를 호출하는 경우가 종종 있는데, 이걸 재귀함수라고 봐야할지는 모르겠지만, 어쨌든 게속해서 함수 스택이 쌓이기 때문에 자원이 너무 낭비되는 것이 아닌가 신경쓰인다. 

또한, B-Tree의 삭제 과정에서 일반화 된 정의로 잘 설명이 안되는 (혹은 내가 이해가 잘 안 되는) 경우가 하나 있었는데, 오늘 구현한 코드에서 그 예외를 재대로 처리하는지 좀 걱정된다. 대공사를 해야할 것만 같은 불안감이 든다.

어쨌든 지금 고민해봐야 도움도 별로 안되니 이 부분은 내일 고민하도록 하겠다. 계속 오류와 싸우다 보면 길이 보이리라 생각한다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  