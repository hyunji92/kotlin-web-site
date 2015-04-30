### 안전

귀찮은 NullPointerException을 제거하세요, Billion Dollar Mistake 말입니다.

``` kotlin
var output : String
output = null
```

또한 물론, Kotlin은 Java에서의 nullable 타입에 대한 실수 연산까지도 보호해줍니다.

``` kotlin
println(output.length())
```

그리고 타입이 올바른지 체크한 경우에, 컴파일러가 자동으로 형변환을 해줄 것입니다.

``` kotlin
fun calculateTotal(obj: Any) {
  if (obj is Invoice) {
    obj.calculateTotal()
  }
}
```