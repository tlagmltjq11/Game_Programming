## 🔔 회전과 쿼터니언
인스펙터에서 오일러 회전 좌표계를 이용하는 것 처럼 보이지만, 유니티는 쿼터니언 값을 이용해 회전 시스템을<br>
운용하기 때문에, 코드 상에서 회전값을 지정해줄때에는 쿼터니언값을 대입해주어야 한다.<br>
-> 인스펙터에 오일러 값을 넣어도 내부적으로는 쿼터니언으로 변환하여 사용하는 것.<br>

👉 **Rotate(); -> 현재 회전값에서 매개변수로 넣어준 회전값만큼 회전시킨다.**<br>

회전값을 더할때 오일러 회전 좌표계는 +를 이용해서 더하면 되지만, 오일러 좌표계는 매트릭스로 이루어져 있기에<br>
⭐ **곱하기** ⭐ 를 이용해서 더해주어야 한다.<br>

```c#
Quaternion originalRotation = Quaternion.Euler(new Vector3(45, 0, 0));

Quaternion plusRotation = Quaternion.Euler(new Vector3(30, 0, 0));

Quaternion targetRotation = originalRotation * plusRotation; 
```
<br>

⭐ **회전은 대부분 로컬좌표계를 기준으로** ⭐ 이루어진다는 점을 유의해야 한다.<br>
(30, 45, 60)도로 회전되어있는 오브젝트를 x축으로 30도 더 회전 시킨다면 어떻게 될까?<br>
-> (60, 45, 60)이 아니라 (40.505, 79.715, 80.538)이 된다.<br>
그 이유는 글로벌 기준이었다면 (60, 45, 60)이 됐겠지만, 이미 (30, 45, 60)만큼 회전되어있는 **로컬좌표계** 를 기준으로<br>
더 회전을 하기 때문에 전혀 다른 값이 나오는 것이다.<br>
<br>
<br>


⭐ **자세한 설명은 아래!** ⭐<br>

## Euler
<img src="https://user-images.githubusercontent.com/43705434/121767831-c88a3b00-cb95-11eb-8e65-1eca095cceec.PNG" width="400" height="250"><br>
<br>

오일러 좌표계란 **x,y,z 3개의 축을 기준으로 0~360도 만큼 회전시키는** 우리에게 친숙한 좌표계이다.<br>
이러한 오일러 좌표계에서 x, y, z 축은 **계층구조**를 띄고있다. 즉 종속적인 관계를 갖고있다는 얘기다.<br>
예를들자면 다음 그림과 같은 구조로되어 있어 한 축이 회전하게되면 아직 회전하지 않은 축이 함께 회전하게 된다.<br>
<br>

(y축으로 회전하면 하위 모든 축이 함께 회전되며, x축으로 회전하면 z축이 함께 회전하게 된다.)<br>

<img src="https://user-images.githubusercontent.com/43705434/121767833-c88a3b00-cb95-11eb-9bbd-88ce1ba1b427.PNG" width="400" height="250"><br>
<br>
<br>

**그렇다면 왜 종속적인 관계를 가질까?** <br>
<br>
이 3축이 회전에서 종속적인 이유는 바로 오일러 각에서 회전 자체를 이 3축으로 나눠서 **순서대로 계산하기 때문**이다.<br>
수학적으로 표현된 강체를 돌리기 위해서 우리는 3축으로 회전 방향을 디지털화 시켰다.<br>
x에 대해서 회전하고 y에 대해서 회전하고 z에 대해서 회전시켰다고 생각해보자.<br>
**a**는 x에 대해서 회전시키면 이미 x로 회전된 **a'** 의 상태이고 우리는 a에 대해서 y축으로 회전하는 것이 아니라 a' 에 대해서<br>
y축으로 회전하는 것이기 때문에 **a"** 가 된다. 그리고 마지막으로 z에 대해서 회전시킬 때에는 이미 우리가 알던 a가 아니라<br> 
a"에 대해서 회전시키는 것이다. 이미 y축으로 돌릴 차례에는 x로 돌아간 상태이기 때문에 두 축에 대한 계산이 독립적일 수가 없다.<br>
<br>
<br>

