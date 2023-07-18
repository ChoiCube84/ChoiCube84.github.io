---
layout: post
title: "B-Tree 프로젝트 개발일지 16일차"
subtitle: "두번째 대격변"
date: 2023-07-17 23:57:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

평소보다 수정사항이 매우 많아, 빠뜨린 내용이 있을 수 있으니 양해바란다. 그런 내용이 혹시라도 발견된다면 댓글을 달아주기 바란다. 또한, 모든 수정사항에 대한 코드를 다 붙여넣기는 힘들고 중요한 부분의 코드만 달도록 하겠다.

### 공통 수정 사항

1. 각 클래스의 order 멤버 변수 const화 및 public화:

    객체가 처음 생성 될 때 order가 초기화되는데, 한 번 초기화 된 이후로는 변동이 일어날 수 없으므로 const 변수로 설정하였고, 접근자를 public으로 설정하였다.

2.  멤버 함수 위치 조정:

    멤버 함수들의 위치와 순서를 조정하였다. 이 과정에서 동일한 접근 제어자를 여러 번 사용하였다. 이 위치는 추후에 다시 조정될 수 있다.

3. TODO 주석 전부 삭제:
    
    TODO 주석으로 우려스러운 부분을 기록해두었는데, 모두 해결하였고, 현 시점에서 남아있는 TODO 주석은 없다.

4. `search` 함수 추가:

    기존의 findIndex 함수는 인덱스 값을 반환하는 함수이긴 하나, 어떤 키 값이 존재하는지를 알기 위해서는 더 복잡한 과정을 거쳐야 했다. 이를 해결하기 위해 각 클래스에 `search` 함수를 추가하여 존재 여부를 반환하도록 하였다.

### BKeyList 클래스

1. `setKeyByIndex` 함수 삭제:

    이 함수가 필요했던 건 delete 과정 중 어떤 separator를 대체해야 하는 과정에서 사용하는 함수였는데, 이를 remove와 delete 함수를 사용하여 대체하였다.

### BNode 클래스

1. 소멸자 수정:

    기존의 소멸자는 자식 노드들도 모두 삭제하는 방식이었다면, 이제는 자기 자신만 삭제하는 방식으로 소멸자가 작동한다.

    기존 코드:
    ```cpp
    ~BNode()
	{
		delete keys;

		//TODO: This condition statement should be revised
		if (!isLeaf && children != nullptr) {
			for (size_t i = 0; i < order; i++) {
				BNode* child = children[i];
				delete child;
			}
			delete[] children;
		}

		std::cout << "B Node was destructed" << std::endl;
		delete[] children;
	}
    ```

    새 코드:
    ```cpp
    ~BNode()
	{
		delete keys;
		delete[] children;
	}
    ```


2. 노드 분할 과정에서 사용되던 생성자 삭졔:

    기존에 노드 분할 과정에서 사용되던 생성자를 삭제하고, 기본 생성자를 이용하여 노드 분할을 진행하도록 하였다.

    기존 코드:
    ```cpp
    BNode* splitTailNode(void) {
		BNode* tail = new BNode(this);
		passChildrenToTailNode(tail);

		return tail;
	}

	BNode(BNode* currentNode)
		: order(currentNode->order), childIndex(currentNode->childIndex + 1), isLeaf(currentNode->isLeaf), keys(currentNode->keys->split()), parent(currentNode->parent), children(nullptr)
	{
		if (!isLeaf) {
			children = new BNode * [order + 1];
			for (size_t i = 0; i < order + 1; i++) {
				setChildByIndex(nullptr, i);
			}
		}

		std::cout << "B Node was splitted" << std::endl;
	}
    ```

    새 코드:
    ```cpp
    BNode* splitTailNode(void) {
		BNode* tail = new BNode(order, isLeaf);

		tail->childIndex = childIndex + 1;
		tail->keys = keys->split();
		tail->parent = parent;

		if (!tail->isLeaf) {
			tail->children = new BNode * [order + 1];
			for (size_t i = 0; i < order + 1; i++) {
				tail->setChildByIndex(nullptr, i);
			}
			passChildrenToTailNode(tail);
		}

		return tail;
	}
    ```

3. 기존 생성자 수정:

    각 노드의 childIndex는 생성자에서 설정해주지 않더라도 다른 함수를 거쳐 제대로 설정되기 때문에 생성자에서는 0으로 초기화하고, 매개 변수로 받지 않게 하였다.

    기존 코드:
    ```cpp
     BNode(size_t order, size_t childIndex = 0, bool isLeaf = false) 
		: order(order), childIndex(childIndex), isLeaf(isLeaf), keys(new BKeyList<T>(order)), parent(nullptr), children(nullptr)
	{
		if (!isLeaf) {
			children = new BNode*[order + 1];

			for (size_t i = 0; i < order + 1; i++) {
				setChildByIndex(nullptr, i);
			}	
		}
	}
    ```

    새 코드:
    ```cpp
    BNode(size_t order, bool isLeaf = false) 
		: order(order), childIndex(0), isLeaf(isLeaf), keys(new BKeyList<T>(order)), parent(nullptr), children(nullptr)
	{
		if (!isLeaf) {
			children = new BNode*[order + 1];

			for (size_t i = 0; i < order + 1; i++) {
				setChildByIndex(nullptr, i);
			}	
		}
	}
    ```

