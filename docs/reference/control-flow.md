---
type: doc
layout: reference
category: "Syntax"
title: "Control Flow"
---

# Control Flow

## If Expression

Kotlin에서, *if*{: .keyword }는 expression입니다. 즉, 값을 반환합니다.
그러므로 삼항 연산자는 존재하지 않는데, 평범한 *if*{: .keyword }가 같은 역할을 잘 수행하기 때문입니다.

``` kotlin
// Traditional usage 
var max = a 
if (a < b) 
  max = b 
 
// With else 
var max: Int
if (a > b) 
  max = a 
else 
  max = b 
 
// As expression 
val max = if (a > b) a else b
```

*if*{: .keyword } 분기는 블록이 되는데, 이때 마지막 expression이 블록의 값이 됩니다:

``` kotlin
val max = if (a > b) { 
    print("Choose a") 
    a 
  } 
  else { 
    print("Choose b") 
    b 
  }
```

*if*{: .keyword }가 오직 한 분기를 갖거나 분기중 하나가 `Unit`을 반환할 경우, 이는 `Unit` 타입이 됩니다.

[grammar for *if*{: .keyword }](grammar.html#if) 참고.

## When Expression

*when*{: .keyword }은 C와 비슷한 언어들의 switch 연산을 대체합니다. 가장 간단한 형태로는 아래와 같습니다.

``` kotlin
when (x) {
  1 -> print("x == 1")
  2 -> print("x == 2")
  else -> { // Note the block
    print("x is neither 1 nor 2")
  }
}
```

*when*{: .keyword }은 어떤 분기의 조건이 만족할 때까지 매개 변수에 대해 모든 분기를 계속해서 대조합니다.
*when*{: .keyword }은 expression이 될 수도 있고 statement가 될 수도 있습니다. Expression으로 사용될 경우, 만족하는 분기의 값이 전체 expression의 값이 됩니다. Statement의 경우에는, 개별 분기들의 값은 무시됩니다. (*if*{: .keyword }와 같이, 각각의 분기는 블록이 되고 블록의 마지막 expression의 값이 그의 반환값이 됩니다.)

*else*{: .keyword } 분기는 어떠한 분기도 만족하지 않을 경우에 처리됩니다.
*when*{: .keyword }이 expression으로 사용된 경우, 컴파일러가 모든 가능한 경우들이 분기 조건들로 전부 덮여있다는 것을 증명할 수 없는 한 *else*{: .keyword } 분기는 의무적으로 명시해야 합니다.

많은 경우들이 같은 방식으로 다루어져야 한다면, 분기 조건들은 콤마로 합쳐서 사용할 수 있습니다:

``` kotlin
when (x) {
  0, 1 -> print("x == 0 or x == 1")
  else -> print("otherwise")
}
```

상수 뿐만 아니라 expression도 분기 조건이 될 수 있습니다.

``` kotlin
when (x) {
  parseInt(s) -> print("s encodes x")
  else -> print("s does not encode x")
}
```

Collection이나 [range](ranges.html)의 *in*{: .keyword }과 *!in*{: .keyword }을 사용하여 값을 체크할 수도 있습니다:

``` kotlin
when (x) {
  in 1..10 -> print("x is in the range")
  in validNumbers -> print("x is valid")
  !in 10..20 -> print("x is outside the range")
  else -> print("none of the above")
}
```

다른 방법으로 *is*{: .keyword }나 *!is*{: .keyword }로 값의 특정 타입을 체크하는 것도 가능합니다. 이 때, [smart casts](typecasts.html#smart-casts) 덕분에 특별한 다른 체크 없이 타입의 메소드와 프로퍼티에 접근할 수 있게 된다는 점을 알아두세요.

```kotlin
val hasPrefix = when(x) {
  is String -> x.startsWith("prefix")
  else -> false
}
```

*when*{: .keyword }은 또한 *if*{: .keyword }-*else*{: .keyword } *if*{: .keyword } 체인을 대체할 수 있습니다.
아무 매개 변수도 전달받지 않으면, 분기 조건은 단순한 불 expression이 되고, 분기는 해당 조건이 true일 때 실행됩니다:

``` kotlin
when {
  x.isOdd() -> print("x is odd")
  x.isEven() -> print("x is even")
  else -> print("x is funny")
}
```

[grammar for *when*{: .keyword }](grammar.html#when) 참고.


## For 반복

*for*{: .keyword }는 이터레이터를 제공하는 모든 것들을 반복합니다. 문법은 다음과 같습니다:

``` kotlin
for (item in collection)
  print(item)
```

The body can be a block.

``` kotlin
for (item: Int in ints) {
  // ...
}
```

위에 설명했듯이, *for*{: .keyword }는

* 인스턴스 혹은 확장 함수 `iterator()`를 갖고,
  * 반환 타입이 인스턴스 혹은 확장 함수 `next()`를 갖고,
  * `Boolean`을 반환하는 인스턴스 혹은 확장 함수 `hasNext()`를 갖는 이터레이터를 제공하는 모든 것들을 반복할 수 있습니다.

배열이나 리스트를 인덱스와 함께 반복하고 싶을 경우, 아래의 방법처럼 할 수 있습니다:

``` kotlin
for (i in array.indices)
  print(array[i])
```

이러한 "범위 내에서의 반복"이 특별한 객체 생성이 없는 최적의 구현에 맞추어 컴파일된다는 점을 알아두세요.

[grammar for *for*{: .keyword }](grammar.html#for) 참고.

## While 반복

*while*{: .keyword }과 *do*{: .keyword }..*while*{: .keyword }은 평범하게 사용 가능합니다.

``` kotlin
while (x > 0) {
  x--
}

do {
  val y = retrieveData()
} while (y != null) // y is visible here!
```

[grammar for *while*{: .keyword }](grammar.html#while) 참고.

## 반복에서의 Break와 continue

Kotlin은 반복에서 전통적인 *break*{: .keyword }과 *continue*{: .keyword }연산자를 지원합니다. [Returns and jumps](returns.html)를 참고하세요.


