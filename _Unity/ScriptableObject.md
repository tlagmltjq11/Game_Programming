## π ScriptableObject
**μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈ(Scriptable Object)λ μ λν°μμ μ κ³΅νλ λλμ λ°μ΄ν°λ₯Ό μ μ₯νλ λ° μ¬μ©ν  μ μλ λ°μ΄ν° μ»¨νμ΄λμ΄λ€.**<br>
μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈλ₯Ό μ¬μ©νλ©΄ **κ°μ μ¬λ³Έμ΄ μμ±λλ κ²μ λ°©μ§νμ¬ νλ‘μ νΈμ λ©λͺ¨λ¦¬ μ¬μ©μ μ€μΌ μ** β­ μμΌλ©°<br>
μ΄κ²μ MonoBehaviour μ€ν¬λ¦½νΈμ **λ³κ²½λμ§ μλ λ°μ΄ν°λ₯Ό μ μ₯νλ νλ¦¬νΉμ μ¬μ©νλ νλ‘μ νΈμμ μ μ©νλ€.**<br>
λ³κ²½λμ§ μλ λ°μ΄ν°λ₯Ό μ¬μ©νλ νλ¦¬νΉμ λ°μ΄ν°λ₯Ό μΌλ° λ³μλ‘ κ΅¬νν  κ²½μ° **μΈμ€ν΄μ€ν ν λλ§λ€ νλ¦¬ν©μ μ΄ λ°μ΄ν°μ λν<br>
μμ²΄ μ¬λ³Έμ΄ μμ±**λλλ°, μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈλ₯Ό μ¬μ©νλ©΄ λ©λͺ¨λ¦¬μ μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈμ λ°μ΄ν° μ¬λ³Έλ§μ μ μ₯νκ³ <br>
μ΄λ₯Ό **μ°Έμ‘°νλ λ°©μμΌλ‘ μλ** β­ νλ€.<br>

μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈ ν΄λμ€λ λͺ¨λΈλΉν€μ΄λΉμ΄ ν΄λμ€μ λ§μ°¬κ°μ§λ‘ κΈ°λ³Έ μ λν° μ€λΈμ νΈ(Unity Object)μμ νμλμ§λ§,<br>
λͺ¨λΈλΉν€μ΄λΉμ΄μ λ¬λ¦¬, κ²μ μ€λΈμ νΈμ μ»΄ν¬λνΈλ‘ λΆμ°©ν  μ μκ³ , **νλ‘μ νΈμ μμμΌλ‘ μ μ₯λλ€.** β­<br>

* μ λν° μ€λΈμ νΈλ₯Ό μμλ°μ ν΄λμ€λ‘ **Unity μλ¦¬μΌλΌμ΄μ¦ μμ€ν**μ κ°μ§κ³  μλ€.<br>
(μλ¦¬μΌλΌμ΄μ¦λ ν΄λμ€λ μ€λΈμ νΈλ₯Ό **μ€νΈλ§μ΄λ μΌμ°¨μ λ°°μ΄ ννλ‘ λ³ννλ κ²**μ μκΈ°ν¨)<br>
-> μ€μ  νμΌμ μ΄μ΄λ³΄λ©΄ μ μ₯λ νμ€νΈ λ΄μ©μ νμΈν  μ μλ€.<br>

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

## π ScriptableObjectμ νΉμ§
π μ λν° μ½λ°± μ€ **OnEnable OnDisable OnDestroy** λ§ λμνλ€.<br>
* OnEable => ScriptableObjectκ° instantiated/loaded λ  λ,<br>
ScriptableObject.CreateInstance() ν¨μ νΈμΆ μ,<br>
μλν°μμ μ€ν¬λ¦½νΈκ° μ»΄νμΌ λ νμ νΈμΆ<br> 
* OnDisable => ScriptableObjectκ° νκ΄΄λκΈ° μ μ νΈμΆ,<br>
μλν°μμ μ€ν¬λ¦½νΈκ° μ»΄νμΌ λκΈ° μ μ νΈμΆ<br>
* OnDestroy => ScriptableObjectκ° νκ΄΄ λ λ ( Object.Destroy )<br>
<br>

π μ΄μμ μΈμ€ν°μ±μ΄ λμ§ μκ³  **κ³ μ ν νλμ νμΌλ‘ μ μ₯λλ€.**<br>