이렇게 오일러 좌표계는 회전에 있어서 정해진 순서대로 축 회전을 계산하기에 종속적인 관계로 인한 **짐벌락이라는 문제를 초래**하게 된다.<br>
<br>
<br>

## Gimbal Lock
![짐벌락](https://user-images.githubusercontent.com/43705434/121768927-dfcc2700-cb9b-11eb-919a-f8de5c22df3b.PNG)<br>

짐벌락이란 설명했듯이 오일러 좌표계를 사용했을 때 발생하는 문제로, 두 회전축이 겹쳐 한 축에 대한 자유도를 잃어버리게 되는 것이다.<br>
(**즉, 두 축이 겹치게되어 한 축을 소실하게 되는 문제를 일컫는다.**)<br>

겹쳐진 두 개의 축을 더 이상 움직일 수 없게 된다는 의미가 아니라 그 두 개가 하나의 축으로 합쳐져서 각각 회전시켜도<br>
**같은 축을 기준으로 회전하게 되는 것**을 의미한다.<br> 

▶ 두 축이 겹쳐지며 일어날 수 있는 상황들은 다음과 같다.
1. 물체를 회전시켰을때의 좌표축이 회전하기 전의 좌표축과 일치하게된다면 데이터가 소실된 것.
2. 겹쳐진 축에 의해 원하는 방향으로 제대로 회전할 수 없게 된다.<br>

y-x-z 계층구조로 이루어진 경우를 예로들어보자면, x축을 90도로 회전하게되면 z축이 따라 회전하게되며 y축과 겹치게 된다.<br>
이때 y축 z축으로 각 회전값을 줬을때 전혀 회전하지 않는 경우가 생길 수 있다. 이는 두 좌표축이 겹쳐있기 때문이다.<br>
이는 **1번처럼** 데이터가 소실된 것으로 볼 수 있다.<br>

또한 회전하기 전 z축은 좌우를 둘러보는 역할을 할 수 있었는데, x축의 회전에 의해 y와 z축이 겹치게 된다면<br>
더 이상 좌우를 둘러볼 수 없게 된다. 이는 **2번처럼** 원하는 방향으로 제대로 회전할 수 없음을 의미한다.<br>

물론 두번째 축을 다시 회전시킴으로써, 겹쳐진 두 축을 분리해낼 수 있기 때문에 특정방향으로 아예 회전시킬 수 없는 것은 아니다.<br>
하지만 축의 변화를 고려하기 힘들어 연속적인 변환에 있어서는 오히려 오일러는 직관적이지 않다는 것이다.<br>
<br>

**그렇다면 어떻게 짐벌락 문제를 해결할 수 있을까?** <br>
우선 짐벌락을 완벽하게 해결할 수는 없다고 한다. 고로 우리는 회피할 수 있는 방식을 찾아야 한다.<br>

1. 오일러 각 회전의 순서를 바꿔보자.
1) 가장 자주 회전하게 될 축을 먼저 회전한다. <br>
2) 가장 회전을 안하게 될 축을 2번째로 회전한다. <br>
3) 남은 축을 회전한다. <br>

순서에 의한 짐벌락 현상은 두번쨰 회전하는 축이 90도(-90도)를 회전하게 되면 1번째 축과 3번째 축이 겹치면서 발생한다.<br>
즉, 순서를 바꿔서 최대한 피하는 방법은 두번째 회전하는 축이 90도가 되지 않으면 된다.<br>
다시 말해 **두번째 회전하는 축이 회전을 자주하지 않으면 90도가 될 확률이 낮기에 위와같은 순서**로 회전한다.<br>
<br>
<br>

