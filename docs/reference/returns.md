---
type: doc
layout: reference
category: "Syntax"
title: "Returns and Jumps"
---

# Returns and Jumps

Kotlin은 세 가지의 구조적 점프 연산을 갖습니다.

* *return*{: .keyword }. 기본적으로 가장 가깝게 싸여있는 함수 혹은 [function expression](lambdas.html#function-expressions)으로부터 반환합니다.
* *break*{: .keyword }. 가장 가까운 반복을 종료합니다.
* *continue*{: .keyword }. 가장 가까운 반복의 다음 단계로 진행합니다.

## Break와 Continue Label

Kotlin의 expression은 *label*{: .keyword }로 표기될 수 있습니다.
Label은 식별자로 `@` 표시를 한 형태를 갖는데, 예를 들면: `@abc`, `@fooBar`와 같은 것들이 유효한 label이라고 할 수 있습니다([grammar](grammar.html#label) 참고).
Expression에 label을 붙이려면, 단순히 앞에 label을 붙이기만 하면 됩니다.

``` kotlin
@loop for (i in 1..100) {
  // ...
}
```

이제, label을 통해 *break*{: .keyword }와 *continue*{: .keyword }를 특정할 수 있게 됩니다:

``` kotlin
@loop for (i in 1..100) {
  for (j in 1..100) {
    if (...)
      break@loop
  }
}
```

*break*{: .keyword }에 label을 붙여 해당 label의 반복 바로 다음의 실행 지점으로 점프하도록 지정했습니다.
*continue*{: .keyword }는 해당 반복의 다음 차례를 진행합니다.


## Label에서의 반환

Kotlin에서 함수는 함수 리터럴, 지역 함수와 객체 expression에 포함될 수 있습니다. 
지정된 *return*{: .keyword }은 이때 바깥쪽 함수로부터 반환할 수 있도록 합니다. 
가장 중요한 사용 예시는 함수 리터럴에서 반환할 때 입니다. 아래와 같이 작성할 때를 기억하세요:

``` kotlin
fun foo() {
  ints.forEach {
    if (it == 0) return
    print(it)
  }
}
```

*return*{: .keyword }-expression returns은 아래 코드의 `foo`와 같이 가장 가까운 함수로부터 반환합니다.
(비지역 반환은 [inline-functions](inline-functions.html)에 전달된 함수 리터럴에 대해서만 지원합니다.)
함수 리터럴로부터 반환해야 한다면, *return*{: .keyword }에 label을 지정해야 합니다:

``` kotlin
fun foo() {
  ints.forEach @lit {
    if (it == 0) return@lit
    print(it)
  }
}
```

이제 오직 함수 리터럴로부터 반환하도록 지정되었습니다. 종종 암시적인 label을 사용하는 것이 더 편할 수 있습니다:
label이 람다가 전달된 함수와 같은 이름일 경우입니다.

``` kotlin
fun foo() {
  ints.forEach {
    if (it == 0) return@forEach
    print(it)
  }
}
```

다른 방식으로, 함수 리터럴을 [function expression](lambdas.html#function-expressions)로 대체할 수 있습니다.
함수 expression에서의 *return*{: .keyword }문은 자기 자신으로부터 반환될 것입니다.

``` kotlin
fun foo() {
  ints.forEach(fun(value: Int) {
    if (value == 0) return
    print(value)
  })
}
```

값을 반환할 때, 파서는 지정된 반환에 설정을 부여합니다. 예를 들면,

``` kotlin
return@a 1
```

위의 코드는 "label이 붙은 expression `(@a 1)`를 반환" 하는 것이 아니라 "label `@a`에 `1`을 반환" 한다는 의미입니다.

이름이 있는 함수는 자동으로 label이 정의됩니다:

``` kotlin
fun outer() {
  fun inner() {
    return@outer // the label @outer was defined automatically
  }
}                                                                             
```
