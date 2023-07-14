---
layout: post
title: "B-Tree 프로젝트 개발일지 5일차"
subtitle: "BNode 클래스"
date: 2023-07-06 23:48:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# 들어가기 앞서

어제 작성했던 글을 보니 중대한 실수를 저질렀다. 어제 개발일지는 4일차였는데, 그걸 5일차로 제목에 적어버린 것이다. 오늘은 수정해두었고, 오늘이 진짜 5일차 일지임을 알리는 바이다. 그리고 그거 고치는 김에 블로그 글 마크다운 파일 이름도 좀 바꿨다.

이제 B-Tree 프로젝트 진척 상황에 대해 이야기 해보겠다.

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

1. Visual Studio 솔루션 생성: 어제까지 작성한 코드가 제대로 작동하지 않았다. 테스트와 코드 수정을 좀 더 원활하게 하기 위해서 솔루션을 생성하였다.

2. insert 함수 수정: 어제까지 작성한 BNode 클래스에서의 insert 함수가 제대로 작성하지 않아 열심히 수정을 가했다. 현재 대부분의 경우에서 노드 분할을 포함한 삽입 작업이 제대로 이루어지고 있으나, 몇몇 노드를 인식하지 못하는 문제점이 여전히 남아있다.

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

	새 코드:

	```cpp 
	void insert(const T& key) {
		size_t index = keys->findIndex(key);
		BNode* child = nullptr;

		if (isLeaf) {
			keys->insert(key);
		}
		else {
			child = children[index];
			if (child != nullptr) child->insert(key);
		}

		bool splitRequired = keys->splitRequired();
		
		if (splitRequired) {
			T promoted = keys->operator[](static_cast<int>(order / 2));
			BNode* tail = split();

			size_t index = parent->keys->findIndex(promoted);
			parent->keys->insert(promoted);

			for (size_t i = parent->keys->getCurrentSize(); i > index + 1; i--) {
				parent->children[i] = parent->children[i - 1];
			}

			parent->children[index + 1] = tail;
		}
	}
	```

3. 테스트 코드 통합: 기존에 분리되어 있던 각 클래스별 테스트 코드를 솔루션을 생성하는 과정에서 test.cpp에 하나로 통합하였다. 

	새 코드: 

	```cpp 
	#include <iostream>
    #include <vector>

    #include "b_key_list.h"
    #include "b_node.h"
    #include "b_tree.h"

    using namespace std;

    template <typename T>
    void insertAndPrint(BKeyList<T>* list, T key);
    template <typename T>
    void deleteAndPrint(BKeyList<T>* list, T key);
    template <typename T>
    void insertAndPrint(BNode<T>* node, T key);
    template <typename T>
    void deleteAndPrint(BNode<T>* node, T key);
    template <typename T>
    void insertAndPrint(BTree<T>* tree, T key);
    template <typename T>
    void deleteAndPrint(BTree<T>* tree, T key);


    void BKeyListTest(void);
    void BNodeTest(void);
    void BTreeTest(void);

    int main(void) {
        // BKeyListTest();
        BNodeTest();
        // BTreeTest();

        return 0;
    }

    template <typename T>
    void insertAndPrint(BKeyList<T>* list, T key) {
        cout << "Inserting " << key << endl;
        cout << "Before Insert: " << list->traverse() << endl;
        list->insert(key);
        cout << "After Insert: " << list->traverse() << endl;

        if (list->splitRequired()) {
            cout << "Split Required" << endl;
        }

        cout << endl;
    }

    template <typename T>
    void deleteAndPrint(BKeyList<T>* list, T key) {
        cout << "Before deletion: " << list->traverse() << endl;
        
        int check = list->remove(key);
        if (check == -1) {
            std::cout << "Deletion Success" << std::endl;
        }
        else {
            std::cout << "Deletion Failed" << std::endl;
        }

        cout << "After deletion: " << list->traverse() << endl;
        cout << endl;
    }

    template <typename T>
    void insertAndPrint(BNode<T>* node, T key) {
        cout << "Inserting " << key << endl;
        cout << "Before Insert: " << node->preOrder() << endl;
        node->insert(key);
        cout << "After Insert: " << node->preOrder() << endl;
    }

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

    void BKeyListTest(void) {

        vector<int> keys = { 4, 7, 3, 6, 2, 5, 1, 9, 10, 8, 11 };
        BKeyList<int>* first = new BKeyList<int>(11);

        cout << "========== Insert test start ==========" << endl;
        for (auto i : keys) {
            insertAndPrint<int>(first, i);
        }
        cout << "========== Insert test done! ==========" << endl;

        cout << endl;

        cout << "========== Split test start ==========" << endl;
        BKeyList<int>* second = first->split();
        cout << first->traverse() << " | " << second->traverse() << endl;
        delete second;
        cout << "========== Split test done! ==========" << endl;

        cout << endl;

        cout << "========== Delete test start ==========" << endl;
        keys = { 3, 2, 5, 5, 1, 4 };
        for (auto i : keys) {
            deleteAndPrint<int>(first, i);
        }
        cout << "========== Delete test done! ==========" << endl;

        delete first;
    }

    void BNodeTest(void) {
        BNode<int>* node = new BNode<int>(5, nullptr, true);
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
            cout << endl;
        }
        cout << "========== Insert test done! ==========" << endl;

        delete node;
    }

    void BTreeTest(void) {
        BTree<int> tree(3);
        cout << "Hello B Tree!" << endl;
    }
	```

## 마무리

오늘은 계절학기 중간고사 날이라 이동시간도 없고 여유로워서 각잡고 BNode 클래스 개발에 집중했다. 상당히 많이 코드를 건드린 것 같은데, 여전히 오류가 많이 남아있는 것 같다. 이번 방학 안에 잘 마무리 지었으면 좋겠는데 어떻게 될지 모르곘다.

그리고 코드를 짤 때 우선 급한 오류를 고치며 짜다보니 코드가 지저분해지고 읽기 힘들어지고 있다. 이런 부분을 조금 더 신경쓰면서 코딩해야 되는데, 마음처럼 잘 안되는 것 같다. 내일은 오늘보다는 좀 더 신경써서 코딩을 해야겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  