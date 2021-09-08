## Main Concept
물체가 `충돌` 했다는 것은 수학적으로 해석하면 두 다면체나 다각형에<br>
**동시에 포함되는 공간이 존재한다고 생각할 수 있다.**<br>
그래서 물체 충돌 알고리즘이라는 것도 **두 도형이 겹쳤는지를 판단하는** 문제를 해결하는 방법이라고 보면 된다.<br>
일단 게임내의 물체의 **충돌을 판정하기 위해서는 충돌면이 필요하다.**<br>
<br>

![충돌](https://user-images.githubusercontent.com/43705434/132451699-dbe8906b-e2e8-46ea-8bb3-e85966c88d0a.PNG)<br>
<br>

그 물체를 이루는 모든 폴리곤으로 충돌을 체크한다면 정말 정교한 충돌을 구현할 수 있겠지만,<br>
몇천 폴리곤이나 되는 물체의 폴리곤 하나하나를 검사한다는 것은 정말 **엄청난 과부하**를 가져올 것이다.<br>
그래서 **보통은 물체를 감싸는 단순한 형태의 충돌면을 정의**하여 그 충돌면끼리의 충돌을 체크한다.<br>
이때 **구모양이나 실린더, 사각형 등**의 형태의 충돌면이 쓰인다.<br>
<br>

충돌 검사 알고리즘에는 여러 방식이 존재하지만 **대표적인 4가지**만 다루어 보도록 하겟다.<br>
<br>
<br>

## Pixel Collision Detection (픽셀 검사)
두 오브젝트의 모든 픽셀을 일일이 비교하여 충돌했는지 검사하는 방법이다.<br>
어떻게보면 **제일 정확한 방식이지만, 제일 느린 방법이기 때문에**<br>
충돌 감지를 아주 적게 필요로하는 경우에만 사용해야 한다.<br>
대표적으로 포트리스에서 사용하고 있다.<br>
<br>
<br>

## Bounding Sphere (원, 구)

![구충돌](https://user-images.githubusercontent.com/43705434/132451702-6f7a20a2-7818-4cba-b676-a9dec6a619a0.PNG)<br>
<br>

오브젝트를 원(구 포함)으로 감싼 후 충돌을 체크하는 방식이다.<br>
**두 오브젝트의 원의 중심 사이의 거리와 두 원의 반지름의 합의 대소관계를 통해 충돌을 감지하게 된다.**<br>
두 원의 중심 사이의 거리를 피타고라스를 통해 구하고, 해당 값이 두 원의 반지름의 합보다 작거나 같으면<br>
충돌인 것이다. (두 원 사이의 거리 <= 두 원의 반지름의 합)<br>
-> **정확하지는 않지만 제일 빠른 방식이다.**<br>

제곱근은 연산량이 크기 때문에, 아래와 같이 최적화하여 사용할 수 있다.<br>
```c#
(x2 - x1)^2 + (y2 - y1)^2 <= (A 반지름 + B 반지름)^2
-> 충돌!
```
<br>

Bounding Box를 쓰느냐 Sphere을 쓰느냐의 문제는 그 물체가 박스로 근사시킬 때<br>
유리한가 구로 근사시킬때 유리한가의 문제라고 볼 수 있다.<br>
+ **정확한 충돌체크**를 해야하는가 그렇지 않은가의 문제도 선택의 영향을 줄 수 있다.<br>
<br>
<br>

## AABB
**AABB는 Axis-Aligned Bounding Box의 줄임말로 기저 축에 정렬(평행)된 충돌 박스를 사용하는 방식이다.**<br>
박스를 이루는 모든 사각형의 축들이 평행하기 때문에 해당 축으로 검사하면 되기에<br>
**충돌 검출이 편리하다. 즉 계산량이 비교적 적다.**<br>
<br>

![AABB](https://user-images.githubusercontent.com/43705434/132451707-4bc0d083-093a-48da-8e51-dbadc370c9c5.PNG)<br>
<br>
**하지만 오브젝트가 회전을 하더라도 사각형의 축은 변할 수 없기에, Box의 크기를 변하게 한다.**<br>
이러한 특성 때문에 대부분의 경우 **Box와 오브젝트 형태의 차이가 심하여 정확한 충돌 검출이 어렵다.**<br>
-> 또한 오브젝트가 회전함에 따라 **Box의 크기를 재설정해야 하기 때문에,<br>
-> 앞선 방식들에 비교해 성능상 부담**이 될 수 있다.<br>

그렇기 때문에 주로 움직이지 않는, 회전하지 않는 물체들에 적용하는 것이 대부분이다.<br>
<br>
<br>
<br>

![AABB좌표2](https://user-images.githubusercontent.com/43705434/132451708-0645057b-910c-459d-ba17-b4f702e1bbc8.PNG)<br>
<br>
AABB를 설정하려면, 해당 물체의 **x, y, z축 방향으로의 최대, 최소값만 알면 된다.**<br> 
그 값들의 조합으로 나오는 **8가지의 점( x값 2개 * y값 2개 * z값 2개 = 8개 )으로<br>
정의된 상자가 그 물체의 AABB다.**<br>
<br>
<br>

![AABB변이겹치는가](https://user-images.githubusercontent.com/43705434/132451710-7eaad956-624f-4c27-840e-65acbc7d3417.PNG)<br>
<br>
충돌체크 방식은 간단하다. **두 Box의 각 x,y,z축 변이 모두 겹치는지 체크하면 된다.**<br>
x, y, z 축이 모두 겹치게 된다면 두 물체는 충돌된 상태이다.<br>
이 때 위에서 AABB를 설정하기 위한 **최소 좌표, 최대 좌표 2가지만 기억하고 있다면<br>
아래와 같이 쉽게 계산 할 수 있다.**<br>
<br>

```c#
void CheckAABB(BoundingBox targetBox) 
{ 
  //x, y, z축 변이 모두 겹치는가?
  if (boundingBox.minPos[0] <= targetBox.maxPos[0] && boundingBox.maxPos[0] >= targetBox.minPos[0]
  && boundingBox.minPos[1] <= targetBox.maxPos[1] && boundingBox.maxPos[1] >= targetBox.minPos[1]
  && boundingBox.minPos[2] <= targetBox.maxPos[2] && boundingBox.maxPos[2] >= targetBox.minPos[2]) 
  { 
    aabbCollide = true;
    return; 
  }
  
  aabbCollide = false; 
}
```
<br>

조금 해설을 해보자면, x축에 대해서는  A오브젝트의 A_minX 보다 B오브젝트의 B_maxX가 크거나 같고<br>
A_maxX 보다 B_minX가 작거나 같아야만 X축 변이 겹치는 경우이다.<br>
이는 y, z 축에도 마찬가지로 작용한다. 고로 3가지 경우에 모두 해당한다면<br>
충돌 처리를 해주면 된다.<br>
<br>
<br>

이러한 AABB 방식은 **터널링 문제** (물체를 뚫고 지나가는 현상)를 갖고 있다고 한다.<br>
해당 문제는 **오브젝트의 현재 속도를 이용하여 다음 프레임에 위치할 곳을 예상하고<br>
현재 위치부터 해당 위치까지 AABB 박스를 팽창시켜버리면 이동 궤도에 존재하는<br>
물체에 대해서 충돌을 검출 할 수 있다.**<br>
[speculative **CCD** 방식](https://docs.unity3d.com/kr/2019.4/Manual/ContinuousCollisionDetection.html)<br>
<br>
<br>

## OBB
<br>
<br>

## Collision Culling
하복 엔진같은 경우 게임상의 모든 오브젝트에 대해서 충돌 검출을 실시간으로<br>
연산하기에는 무리가 있기에 다음과 같은 방식을 사용한다고 한다.<br>

1. 1차적으로 AABB를 통해 충돌 검출 연산이 필요하지 않은 오브젝트들을 걸러낸다.<br>
2. 2차적으로 충돌 처리를 위한 복잡도를 낮추기 위해, 충돌 지점 주위의 몇가지 폴리곤들로만 추려낸다.<br>
3. 3차적으로 실제 충돌 검출 연산을 진행한다.<br>
<br>

이렇게 **충돌검사를 간략화하고 필요없는 부분에 대해 생략하는 것을 Collision Culling이라고 한다.**<br>
충돌연산의 최적화에 있어서 이것은 정말 중요하다.<br>
연산시간을 줄이는 가장 좋은 방법은 아예 연산을 하지 않는 것이기 때문이다.<br>
<br>
<br>

**직접 적용해 볼 수 있는 컬링 예**<br>
오브젝트에 Bounding Box를 씌우고, 그 위에 Sphere를 한번 더 씌워서 다음과 같이 사용한다.<br>

**1차적으로 구 영역으로 충돌을 검출하고**, 만약 충돌이 감지되었다면 그 안쪽의 Box를 이용하여<br>
**정밀 검출**을 하도록 한다. 구와 충돌이 되지 않았다면 더 이상 충돌 체크를 진행하지 않는다.<br>

Sphere를 통한 충돌 체크는 비용이 굉장히 적게 들기 때문에 이 방식으로 **1차적으로 미충돌을<br>
걸러내게 되면**, Box에는 당연히 충돌되지 않을 것이기 때문에 **연산을 생략할 수 있어<br>
연산의 최적화**를 이룰 수 있게 된다.<br>
<br>
<br>

## 참조링크
https://blog.naver.com/PostView.nhn?blogId=jerrypoiu&logNo=221172549241 <br>
https://blog.naver.com/wjdalsdl1016/220768308382 <br>
https://handhp1.tistory.com/6 (AABB) <br>
http://lab.gamecodi.com/board/zboard.php?id=GAMECODILAB_QnA_etc&no=5631 (Collision Culling) <br>
https://3dev.tistory.com/53 (Collision Culling) <br>
<br>
<br>
