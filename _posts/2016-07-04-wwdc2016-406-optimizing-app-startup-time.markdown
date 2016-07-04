---
published: true
title: WWDC2016 #406 Optimizing app startup time
layout: post
---
### Improving launch times
* 좋은 런치 타임은 400ms
* 테스트는 가장 느린 폰에서 해야한다
* 20초 이상 반응이 없으면 앱은 시스템에 의해 종료됨

* warm launch vs cold launch
  * warm launch : app 과 data 가 memory 에 이미 있는 경우
  * cold launch : app 과 data 가 memory 에 없는 경우

* Rebooting 을 해서 cold launch 테스트를 진행

* 환경변수를 사용하여 launch time 에 대한 정보를 얻을 수 있다
```
DYLD_PRINT_STATISTICS = 1
```

### Solutions
* dylib 를 merge 한다.
* Swift 를 써라.