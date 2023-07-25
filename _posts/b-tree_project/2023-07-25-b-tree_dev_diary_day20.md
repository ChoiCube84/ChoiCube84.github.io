---
layout: post
title: "B-Tree 프로젝트 개발일지 20일차"
subtitle: "The End"
date: 2023-07-25 23:58:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서도 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다. 참고로 오늘의 일지가 B-Tree 프로젝트 개발일지의 마지막이다.

## 수정사항

1. `README.md` 파일 수정

	프로젝트에서 구현하기로 한 핵심 기능이 모두 구현되었기 떄문에 (Not Yet Implemented) 부분을 삭제하였다. 또한, 리드미 파일에서 이 프로그램을 사용하는 방법을 적어두었다.

2. `b_tree_errors.hpp` 파일 추가

	B-Tree에서 발생할 수 있는 다양한 예외들을 정의하는 파일을 만들고 2가지 예외 클래스를 정의하였다.

	`InvalidOrder` 클래스는 `std::invalid_argument` 클래스를 상속 받아 order의 값이 잘못된 값인 예외를 나태낸다.

	`DeletionFailure` 클래스는 `std::logic_error` 클래스를 상속 받아 B-Tree에서 삭제에 실패한 예외를 나타낸다.

	새 코드:
	```cpp
	#ifndef __B_TREE_ERRORS_H__
	#define __B_TREE_ERRORS_H__

	#include <string>
	#include <stdexcept>

	class InvalidOrder : public std::invalid_argument {
	public:
		InvalidOrder(const std::string& message) : invalid_argument(message) {
		}
	};

	class DeletionFailure : public std::logic_error {
	public:
		DeletionFailure(const std::string& message) : logic_error(message) {
		}
	};

	#endif // !__B_TREE_ERRORS_H__
	```

3. `b_tree_constraints.hpp` 파일 추가

	B-Tree가 가져야할 제약 조건들을 모아둔 파일을 추가하였다. C++20부터 추가된 기능인 concepts를 활용하여 B-Tree의 키값으로 사용할 수 있는 타입의 특성을 지정하였다. 
	
	`Comparable` 컨셉은 해당 타입이 4가지의 비교연산을 지원하는지 확인하도록 하고, `StringStreamUsable` 컨셉은 해당 타입이 `std::stringstream`에 `<<` 연산자를 통해 문자열로 변환되어 저장될 수 있는지를 확인하도록 한다. `UsableInBTree`는 이 두 가지 컨셉을 모두 충족해야하는 컨셉으로, 다른 파일에서 `template<typename T>`가 아닌 `template<UsableInBTree T>`와 같이 사용하여 키값으로 쓸 수 있는 타입을 제한하도록 한다.

	또한, `ensureValidOrder` 함수를 이용하여 만약 order의 값이 3보다 작다면 `InvalidOrder` 예외를 throw 하도록 한다. 생성자에서 인자로 들어오는 order를 그대로 제공하는 대신 `ensureValidOrder`함수에 감싸 전달하는 방식으로 코드를 수정하였다.

	새 코드:
	```cpp
	#ifndef __B_TREE_CONSTRAINTS_H__
	#define __B_TREE_CONSTRAINTS_H__

	#include <string>
	#include <sstream>
	#include <concepts>

	#include "b_tree_errors.h"

	template<typename T>
	concept Comparable = requires (T a, T b) {
		{ a == b } -> std::convertible_to<bool>;
		{ a != b } -> std::convertible_to<bool>;
		{ a < b } -> std::convertible_to<bool>;
		{ a > b } -> std::convertible_to<bool>;
	};

	template<typename T>
	concept StringStreamUsable = requires (T a, std::stringstream ss) {
		{ ss << a };
	};

	template<typename T>
	concept UsableInBTree = Comparable<T> && StringStreamUsable<T>;

	size_t ensureValidOrder(size_t order) {
		if (order < 3) {
			throw InvalidOrder("The order of B-Tree should be bigger than 2");
		}

		return order;
	}

	#endif // !__B_TREE_CONSTRAINTS_H__
	```

