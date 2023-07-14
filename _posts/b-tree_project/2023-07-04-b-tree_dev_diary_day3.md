---
layout: post
title: "B-Tree 프로젝트 개발일지 3일차"
subtitle: "BNode 클래스"
date: 2023-07-04 23:57:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

오늘 수정하여 push한 코드들의 수정사항은 다음과 같다.

1. BNode에 parent 변수 추가 및 생성자 수정: 기존의 코드에서는 현재 노드에서 부모 노드를 가리키는 포인터 변수가 존재하지 않았다. 노드 분할이 이루어질 때 자식 노드를 분할시킨다는 느낌으로 코드를 작성하려 했기 때문이다. 
그러나 특정 노드에서 분할 함수가 호출되었을 때 자기 자신이 분할되는 것이 자연스럽다고 생각했고, 이 경우 부모 노드에 키값을 승격시켜야 하는 경우가 발생하기 때문에 parent 포인터 변수를 추가하게 되었다.
이에 맞추어, 기존에 존재하던 생성자들이 parent도 제대로 초기화 하도록 수정하였다. 

	기존 코드: 

    ```cpp 
    BKeyList<T>* keys; // A pointer that points to a BKeyList holding the keys of this node
    BNode** children;  // An array of pointers that point to each child node
    size_t order;	   // The order of this B-Tree
    bool isLeaf;	   // Indicator of whether this node is a leaf node
    ```

	새 코드:

	```cpp 
	BKeyList<T>* keys; // A pointer that points to a BKeyList holding the keys of this node
	BNode* parent;	   // A pointer that points to the parent node
	BNode** children;  // An array of pointers that point to each child node
	size_t order;	   // The order of this B-Tree
	bool isLeaf;	   // Indicator of whether this node is a leaf node
	```

2. insert 함수 수정: 어제 구조를 수정한 split 함수에서 실질적으로 분할을 제대로 수행할 수 있도록 코드를 추가하였다. 우선 루트 노드인 경우와 그렇지 않은 경우를 구별하여 코드가 진행되도록 하였고, 루트 노드가 아닌 경우를 우선 구현하였다.

	기존 코드:
		
	```cpp
	void insert(const T& key) {
		// std::cout << "Not Implemented yet" << std::endl;
		// return false;

		size_t index = keys->findIndex(key);
		
		BNode* child = nullptr;

		if (isLeaf) {
			keys->insert(key);
		}
		else {
			child = children[index];
			child->insert(key);
		}

		bool splitRequired = keys->splitRequired();

		if (splitRequired) {
			// Should determine if this is root node or not
			// Split the node
			// If this is root node, make new root node
		}
	}
	```

	새 코드: 

	```cpp 
	void insert(const T& key) {
		// std::cout << "Not Implemented yet" << std::endl;
		// return false;

		size_t index = keys->findIndex(key);
		
		BNode* child = nullptr;

		if (isLeaf) {
			keys->insert(key);
		}
		else {
			child = children[index];
			child->insert(key);
		}

		bool splitRequired = keys->splitRequired();

		if (splitRequired) {
			if (parent == nullptr) {
				 // Handle when it is root node
			}
			else {
				T promoted = keys[order / 2];

				int index = parent->keys->findIndex(promoted);
				parent->keys->insert(promoted); 

				BKeyList<T>* tail = keys->split();

				for (int i = parent->keys->getCurrentSize(); i > index + 1; i--) {
					parent->children[i] = parent->children[i - 1];
				}

				parent->children[index + 1] = new BNode(order, parent, tail, isLeaf);
			}
		}
	}
	```

## 마무리

오늘 며칠 전에 주문했던 Clean Code라는 책이 도착했다. 조금 읽어보느라 코드는 별로 많이 짜지 못했다. 내일은 좀 더 많이 일할 수 있도록 노력해보겠다. 오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  