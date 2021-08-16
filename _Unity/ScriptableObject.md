## 🔔 ScriptableObject
**스크립터블 오브젝트(Scriptable Object)는 유니티에서 제공하는 대량의 데이터를 저장하는 데 사용할 수 있는 데이터 컨테이너이다.**<br>
스크립터블 오브젝트를 사용하면 **값의 사본이 생성되는 것을 방지하여 프로젝트의 메모리 사용을 줄일 수** ⭐ 있으며<br>
이것은 MonoBehaviour 스크립트에 **변경되지 않는 데이터를 저장하는 프리팹을 사용하는 프로젝트에서 유용하다.**<br>
변경되지 않는 데이터를 사용하는 프리팹의 데이터를 일반 변수로 구현할 경우 **인스턴스화 할때마다 프리펩에 이 데이터에 대한<br>
자체 사본이 생성**되는데, 스크립터블 오브젝트를 사용하면 메모리에 스크립터블 오브젝트의 데이터 사본만을 저장하고<br>
이를 **참조하는 방식으로 작동** ⭐ 한다.<br>

스크립터블 오브젝트 클래스는 모노비헤이비어 클래스와 마찬가지로 기본 유니티 오브젝트(Unity Object)에서 파생되지만,<br>
모노비헤이비어와 달리, 게임 오브젝트에 컴포넌트로 부착할 수 없고, **프로젝트에 에셋으로 저장된다.** ⭐<br>

* 유니티 오브젝트를 상속받은 클래스로 **Unity 시리얼라이즈 시스템**을 가지고 있다.<br>
(시리얼라이즈는 클래스나 오브젝트를 **스트링이나 일차원 배열 형태로 변환하는 것**을 얘기함)<br>
-> 실제 파일을 열어보면 저장된 텍스트 내용을 확인할 수 있다.<br>

```
%YAML 1.1
%TAG !u! tag:unity3d.com,2011:
--- !u!114 &11400000
MonoBehaviour:
  m_ObjectHideFlags: 0
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_GameObject: {fileID: 0}
  m_Enabled: 1
  m_EditorHideFlags: 0
  m_Script: {fileID: 11500000, guid: c9f7761a9a0c6f840a7561b294321975, type: 3}
  m_Name: OrcStat
  m_EditorClassIdentifier: 
  moveSpeed: 10
  view: 20
  atk: 50
  def: 60
```
<br>
<br>

## 🔔 ScriptableObject의 특징
👉 유니티 콜백 중 **OnEnable OnDisable OnDestroy** 만 동작한다.<br>
* OnEable => ScriptableObject가 instantiated/loaded 될 때,<br>
ScriptableObject.CreateInstance() 함수 호출 시,<br>
에디터에서 스크립트가 컴파일 된 후에 호출<br> 
* OnDisable => ScriptableObject가 파괴되기 전에 호출,<br>
에디터에서 스크립트가 컴파일 되기 전에 호출<br>
* OnDestroy => ScriptableObject가 파괴 될때 ( Object.Destroy )<br>
<br>

👉 어셋은 인스터싱이 되지 않고 **고유한 하나의 파일로 저장된다.**<br>

👉 **어셋의 내용을 변경하면 모든 참조하는 곳에 동일하게 변경된 내용이 반영된다.** ⭐<br>

👉 ScriptableObject는 기존에 없던 시스템을 Unity가 새로 만든 것은 아니다.<br>
**Json, xml, csv 등으로 관리하던 데이터를 Unity에서 쉽게 사용할 수 있도록 ⭐ 편의적인 기능으로 제공하는 것으로 이해할 수 있다.**<br>

👉 소규모의 데이터를 편하게 관리할 수 있는 장점이 있지만, 대용량의 데이터를 ScriptableObject로 관리하기엔 어려움이 있을 것으로 생각<br>
**대규모의 데이터는 기본적으로 엑셀로 관리하는 것이 편하기 때문이다. ⭐** 만약 ScriptableObject로 데이터를 관리한다고 할 경우 프로젝트에 맞는 툴 개발이 필요할 것이다.<br>

👉 **외부 데이터를 파싱해서 사용하는 번거로움을 피하고, 데이터의 관리 부하가 크지 않다면 ScriptableObject를 사용하는 것이 편하고 ⭐ 최적화에도 도움이 될 것으로 생각한다.**
또는 에디터에서 편하게 관리가 필요한 데이터만 ScriptableObject를 사용하는 것도 좋은 개발 방향이 될 수 있을 것으로 생각된다.<br>

👉 에디터에서는 스크립터블 오브젝트에 데이터를 저장하는 작업이 언제나 가능하지만, **배포된 빌드에서는 데이터를 저장할 수 없고<br>
개발시 설정한 스크립터블 오브젝트 에셋에 저장된 데이터만을 사용할 수 있다.** ⭐<br>