4. `mergeTwoChildrenNode` 함수 매개 변수 수정:

    기존의 매개 변수는 두 자식 노드의 인덱스를 받았는데, 만약에 두 인덱스의 차이가 1이 나지 않거나 크기 순서가 바뀌는 등의 문제가 생길 수 있기 때문에, 중간의 separator의 인덱스를 매개 변수로 전달하는 방식으로 함수를 수정하였다.

    기존 코드:
    ```cpp
    void mergeTwoChildrenNode(size_t leftChildNodeIndex, size_t rightChildNodeIndex) {
		// Warning: leftNodeIndex + 1 == rightNodeIndex should be true
		BNode* leftChildNode = getChildByIndex(leftChildNodeIndex);
		BNode* rightChildNode = getChildByIndex(rightChildNodeIndex);

		size_t numberOfChildrenOfLeftChildNode = leftChildNode->keys->getCurrentSize() + 1;
		size_t numberOfChildrenOfRightChildNode = rightChildNode->keys->getCurrentSize() + 1;

		size_t separatorIndex = leftChildNodeIndex;
		T separator = keys->getKeyByIndex(separatorIndex);

		keys->remove(separator);
		leftChildNode->keys->insert(separator);

		leftChildNode->keys->mergeWithOtherBKeyList(rightChildNode->keys);
		for (size_t i = keys->getCurrentSize() + 1; i > separatorIndex + 1; i--) {
			BNode* childToShift = getChildByIndex(i);
			setChildByIndex(childToShift, i - 1);
		}

		// TODO: Revise this condition statement
		if (!leftChildNode->isLeaf && !rightChildNode->isLeaf) {
			for (size_t i = 0; i < numberOfChildrenOfRightChildNode; i++) {
				BNode* childToShift = rightChildNode->getChildByIndex(i);
				leftChildNode->setChildByIndex(childToShift, numberOfChildrenOfLeftChildNode + i);
				rightChildNode->setChildByIndex(nullptr, i);
			}
		}
		delete rightChildNode;
		if (hasParent() && isDeficientNode()) {
			rebalance();
		}
	}
    ```

    새 코드:
    ```cpp
    void mergeTwoChildrenNode(size_t separatorIndex) {
		size_t leftChildNodeIndex = separatorIndex;
		size_t rightChildNodeIndex = separatorIndex + 1;

		BNode* leftChildNode = getChildByIndex(leftChildNodeIndex);
		BNode* rightChildNode = getChildByIndex(rightChildNodeIndex);

		size_t numberOfChildrenOfLeftChildNode = leftChildNode->keys->getCurrentSize() + 1;
		size_t numberOfChildrenOfRightChildNode = rightChildNode->keys->getCurrentSize() + 1;

		T separator = keys->getKeyByIndex(separatorIndex);

		keys->remove(separator);
		leftChildNode->keys->insert(separator);

		leftChildNode->keys->mergeWithOtherBKeyList(rightChildNode->keys);
		for (size_t i = keys->getCurrentSize() + 1; i > separatorIndex + 1; i--) {
			BNode* childToShift = getChildByIndex(i);
			setChildByIndex(childToShift, i - 1);
		}

		bool isEitherChildLeaf = leftChildNode->isLeaf || rightChildNode->isLeaf;

		if (!isEitherChildLeaf) {
			for (size_t i = 0; i < numberOfChildrenOfRightChildNode; i++) {
				BNode* childToShift = rightChildNode->getChildByIndex(i);
				leftChildNode->setChildByIndex(childToShift, numberOfChildrenOfLeftChildNode + i);
				rightChildNode->setChildByIndex(nullptr, i);
			}
		}

		delete rightChildNode;

		if (hasParent() && isDeficientNode()) {
			rebalance();
		}
	}
    ```

5. `traverse` 함수 추가 및 `preOrder`, `inOrder`, `postOrder` 함수 삭제:

    BTree 클래스에서 preorder, inorder, postorder 탐색을 직접 진행하고자, BNode 클래스는 BKeyList에 가지고 있는 키값들만을 반환하도록 하였다.

    새 코드:
    ```cpp
    std::string traverse(void) {
		return keys->traverse();
	}
    ``` 

### BTree 클래스

