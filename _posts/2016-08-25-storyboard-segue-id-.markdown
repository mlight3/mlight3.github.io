---
layout: post
title: "Storyboard Segue Id 관리하기"
date: "2016-08-25 17:50:12 +0900"
tags: [swift, code, ios, ui]
categories: [swift, ios]
---

Storyboard 를 사용할때는 언제나 Segue identifier 관리가 번거롭다. 특히 가장 짜증났던것들은 오타문제들과 처리안된 segue 찾기.


Storyboard 파일에서 identifier 설정하고, CTRL+CV 를 통하여 코드에 넣어두는데 하나의 뷰에서 여러개의 `segue` 가 있는 경우에는 `prepareSegue::` 함수가 꽤나 지져분하다.

```swift
override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
  guard let deal = self.selectedDeal else { return }
  if segue.identifier == "sg_detail" {
    let vc = segue.destinationViewController as? SelectSellerViewController
    vc?.deal = deal
  } else if segue.identifier == "sg_comment" {
    let vc = segue.destinationViewController as? CommentViewController
    vc?.deal = deal
  }
}
```
이런식의 `if-else` 문이 길어지게 된다. 물론 `switch` 를 써도 되지만, 필요없는 `default` 문이 반드시 필요해지고, `segue.identifier` 의 type 이 `String?` 이기 때문에 unwrapping 을 해주던가 아니면 `case "sg_detail"?` 처럼 `?` 를 뒤에 붙여야한다.

그 모든 것을 `enum`, `protocol`, 그리고 `protocol extension` 를 사용하여 해결해 놓은 방법을 소개하고자 한다. [원글: Protocol-Oriented Segue Identifiers in Swift ](https://www.natashatherobot.com/protocol-oriented-segue-identifiers-swift/#)

주요코드는 다음과 같다. (필자는 원글의 protocol 이름을 Segueing 으로 수정해서 사용 중. )
Segue 를 사용하는 ViewController 는 `Segueing` protocol 을 따르도록 선언한 후, `SegueIdentifier` enum 을 구현하면 된다.

```swift
protocol Segueing {
  associatedtype SegueIdentifier: RawRepresentable
}

extension Segueing where Self: UIViewController, SegueIdentifier.RawValue == String {
  func segueIdentifierForSegue(segue: UIStoryboardSegue) -> SegueIdentifier {
    guard let identifier = segue.identifier,
      segueIdentifier = SegueIdentifier(rawValue: identifier) else {
        fatalError("Invalid segue identifier \(segue.identifier).") }

    return segueIdentifier
  }

  func performSegueWithIdentifier(segueIdentifier: SegueIdentifier,
                                  sender: AnyObject?) {
    performSegueWithIdentifier(segueIdentifier.rawValue, sender: sender)
  }
}
```


```swift
class ViewController: UIViewController {
  @IBAction func didTouchPushButton() {
    performSegueWithIdentifier(.PushViewController, sender: nil)
  }

  override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
    switch segueIdentifierForSegue(segue) {
    case .ShowDetail:
      print("Show Detail")
    case .PushViewController:
      print("Push VC")
    }
  }
}

// MARK: Segueing
extension ViewController: Segueing {
  enum SegueIdentifier: String {
    case ShowDetail = "sg_show_detail"
    case PushViewController = "sg_push_view"
  }
}
```

이 방법의 장점은 원글에 나와있듯이
- 처리되지 못한 segue를 compile 오류로 쉽게 잡아 낼 수 있다.
- 재사용이 용이
- 편리한 코드

`Segueing` 를 UIViewController 이외에 사용하고 싶으면 마찬가지로 `protocol extension` 을 하면 된다.

좋네!
