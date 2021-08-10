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

자세한 설명은 [다음 링크](https://github.com/tlagmltjq11/Math_and_Graphics/blob/main/%EA%B7%B8%EB%9E%98%ED%94%BD%EC%8A%A4%20%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8/Euler%20and%20Quaternion.md) 를 통해 확인하자<br>
<br>
<br>
