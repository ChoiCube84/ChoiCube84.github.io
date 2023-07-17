---
layout: post
title: "B-Tree 프로젝트 개발일지 8일차"
subtitle: "BNode 클래스 - Delete"
date: 2023-07-09 23:54:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. rebalance 함수 추가: BNode 클래스에서 추후에 구현할 리밸런싱을 다루는 함수를 미리 만들어두었다. 

    새 코드:

    ```cpp
    void rebalance(void) {
		std::cout << "Not implemented yet" << std::endl;
	}
    ```

2. BKeyList에서 getSmallestKey, getLargestKey 함수 추가: BKeyList 클래스에서 가장 큰 키값과 가장 작은 키값을 반환하는 함수를 추가하였다.

    새 코드:

    ```cpp
    const T& getSmallestKey(void) const {
		return keys[0];
	}

	const T& getLargestKey(void) const {
		return keys[currentSize - 1];
	}
    ```

3. BKeyList에서 replaceKeyByIndex 함수 추가: B-Tree의 키값 삭제 과정에서 separator를 대체하는 과정이 있다. 이 동작을 위한 함수를 BKeyList에 추가하였다. 다만, 이런 형태의 함수를 public으로 제공해도 괜찮은지는 잘 모르겠다. 고민을 좀 해봐야할 것 같다.

	새 코드:

	```cpp
	// TODO: Reconsider using this function
	void replaceKeyByIndex(const T& newKey, size_t index) {
		keys[index] = newKey;
	}
	```

4. BNode에서 getLeftMostLeafNode, getRightMostLeafNode, getLeftMostChild, getRightMostChild 함수 추가: BNode 클래스에 가장 왼쪽에 있는 리프 노드와 가장 오른쪽에 있는 리프 노드의 포인터를 반환하는 함수를 추가하였고, 이 과정에서 가장 왼쪽이 있는 자식 노드와 가장 오른쪽에 있는 자식 노드의 포인터를 반환하는 함수들도 추가하였다.

    새 코드:

    ```cpp
    BNode* getLeftMostLeafNode(void) {
		if (isLeaf) {
			return this;
		}
		else {
			BNode* leftMostChild = getLeftMostChild();
			return leftMostChild->getLeftMostLeafNode();
		}
	}

	BNode* getRightMostLeafNode(void) {
		if (isLeaf) {
			return this;
		}
		else {
			BNode* rightMostChild = getRightMostChild();
			return rightMostChild->getRightMostLeafNode();
		}
	}

	BNode* getLeftMostChild(void) {
		return children[0];
	}

	BNode* getRightMostChild(void) {
		return children[keys->getCurrentSize()];
	}
    ```

5. BNode 클래스의 remove 함수 수정: 해당 노드가 리프 노드인지, 리프 노드가 아닌지 경우를 구분하여 삭제를 하도록 구현했다. 리프 노드가 아닌 경우에, 리프 노드에서 우선적으로 중복되는 키값이 있다면 확인하고 삭제하도록 코드를 구성하였다. 프로젝트를 처음 시작할 때 부터 이 B-Tree에서는 중복된 키값을 허용하기로 했기 때문에 이러한 상황이 발생하였다.

	기존 코드:

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

	새 코드:

	```cpp
	// TODO: Implement this function to make sure it deletes from leaf node
	bool remove(const T& key) {
		bool deletionResult = false;

		if (isLeaf) {
			deletionResult = keys->remove(key);

			if (keys->isEmpty()) {
				rebalance();
			}
		}

		else {
			size_t indexOfKey = keys->findIndex(key);
			BNode* child = getChildByIndex(indexOfKey);

			deletionResult = child->remove(key);

			if (!deletionResult) {
				if (key == keys->getKeyByIndex(indexOfKey)) {
					BNode* leftChild = getChildByIndex(indexOfKey);
					BNode* rightChild = getChildByIndex(indexOfKey + 1);

					// TODO: New separator is selected from left subtree. Check if we should consider about right subtree as well.
					BNode* leftMostLeafNode = getLeftMostLeafNode();
					T newSeparator = leftMostLeafNode->keys->getLargestKey();

					keys->replaceKeyByIndex(newSeparator, indexOfKey);

					deletionResult = leftMostLeafNode->remove(newSeparator);
				}
			}
		}

		return deletionResult;
	}
	```

## 마무리

remove 함수를 만드는게 생각보다 꽤 어려운 것 같다. insert 때와는 달리 트리의 리밸런싱을 시키는 방식이 복잡해서 그런가 코드를 짜면 짤수록 미궁에 빠지는 것 같다. 하지만 이 고비만 무사히 넘기면, B-Tree 프로젝트의 핵심적인 기능은 모두 끝낼 수 있으리라 생각한다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  