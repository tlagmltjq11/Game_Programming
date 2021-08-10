## 🔔 Move Component
👉 **Transform**<br>
직접 포지션값을 넣어 평행이동을 시키거나 translate 등의 기능을 사용하여 오브젝트를 이동 시킬 수 있다.<br>
**translate는 옵션에따라 로컬좌표 기준 이동, 월드좌표 기준 이동으로 나뉘게 된다.**<br>
<br>

👉 **CharacterController**<br>
지정된 값에 의한 (월드)좌표이동을 한다. 옵션조정을 통해 게임에 맞는 속도와 움직임을 만들기가 편하다.<br>	
앞서 말했듯이 rigidbody를 사용하지 않기 때문에, **중력효과등의 연출**을 위해서 **별도의 처리과정**이 필요하게 된다.<br>
⭐ **CharacterController는 내부적으로 Rigidbody를 가지고 있기 때문에 따로 Rigidbody Component를 추가할 필요는 없다.<br>
그리고, CharacterController의 충돌체크는 OnControllerColliderHit(hit:ControllerColliderHit)를 사용할 수 있다.**<br>

[참조]<br>
OnControllerColliderHit(hit:ControllerColliderHit)<br>
⭐ **CharacterController 가 Move 함수로 이동하는 도중 다른 Collider 와 충돌했을 때 호출됩니다.**<br>
(CharacterController 가 붙어있는 gameObject 를 Translate 로 움직여서 충돌할 때는 호출되지 않습니다.<br>
무조건 CharacterController 의 **Move 함수로 움직일 때만 호출됩니다.**)<br>
CharacterController 는 이전 충돌 함수 때처럼 따로 Rigidbody, Collider 가 필요없습니다.<br>
-> 내부적으로 지니고 있기 때문<br>
<br>

👉 **RigidBody**<br>
⭐ **물리를 이용한 이동을 하며, 충돌처리에도 필수적으로 이용된다.**<br>
게임은 비현실적인 움직임이 많아 이런 부분에 대한 미세 조정을 따로 해줘야 한다.<br>
- 벽을 타고 내려올때 자유낙하가 아닌 마찰에 의해 낙하를 해서 원하는 움직임이 나오지 않는다.<br>
- 점프나 낙하시 원하는 속도로 이동하지 않기 때문에 velocity를 프레임마다 조정해줘야 한다.<br>
- 다른 컴포넌트와 달리 FixedUpdate 루틴을 사용하기 때문에 Rigidbody 를 이용하지 않는 오브젝트와 Update() 를 일치시켜줘야 한다.<br>

실제 물리와 동일하게 움직이기 때문에 캐릭터도 **비현실적인 속도와 크기를 가질 수 없다.**<br>
(ex 스케일 100인 캐릭터는 100미터인 거인과 동일해짐)<br>
⭐ **원하는 움직임을 만들기 위해 조정하는 값들을 셋팅하기 어렵다.**<br>
<br>

👉 **NavMeshAgent**<br>	
길찾기, 장애물사이를 자연스럽게 이동하게 만든다.<br>
이동할수 없는 곳은 OffMeshLink를 이용해 불가능한 이동도 구현 가능.<br>
이동가능한 영역이 정해져 있고, 길을 찾는 로직이 포함되서 AI작업이 편리해진다.<br>
이동시 Agent간의 우선순위를 둘수 있어 적과 아군 이동 간섭을 구분지을 수 있다.<br>
⭐ **지정된 위치로만 이동가능하기 때문에 점프와 같은 행동을 정의하기 어렵다(NavmeshAgent컴포넌트를 끄고 점프 한다던지)**<br>
기본적인 충돌처리를 위해 Rigidbody가 따로 필요하다.<br>
<br>
<br>

출처<br>
1. https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dieofflee&logNo=220716748687 <br>
2. https://openplay.tistory.com/3 <br>
<br>
<br>