👉 그리고 스크립터블 오브젝트는 에셋 파일 형태로 관리되기 때문에 **에셋번들 태그를 이용해서<br>
에셋 번들로 빌드하고 배포하는 방식으로 게임 데이터를 업데이트시키는데 사용할 수도 있다.**<br>
-> 빌드파일에 포함되기 때문에 **유저들이 조작할 가능성이 존재한다. ⭐ 고로 멀티 게임 같은 경우 중요 데이터를 스크립터블 오브젝트로 운용해서는 안된다.**<br>
<br>
<br>

<br>
<br>

## 🔔 ScriptableObject 활용 시 장점
* 데이터 컨테이너로 **데이터 공유**에 사용할 수 있고, 이를 통해 **메모리 사용량 줄일 수 있다.** ⭐⭐<br>
기존 시스템은 하나의 컴포넌트에 변수를 정의하면 프리팹을 인스터싱해서 만드는 만큼 메모리 할당이 발생하지만<br>
**ScriptableObject는 데이터를 참조 형태로 오브젝트에서 가져올 수 있게 함으로써 메모리 할당을 최소화하고 고유한 값을 가질 수 있게한다.**<br>
프리펩에서 변화가 없는 동일한 데이터를 사용하는 경우, ScriptableObject로 해당 데이터를 관리하면 **편리성과 최적화 효과**를 얻을 수 있다.<br>
<br>
<br>

![1](https://user-images.githubusercontent.com/43705434/129469410-d3cd7408-35dc-43a7-a4ec-a08bf61b8517.PNG)<br>
<br>
<br>

## 🔔 ScriptableObject 사용법
1. ScriptableObject 코드를 작성한다.<br>

```c#
using UnityEngine;
// 에디터에서 쉽게 사용할 수 있도록 메뉴를 만든다.
[ CreateAssetMenu( fileName = "MonsterStat", menuName = "Scriptable Object Asset/MonsterStat" )]
public class MonsterStat : ScriptableObject
{
	public int moveSpeed = 10;
	public int view = 20;
}
```
<br>
<br>

![2](https://user-images.githubusercontent.com/43705434/129469412-808779f9-30fd-48af-98f4-825ac6c15bee.PNG)<br>
<br>
<br>

2. MonsterStat 이라는 ScriptableObject를 Menu를 통해 생성한다.<br>

![3](https://user-images.githubusercontent.com/43705434/129469415-a0244d19-23dd-44f4-8288-56fead5108dd.PNG)<br>
<br>
<br>

3. 사용하려는 오브젝트와 컴포넌트를 만들고 MonsterStat ScriptableObject를 연결 시킨다.<br>

```c#
// Orc.cs
public class Orc : MonoBehaviour
{
	public MonsterStat stat;

	private void OnEnable()
	{
		Debug.LogWarning( " Orc - MoveSpeed : " + stat.moveSpeed + " view : " + stat.view );
	}
}

// Human.cs
public class Human : MonoBehaviour
{
	public MonsterStat stat;

	private void OnEnable()
	{
		Debug.LogWarning( " Human - MoveSpeed : " + stat.moveSpeed + " view : " + stat.view );
	}
}

// Elf.cs
public class Elf : MonoBehaviour
{
	public MonsterStat stat;

	private void OnEnable()
	{
		Debug.LogWarning( " Elf - MoveSpeed : " + stat.moveSpeed + " view : " + stat.view );
	}
}
```
<br>
<br>

![4](https://user-images.githubusercontent.com/43705434/129469417-a3eaae25-ca7f-4dbb-8d34-a344659c7b4b.PNG)<br>

![5](https://user-images.githubusercontent.com/43705434/129469418-c132a8b8-eee6-469f-bdb3-9be05b66195a.PNG)<br>
<br>
<br>

4. 만약 연결한 데이터의 값을 변경하는 경우, 변경된 데이터 값이 참조하는 모든 오브젝트에 동일하게 적용 된다.<br>

![6](https://user-images.githubusercontent.com/43705434/129469419-80d7e15c-b2c9-456e-a171-6bed0815ecc6.PNG)<br>

![7](https://user-images.githubusercontent.com/43705434/129469420-a1b05ffc-a287-4750-a1f9-e452cf08c221.PNG)<br>
<br>
<br>

5. ScriptableObject는 상속해서 사용도 가능하다.<br>

```c#
using UnityEngine;

[CreateAssetMenu( fileName = "MonsterStat", menuName = "Scriptable Object Asset/Orc Stat" )]
public class OrcMonsterStat : MonsterStat
{
	public int atk;
	public int def;
}
```
<br>

![8](https://user-images.githubusercontent.com/43705434/129469421-bac45e71-ed31-4fdf-82da-a1208ab2d2b9.PNG)<br>
<br>
<br>

6. 다른 ScriptableObject를 변수로 사용할 수 있어 데이터의 **조합**도 가능하다.<br>
-> OrcStat내에 MonsterStat 스크립터블 오브젝트를 변수로 등록한 것.<br>

![9](https://user-images.githubusercontent.com/43705434/129469422-d2dd634b-4459-4498-a72d-c74b123048f6.PNG)<br>
<br>
<br>

## 🔔 참조링크
https://everyday-devup.tistory.com/53 <br>
https://www.youtube.com/watch?v=7Qt4QNhM4nY <br>
