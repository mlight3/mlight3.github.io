---
layout: post
title: "Phantom Types"
date: "2016-08-25 16:36:41 +0900"
tags: [swift, code, ios, type]
categories: [swift, ios]
---

특정 Type 에 권한 혹은 SubType 등등을 마킹하고 싶을 때 사용할 수 있는 방법으로 Phantom type 이 소개가 되었다. (원글: [objc.io Functional Snippet #13: Phantom Types](https://www.objc.io/blog/2014/12/29/functional-snippet-13-phantom-types/))

```
struct FileHandle<A> {
    let handle: NSFileHandle
}
```

```
struct Read {}
struct Write {}
```

```
func openForReading(path: String) -> FileHandle<Read>? {
    return NSFileHandle(forReadingAtPath: path).map { FileHandle(handle: $0) }
}

func openForWriting(path: String) -> FileHandle<Write>? {
    return NSFileHandle(forWritingAtPath: path).map { FileHandle(handle: $0) }
}

func readDataToEndOfFile(handle: FileHandle<Read>) -> NSData {
    return handle.handle.readDataToEndOfFile()
}
```
`openForReading` 으로 얻은 FileHandle 은 writing 과 관련된 함수를 호출하지 못 하게 함으로써 코딩 시 오류를 줄이기 위해 사용됨.

확실히 `canRead`, `canWrite` 와 같은 함수로 코드 방어하는 것보다 훨씬 좋은 느낌이다.

위에서는 struct 를 사용하였는데 enum 으로 구현할 수도 있다. [여기서 확인](https://www.natashatherobot.com/swift-money-phantom-types/)
