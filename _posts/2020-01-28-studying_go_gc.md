---
layout: post
title: "Go 가비지컬랙션"
date: 2020-01-28 23:00:00
categories: programming
---

# Go GC(Golang Garbege Collection)

> **Go**lang 를 공부하던 중 궁금증이 생겼다.  
> **Go**는 **GC**(가비지컬렉터)가 있는 언어이다.  
> 하지만 자바와 달리 언제 stack을 쓰고 heap을 쓰는건지에 대해 명확하게 설명된 부분이 없었다.  
> 자바의 경우 Primitive Type과 Reference Type에 따라 할당되는 메모리의 영역이 각각 statck과 heap으로 나뉜다고 알려져있다.  
> 하지만 Go의 경우 어떨지가 궁금해서 나름 조사를 해보았다.

### 1. GC(가비지 컬랙터)가 무엇인가?



### 2. Go의 GC는 다른 언어의 GC와 어떤 점에서 다른가?

## References
1. https://blog.golang.org/ismmkeynote
1. https://www.geeksforgeeks.org/mark-and-sweep-garbage-collection-algorithm/
1. https://ai.google/research/pubs/pub40801
1. https://engineering.linecorp.com/ko/blog/go-gc/
1. https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html
1. https://github.com/v8/v8/tree/master/src/heap
1. https://v8.dev/blog/trash-talk