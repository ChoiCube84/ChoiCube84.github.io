---
layout: post
title: "B-Tree 프로젝트 개발일지 5일차"
subtitle: "BNode 클래스"
date: 2023-07-05 23:58:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

오늘 수정하여 push한 코드들의 수정사항은 다음과 같다.

1. insert 함수 수정: 어제 수정에 이어서, 루트 노드인 경우 새로운 parent 노드를 생성하고 삽입을 진행하도록 코드를 추가하였다.

	기존 코드: 

    ```C++ 
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

	새 코드:

	```C++ 
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
			T promoted = keys->operator[](order / 2);
			BNode* tail = split();

			if (parent == nullptr) {
				parent = new BNode(order, this, tail);
			}

			int index = parent->keys->findIndex(promoted);
			parent->keys->insert(promoted);

			for (int i = parent->keys->getCurrentSize(); i > index + 1; i--) {
				parent->children[i] = parent->children[i - 1];
			}

			parent->children[index + 1] = tail;
		}
	}
	```

2. BNode 클래스에 preOrder 함수 추가: BNode가 제대로 작동하는지 테스트해보기 위해 트리의 상태를 확인할 수 있는 preOrder 함수를 BNode 클래스에 추가하였다.

	새 코드: 

	```C++ 
	std::string preOrder() {
		std::stringstream ss;
		BNode* child = nullptr;

		ss << keys->traverse();

		if (!isLeaf) {
			for (size_t i = 0; i < keys->getCurrentSize(); i++) {
				child = children[i];
				ss << "( " << child->preOrder();
				/*ss << " " << child->preOrder();*/
			}
		}

		ss << ")";

		return ss.str();
	}
	```

## 마무리

드디어 본격적으로 BNode의 큰 기능을 시험해볼 때가 온 것 같다. 조금 전에 테스트를 잠깐 돌려본 바로는 작동이 전혀 안되던데, 뭐가 문제인지는 아직 모르겠다. 아마 내일 열심히 연구해봐야 할 내용이 될 것 같다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  