2. 쿼터니언을 사용하자.<br>
위 방식은 임시방편과 같은 해결법이다. 우리는 쿼터니언이라는 사원수를 사용해서 짐벌락을 효율적으로 피할 수 있게된다.<br>
**아래 설명**<br>
<br>
<br>

## Quaternion
쿼터니언이란 3차원 그래픽에서 회전을 표현할 때, 행렬 대신 사용하는 수학적 개념으로 **4개의 값으로 이루어진 복소수 체계**이다.<br>
이러한 사원수를 사용하는 이유는 행렬에 비해 연산속도가 빠르고, 차지하는 메모리의 양도 적으며 오류가 날 확률이 적기 때문이다.<br>
또한 쿼터니언은 **각 축을 한꺼번에 계산하기 때문에 짐벌락 문제가 발생하지 않는다.**<br>

Euler angle 과는 다르게 쿼터니언은 4개의 성분(x, y, z, w)으로 이루어져 있다.<br>
해당 성분은 벡터(x, y, z)와 스칼라(w)를 의미한다.<br>

쿼터니언은 내부가 수학적으로 복잡하게 구현되어있어 이를 제대로 이해하지 못한다면 자유자재로 다루기는 상당히 까다롭다.<br>
이러한 쿼터니언의 회전은 한 orientation 에서 다른 orientation 으로 측정하기에 **180 보다 큰 값을 표현할 수 없다는 단점이 있다.**<br> 
이 점이 쿼터니언을 직관적으로 이해할 수 없는 큰 이유 중 하나이다.<br>

다행히 유니티에는 이러한 쿼터니언을 간단하게 사용 가능하도록 만들어진 함수들이 다양하게 존재한다.<br>
유니티 공식 문서에도 쿼터니언은 쓰기 어려우니 공식 레퍼런스를 사용하도록 권장한다.<br>
<br>
<br>

▶ API <br>
1. Quaternion.Euler<br>
Quaternion.Euler 함수를 통해서 오일러각을 쿼터니언으로 변경시켜 사용한다.<br>
해당 함수 인자에 오일러각을 넣으면 쿼터니언으로 변환된 값을 반환시켜준다. <br>

```c#
public static Quaternion Euler(float x, float y, float z);

//Ex)
transform.roation = Quaternion.Euler(new Vector3(120,60,100)); 
```

<br>

2. Quaternion.LookRotation<br>
첫 번째 인자에 방향벡터를 입력하면 해당 방향을 바라보게 된다.<br>
어떠한 타겟을 향해 회전시키고 싶다면 다음과 같이 사용하면 된다.<br>

```c#
public static Quaternion LookRotation(Vector3 forward, Vector3 upwards = Vector3.up);

//Ex)
Vector3 direction = (traget.position - this.transform.position).normalized;
Quaternion lookRotation = Quaternion.LookRotation(direction);
this.transform.rotation = lookRotation;
```

<br>

3. Quaternion.Slerp<br>
Quaternion.Slerp 함수는 두 쿼터니언의 중간값을 리턴 시켜준다. (구면선형보간법 기반)<br>

```c#
public static Quaternion Slerp(Quaternion a, Quaternion b, float t);

//Ex)
transform.rotation = Quaternion.Slerp(A.transform.rotation, B.transform.rotation, Time.deltaTime);
```

<br>

4. Quaternion.FromToRotation<br>
FromToRotaion 함수는 fromDirection 의 방향벡터를 toDirection 으로 회전한 쿼터니언을 반환한다.<br>

```c#
public static Quaternion FromToRotation(Vector3 fromDirection, Vector3 toDirection);

//Ex) 아래를 싱행하면 z축으로 90도 회전한 결과가 나오게 됨.
transform.rotation = Quaternion.FromToRotation(Vector3.up, Vector3.right);
```

<br>
<br>

## 참고링크
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dj3630&logNo=221447943453 <br>
https://hub1234.tistory.com/21 <br>