π **μ΄μμ λ΄μ©μ λ³κ²½νλ©΄ λͺ¨λ  μ°Έμ‘°νλ κ³³μ λμΌνκ² λ³κ²½λ λ΄μ©μ΄ λ°μλλ€.** β­<br>

π ScriptableObjectλ κΈ°μ‘΄μ μλ μμ€νμ Unityκ° μλ‘ λ§λ  κ²μ μλλ€.<br>
**Json, xml, csv λ±μΌλ‘ κ΄λ¦¬νλ λ°μ΄ν°λ₯Ό Unityμμ μ½κ² μ¬μ©ν  μ μλλ‘ β­ νΈμμ μΈ κΈ°λ₯μΌλ‘ μ κ³΅νλ κ²μΌλ‘ μ΄ν΄ν  μ μλ€.**<br>

π μκ·λͺ¨μ λ°μ΄ν°λ₯Ό νΈνκ² κ΄λ¦¬ν  μ μλ μ₯μ μ΄ μμ§λ§, λμ©λμ λ°μ΄ν°λ₯Ό ScriptableObjectλ‘ κ΄λ¦¬νκΈ°μ μ΄λ €μμ΄ μμ κ²μΌλ‘ μκ°<br>
**λκ·λͺ¨μ λ°μ΄ν°λ κΈ°λ³Έμ μΌλ‘ μμλ‘ κ΄λ¦¬νλ κ²μ΄ νΈνκΈ° λλ¬Έμ΄λ€. β­** λ§μ½ ScriptableObjectλ‘ λ°μ΄ν°λ₯Ό κ΄λ¦¬νλ€κ³  ν  κ²½μ° νλ‘μ νΈμ λ§λ ν΄ κ°λ°μ΄ νμν  κ²μ΄λ€.<br>

π **μΈλΆ λ°μ΄ν°λ₯Ό νμ±ν΄μ μ¬μ©νλ λ²κ±°λ‘μμ νΌνκ³ , λ°μ΄ν°μ κ΄λ¦¬ λΆνκ° ν¬μ§ μλ€λ©΄ ScriptableObjectλ₯Ό μ¬μ©νλ κ²μ΄ νΈνκ³  β­ μ΅μ νμλ λμμ΄ λ  κ²μΌλ‘ μκ°νλ€.**
λλ μλν°μμ νΈνκ² κ΄λ¦¬κ° νμν λ°μ΄ν°λ§ ScriptableObjectλ₯Ό μ¬μ©νλ κ²λ μ’μ κ°λ° λ°©ν₯μ΄ λ  μ μμ κ²μΌλ‘ μκ°λλ€.<br>

π μλν°μμλ μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈμ λ°μ΄ν°λ₯Ό μ μ₯νλ μμμ΄ μΈμ λ κ°λ₯νμ§λ§, **λ°°ν¬λ λΉλμμλ λ°μ΄ν°λ₯Ό μ μ₯ν  μ μκ³ <br>
κ°λ°μ μ€μ ν μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈ μμμ μ μ₯λ λ°μ΄ν°λ§μ μ¬μ©ν  μ μλ€.** β­<br>

π κ·Έλ¦¬κ³  μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈλ μμ νμΌ ννλ‘ κ΄λ¦¬λκΈ° λλ¬Έμ **μμλ²λ€ νκ·Έλ₯Ό μ΄μ©ν΄μ<br>
μμ λ²λ€λ‘ λΉλνκ³  λ°°ν¬νλ λ°©μμΌλ‘ κ²μ λ°μ΄ν°λ₯Ό μλ°μ΄νΈμν€λλ° μ¬μ©ν  μλ μλ€.**<br>
-> λΉλνμΌμ ν¬ν¨λκΈ° λλ¬Έμ **μ μ λ€μ΄ μ‘°μν  κ°λ₯μ±μ΄ μ‘΄μ¬νλ€. β­ κ³ λ‘ λ©ν° κ²μ κ°μ κ²½μ° μ€μ λ°μ΄ν°λ₯Ό μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈλ‘ μ΄μ©ν΄μλ μλλ€.**<br>
<br>
<br>

<br>
<br>

