---
layout: post
title: "2023년 7월 11일 블로그 업데이트"
subtitle: "코드 스니펫 하이라이트"
date: 2023-07-11 16:03:00+0900
background: '/img/posts/blog-update.jpg'
katex: true
category: Project
tags: [ blog ]
---

# 블로그 업데이트

## 수정사항

1. 블로그의 코드 스니펫에서 하이라이트를 적용하였다. 꽤 복잡한 과정을 거쳤는데, 찾아본 글마다 이야기가 다 달러서 머리가 많이 아팠다. 결국 이 글<sup>[1](#footnote_1)</sup>과 이 글<sup>[2](#footnote_2)</sup>의 방법을 혼합하여 적용하는데 성공하였고, C++ 언어에 하이라이트를 적용하기 위해선 ```` ```C++ ```` 이 아니라, ```` ```cpp ```` 를 이용해야 한다는 사실을 이 글<sup>[3](#footnote_3)</sup>을 통해 알게되었다.

    내가 사용한 방식을 설명하자면, 1번 글의 방식을 거의 따라하되, _bootstrap-overrides.scss 파일이 없었기 때문에 2번의 마지막 글처럼 생성된 css 파일을 assets/css 경로에 두고, assets 경로에 있는 main.scss 파일에 ``` @import "css/highligter.css"; ``` 를 추가하였다.

2. 사이트의 favicon을 추가했다. pixabay에 있는 큐브 일러스트<sup>[4](#footnote_4)</sup>를 사용했다. 이 블로그 글<sup>[5](#footnote_5)</sup>의 도움을 받아 추가하였다.

## 발견된 문제상황

분명히 네이버 서치어드바이저에서 내 블로그를 읽어오도록 세팅을 해둔 것 같은데 제대로 작동하지 않는 것 같다. 나중에 시간이 나면 제대로 확인을 해봐야 할 것 같다.

## 마무리

평소에 내가 블로그 글을 올리고 나서 코드를 보면 항상 밋밋하고 심심했는데, 드디어 하이라이트를 적용하니 볼만해졌다. 참고로 적용한 테마의 이름은 tulip으로 내 친구가 골라준 테마이다. 이 친구에 관심이 있다면 이 블로그<sup>[6](#footnote_6)</sup>를 찾아가는 것을 추천한다.

시간이 너무 늦어졌다. 한숨 자고 일어나서 빡세게 코딩한 다음 빨리 개발일지를 써야겠다. 오늘의 블로그 업뎃은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://syki66.github.io/blog/2020/02/08/clean-blog-highlighter.html>  
<a name="footnote_2">2</a>: <http://sangsoonam.github.io/2019/01/20/syntax-highlighting-in-jekyll.html>  
<a name="footnote_3">3</a>: <https://oleksandrkvl.github.io/2020/08/09/use-prismjs-with-jekyll.html>  
<a name="footnote_4">4</a>: <https://pixabay.com/vectors/rubik-cube-puzzle-3d-problem-25817/>  
<a name="footnote_5">5</a>: <https://piaflu.tistory.com/103>  
<a name="footnote_6">6</a>: <https://blog.naver.com/nerdystudent>  