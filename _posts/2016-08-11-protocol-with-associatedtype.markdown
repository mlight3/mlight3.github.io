---
layout: post
title: "Protocol with associatedtype"
date: "2016-08-11 17:16:28 +0900"
---

#### With Storyboard

Storyboard 사용 시, ViewController 간의 Data 전달을 하는게 굉장히 짜증스럽다;;
보통 많이 쓰는 방법으로는 Data 를 public 으로 선언하고 `prepareForSegue` 함수내에서 전달하는 방법이 있다.

```
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

다음은 protocol 을 사용하여 Data 를 전달하는 방법이다.
```
protocol Injectable {
  associatedtype Item
  func Inject(item: Item)
}
```

```
class ProfileViewController: Injectable {
  private var user: User!

  func inject(item: User) {
    self.user = user
  }
}
```

```
override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
  guard let user = self.selectedUser else { return }
  if segue.identifier == "sg_profile" {
    let vc = segue.destinationViewController as? ProfileViewController
    vc?.inject(user)
  }
}
```

실제 사용해보니 꽤 좋다.
ViewController 선언을 보면 `Injectable` 이기 때문에 외부에서 Data 가 전달된다는 것을 알 수 있다.
그 이전에는 선언된 var 들을 보면서 ! 인지, ? 인지에 따라서 이게 외부에서 들어오는 건지, 내부에서만 쓰는건지 추측 및 판단을 했어야하는데 그런 과정이 줄어 듬으로써 코드 파악이 쉬워졌다.

굳!