## π ScriptableObject νμ© μ μ₯μ 
* λ°μ΄ν° μ»¨νμ΄λλ‘ **λ°μ΄ν° κ³΅μ **μ μ¬μ©ν  μ μκ³ , μ΄λ₯Ό ν΅ν΄ **λ©λͺ¨λ¦¬ μ¬μ©λ μ€μΌ μ μλ€.** β­β­<br>
κΈ°μ‘΄ μμ€νμ νλμ μ»΄ν¬λνΈμ λ³μλ₯Ό μ μνλ©΄ νλ¦¬νΉμ μΈμ€ν°μ±ν΄μ λ§λλ λ§νΌ λ©λͺ¨λ¦¬ ν λΉμ΄ λ°μνμ§λ§<br>
**ScriptableObjectλ λ°μ΄ν°λ₯Ό μ°Έμ‘° ννλ‘ μ€λΈμ νΈμμ κ°μ Έμ¬ μ μκ² ν¨μΌλ‘μ¨ λ©λͺ¨λ¦¬ ν λΉμ μ΅μννκ³  κ³ μ ν κ°μ κ°μ§ μ μκ²νλ€.**<br>
νλ¦¬ν©μμ λ³νκ° μλ λμΌν λ°μ΄ν°λ₯Ό μ¬μ©νλ κ²½μ°, ScriptableObjectλ‘ ν΄λΉ λ°μ΄ν°λ₯Ό κ΄λ¦¬νλ©΄ **νΈλ¦¬μ±κ³Ό μ΅μ ν ν¨κ³Ό**λ₯Ό μ»μ μ μλ€.<br>
<br>
<br>

![1](https://user-images.githubusercontent.com/43705434/129469410-d3cd7408-35dc-43a7-a4ec-a08bf61b8517.PNG)<br>
<br>
<br>

## π ScriptableObject μ¬μ©λ²
1. ScriptableObject μ½λλ₯Ό μμ±νλ€.<br>

```c#
using UnityEngine;
// μλν°μμ μ½κ² μ¬μ©ν  μ μλλ‘ λ©λ΄λ₯Ό λ§λ λ€.
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

2. MonsterStat μ΄λΌλ ScriptableObjectλ₯Ό Menuλ₯Ό ν΅ν΄ μμ±νλ€.<br>

![3](https://user-images.githubusercontent.com/43705434/129469415-a0244d19-23dd-44f4-8288-56fead5108dd.PNG)<br>
<br>
<br>

3. μ¬μ©νλ €λ μ€λΈμ νΈμ μ»΄ν¬λνΈλ₯Ό λ§λ€κ³  MonsterStat ScriptableObjectλ₯Ό μ°κ²° μν¨λ€.<br>

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

4. λ§μ½ μ°κ²°ν λ°μ΄ν°μ κ°μ λ³κ²½νλ κ²½μ°, λ³κ²½λ λ°μ΄ν° κ°μ΄ μ°Έμ‘°νλ λͺ¨λ  μ€λΈμ νΈμ λμΌνκ² μ μ© λλ€.<br>

![6](https://user-images.githubusercontent.com/43705434/129469419-80d7e15c-b2c9-456e-a171-6bed0815ecc6.PNG)<br>

![7](https://user-images.githubusercontent.com/43705434/129469420-a1b05ffc-a287-4750-a1f9-e452cf08c221.PNG)<br>
<br>
<br>

5. ScriptableObjectλ μμν΄μ μ¬μ©λ κ°λ₯νλ€.<br>

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

6. λ€λ₯Έ ScriptableObjectλ₯Ό λ³μλ‘ μ¬μ©ν  μ μμ΄ λ°μ΄ν°μ **μ‘°ν©**λ κ°λ₯νλ€.<br>
-> OrcStatλ΄μ MonsterStat μ€ν¬λ¦½ν°λΈ μ€λΈμ νΈλ₯Ό λ³μλ‘ λ±λ‘ν κ².<br>

![9](https://user-images.githubusercontent.com/43705434/129469422-d2dd634b-4459-4498-a72d-c74b123048f6.PNG)<br>
<br>
<br>

## π μ°Έμ‘°λ§ν¬
https://everyday-devup.tistory.com/53 <br>
https://www.youtube.com/watch?v=7Qt4QNhM4nY <br>
