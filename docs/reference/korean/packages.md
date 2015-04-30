---
type: doc
layout: reference
category: "Syntax"
title: "Packages"
---

# Packages

소스 파일은 패키지 선언으로 시작합니다:

``` kotlin
package foo.bar

fun baz() {}

class Goo {}

// ...
```

소스 파일의 모든 내용(클래스와 함수 등)은 선언된 패키지에 포함됩니다.
그래서 위의 예시에서 `baz()`의 정확한 이름은 `foo.bar.baz`, `Goo`의 정확한 이름은 `foo.bar.Goo`가 됩니다. 
 
패키지가 설정되지 않았다면, 그 파일의 내용은 이름 없는 "default" 패키지에 속합니다.

## Import 하기

모듈에 의해 선언된 기본 import와는 별개로, 각 파일들은 자신의 import 방향성을 포함합니다.
import에 대한 문법은 [grammar](grammar.html#imports)에 설명되어 있습니다.

특정한 하나를 import할 수 있습니다. 예를 들면,

``` kotlin
import foo.Bar // Bar is now accessible without qualification
```

혹은 한 스코프의 접근 가능한 모든 내용들을 import할 수도 있습니다(패키지, 클래스, 객체 등):

``` kotlin
import foo.* // everything in 'foo' becomes accessible
```

이름에 충돌이 생길 경우, *as*{: .keyword } 키워드를 붙여 지역적으로 충돌하는 개체의 이름을 변경하여 애매함을 정리할 수 있습니다:

``` kotlin
import foo.Bar // Bar is accessible
import bar.Bar as bBar // bBar stands for 'bar.Bar'
```

## 가시성과 패키지 네스팅

최상위 선언이 *private*{: .keyword }로 표시되어 있을 때, 그가 선언된 패키지에 private한 속성을 갖습니다([Visibility Modifiers](visibility-modifiers.html) 참고).
`foo.bar` 패키지가 `foo`의 멤버로 간주되듯이 Kotlin에서 패키지는 매우 네스팅되어 있기 때문에, 어떤 것이 한 패키지에서 *private*{: .keyword }일 경우, 그의 모든 서브패키지들 또한 접근이 가능합니다.

`foo`의 import 없이 `foo.bar`에 접근할 수 없는 것처럼 외부 패키지의 멤버들은 기본적으로 import되지 **않는**다는 것을 알아두세요.
