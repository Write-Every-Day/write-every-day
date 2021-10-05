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
  + T : Something -> Something이라는 고정된 클래스에 상한이 걸려있다
  + out T -> T는 가변적인 요소에 상한이 걸린다.
  + out T : Something -> 위 두 요소를 합친 것

## 변성
`변성`이라는 개념은 List`<String>`와 List`<Any>`의 관계 같이 기저 타입이 같고 타입 인자가 다른 여러 타입이 서로 어떤 관계가 있는지 설명하는 개념이다.  
변성을 잘 활용하면 사용에 불편하지 않으면서 타입 안전성을 보장하는 API를 만들 수 있다.

List`<Any>` 타입의 파라미터를 받는 함수에 List`<String>`을 넘기면 안전할까?  
String 클래스는 Any를 확장하므로 Any 타입 값을 파라미터로 받는 함수에 String 값을 넘겨도 안전하지만, Any와 String이 List 인터페이스의 타입 인자로 들어가는 경우에는 이야기가 달라진다.

## 공변성
### 하위 타입 관계를 유지하는 성질
- B가 A의 하위 타입일 때, List`<B>`는 List`<A>`의 하위 타입이다.
- 생산자의 역할을 한다 (PECS)
- Producer class

## 반공변성
### 말 그대로 변성의 반대, 뒤집힌 하위 타입 관계
- B가 A의 하위 타입일 때, List`<A>`는 List`<B>`의 하위 타입이다.
- 소비자의 역할을 한다.
- Consumer class

## Star Projection
- 타입 검사와 캐스트에 대해 설명할 때 제네릭 타입 인자 정보가 없음을 표현하기 위해 스타 프로젝션을 사용

```kotlin
val list: MutableList<Any?> = mutableListOf('a', 1)
val chars = mutableListOf('a', 'b')
val unknownElements: MutableList<*> =                
...         if (Random().nextBoolean()) list else chars
>>> unknownElements.add(42) // 컴파일러는 이 메소드 호출을 금지한다.                         
Error: Out-projected type 'MutableList<*>' prohibits
the use of 'fun add(element: E): Boolean'
>>> println(unknownElements.first()) // 대신 원소를 가져오는건 허용. first()는 Any? 타입의 원소를 반환한다. 
a
```

## MutableList<*>와 MutableList<Any?>의 차이는?
Any?는 말 그대로 어느 타입이라도 올 수 있다. 따라서 위 코드의 list처럼 다양한 원소를 리스트에 대입할 수 있다.  
하지만 Star projection은 `구체적인 원소`를 받기는 하지만, 그 원소가 어떤 타입인지를 모른다는 거다. 즉, 정해진 원소는 1개이고 이 1개는 어떤건지는 모르겠다. 정도로 이해하면 될 것 같다.

```kotlin
fun printFirst(list: List<*>) {  // 모든 리스트를 인자로 받을 수 있다. 
    if (list.isNotEmpty()) { // isNotEmpty()에서는 제네릭 타입 파라미터를 사용하지 않는다. 
        println(list.first()) // first()는 이제 Any?를 반환하지만 여기서는 그 타입만으로 충분하다. 
    }
}
printFirst(listOf("Svetlana", "Dmitry"))
>>> Svetlana
```