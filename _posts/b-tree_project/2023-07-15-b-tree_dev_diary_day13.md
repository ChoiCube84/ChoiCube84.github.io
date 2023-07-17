---
layout: post
title: "B-Tree 프로젝트 개발일지 13일차"
subtitle: "BNode 클래스 - Delete"
date: 2023-07-15 23:11:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항


1. `BKeyList::isExistingKey` 함수 추가: BKeyList 클래스에서 특정 키값이 존재하는 지 검사하는 함수를 추가하였다.

    새 코드:
    ```cpp
    bool isExistingKey(const T& key) {
		size_t indexOfKey = findIndex(key);
		return (key == getKeyByIndex(indexOfKey));
	}
    ```

2. `BNode::isDeficientNode` 함수 조건 수정: 해당 함수에서 검사하는 노드가 루트 노드인 경우, 즉 검사하는 노드의 부모 노드가 존재하지 않는 경우 false를 반환하도록 수정하였다. 이 함수는 어떤 노드가 가져야 할 키의 개수보다 적은 경우를 검사하는데, 루트 노드는 검사 대상이 아니기 때문에 조건을 이렇게 수정하였다.

    기존 코드:
    ```cpp
    bool isDeficientNode(void) {
		size_t numberOfChildrenNodes = keys->getCurrentSize() + 1;
        size_t minimumNumberOfRequiredKeys = (order + 1) / 2;

        return numberOfChildrenNodes < minimumNumberOfRequiredKeys;
	}
    ```

    새 코드:
    ```cpp
    bool isDeficientNode(void) {
		if (parent == nullptr) {
			return false;
		}
		else {
			size_t numberOfChildrenNodes = keys->getCurrentSize() + 1;
			size_t minimumNumberOfRequiredKeys = (order + 1) / 2;

			return numberOfChildrenNodes < minimumNumberOfRequiredKeys;
		}
	}
    ```

