# Strick Mode
자바스크립트의 Strict Mode 는 ECMAScript5에서 처음으로 소개됨.
일부 이전에 허용되었던 실수를 오류로 바꿔 개발자가 즉각 그것을 발견하고 고칠 수 있도록 함.

## 1.1 엄격 모드 적용하기
자바스크립트 파일 상단에 'use strict'; 추가
또는 함수 내부에 'use strict'; 를 추가하여 사용 가능

### 1.2 에러 발생 예제
```
1) "use strict";  
x = 3.14; // This will cause an error because x is not declared

2) 
"use strict";  
myFunction();  
  
function myFunction() {  
y = 3.14; // This will also cause an error because y is not declared  
}
```

