# Covariant
## 공변성

실무에서 in, out을 그렇게 많이 걸 일은 없지만, 안쓰면 까먹으니까 항상 같이 나오는 요소들이랑 같이 정리 해보려고 한다.

## 타입 파라미터 제약

`타입 파라미터 제약`은 클래스 혹은 함수에 사용 가능한 타입 인자를 제한하는 기능이다.
어떤 타입을 제네릭 타입 파라미터에 대한 상한 (upper bound) 로 지정하면, 해당 제네릭 타입을 인스턴스화 할 때 타입 인자는 반드시 상한 타입 혹은 하위 타입이여야 한다.  
제약을 지정하려면 타입 파라미터 명 뒤에 콜론 (:) 을 사용해 상한 타입을 명시하면 된다.    
  
다음 예제는 자바의 <T extends Number> T sum (List<T> list) 와 동일하다.
```kotlin
fun <T : Number> List<T>.sum() : T = get(0)
```

필요하다면, where 키워드를 사용해 타입 파라미터에 둘 이상의 제약을 걸 수도 있다.
```kotlin

fun <T> ensureTrailingPeriod(seq: T)
    where T : CharSequence, T: Appendable {
        if (!seq.endsWith('.')) {
            seq.append('.')
        }
}
```

## 타입 파라미터를 널이 될 수 없는 타입으로 한정
타입 파라미터 상한을 정하지 않은 파라미터는 Any? 를 상한 타입으로 지정한 것과 같다.  
`중요 포인트는, T는 Nullable이라는 것`
```kotlin
class Process<T> {
    fun process(value: T) {
        value?.hashCode()
    }
}

class ProcessNotNull<T : Any> {
    fun process(value: T) {
        value.hashCode()
    }
}
```

## 실체화한 타입 파라미터를 사용한 함수 선언
inline 으로 선언된 함수의 타입 파라미터는 실행 시점에 인라인 함수의 타입 인자를 알 수 있다.  
인라인 함수는 컴파일 시점에 해당 함수를 호출한 호출 식을 모두 함수 본문으로 바꾼다.

```kotlin
inline fun <reified T> isA(value: Any) = value is T

fun main() {
    println(isA<String>("abc"))
}
```

## 인라인 함수에서만 타입 인자를 사용할 수 있는 이유 ?
컴파일러는 인라인 함수의 본문을 구현한 바이트 코드를 함수 호출 시점에 모두 삽입한다.  
이는 실체화한 타입 인자를 사용해 정확한 타입 인자를 알수 있기 때문이고, 타입 파라미터가 아닌 구체적인 타입을 사용하기 때문에 실행시점에 발생하는 타입 소거의 영향을 받지 않는다.  
`이와 같은 특성때문에 자바에서는 reified 타입 파라미터를 사용하는 inline 함수를 호출할 수 없고, 코틀린 인라인 함수를 자바에서는 일반 함수처럼 호출한다.`


+ :(콜론)을 사용하는 파라미터 제약과, 공변성 out이 어떻게 다른가?
  + T : Something, out T 1번은 Something이라는 고정된 클래스에 상한이 걸려있다면, T는 가변적인 요소에 상한이 걸린다.