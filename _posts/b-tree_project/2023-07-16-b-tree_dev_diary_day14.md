---
layout: post
title: "B-Tree 프로젝트 개발일지 14일차"
subtitle: "BNode 클래스 - Delete"
date: 2023-07-16 23:05:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. `BNode::setChildByIndex` 함수 수정:

    어제 마지막으로 발견한 버그를 해결한 수정사항이다. 형제 노드를 찾아주는 함수를 호출했는데 사촌 노드를 불러오는 엉뚱한 문제가 발생하는 문제점은 노드의 분할 과정에서 부모의 설정이 제대로 이루어지지 않았기 때문이라고 생각했고, 노드를 분할해주는 함수인 `BNode::splitTailNode` 함수에서 사용된 이 함수의 코드를 살펴본 결과 각 자식노드의 `childIndex`는 제대로 업데이트 해주지만 정작 부모 노드는 제대로 업데이트 해주지 않는 것을 확인할 수 있었다.

    기존 코드:
    ```cpp
    void setChildByIndex(BNode* child, size_t idx) { 
		if (child != nullptr) {
			child->childIndex = idx;
		}

		children[idx] = child; 
	}
    ```

    새 코드:
    ```cpp
    void setChildByIndex(BNode* child, size_t idx) { 
		if (child != nullptr) {
			child->parent = this;
			child->childIndex = idx;
		}

		children[idx] = child; 
	}
    ```

2. `BNode::mergeWithSiblingNode` 함수 리팩토링 및 수정:
    
    이 함수에서 각각 왼쪽 형제 노드와 오른쪽 형제 노드와 합치는 과정을 `BNode::mergeWithLeftSibling`, `BNode::mergeWithRightSibling` 함수로 분리하였다. 또한, parent 노드의 리밸런싱을 진행하는 과정에서 조건문에서 부모 노드가 '부모 노드의 부모노드'를 가지고 있는지 확인하는 부분을, parent의 hasParent 함수를 호출하는 방식으로 수정하여 가독성을 높였다.

    기존 코드:
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

    새 코드:
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

	void mergeWithLeftSibling(void) {
		BNode* leftSibling = getLeftSibling();

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

	void mergeWithRightSibling(void) {
		BNode* rightSibling = getRightSibling();

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
    ```

3. `BNode::isEmpty` 함수 추가:

    특정 노드가 비어있는지 여부를 반환한다. 해당 노드가 가지고 있는 keys의 isEmpty 함수의 반환값을 그대로 반환한다.

    새 코드:
    ```cpp
    bool isEmpty(void) {
		return keys->isEmpty();
	}
    ```

4. `BNodeTest` 함수 수정:
    
    이번에 수정한 코드들로 20, 5, 15를 삭제하는 테스트는 성공하였다. 노드 병합 등을 테스트 해보기 위한 테스트 케이스로 1을 추가하였고, 현재 여기서 문제점이 남아있는 것을 확인할 수 있었다. 또한 `BNode::isEmpty` 함수를 활용하여 루트 노드가 비게 되면 그 자식 노드를 새로운 루트 노드로 가리켜 테스트를 진행하도록 설정하였다.

    기존 코드:
    ```cpp
    void BNodeTest(void) {
        BNode<int>* node = new BNode<int>(5, 0, true);
        vector<int> keys = { 4, 7, 3, 6, 2, 5, 1, 9, 10, 8, 11, 13, 12, 14, 15, 16, 17, 18, 19 };

        cout << "========== Insert test start ==========" << endl;
        for (auto i : keys) {
            if (node->hasParent()) {
                cout << "Parent Detected!" << endl;
                node = node->getParent();
                
                cout << "Current Status: " << node->preOrder() << endl;

                cout << endl;
            }
            insertAndPrint<int>(node, i);
        }
        cout << "========== Insert test done! ==========" << endl;

        cout << endl;

        cout << "========== Delete test start ==========" << endl;
        keys = { 20, 5, 15 }; // TODO: Add more tests
        for (auto i : keys) {
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

## 마무리

최근 며칠 동안 Visual Studio를 켤 때마다 두려운 마음이 든다. '오늘은 또 무슨 버그가 날 기다리고 있지?' '열심히 코딩해도 진척이 하나도 없으면 어떡하지?' 그런 걱정이 드는 것이다. 그러나 다행이도 매일 조금씩 진척이 있어왔다. 남은 문제들이 점점 어려운 문제들만 남는 것 같아 점점 더 크게 걱정되지만, 다음 날 노트북 앞에 앉으면 거짓말같이 오류가 해결되는 기적이 일어났다. 아마 내일도 똑같은 기분을 느낄 것이고, 다시 한 번 기적이 일어나면 좋겠다.

이 말을 하는 이유는, 이번에 나오는 버그가 이제까지 발견한 버그 중 제일 어려운 버그가 될 것 같다. 디버깅을 살짝 해본 결과, 분명히 다른 노드를 가리켜야 하는 두 자식 노드 포인터가 같은 자식을 가리키는 모습을 확인할 수 있었다. `BNode::mergeWithLeftSibling`, `BNode::mergeWithRightSibling` 함수에서 자식 노드를 옮기는 과정에서 이러한 문제가 생긴 것 같으니 내일 한 번 확인해보도록 하겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  