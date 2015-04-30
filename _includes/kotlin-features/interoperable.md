### 상호 호환

Java 코드를 생성하고 소모하면

``` kotlin
import io.netty.channel.ChannelInboundMessageHandlerAdapter
import io.netty.channel.ChannelHandlerContext

public class NettyHandler: ChannelInboundMessageHandlerAdapter<Any>() {
  public override fun messageReceived(p0: ChannelHandlerContext?, p1: Any?) {
    throw UnsupportedOperationException()
  }
}
```

아니면 SAM 지원을 포함한 호환성 100% JVM의 기존의 라이브러리를 사용합니다.

JVM이나 JavaScript를 타겟으로 합니다. Kotlin으로 코드를 작성하고, 어디에 배포할 것인지 결정하세요.

``` kotlin
import js.dom.html.*

fun onLoad() {
  window.document.body.innerHTML += "<br/>Hello, Kotlin!"
}
```