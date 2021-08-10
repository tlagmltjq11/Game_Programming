## 🔔 Cinemachine
시네머신은 카메라의 움직임을 손쉽게 제어할 수 있는 유니티 공식 패키지이다.<br>
-> ⭐ **복잡한 코드없이 간편하게 다양한 카메라 연출을 사용할 수 있다.** ⭐<br>
<br>
<br>

👉 **Camera**<br>

* Brain 카메라<br>
게임 월드를 ⭐ **실제로 촬영**하는 카메라로 씬 안에 단 하나만 존재해야 한다.<br>

* Virtual 카메라<br>
Brain 카메라의 분신 역할이 되며 씬의 여러 곳에 존재한다.<br>
실제 카메라로 동작하지는 않으며, 단지 Brain 카메라가 사용할 여러가지 ⭐ **설정값을 제공한다.**<br>

⭐⭐ **Brain 카메라는 Virtual 카메라들 중 하나를 선택해서 현재 활성화 된 카메라로 지정하여 해당 Virtual 카메라로 이동하여 촬영한다.<br>
즉 가상 카메라들은 Brain 카메라가 특정 상황에 사용해야 할 설정값들을 미리 지정해두는 역할을 하는 것이다.** ⭐⭐<br>
<br>

Brain 카메라가 선택한 Virtual 카메라로 이동하는 과정에서 시점은 부드럽게 이동된다.<br>
-> Virtual 카메라의 설정값들이 Brain 카메라의 설정 값이 됨.<br>
<br>

* FreeLook 카메라<br>
Virtual Camera에서 좀 더 **확장된 카메라로 3인칭 카메라**를 위한 파라미터들이 추가로 구현되어 있다.<br>
<br>

⭐ 확장되어 구현된 가상 카메라들이 이외에도 많이 존재함.<br>
<br>
<br>

👉 **Field**<br>

* Follow 필드<br>
가상 카메라는 Follow 필드에 **할당된 게임 오브젝트를 따라다닌다.**<br>
따라다니기 위해서 가상 카메라의 ⭐ **위치**를 변경한다.<br>
<br>
 
* LookAt 필드<br>
Look At 필드에 **할당된 게임 오브젝트를 주시한다.**<br>
주시하기 위해서 가상 카메라의 ⭐ **회전**을 변경한다.<br>
<br>
<br>

👉 **Zone**<br>

![시네머신](https://user-images.githubusercontent.com/43705434/126930059-4bc96072-309e-4d7e-b923-7e821d750b24.PNG)<br>
<br>

Look at으로 주시할 대상을 할당하면 게임창 화면에 그리드가 보이게 되는데 각 칸 영역을 다음과 같이 부른다.<br>

* DeadZone (정 가운데 1칸)<br>
주시 대상(노란 점)이 데드존 내에서 움직이는 동안에는 카메라가 회전하지 않는다.<br>

* SoftZone (가운데 8칸)<br>
⭐ **SoftZone, HardLimit은 가상 카메라의 자연스러운 추적을 위해 사용된다.** ⭐<br>
주시 대상(노란 점)이 소프트존 내에 있다면 **카메라가 부드럽게 회전하여 주시 대상이 데드존 안으로 들어가게끔 한다.**<br>

* HardLimit (테두리)<br>
**주시하는 물체가 빠르게 움직여서 소프트존을 벗어나 하드리밋에 도달하면 카메라가 격하게 회전한다.**<br>
<br>

데드존과 소프트존의 크기를 줄이면 물체가 조금이라도 화면 중앙을 벗어나려 할 때 지연시간없이 카메라가 즉시 물체를 향해 회전한다.<br>
하지만 화면의 움직임이 딱딱하게 느낄 수 있다.<br>
데드존과 소프트존의 크기를 늘리면 물체를 쫓는 화면의 움직임이 느리게 느껴진다.<br>
<br>
<br>

👉 **Camera Collider**<br>

![시네머신콜라이더](https://user-images.githubusercontent.com/43705434/126930062-80a5de9a-8487-4bb3-b224-c28d85c791ee.PNG)<br>
<br>

⭐ **3인칭 카메라가 플레이어를 따라다니다가 벽이나 다른 물체들에 파묻히거나 가려지는 경우를 해결해주는 컴포넌트이다.** ⭐<br>
카메라에게 가상의 Collider 할당하여 오브젝트와의 충돌을 감지 후 다음과 같은 상황들을 해결할 수 있음.<br> 
<br>


1. 충돌되는 물체에 파묻히지 않도록 벽에서 밀려나와 촬영한다.<br>

![카메라콜라이더1](https://user-images.githubusercontent.com/43705434/126931585-8dc6188c-d791-4a1a-a5c1-7c8c99df12bb.PNG)<br>
<br>

2. 카메라와 플레이어 사이에 있는 물체가 카메라를 가리지 않도록 물체를 넘어가서 촬영한다.<br>

![카메라콜라이더2](https://user-images.githubusercontent.com/43705434/126931586-4989d6c4-1af8-46d7-9912-9878e0bc9f34.PNG)<br>
<br>
<br>

카메라가 플레이어는 피하면 안되니까 Ignore Tag에서 Player를 선택해준다.<br>
<br>
<br>