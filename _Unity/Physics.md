## 🔔 Physics

👉 **Ray, Box, Capsule, Sphere Cast**<br>
RayCast는 직선 상에 가상의 빔을 쏘아서 충돌하는 Collider가 있는 지 체크하는 함수이다.<br>
이와 비슷한 기능으로 BoxCast, SphereCast, CapsuleCast가 있는데, 이것도 모두 RayCast의 일종이다.<br>
앞에 있는 Box, Sphere, Capsule의 형태로 RayCast를 한다고 보면 된다.<br>

기즈모를 통해 가시적으로 확인해보면 다음과 같다.<br>
<br>

![cast](https://user-images.githubusercontent.com/43705434/128006559-9dbd221e-0a68-4e1f-934c-2b8d025c2e4d.PNG)<br>
<br>

**true or false를 반환한다.**<br>
**out 키워드를 사용해서 RayCastHit을 받아와야 한다.**<br>
<br>
<br>

👉 **Ray, Box, Capsule, Sphere CastAll**<br> 
위에서 설명한 Cast와 동일하게 뻗어나가지만 All이 붙어 모든 충돌대상을 돌려주기 때문에<br>
사실은 각자의 모양대로 아래 그림과 같이 ⭐ **도형이 늘어져 길게 뻗어나가는 모양을 상상하면 된다.** ⭐<br>

![castall](https://user-images.githubusercontent.com/43705434/128006565-e24c434d-c083-4445-ae39-3b4a79cf9d19.PNG)<br>
<br>

**RayCastHit 배열을 반환한다.** ⭐<br>
<br>
<br>

👉 **Overlap Box, Capsule, Sphere**<br>
매개변수로 넘겨준 포지션과 반경으로 부터 범위 내에 있는 ⭐ **모든 Collider 들을 배열로 리턴한다.** ⭐<br>

**발생할 수 있는 문제**<br>
오브젝트 정면에 OverlapSphere을 이용해서 충돌되는 대상에게 데미지를 주는 코드를 작성한다고 할 때<br>
공격 주체가 1프레임만에 너무 빠르게 움직여버리면 감지가 안되는 문제가 발생 할 수 있다.<br>
아래 그림들을 보면 이해가 될 것이다. 고로 이러한 경우에는 SphereCastAll 또는 SphereCastNonAlloc를 사용해야 한다.<br>
공격 주체가 현재 이동중인 속도에 Time.delatTime을 곱해 1프레임에 얼마나 움직일지 구한 후 해당 값만큼<br>
SphereCastAll을 쏴주면 모두 감지할 수 있을 것이다.<br>

![overlap문제점1](https://user-images.githubusercontent.com/43705434/128006569-e2e502da-50bd-4b81-bd40-77b86512aa01.PNG)<br>
<br>

![overlap문제점2](https://user-images.githubusercontent.com/43705434/128006568-ecb0c7d8-2655-4016-aae7-1631e9b2b49f.PNG)
<br>
<br>
<br>

👉 **LineCast**<br>
**Linecast는 정확한 종료 지점을 알아서 시작~종료 범위 내에서의 충돌을 감지하려 할 때 사용하는 Cast 함수이다.**<br>
-> Raycast는 방향만 알 때 사용하는 것이라 생각하면 쉽게 이해 할 수 있다.<br>

**true or false를 반환한다.**<br>
**out 키워드를 사용해서 RayCastHit을 받아와야 한다.**<br>
<br>
<br>

👉 **NonAlloc이 붙은 메서드들**<br>
해당 메서드들은 All, 혹은 Overlap과 유사하게 동작하지만 내부적으로 다른 차이가 있다.<br>
⭐⭐ **CastAll은 반환할때 RayCastHit[] 배열을 반환하는데, NonAlloc을 사용하지 않으면<br>
매번 해당 배열을 생성해서 반환하기 때문에 성능 상 좋지 않다.<br>
고로 NonAlloc은 사용자가 한번 생성해둔 배열을 매개변수로 넘겨주면 해당 배열에<br>
담아서 보내주는 방식이다. 고로 실행시마다 배열을 생성하지 않아도 되는 성능 상 장점을 가진다.** ⭐⭐<br>

다만 찾아낸 충돌체들의 수가 인수로 넘긴 배열의 사이즈보다 적을 수도 많을 수도 있기 때문에<br>
**리턴으로 주는 int 사이즈, 즉 충돌체들의 개수를 잘 확인해야 한다.** ⭐<br>

**추가 팁**<br>
1. 직전 프레임까지 어떤 RaycastHit 정보가 있었는지를 알 수 있다.<br>

2. SphereCastNonAlloc를 위해 sphere가 움직이려고 하자마자 충돌되는 콜라이더가<br>
존재한다면 즉 실행하기전에 이미 겹쳐져있던 콜라이더가 존재한다면 해당 콜라이더의<br>
hit.point는 zero 벡터, dist는 0으로 나오게 된다.<br>
<br>
<br>