1. `destructorHelper` 함수 추가 및 소멸자 수정:

    루트 노드와 그 자손 노드들을 모두 BTree 클래스에서 소멸자가 호출되었을 때 함께 소멸시키도록 소멸자를 수정하였다. 이 때, `destructorHelper`를 활용하였다.
    `destructorHelper` 함수는 자식 노드들을 인자로 하는 동일한 함수를 호출하며, 그 이후 자기 자신을 소멸시킨다.

    기존 코드:
    ```cpp
    ~BTree() {
		delete root;
		std::cout << "B Tree was destructed" << std::endl;
	}
    ```

    새 코드:
    ```cpp
    void destructorHelper(BNode<T>* currentNode) {
		if (!currentNode->isLeaf) {
			for (size_t i = 0; i < currentNode->getCurrentSize(); i++) {
				BNode<T>* child = currentNode->getChildByIndex(i);
				destructorHelper(child);
			}
		}
		delete currentNode;
	}

    ~BTree() {
		destructorHelper(root);
	}
    ```

2. `traverseChildNode`, `preOrderHelper`, `inOrderHelper`, `postOrderHelper` 함수 추가 및 `preOrder`, `inOrder`, `postOrder` 수정:

    기존에 BNode 클래스에서 수행하던 각 탐색들을 BTree 클래스에서 하도록 수정하였다.

    새 코드:
    ```cpp
    std::string preOrder(bool useBrackets = false) {
		std::stringstream ss;
		preOrderHelper(ss, root, useBrackets);
		return ss.str();
	}

	std::string inOrder(void) {
		std::stringstream ss;
		inOrderHelper(ss, root);
		return ss.str();
	}

	std::string postOrder(bool useBrackets = false) {
		std::stringstream ss;
		postOrderHelper(ss, root, useBrackets);
		return ss.str();
	}
    
    void traverseChildNode(
		std::stringstream& ss, 
		std::function<void(std::stringstream&, BNode<T>*, bool)> orderType,
		BNode<T>* child, 
		bool useBrackets) 
	{
		ss << " ";

		if (useBrackets) {
			ss << "(";
		}

		orderType(ss, child, useBrackets);

		if (useBrackets) {
			ss << ")";
		}
	}

	void preOrderHelper(std::stringstream& ss, BNode<T>* currentNode, bool useBrackets) {
		BNode<T>* child = nullptr;

		ss << currentNode->traverse();

		if (!currentNode->isLeaf) {
			for (size_t i = 0; i < currentNode->getCurrentSize() + 1; i++) {
				child = currentNode->getChildByIndex(i);
				traverseChildNode(
					ss, 
					[this](std::stringstream& ss, BNode<T>* currentNode, bool useBrackets) {
						this->preOrderHelper(ss, currentNode, useBrackets);
					},
					child, 
					useBrackets
				);
			}
		}
	}

	void inOrderHelper(std::stringstream& ss, BNode<T>* currentNode) {
		BNode<T>* child = nullptr;

		if (currentNode->isLeaf) {
			ss << currentNode->traverse();
		}
		else {
			for (size_t i = 0; i < currentNode->getCurrentSize(); i++) {
				child = currentNode->getChildByIndex(i);
				inOrderHelper(ss, child);
				ss << " " << currentNode->getKeyByIndex(i) << " ";
			}
			child = currentNode->getChildByIndex(currentNode->getCurrentSize());
			inOrderHelper(ss, child);
		}
	}

	void postOrderHelper(std::stringstream& ss, BNode<T>* currentNode, bool useBrackets) {
		BNode<T>* child = nullptr;

		if (!currentNode->isLeaf) {
			for (size_t i = 0; i < currentNode->getCurrentSize() + 1; i++) {
				child = currentNode->getChildByIndex(i);
				traverseChildNode(
					ss,
					[this](std::stringstream& ss, BNode<T>* currentNode, bool useBrackets) {
						this->postOrderHelper(ss, currentNode, useBrackets);
					},
					child,
					useBrackets
				);
			}
		}

		ss << currentNode->traverse();
	}

    ```

### test.cpp 파일

## 마무리

하루동안 너무 많은 코드를 수정해서 제대로 수정사항을 기억하고 추적하는데 실패했다. 다음부터 일지를 쓸 때는 동시에 기록을 해두는 것이 좋을 것 같다. 특히 오늘은 함수들의 위치도 많이 바꿔놓으면서, 더더욱 수정사항을 추적하기가 힘들었다.

그래도 거의 대부분의 동작이 잘 이루어지는 것 같아서 다행이다. 그러나 랜덤으로 테스트를 진행하는 코드를 만들어 실행한 결과 삭제 과정에서 아직 여전히 버그가 남아있는 것 같다. 내일은 그 버그를 추적하여 수정해보도록 하겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  