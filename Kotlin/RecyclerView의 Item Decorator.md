# RecyclerView의 Item Decoration
## 왜 데코레이터를 사용하는가

뷰를 새로 만드는데 B, C안의 margin을 동적으로 다르게 설정해야 하는 일이 있었다.
이 때 보통 view 내부에서 처리하거나, Carousel 등 view 내부에 다른 커스텀 RecyclerView가 있을 때만 ItemDecorator를 사용해서 처리했는데, 코드리뷰를 받다가
ItemDecorator 사용을 제안하셔서, 해당 구조에서 적용이 가능한가? 싶어서 시도해봤고, 결론적으로 가능했다. (삽질을 좀 하긴 했지만..)

그냥 오랜만에 사용하기도 했고, 막혔던 부분도 정리할 겸 ItemDecoration를 정리해보자.

오버라이드 가능한 내부 함수
- `onDraw`
- `onDrawOver`
- `getItemOffsets`


## onDraw
RecyclerView 내부에서 View Holder를 그리기 전에 호출한다.
이 때 삽질했던 부분은, getItemOffsets에서 Margin을 추가하게 되면, 기존 배경색과 맞지 않는 의도치 않은 색깔이 추가되는데 이거때문에 onDraw를 사용해서 코드를 작성해보았다.
... 결론은 코드가 너무 지저분해지고 도저히 해당 부분을 왜 사용하는지 의문일 정도로 (코드리뷰할때 백퍼 반려당함) 이상해져서 바로 지웠다.

구조는 다음과 같다.

```kotlin
override fun onDraw(c: Canvas, parent: RecyclerView, state: RecyclerView.State) {
        drawDivider(c, parent, color = Color.RED)
    }
```
Draw라는 함수 이름답게 Canvas를 사용할 수 있고, 필요한 색깔, 모양을 그릴 수 있다. 나는 여기서 의도치 않은 Margin 색깔을 Canvas를 이용해서 지우려고 했었다.

## onDrawOver
onDraw와 가장 큰 차이점은, 그려지는 시점이다.
onDraw는 VH가 그려지기 전, onDrawOver은 VH가 모두 그려진 후 (RecyclerView가 모두 채워진 후) 호출된다.
따라서, onDraw는 VH에 가려질 수 있고, onDrawOver은 VH를 가릴 수 있다. (요게 가장 핵심)

구조는 다음과 같은데, 뭐 볼 것도 없다. onDraw랑 똑같다.
```kotlin
override fun onDrawOver(c: Canvas, parent: RecyclerView, state: RecyclerView.State) {
        drawDivider(c, parent, color = Color.RED)
    }
```

## getItemOffsets

```kotlin
override fun getItemOffsets(
    outRect: Rect,
    view: View,
    parent: RecyclerView,
    state: RecyclerView.State
) {
    outRect.top = dividerHeight //이런식으로 outRect에 Margin(Padding X)를 표현할 수 있다.
}
```
주석 내용처럼, outRect에 margin을 추가해서 사용할 수 있다.
여기에 margin을 추가하면서 개삽질했던 부분은, 위에도 몇 번 언급했듯이 계속 의도치 않게 margin color가 적용이 되었고, 나는 이 색을 onDraw, onDrawOver를 통해서 지우려고 했었다.(멍청)
근데 parameter를 보라. 멀쩡하게 사용할 수 있는 view가 있는데?
여기서 padding을 구할 수 있는 method를 따로 작성해서 Paddings 데이터 클래스를 받아와서 view.setPadding()으로 동서남북 패딩을 적용해주니, 말끔하게 해결 되고, 위에 쓰레기같은 두 메소드는 사용할 필요가 없어졌다.

## 왜 데코레이터를 사용하는가?
솔직히, 이 부분은 코드 히스토리 파악도 어려워지는 것 같고, 뷰 내부에서 바로 볼 수 있는 메소드로 관리하는 게 더 좋을 것이라고 느꼈고, 그렇게 생각했는데, 뭐가 더 좋길래
굳이 다른 영역에 있는 Fragment에 데코레이터를 추가해서 사용하는걸까?

- 구글에서 권장하는 RecyclerView Decorator다. 뭐 일단 이것만해도 쓸 이유는 충분하다. 구글이니깐.
  - view마다 margin을 추가하는 방식은 비효율적이다. (view가 내부적으로 그려지는 방식 때문)
  - 이 때문에 getItemOffsets의 outRect 혹은 view를 사용하는 것이 뷰 내부에 메서드를 구현하는 것 보다 효율적이다.
- 좌우 스와이프를 할 때, view에서 구현한 녀석들도 따라올 수 있다.
  - 즉, Item Decoration을 사용하면, VH와 별개로 동작할 수 있는 것이다. (이는 위에서 겪은 거지같은 margin color가 VH에 맞지 않는 색깔로 바뀌어서 추가되는 이유와 같다.)

