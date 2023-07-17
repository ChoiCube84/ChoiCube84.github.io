---
layout: post
title: "B-Tree 프로젝트 개발일지 6일차"
subtitle: "BNode 클래스 - Insert"
date: 2023-07-07 23:41:00+0900
background: '/img/posts/2023/July/B-Tree.png'
katex: true
category: Project
tags: [ b-tree ]
---

# B-Tree 프로젝트

오늘도 B-Tree 프로젝트에서 추가로 개발하고 수정한 사항들을 정리하겠다. 오늘의 개발일지에서는 BNode 클래스에서의 코드의 변경사항들을 다룰 것이다. 다른 클래스들의 코드나 최신 코드를 보고 싶다면 이 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 수정사항

평소보다 수정사항이 매우 많아, 빠뜨린 내용이 있을 수 있으니 양해바란다. 그런 내용이 혹시라도 발견된다면 댓글을 달아주기 바란다. 또한, 모든 수정사항에 대한 코드를 다 붙여넣기는 힘들어서 오늘은 어떤 수정이 일어났는지 나열만 하도록 하겠다.

1. BKeyList의 operator[] 삭제
2. BKeyList의 remove 함수의 리턴값의 자료형을 bool로 변경
3. BKeyList의 insert(const T& key, size_t idx) 함수 이름을 insertByIndex로 변경
3. getChildren, getKeys 함수 삭제
4. setParent, getParent, setChildByIndex, getChildByIndex 함수 추가
5. 불필요한 주석들 삭제
6. BNode의 루트 노드 constructor 삭제 및 일반 constructor로 대체
7. BNode의 노드 분할용 constructor의 인수 변경 및 접근 제어자를 private으로 변경함
8. BNode에서 passChildrenToTailNode, makeNewRootNode, connectTailNodeToParentNode, shiftChildrenOfParentNode 함수를 추가형 기존의 split 함수에서 처리하던 다양한 역할 들을 각 함수로 분리

## 마무리

블로그 글을 쓰면서 한 번 언급했던 책이 있다. Robert C. Martin<sup>[2](#footnote_2)</sup>의 Clean Code<sup>[3](#footnote_3)</sup>라는 책이다. 친구가 언급한 적이 있는 책인데, 한 번 읽어보고 싶다는 생각이 들어 사서 읽어보게 되었다.

책을 처음 펴고서 무시무시한 이야기를 듣게 되었다.

> 이 책을 읽는 동안 마음 고생할 준비를 하기 바란다. 비행기 안에서 심심풀이로 읽어보는 "기분 좋은" 책이 아니다. 열심히, **아주 열심히** 독파해야 하는 책이다.

어느 정도는 심심풀이로 읽어볼 생각이었던 나에게는 큰 충격이었다. (사실 막 세상이 빙빙돌고 다리가 휘청거리는 그런 의미의 충격은 아니었다.) 그냥 코드를 깔끔하게 짜는 방법을 대충 배울 수 있겠지? 라는 생각으로 펼쳤던 책이었는데, 뭔가 굉장한거를 시키려는게 초장부터 눈에 보였기 때문이다.

어쨌든 그런 경고 문구를 읽은 이상 대충 읽을 수는 없었고, 나온 내용들을 기반으로 나름대로 내 B-Tree 프로젝트 코드에 적용해본 결과가 이것이다. 나중에 다시 이 코드를 읽어보게 되면 엉성하고 난해한 코드처럼 보일지도 모르겠지만, 적어도 현 시점에서는 평소보다 훨씬 깔끔하고 이해가 잘 되도록 코드가 작성된 것 같다.

이 책을 빠른 시일내에 다 읽어보고 열심히 연습을 해두어야겠다. 컴공으로써 앞으로 코딩을 할 일은 무궁무진할테니 미리 연습해둔다는 느낌으로 말이다. 그리고 시간이 된다면 내가 책을 읽고 배운 내용들을 감상문의 형태로 블로그에 작성해보고 싶다.

오늘의 개발일지는 여기서 마무리하도록 하겠다. 내일부터는 BNode 클래스의 remove 함수에 집중하게 될 것 같다. 오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/B-Tree-cpp-implementation>  
<a name="footnote_2">2</a>: <https://en.wikipedia.org/wiki/Robert_C._Martin>  
<a name="footnote_3">3</a>: [Clean Code](https://ebook.insightbook.co.kr/book/79#:~:text=%EC%95%A0%EC%9E%90%EC%9D%BC%20%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4%EC%9D%98%20%ED%98%81%EB%AA%85%EC%A0%81%EC%9D%B8,Code%20%ED%81%B4%EB%A6%B0%20%EC%BD%94%EB%93%9C%E3%80%8F%EC%97%90%20%EB%8B%B4%EC%95%98%EB%8B%A4.)