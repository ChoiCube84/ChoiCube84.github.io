---
layout: post
title: "B-Tree 프로젝트 개발일지 7일차"
subtitle: "BNode 클래스 - Delete"
date: 2023-07-08 23:58:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. BNode 멤버 변수로 childIndex 추가: 어떤 노드가 부모 노드를 기준으로 children의 몇 번째 인덱스에 의해 가리켜지는지 저장한다. 쉽게 말해, parent->children[childIndex]가 자기 자신을 가리킨다고 보면 된다. B-Tree의 delete 과정에서 형제 노드들에 접근하기 위해 추가된 멤버 변수다.

    기존 코드:

    ```cpp
    BKeyList<T>* keys;
	BNode* parent;
	BNode** children;
	size_t order;
	bool isLeaf;
    ```

    새 코드:

    ```cpp
    size_t order;
	size_t childIndex;
	bool isLeaf;
	BKeyList<T>* keys;
	BNode* parent;
	BNode** children;
    ```

2. setChildByIndex 함수 수정: 부모노드에서 자식노드를 설정하는 과정에서 자식노드에 부모노드에서 가리키는 인덱스를 저장하도록 수정했다.

    기존 코드:

    ```cpp
    void setChildByIndex(BNode* child, size_t idx) { children[idx] = child; }
    ```

    새 코드:

    ```cpp
    void setChildByIndex(BNode* child, size_t idx) { 
		if (child != nullptr) {
			child->childIndex = idx;
		}

		children[idx] = child; 
	}
    ```

3. getLeftSibling, getRightSibling 함수 추가: 앞서 언급했던 것처럼 B-Tree의 delete 과정에서 형제 노드들에 접근해야 하는데, 그 기능을 수행하는 함수이다.

    새 코드:

    ```cpp
    BNode* getLeftSibling(void) {
		if (childIndex == 0) {
			return nullptr;
		}
		else {
			return parent->children[childIndex - 1];
		}
	}

	BNode* getRightSibling(void) {
		if (childIndex == parent->keys->getCurrentSize()) {
			return nullptr;
		}
		else {
			return parent->children[childIndex + 1];
		}
	}
    ```
4. remove 함수 구현 시작: BNode 클래스의 remove 함수의 구현을 시작하였다. 리프 노드와 아닌 경우를 우선 구분하며, 리프 노드인 경우부터 구현을 시작할 것이다.

    기존 코드:

    ```cpp
    void remove() {
		std::cout << "Not Implemented yet" << std::endl;
	}
    ```

    새 코드:

    ```cpp
    bool remove(const T& key) {
		if (isLeaf) {
			bool deletionSuccess = keys->remove(key);
			
			if (keys->isEmpty()) {
				// TODO: Rebalancing required
			}
		}

		else {
			// TODO: Handle when it is internal node
		}
	}
    ```
	
## 마무리

코드를 자세히 본 사람들은 눈치챘겠지만, BNode 클래스의 메서드들은 단순히 정보들을 저장하는 역할만 하지는 않는다. BNode 클래스들은 트리 내부의 어떤 노드의 역할을 수행하지만, 그 자체가 하나의 B-Tree의 루트 노드<sup>[2](#footnote_2)</sup>처럼 행동한다. 이 점을 유념하여 코드를 읽어주길 바란다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  
<a name="footnote_2">2</a>: 진짜로 그 자체가 루트 노드인 것은 아니다. 루트 노드는 부모 노드가 존재하지 않는데, BNode 클래스의 insert 과정 등에서 부모 노드를 새로 생성하고 배정하는 모습을 확인할 수 있다. 루트 노드처럼 행동한다는 의미는, 자식 노드들을 거느리고 관리하는 역할 또한 수행한다는 의미로 받아들이면 된다.