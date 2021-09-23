# 코틀린 5가지 스코프 함수 정리
## let, apply, with, run, also

솔직히 업무하면서 하루도 빠짐없이 자주 사용하는 스코프 함수이지만, 글로 한번 쓰면서 정리해보고 싶었다.

스코프 함수라는 말에서도 알 수 있듯이, 해당 함수를 람다식을 통해서 호출을 하게 되면 Scope라는 범위가 생성이 되고, 이 범위에서 it, this라는 Context Object를 사용할 수 있게 된다.

이 서로 다른 Scope 함수에는 크게 두 가지 주요 차이점이 있다.
- Context Object를 참조하는 방법
- Return value

### run, apply, with
해당 스코프 함수들은 Context Object를 "this"로 참조한다.
this는 생략이 가능하지만, 동일한 이름의 멤버가 있을 경우 이를 구분하기 어려울 수 있기 때문에 가급적 this를 붙여 사용하는 것이 좋다.

### let, also
이 두 녀석은 Context Object를 "it"으로 참조한다.
다만 this와는 다르게 따로 전달 인자명을 지정할 수 있다.

### Return value
동일한 Context Object를 참조하는 그룹과는 약간 달라 헷갈릴 수 있다. 이런건 외우는것 보다 자주 사용하면서 그냥 특정 스코프는 이러한 결과를 가진다라는 것을 몸으로 체득하는 게 가장 효율적인 방법인 것 같다.

- apply, also : Context Object 객체를 반환 (객체 그 자체를 반환)
- let, run, with : 람다식 결과를 반환

람다 결과식을 반환한다는건, 람다 스코프 마지막에 있는 값을 반환함을 의미한다.
```kotlin
val width = run {
    val pi = 3.14f
    pi * 0.5 // 해당 라인이 반환값이 됨
}
```

표로 정리하면 다음과 같다.

| Function | Context Object | Return Value |
|---|:---:|---:|
| `let` | it | `Lambda Result` |
| `run` | this | `Lambda Result` |
| `with` | this | `Lambda Result` |
| `apply` | this | `Context Object` |
| `also` | it | `Context Object` |


## 각 스코프 함수들은 언제 사용할까?
5개의 스코프 함수가 2가지 형태의 Context Object를 사용하고, 또 다른 Return Value를 사용한다는 것을 확인했다.

그렇다면, 각 스코프 함수는 어떠한 상황에 사용할 수 있을까?

### let
평소에 가장 많이 사용하는것 같은 스코프 함수이고, 가장 대표적으로 non-null value를 가지고 코드 블록을 실행하고자 할 때 사용하며, 아래와같이 체인에 이어서 사용할 수 있다.

```kotlin
listOf("1", null, "3")
    .forEach {
        it?.let { // 그냥 예시용, filterNotnull() 로 대체 가능
            println("$it, ")
        }
    }
```


### with
Context Object로 "this"를 사용하며, 단어에서도 알 수 있듯이, 이 객체 자체를 가지고 무엇을 할 때 사용한다.
Return 값으로 람다 결과를 리턴받지만, 보통 리턴을 넣지 않고 사용하는 것 같다.

```
with(arg) {
    "$this"
    "$size" //this 생략 가능
}
```

### run
with 과 동일한 Context Object를 참조하고, Return Value도 같지만, 객체를 받는 위치가 다르다.
필요에 따라서 let처럼 safeCall 을 사용하여 non-null 상태일 경우에만 실행할 수 있다.

보통 다음 3가지 경우에서 사용한다.
1. 객체를 초기화하고 return 값의 계산이 필요한 경우
2. expression이 필요한 여러줄의 코드를 한 번에 처리할 때
3. let + run의 조합으로 사용할 수 있지만, let의 람다 결과로 null을 반환하게 될 경우 의도치 않게 run 스코프가 실행될 수 있다.

```kotlin
a?.let { 
    null // null 반환할 경우 run 구문 실행됨
} ?: run {
    
}
```

### also 
객체를 수정하지 않고, 부가적인 일을 처리할 때 보통 사용한다.
안드로이드의 경우에는 다음과 같이 사용할 수 있다.
```kotlin
Intent(this, Activity::class.java)
    .also { startActivity(it) }
```

### apply
자기 자신의 객체가 리턴되며, 보통 초기화하는 작업에 많이 사용한다.

---


### 공식 문서에서 추천하는 목적별 사용 구분
무조건적인 공식은 아니지만, 다음과 같은 상황에서 특정 스코프함수 사용을 제안한다.
| Function | 목적 | 
|---|:---:|
| `let` | non-null인지를 검사하여 lambda를 사용하고자 할 경우 | 
| `run` | 결과값 계산 | 
| `with` | 호출 함수들을 grouping 할 때 |
| `apply` | 객체 내용을 수정하고자 할 경우 | 
| `also` | 추가적인 작업이 필요할 경우 | 