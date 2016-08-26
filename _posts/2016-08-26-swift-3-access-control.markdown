---
layout: post
title: "Swift 3 Access Control"
date: "2016-08-26 13:24:33 +0900"
tags: [swift, code, ios]
categories: [swift, ios]
---

Swift 2.2 에서는 `public`, `internal`, `private` 세 개의 access level (접근 레벨) 을 사용했었는데 Swift 3.0 에서는 `open`, `fileprivate` 2가지 레벨이 추가되었으며, 기존의 `public`, `private` 접근 권한(?) 이 변경되었다.

우선 변경된 것을 먼저 보자면,
#### public
- public 으로 선언된 class 를 subclass 할 수 없다.
- public 으로 선언된 function 을 override 할 수 없다.

#### private
- **private 으로 선언된 vars 들을 extension 에서 접근 할 수 없다.**

POP 로 인해 class 를 subclass 하는 경우가 별로 없다지만, private 의 변경된 내용은 알아두어야할 듯하다. (귀찮게 시리!)

추가된 레벨과 내용은 다음과 같다.
#### open
- class subclass / function override 제공해야한다면 open 으로 선언을 해야한다.
- 기존의 Objective-C 코드들은 `open` 으로 imported 된다.

### fileprivate
- fileprivate 선언한 var 를 같은 파일 내에서는 접근 가능하다.
- 기존의 private vars 를 extension 에서 사용했다면 fileprivate 로 변경해야한다.


한장의 그림으로 정리하자면
![aa](https://raw.githubusercontent.com/swiftingio/blog/%2321-Swift3-Access-Control/%2321%20Swift3%20Access%20Control/Swift3AcccesControl.png)

#### Reference
- [#22 Swift 3 Access Control (Xcode 8 Beta 6)
](https://swifting.io/blog/2016/08/17/22-swift-3-access-control-beta-6/?utm_campaign=This%2BWeek%2Bin%2BSwift&utm_medium=email&utm_source=This_Week_in_Swift_100)