3. `BNode::getLeftMostLeafNode`, `BNode::getRightMostLeafNode` 함수 수정: 두 함수가 기존에는 어떤 자식 노드의 동일한 이름의 함수를 호출하고 그 반환값을 반환하는 방식이으로 작동했다면, 새로운 코드에서는 while 문을 이용하여 currentNode라는 변수에 현재 노드의 가장 왼쪽에 있는 자식 노드를 저장하고 현재 노드가 리프 노드이면 그 노드를 반환하는 방식으로 작동한다.

    기존 코드:
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
    ```

    새 코드:
    ```cpp
    BNode* getLeftMostLeafNode(void) {
		BNode* currentNode = this;

		while (currentNode->isLeaf == false) {
			currentNode = currentNode->getLeftMostChild();
		}

		return currentNode;
	}

	BNode* getRightMostLeafNode(void) {
		BNode* currentNode = this;

		while (currentNode->isLeaf == false) {
			currentNode = currentNode->getRightMostChild();
		}

		return currentNode;
	}
    ```

4. `BNode::leftRotation`, `BNode::rightRotation` 함수 수정: 삭제가 진행된 노드에서 부족한 키값을 기존의 separator로 채우는 과정에서 BNode 함수의 insert 함수를 거치지 않고 BKeyList에 즉시 삽입하도록 하였다.

    기존 코드:
    ```cpp
    void leftRotation(void) {
		size_t separatorIndex = childIndex;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		insert(oldSeparator);

		BNode* rightSibling = getRightSibling();
		T newSeparator = rightSibling->keys->getKeyByIndex(0);
		rightSibling->remove(newSeparator);

		//TODO: Reconsider using this function
		parent->keys->setKeyByIndex(newSeparator, separatorIndex);
	}

	void rightRotation(void) {
		size_t separatorIndex = childIndex - 1;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		insert(oldSeparator);

		BNode* leftSibling = getLeftSibling();
		T newSeparator = leftSibling->keys->getKeyByIndex(leftSibling->keys->getCurrentSize() - 1);
		leftSibling->remove(newSeparator);

		//TODO: Reconsider using this function
		parent->keys->setKeyByIndex(newSeparator, separatorIndex);
	}
    ```

    새 코드:
    ```cpp
    void leftRotation(void) {
		size_t separatorIndex = childIndex;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		keys->insert(oldSeparator);

		BNode* rightSibling = getRightSibling();
		T newSeparator = rightSibling->keys->getSmallestKey();
		rightSibling->remove(newSeparator);

		//TODO: Reconsider using this function
		parent->keys->setKeyByIndex(newSeparator, separatorIndex);
	}

	void rightRotation(void) {
		size_t separatorIndex = childIndex - 1;
		T oldSeparator = parent->keys->getKeyByIndex(separatorIndex);

		keys->insert(oldSeparator);

		BNode* leftSibling = getLeftSibling();
		T newSeparator = leftSibling->keys->getLargestKey();
		leftSibling->remove(newSeparator);

		//TODO: Reconsider using this function
		parent->keys->setKeyByIndex(newSeparator, separatorIndex);
	}
    ```

5. `BNode::remove` 함수 수정: 리프 노드에서부터 삭제해야 할 이유가 없다고 판단되어 구조를 수정하였다.

    기존 코드:
    ```cpp
    // TODO: Implement this function to make sure it deletes from leaf node
	bool remove(const T& key) {
		bool deletionResult = false;

		if (isLeaf) {
			deletionResult = keys->remove(key);

			if (isDeficientNode()) {
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

					// TODO: Reconsider using this function
					keys->setKeyByIndex(newSeparator, indexOfKey);

					deletionResult = leftMostLeafNode->remove(newSeparator);
				}
			}
		}

		return deletionResult;
	}
    ```

    새 코드:
    ```cpp
    bool remove(const T& key) {
		bool deletionResult = false;

		if (isLeaf) {
			deletionResult = keys->remove(key);

			if (isDeficientNode()) {
				rebalance();
			}
		}
		else {
			size_t indexOfKey = keys->findIndex(key);

			if (keys->isExistingKey(key)) {
				BNode* leftChildOfKey = getChildByIndex(indexOfKey);
				BNode* rightChildOfKey = getChildByIndex(indexOfKey + 1);

				// TODO: New separator is selected from right subtree. Check if we should consider about left subtree as well.
				BNode* leftMostLeafNodeOfRightSubtree = rightChildOfKey->getLeftMostLeafNode();
				T newSeparator = leftMostLeafNodeOfRightSubtree->keys->getSmallestKey();

				// BNode* rightMostLeafNodeOfLeftSubtree = leftChildOfKey->getRightMostLeafNode();
				// T newSeparator = rightMostLeafNodeOfLeftSubtree->keys->getLargestKey();

				// TODO: Reconsider using this function
				keys->setKeyByIndex(newSeparator, indexOfKey);

				deletionResult = leftMostLeafNodeOfRightSubtree->remove(newSeparator);
				// deletionResult = rightMostLeafNodeOfLeftSubtree->remove(newSeparator);
			}

			else {
				BNode* child = getChildByIndex(indexOfKey);
				deletionResult = child->remove(key);
			}
		}

		return deletionResult;
	}
    ```

6. `deleteAndPrint`, `insertAndPrint` 함수 완성: BNode 클래스와 BTree 클래스에서 insert과정과 delete 과정을 테스트하기 위한 함수들을 완성하였다.

    기존 코드:
    ```cpp
    template <typename T>
    void deleteAndPrint(BNode<T>* node, T key) {
        // TODO: Make the test code
    }

    template <typename T>
    void insertAndPrint(BTree<T>* tree, T key) {
        // TODO: Make the test code
    }

    template <typename T>
    void deleteAndPrint(BTree<T>* tree, T key) {
        // TODO: Make the test code
    }
    ```

    새 코드:
    ```cpp
    template <typename T>
    void deleteAndPrint(BNode<T>* node, T key) {
        cout << "Before deletion: " << node->preOrder(true) << endl;

        bool deletionSuccess = node->remove(key);
        if (deletionSuccess) {
            std::cout << "Deletion Success" << std::endl;
        }
        else {
            std::cout << "Deletion Failed" << std::endl;
        }

        cout << "After deletion: " << node->preOrder(true) << endl;
        cout << endl;
    }

    template <typename T>
    void insertAndPrint(BTree<T>* tree, T key) {
        cout << "Inserting " << key << endl;
        cout << "Before Insert: " << tree->preOrder(true) << endl;
        tree->insert(key);
        cout << "After Insert: " << tree->preOrder(true) << endl;
        cout << endl;
    }

    template <typename T>
    void deleteAndPrint(BTree<T>* tree, T key) {
        cout << "Before deletion: " << tree->preOrder(true) << endl;

        bool deletionSuccess = tree->remove(key);
        if (deletionSuccess) {
            std::cout << "Deletion Success" << std::endl;
        }
        else {
            std::cout << "Deletion Failed" << std::endl;
        }

        cout << "After deletion: " << tree->preOrder(true) << endl;
        cout << endl;
    }
    ```

7. `BNodeTest` 함수 수정: BNode 클래스의 삭제를 담당하는 `BNode::remove` 함수가 제대로 작동하는지 테스트 하는 부분을 추가하였다. 현재, 존재하지 않는 요소를 삭제하려고 하는 경우, 오른쪽으로 '회전'을 진행해야하는 경우, 왼쪽으로 '회전'을 진행해야 하는 경우를 테스트할 수 있다. 이 테스트들에 성공하면 다른 테스트들을 추가로 진행할 수 있도록 코드를 수정할 것 이다.

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

## 마무리

예상했던대로, 테스트를 하는 과정 내내 엄청난 양의 버그가 발생하는 걸 알 수 있었다. 함수들을 리팩토링하고, 잘못된 구조 등을 수정하면서 몇 가지 오류들을 해결할 수 있었으나 여러 가지 문제점들이 남아있는 상태다.

가장 마지막으로 확인했던 버그는 왼쪽으로 '회전'을 진행하는 과정에서 회전이 제대로 이루어지지 않는다는 점이었다. Visual Studio에서 제공하는 디버거를 이용해 내부 상황을 뜯어본 결과, 특정 노드에서 부모 노드를 잘못가리키고 있었기 때문에 왼쪽 형제 노드나 오른쪽 형제 노드를 불러오는 과정에서 '사촌 노드'를 불러와버리는 전혀 엉뚱한 상황이 발생하는 것을 확인하였다.

이 문제는 아무래도 insert 과정에서 눈치채지 못한 버그가 있었음을 의미할지도 모르겠다. 내일은 insert 과정에서 부모 노드를 조정해야 하는 함수들에서 어떤 문제점이 있는지 확인해 볼 생각이다. 물론 delete 과정에 사용되는 여러 함수들의 리팩토링이나 수정도 같이 할 예정이다.

아무래도 이것이 마지막 승부처가 될 것 같다. 이 문제가 해결되면 BNode 클래스에서 꼭 필요한 기능들은 모두 구현하게 되는 것이고, BTree에서의 주요 업무는 루트 노드가 어떤 노드인지를 가리키고 루트 노드에서 기능을 수행하도록 하는 것이기 때문에 비교적 난이도가 간단할 것이라고 생각된다. 힘들고 지치더라도 좀만 더 기운을 내야겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  