4. `BKeyList`, `BNode`, `BTree` 클래스 수정

	B-Tree를 구성하는 3가지 클래스에서 다양한 함수에서 예외처리를 지원하도록 수정하였다. 또한 멤버 변수와 함수 위치 등을 적당히 조절하여 가독성을 높이고자 노력하였다. 또한, 각 클래스의 선언과 정의과 포함된 파일의 확장자명을 `.hpp`로 변경하였다.

	가장 눈에 띄는 변화는 `remove` 함수로, 삭제 성공 여부를 반환하는 대신 삭제 실패시 예외를 반환하도록 코드를 수정하였다.

5. `custom_type.hpp` 파일 추가 및 `test.cpp` 파일 수정

	기존의 `int` 등의 built-in 타입 이외에도 사용자 정의 타입이 B-Tree의 키값으로 사용가능한지 테스트를 해보기 위해서 `custom_type.hpp` 파일에 `Student`라는 이름의 클래스를 직접 정의하였으며, `test.cpp` 파일을 수정하여 이 클래스 타입을 B-Tree의 키값으로 사용가능한지 테스트 하였다.
	
	결과는 `UsableInBTree` 컨셉을 만족하는 한 오류없이 정상적으로 작동하는 모습을 확인할 수 있었다는 것이다. 필요한 비교 연산자들이 존재하지 않거나 `std::stringstream`에 `<<` 연산자를 이용하여 문자열에 합칠 수 없는 경우에는 '의도한대로' 컴파일이 제대로 진행되지 않는 모습을 확인할 수 있었다.

## 마무리

드디어 길고긴 B-Tree 프로젝트가 막을 내린다. 솔직히 더 개선할 수 있는 부분이 없다면 거짓말이겠지만, 점점 의지도 소모되어가고 나름대로는 나쁘지않은 완성도를 갖추었다고 판단이 되어 여기서 B-Tree 프로젝트를 마무리하려고 한다.

이번 프로젝트를 통해서 나름대로 많은 성과를 거두었다.

1. 학기 중에 구현하지 못했던 B-Tree에 대한 복수에 성공하였다.
2. B-Tree에 대한 이해가 많이 깊어졌다. 다시는 까먹지 않을 것 같은 느낌이다.
3. 시간과 노력을 들여 프로젝트를 끝까지 완수하는 추진력과 인내심을 기를 수 있었다.
4. 초보적인 수준이긴 해도 개발일지 작성활동 등을 통해 글쓰기 실력이 향상되었다.
5. 개발일지를 작성하는 과정에서 Markdown 문법을 제대로 배우게 되었다.
6. 개발일지를 저장할 블로그를 만들어두었기 때문에 이 블로그를 계속해서 활용할 수 있다.
7. 작년에 배운 객체지향프로그래밍의 다양한 개념과 약간의 데이터구조 개념을 복습할 수 있었다.
8. 죽어있던 내 Github에 활력을 불어넣어 주었으며, 코딩 침체기에 빠져있던 내가 다시 관성을 회복하도록 도움을 주었다.
9. 코드를 작성하는 과정에서 개발자로써 어떤 소양을 갖추어야할지 나름대로 많은 고민을 할 수 있었고, 이를 갖추기 위한 나름대로의 노력을 시작하게 되었다.

사실 생각해보면 더 나올 것 같긴 하지만 이정도로 마무리하겠다.

이번 프로젝트에 대한 소감을 말해보자면, 솔직히 뿌듯하다거나 기쁘다거나 하는 느낌이 별로 들지는 않는 것 같다. 사실 B-Tree 자체의 구현이 복잡한 부분이 있기는 해도 수업만 들어본 사람이라면 시간을 조금 들이면 누구나 구현할 수 있기도 하고, 많은 오류와 싸우면서 조금 지쳤기 때문이다. 그러나 이 과정이 고통스럽기만 했던 것은 아니다.

여러 가지 기능들이 제대로 작동할 때면 나름 뿌듯했고, 새로운 기능을 처음 추가하면서 마구 키보드를 두들길 때는 엄청난 쾌감을 느낄 수 있었다. 개발자가 느끼게 되는 고통뿐만 아니라 즐거움 또한 체험하고 이해할 수 있었던 귀중한 프로젝트라고 할 수 있다.

이러한 것들을 처음 느끼는 것은 아니었지만, 이번 B-Tree 프로젝트에서 매우 생생하게 느낄 수 있었다. 이 경험을 바탕으로 새롭게 진행할 프로젝트도 열심히 해보고 싶다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  