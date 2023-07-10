---
layout: post
title: "B-Tree 프로젝트 개발일지 9일차"
subtitle: "BNode 클래스"
date: 2023-07-10 23:54:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. BNode::preOrder, BNode::inOrder, BNode::postOrder 함수 수정: 매개 변수 인수를 하나 추가하여 괄호를 넣을지 말지 설정할 수 있게 하였다.

    기존 코드:

    ```C++
    void preOrder(std::stringstream& ss) {
		BNode* child = nullptr;

		ss << keys->traverse();

		if (!isLeaf) {
			for (size_t i = 0; i < keys->getCurrentSize() + 1; i++) {
				child = getChildByIndex(i);
				
				// TODO: Delete these brackets later
				ss << " (";
				ss << child->childIndex << "th child: ";
				child->preOrder(ss);
				ss << ")"; 
			}
		}
	}

	void postOrder(std::stringstream& ss) {
		BNode* child = nullptr;

		if (!isLeaf) {
			for (size_t i = 0; i < keys->getCurrentSize() + 1; i++) {
				child = getChildByIndex(i);

				// TODO: Delete these brackets later
				ss << "(";
				child->postOrder(ss);
				ss << ") ";
			}
		}

		ss << keys->traverse();
	}

    std::string preOrder(void) {
		std::stringstream ss;
		preOrder(ss);
		return ss.str();
	}

	std::string postOrder(void) {
		std::stringstream ss;
		postOrder(ss);
		return ss.str();
	}
    ```

    새 코드:

    ```C++
    void preOrder(std::stringstream& ss, bool useBrackets = false) {
		BNode* child = nullptr;

		ss << keys->traverse();

		if (!isLeaf) {
			for (size_t i = 0; i < keys->getCurrentSize() + 1; i++) {
				child = getChildByIndex(i);
				
				ss << " ";

				if (useBrackets) {
					ss << "(";
				}

				child->preOrder(ss, useBrackets);

				if (useBrackets) {
					ss << ")";
				}
			}
		}
	}

	void postOrder(std::stringstream& ss, bool useBrackets = false) {
		BNode* child = nullptr;

		if (!isLeaf) {
			for (size_t i = 0; i < keys->getCurrentSize() + 1; i++) {
				child = getChildByIndex(i);

				if (useBrackets) {
					ss << "(";
				}
				
				child->postOrder(ss, useBrackets);

				if (useBrackets) { 
					ss << ")"; 
				}

				ss << " ";
			}
		}

		ss << keys->traverse();
	}

    std::string preOrder(bool useBrackets = false) {
		std::stringstream ss;
		preOrder(ss, useBrackets);
		return ss.str();
	}

	std::string postOrder(bool useBrackets = false) {
		std::stringstream ss;
		postOrder(ss, useBrackets);
		return ss.str();
	}
    ```

2. BNode::isDeficientNode, BNode::isExceedingNode 함수 추가: 리밸런싱 과정에서 어떤 노드가 키값의 개수가 부족하거나 충분히 많은지 판단하는 함수들을 추가하였다.

    새 코드:

    ```C++
    bool isDeficientNode(void) {
		size_t numberOfChildrenNodes = keys->getCurrentSize() + 1;
		size_t minimumNumberOfRequiredKeys = (order + 1) / 2;

		return numberOfChildrenNodes < minimumNumberOfRequiredKeys;
	}

	bool isExceedingNode(void) {
		size_t numberOfChildrenNodes = keys->getCurrentSize() + 1;
		size_t minimumNumberOfRequiredKeys = (order + 1) / 2;

		return numberOfChildrenNodes > minimumNumberOfRequiredKeys;
	}
    ```

3. BNode::remove 함수 정정: 리프 노드일 때 리밸런싱일 진행하는 기준을 단순히 노드가 비었을 때가 아닌, '부족할 때' 진행하는 것으로 정정하였다.

    기존 코드:
    
    ```C++
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
    
    새 코드:
    
    ```C++
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

					keys->replaceKeyByIndex(newSeparator, indexOfKey);

					deletionResult = leftMostLeafNode->remove(newSeparator);
				}
			}
		}

		return deletionResult;
	}
    ```

4. BNode::rebalance 함수 구현: 리밸런싱을 진행하는 함수의 틀을 짜두었다.

    기존 코드:

    ```C++
    void rebalance(void) {
		std::cout << "Not implemented yet" << std::endl;
	}
    ```

    새 코드:

    ```C++
    void rebalance(void) {
		BNode* leftSibling = getLeftSibling();
		BNode* rightSibling = getRightSibling();

		if (leftSibling != nullptr && leftSibling->isExceedingNode()) {
			// TODO: Implement right rotation
		}
		else if (rightSibling != nullptr && rightSibling->isExceedingNode()) {
			// TODO: Implement left rotation
		}
		else {
			// TODO: Implement merging
		}
	}
    ```

## 마무리

Clean Code와 관련된 다양한 글이나 영상들을 보면서 느낀 점이 있다. 모든 사람들이 Clean Code에서 나오는 원칙들을 좋아하지는 않는다는 것이다. 그도 그럴것이 내가 느끼기에도 Clean Code에서 말한 원칙들을 지키려고 할 때 '이건 좀 뇌절이 아닌가?' 싶은 부분도 종종 있었기 때문이다.

Clean Code라는 책에서 알려주는 깔끔한 코드를 작성하는 방법과 습관을 길들이는 것은 분명한 코딩스타일이 잡혀있지 않는 나에게 좋은 가이드라인이 되는 것 같다. 그러나 이것이 목적이 되어서는 곤란할 것 같다는 걸 느끼게 되었다. 그리고 한 가지 또 느꼈던 것은, 처음부터 그런 클린한 코드를 짜기는 어렵다는 것이다.

insert 기능을 구현할 때도 느꼈던 부분이지만, 처음엔 그냥 되는대로 막 코드를 짜다가 오류가 발생했고, 리팩토링을 진행하면서 함수를 비교적 읽기 쉽게 만들고 나니 어느 시점에서 오류가 발생하는지 발견할 수 있었다. 이 delete 과정도 비슷하리라고 생각한다.

이번 프로젝트에서 B-Tree의 원리 이외에도 어떤 식으로 코딩을 해야하는가 많이 배워가는 기회가 되는 것 같다. 프로젝트가 마루리 되었을 때쯤 성장한 나의 모습이 기대가 된다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  