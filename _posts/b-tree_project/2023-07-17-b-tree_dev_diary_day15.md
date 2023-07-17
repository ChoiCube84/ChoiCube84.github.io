---
layout: post
title: "B-Tree 프로젝트 개발일지 15일차"
subtitle: "BNode 클래스"
date: 2023-07-17 23:05:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BNode::mergeWithLeftSibling`, `BNode::mergeWithRightSibling` 함수 삭제:

    각각 왼쪽 형제 노드와 오른쪽 형제 노드와 merge를 진행하는 두 함수를 삭제하였다. 새로 짠 코드에서는 두 노드의 병합은 그 두 노드의 부모 노드에서 진행하는 것으로 수정하였고, 이 과정에서 기존의 두 함수는 필요가 없게 되었다.

2. `BNode::mergeTwoChildrenNode` 함수 추가:

    기존의 `BNode::mergeWithLeftSibling`, `BNode::mergeWithRightSibling` 함수들이 하던 역할을 부모 노드에서 수행하도록 하는 새로운 함수를 추가하였다.

    새 코드:
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

3. `BNode::mergeWithSiblingNode` 함수 수정:
    
    이 함수에서 각각 왼쪽 형제 노드와 오른쪽 형제 노드와 합치는 과정을 `BNode::mergeWithLeftSibling`, `BNode::mergeWithRightSibling` 함수를 삭제함에 따라 새로 만든 함수인 `BNode::mergeTwoChildrenNode`를 사용하도록 수정하였다. 현재 노드가 부모 노드의 맨 마지막 자식이 아닌 이상 오른쪽 노드와 merge를 진행하도록 하였으며, 그렇지 않은 경우에는 왼쪽 노드와 merge를 진행하도록 하였다.

    기존 코드:
    ```cpp
    void mergeWithSiblingNode(void) {
		BNode* leftSibling = getLeftSibling();

		if (leftSibling != nullptr) {
			mergeWithLeftSibling();
		}
		else {
			mergeWithRightSibling();
		}

		if (parent->hasParent() && parent->isDeficientNode()) {
			parent->rebalance();
		}
	}
    ```

    새 코드:
    ```cpp
    void mergeWithSiblingNode(void) {
		if (childIndex < parent->keys->getCurrentSize()) {
			parent->mergeTwoChildrenNode(childIndex, childIndex + 1);
		}
		else {
			parent->mergeTwoChildrenNode(childIndex - 1, childIndex);
		}
	}
    ```

4. `BNodeTest` 함수 수정:
    
    이번에 수정한 코드들로 20, 5, 15, 1을 삭제하는 테스트는 성공하였다. 대부분의 삭제 동작은 정상적으로 이루어졌으며, 마지막에 11을 삭제하는 테스트 케이스를 통과하지 못한 상태이다. 

    기존 코드:
    ```cpp
    void BNodeTest(void) {
        BNode<int>* node = new BNode<int>(5, 0, true);
        vector<int> keys = { 4, 7, 3, 6, 2, 5, 1, 9, 10, 8, 11, 13, 12, 14, 15, 16, 17, 18, 19 };

        cout << "========== Insert test start ==========" << endl;
        for (auto i : keys) {
            if (node->hasParent()) {
                cout << "New parent Detected!" << endl;
                node = node->getParent();
                
                cout << "Current Status: " << node->preOrder() << endl;

                cout << endl;
            }
            insertAndPrint<int>(node, i);
        }
        cout << "========== Insert test done! ==========" << endl;

        cout << endl;

        cout << "========== Delete test start ==========" << endl;
        keys = { 20, 5, 15, 1 }; // TODO: Add more tests
        for (auto i : keys) {
            if (node->isEmpty()) {
                cout << "Empty parent Detected!" << endl;
                node = node->getLeftMostChild();

                cout << "Current Status: " << node->preOrder() << endl;

                cout << endl;
            }
            deleteAndPrint<int>(node, i);
        }
        cout << "========== Delete test done! ==========" << endl;

        delete node;
    }
    ```

    새 코드:
    ```cpp
    void BNodeTest(void) {
        BNode<int>* node = new BNode<int>(5, 0, true);
        vector<int> keys = { 4, 7, 3, 6, 2, 5, 1, 9, 10, 8, 11, 13, 12, 14, 15, 16, 17, 18, 19 };

        cout << "========== Insert test start ==========" << endl;
        for (auto i : keys) {
            if (node->hasParent()) {
                cout << "New parent Detected!" << endl;
                node = node->getParent();
                
                cout << "Current Status: " << node->preOrder() << endl;

                cout << endl;
            }
            insertAndPrint<int>(node, i);
        }
        cout << "========== Insert test done! ==========" << endl;

        cout << endl;

        cout << "========== Delete test start ==========" << endl;
        keys = { 20, 5, 15, 1, 19, 8, 9, 13, 10, 4, 16, 2, 11 }; // TODO: Add more tests
        for (auto i : keys) {
            if (node->isEmpty()) {
                cout << "Empty parent Detected!" << endl;
                node = node->getLeftMostChild();

                cout << "Current Status: " << node->preOrder() << endl;

                cout << endl;
            }
            deleteAndPrint<int>(node, i);
        }
        cout << "========== Delete test done! ==========" << endl;

        delete node;
    }
    ```

## 마무리

오늘도 다행히 기적이 일어나 주었다. 많은 삭제 테스트 케이스를 통과하는 걸 눈으로 직접 확인할 수 있었다. 가장 어려운 부분인 로직 구현은 마무리 된 것이다. 오늘의 마무리 부분에서는 앞으로 개발이 어떤 식으로 진행될지 써보려고 한다.

우선 현재 오류가 나는 부분은 메모리와 관련된 부분이다. 내 추측으로는, 노드 클래스의 소멸자 부분에서 하위 노드들도 모두 함께 삭제하는 바람에 예기치 못한 메모리 오류가 일어나는 것 같다. 내일은 우선 이부분을 집중적으로 다뤄보려고 한다.

그 이후에는 대망의 BTree 클래스의 구현을 시작할 것이다. BTree 클래스의 로직은 어떤 BNode가 루트 노드가 되어야 하는지를 관리하는 것이 될 것이다. 그리고 BTree 클래스의 구현이 끝나면 TODO 주석으로 급하게 메워둔 구멍들을 차례차례 고쳐나갈 생각이다. 이 과정에서 try-catch 문 등의 C++의 다양한 기능들을 활용하게 될 것 같다.

또한, 사용되지 않는 함수는 없는지, 로직이 중복되어 리팩토링이 가능한 부분은 없는지 점검할 것이고, 함수나 변수의 이름명명 규칙이 일관적인지 다시 한 번 전체적으로 확인할 것이다. 이렇게 되면 1차적으로 프로젝트의 핵심 파트는 마무리 된다.

그 이후로도 추가하고 싶은 기능들은 여전히 있다. 예를 들어, 컴파일 하는 과정에서 BTree의 원소로 사용되기 위해서는 비교 연산자 등을 지원해야 하기 때문에 원소의 타입이 이러한 연산자들을 잘 지원하는지 검사하는 기능 등을 추가하고 싶다. 여기서부터는 이번 방학에 확실히 하게 될지는 잘 모르겠다. 다른 방학에 하게 될 수도 있고, 어쩌면 다시는 안건드리게 될지도 모르겠다.

생각보다 코딩이 잘 되어주는 것 같다. 물론 고작 B-Tree에 막힐 정도면 내 코딩 실력이 형편없다는 의미겠지만, 이 기간동안 많은 걱정을 해왔기 때문에 오늘의 성과는 꽤 만족할 만한 성과라고 생각한다. 그러나 여기서 안일해지지 말고 끝까지 정신줄을 제대로 잡고 멋지게 마무리하여 유종의 미를 거두고 싶다. 내일도 다시 힘내서 코딩해야겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  