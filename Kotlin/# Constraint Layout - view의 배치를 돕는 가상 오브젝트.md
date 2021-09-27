# Constraint Layout - view의 배치를 돕는 가상 오브젝트
## Guideline, Barrier, Group, Placeholder

다중 중첩으로 이루어진 Constraint Layout은 효율성이 그다지 좋지 못하다. (고 코드리뷰 중 알게 되었다.)
이를 해결하기 위한 방법으로 제시된 것 중 하나가, Constraint Layout의 Group이라는 기능을 사용하는 것인데, 처음 봤다..
Group이라는 것을 찾아보다가, Constraint Layout에서 사용할 수 있는 다른 것들도 보여서 정리해보려고 한다.

### Guideline

- 뷰의 위치를 잡는데 도움을 주는 유틸 클래스
- visibility상태는 GONE이기 때문에 나타나진 않고, 오직 다른 뷰들의 배치를 돕는 목적으로만 사용된다.
- 기본 사이즈 : 0dp
- `layout_constraintGuide_begin` : 좌측 또는 상단으로부터 고정된 거리값을 가지고 배치 됩니다.
- `layout_constraintGuide_end` : 우측 또는 하단으로부터 고정된 거리값을 가지고 배치 됩니다.
- `layout_constraintGuide_percent` 0부터 1까지 float값을 넣어 전체 길이의 비례적으로 배치 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <android.support.constraint.Guideline
        android:id="@+id/guide_line_1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintGuide_begin="50dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toLeftOf="@+id/guide_line_1"
        app:layout_constraintRight_toRightOf="@+id/guide_line_2"
        app:layout_constraintTop_toTopOf="parent" />

    <android.support.constraint.Guideline
        android:id="@+id/guide_line_2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintGuide_percent="0.7" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

- guide_line_1은 수직으로 50dp 왼쪽으로부터 떨어져 배치
- guide_line_2는 수직으로 배치되며, 수직으로 배치되었기 때문에 percent값을 0.7입력하면 가로 전체길이 기준으 70%위치되는 지점에 배치된다.

### Barrier
- 말 그대로 장벽을 만들어서 그 이상 뷰들이 넘어오지 못하도록 만든다.
- Guideline과 비슷한데, Guideline이 정적으로 view를 만든 것이었다면, Barrier는 어떤 뷰를 기준으로 동적으로 벽을 만들 수 있다.
- `barrierDirection` : barrier의 방향을 결정. top, bottom, start, end, left, right가 해당된다.
- `constraint_referenced_ids` : 장벽의 기준점으로 참조할 뷰의 아이디를 복수개 참조 할 수 있다.
- `barrierAllowsGoneWidgets` : 참조하고 있던 true 또는 false 값을 통해 참조하고 있던 뷰가 GONE될때의 동작을 지정한다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <TextView
        android:id="@+id/tv1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="동해물과 "
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintBottom_toTopOf="@+id/tv2"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_chainStyle="packed" />

    <TextView
        android:id="@+id/tv2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="마르고 닳도록"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/tv1" />

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier"
        android:layout_width="wrap_content"
        android:layout_height="0dp"
        app:barrierDirection="end"
        app:constraint_referenced_ids="tv1,tv2" />

    <Button
        android:id="@+id/btn1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="barrier"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toEndOf="@id/barrier"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

- TextView가 두개가 위아래로 packed되어 자리를 잡았고, Barrier는 이 TextView들을 end방향으로 참조하고 있어 preview를 보면 그라데이션 장벽이 생긴것을 볼 수 있다.

### Group

- Group은 여러 뷰들을 참조하면서, 참조된 뷰들을 쉽게 hide / show할 수 있는 클래스다.
- 여러 Group들이 동시에 같은 뷰를 참조하여 뷰의 상태를 변경하는 경우, xml에 선언된 순서를 따라 마지막에 적용된 Group의 상태를 따른다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/btn1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="btn1"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btn2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="btn2"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <androidx.constraintlayout.widget.Group
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="visible"
        app:constraint_referenced_ids="btn1,btn2" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

- Group의 visibility를 변경하게 될 경우, referenced_ids에 등록된 모든 view들의 상태에 반영된다. (굿)


### Placeholder

- 이미 존재하는 뷰의 위치를 조정할 수 있는 가상오브젝트
- 보통 어떤 view의 id와 함께 setContent() 메소드를 통해 적용
- 이 때 Placeholder는 효과적으로 해당 뷰를 표현하게 된다.
- content view로 동작하면 원래 가지고 있던 view의 위치는 GONE처럼 동작한다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#55000000"
    tools:context=".MainActivity">

    <androidx.constraintlayout.widget.Placeholder
        android:id="@+id/place_holder"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="100dp"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <ImageView
        android:id="@+id/iv1"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:src="@android:drawable/ic_dialog_alert"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent" />

    <ImageView
        android:id="@+id/iv2"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:src="@android:drawable/ic_dialog_map"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent" />

    <ImageView
        android:id="@+id/iv3"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:src="@android:drawable/ic_dialog_dialer"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintRight_toRightOf="parent" />
    
</androidx.constraintlayout.widget.ConstraintLayout>
```

```java
class PlaceHolder1Activity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_place_holder1)

        iv1.setOnClickListener {
           place_holder.setContentId(R.id.iv1)
        }
        iv2.setOnClickListener {
            place_holder.setContentId(R.id.iv2)
        }
        iv3.setOnClickListener {
            place_holder.setContentId(R.id.iv3)
        }
    }
}
```