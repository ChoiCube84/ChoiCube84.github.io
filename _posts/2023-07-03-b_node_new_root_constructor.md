---
layout: post
title: "B-Tree 프로젝트 개발일지 2일차"
subtitle: "BNode 클래스"
date: 2023-07-03 18:18:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘은 BNode 클래스에서 현재까지 만들어 둔 함수들에 대해 정리하고, 코드에서 수정한 사항들에 대해서 정리해볼 것이다. 이번 글 부터는 코드를 수정하기 전과 후의 코드들을 글에다가 기록해둘 것이지만, 다른 코드들이나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## BNode의 멤버 함수들

지금까지 내가 작성한 BNode 코드들에 대한 내용을 정리해보겠다. 우선 BNode에 내가 만들어둔 멤버 함수들은 다음과 같다.

| 함수명                                			  | 접근 지정자 | 반환 값 자료형 | 기능                                                             |
| --------------------------------------------------- | ----------- | -------------- | ---------------------------------------------------------------- |
| BNode(size_t order, bool isLeaf = false)            | public      |                | BNode를 생성한다.                                                |
| BNode(size_t order, BNode* first, BNode* second) 	  | public      |                | B-Tree의 루트 노드 분할이 일어날 때, 새로운 루트노드를 생성한다. |
| BNode(size_t order, BKeyList<T>* keys, bool isLeaf) | public      |                | B-Tree의 노드 분할이 일어날 때, 새로운 노드를 생성한다.          |
| ~BNode()                           				  | public      |                | BNode를 소멸시킨다.                                              |
| getChildren()               						  | public      | BNode**        | 자식 노드들의 포인터 배열을 반환한다.                            |
| getKeys()      									  | public      | BKeyList<T>*   | 키값들이 저장된 BKeyList의 포인터를 반환한다.                    |
| split()                  						      | private     | BNode*         | 노드를 분할한다.                                                 |
| insert(const T& key)      						  | public      | void           | 노드에 새로운 키값을 삽입한다.                                   |
| remove()                  						  | public      | void           | 노드에 저장된 키값을 삭제한다.                                   |

## 수정사항

오늘 수정하여 push한 코드들의 수정사항은 다음과 같다.

1. BNode 생성자 기능 분리: 기존의 코드에서는 하나의 생성자가 일반적인 BNode를 생성하는 역할과, 루트 노드가 분할될 때 새로이 만들어져야 하는 루트 노드를 생성하는 역할을 동시에 수행했으나, 이를 새로운 생성자를 만들어 분리하였다. 

	<details>
	<summary>기존 코드</summary>
		 
		BNode(size_t order, bool isLeaf = false, T* key = nullptr, BNode* first = nullptr, BNode* second = nullptr) : keys(new BKeyList<T>(order)), order(order), isLeaf(isLeaf)
		{
			if (isLeaf) {
				children = nullptr;
				std::cout << "B Node was made: This is leaf node" << std::endl;
			}
			else {
				children = new BNode*[order + 1];
				children[0] = first;
				children[1] = second;

				for (int i = 2; i < order + 1; i++) {
					children[i] = nullptr;
				}

				keys->insert(*key);

				std::cout << "B Node was made" << std::endl;
			}
		}

	</details>

	<details>
	<summary>새 코드</summary>

		// Basic constructor of BNode
		BNode(size_t order, bool isLeaf = false) : keys(new BKeyList<T>(order)), order(order), isLeaf(isLeaf)
		{
			if (isLeaf) {
				children = nullptr;
				std::cout << "B Node was made: This is leaf node" << std::endl;
			}
			else {
				children = new BNode*[order + 1];
				for (int i = 0; i < order + 1; i++) {
					children[i] = nullptr;
				}
				std::cout << "B Node was made" << std::endl;
			}
		}

		// Constructor of BNode for making a new root node
		BNode(size_t order, BNode* first, BNode* second) : keys(new BKeyList<T>(order)), order(order), isLeaf(false)
		{
			children = new BNode * [order + 1];

			children[0] = first;
			children[1] = second;

			for (int i = 2; i < order + 1; i++) {
				children[i] = nullptr;
			}

			std::cout << "New root B Node was made" << std::endl;
		}

	</details>

2. split 함수 수정: 기존의 split 함수에서는 이 노드가 분할이 필요한지 확인했으나, 그것을 확인하는 것은 이 함수를 호출하는 곳에서 책임을 져야한다고 판단했고, 그래서 확인하지 않고 바로 분할을 진행하도록 수정하였다. 또한, 이 클래스에서만 split 함수를 호출할 수 있도록 접근 지정자를 private으로 변경하였다. 또한, 분할 과정에서 새로이 생성되는 tail 노드가 자식 노드들을 제대로 연결 받도록 설정하였다.

	<details>
	<summary>기존 코드</summary>
		 
		BNode* split() {
			if (keys->getCurrentSize() < order) return nullptr;
			else {
				BNode* tail = new BNode(order, keys->split(), isLeaf);

				currentSize = order / 2;

				for (int i = order / 2 + 1; i < order; i++) {
					tail->children[i] = 
				}

				return tail;
			}
		}

	</details>

	<details>
	<summary>새 코드</summary>

		BNode* split() {
			BNode* tail = new BNode(order, keys->split(), isLeaf);

			currentSize = order / 2;

			for (int i = order / 2 + 1; i < order; i++) {
				tail->children[i - (order / 2 + 1)] = children[i];
				children[i] = nullptr;
			}

			return tail;
		}

	</details>


3. insert 함수 수정: 기존의 insert 함수는 이후 노드에서 분할을 진행해야하는지 확인하기 위해 bool 자료형의 값을 반환했으나, BKeyList에서 노드가 분할되어야 하는지 여부를 확인하는 방식이 바뀌어 더 이상 그러한 값을 반환할 필요가 없어져, 아무런 값도 리턴하지 않게 수정하였다. 또한, 바뀐 BKeyList의 함수에 맞추어 코드를 수정하였다.

	<details>
	<summary>기존 코드</summary>
		 
		bool insert(const T& key) {
			// std::cout << "Not Implemented yet" << std::endl;
			// return false;

			int index = keys->findIndex(key);
			bool splitRequired = false;

			BNode* child = nullptr;

			if (isLeaf) {
				splitRequired = keys->insert(key);
			}
			else {
				child = children[index];
				splitRequired = child->insert(key);
			}

			if (splitRequired) {
				if (isLeaf) {
					return true;
				}
				else {
					T promoted = child->keys[order / 2];
					
					int index = keys->findIndex(promoted);
					splitRequired = keys->insert(promoted); // We should return this value
					BKeyList<T>* temp = keys->split();

					for (int i = keys->getCurrentSize(); i > index + 1; i--) {
						children[i] = children[i - 1];
					}
					children[index + 1] = temp;
					return splitRequired;
				}
			}
			else {
				return false;
			}
		}

	</details>

	<details>
	<summary>새 코드</summary>

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

	</details>


## 마무리

BNode 클래스는 BKeyList의 멤버 함수들을 적절히 활용하여 키값들을 관리하는 동시에 자식 노드들의 포인터 배열도 관리해야 하기 때문에, BKeyList의 구현보다 난이도가 높을 것으로 예상하였다. 그리고 실제로 그런 것 같다고 오늘 코드를 고치면서 느끼게 되었다.

내일은 insert 함수를 제대로 구현하는데 집중하여 코드를 수정해보도록 하겠다. 오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  