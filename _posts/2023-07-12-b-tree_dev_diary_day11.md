---
layout: post
title: "B-Tree 프로젝트 개발일지 11일차"
subtitle: "BNode 클래스"
date: 2023-07-12 23:16:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BKeyList::mergeWithOtherBKeyList` 함수 추가: 다른 BKeyList의 요소들을 모두 옮겨오기 위한 함수를 만들었다.

    새 코드:
    ```cpp
    void mergeWithOtherBKeyList(BKeyList* other) {
		for (size_t i = 0; i < other->getCurrentSize(); i++) {
			T currentKey = other->getKeyByIndex(i);
			insert(currentKey);
		}
	}
    ```

2. `BNode::mergeNode` 함수 이름 수정 및 일부 구현 (새 이름: `BNode::mergeWithSiblingNode`): 왼쪽 형제 노드가 존재하는 경우 왼쪽 형제 노드와 merge를 수행하는 로직의 일부를 구현하였다.

    기존 코드:
    ```cpp
    void mergeNode(void) {
		std::cout << "Not implemented yet" << std::endl;
	}
    ```

    새 코드:
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

3. 

## 마무리

오늘따라 뭔가 코딩할 때 삘이 별로 안 꽃힌다. 다른 날 일지랑 비교할 것도 없이, 오늘은 별로 코딩을 많이 하지 못했다. 어쩌면 못한게 아니라 안한걸지도 모르겠다. 이 함수를 만드는거에 좀 부담을 느끼고 있는 것 같다. 코드가 복잡해지고 지저분해지다 보니 점점 피로해지고, 무슨 오류가 잔뜩 생길지 알지도 모른채로 계속 코딩을 하고 있어서 그런 것 같다. 오늘은 푹 자고 내일 계절 수업 다녀와서 다시 착수해야겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  