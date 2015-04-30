### 간결

단 한줄로 Getter, Setter, `equals()`, `hashCode()`, `toString()`, `copy()`를 포함하는 POJO를 만들거나:

``` kotlin
data class Customer(val name: String, val email: String, val company: String)
```

람다 표현식을 통해 리스트를 필터링할 수 있습니다:

``` kotlin
val positiveNumbers = list.filter {it > 0}
```

싱글톤이요? object를 만들면 됩니다:

``` kotlin
object ThisIsASingleton {
  val companyName: String = "JetBrains"
}
```