## π Main Concept
**Event νΉμ Callbackμ΄λ νΉμ  μ΄λ²€νΈκ° λ°μνμ λ, β­ λ±λ‘ν΄λ λͺ¨λ  λ©μλλ₯Όμ΄ νΈμΆλλ κ²μ μλ―Ένλ€.**<br>
μ΄λ¬ν κ°λμ λ€μκ³Ό κ°μ μν©μμ μκΈ΄νκ² μ¬μ©λ  μ μλ€.<br>

μν©- νλ μ΄μ΄κ° μ£½μμ λ, μ²λ¦¬ν΄μ€μΌ νλ μ¬ν­λ€μ΄ λ§μ κ²½μ° (Text, Reset, Achivement...)<br>
μμ κ°μ μν©μ κ΅¬νν  λ, Event κ°λμ μ¬μ©νμ§ μκ²λλ©΄ λ€μκ³Ό κ°μμ§λ€.<br>

π <br>
```c#
public class UIManager : MonoBehaviour
{
    public Text playerStateText;

    public void OnPlayerDead()
    {
        playerStateText.text = "You Die!";
    }
}

public class AchivementSystem : MonoBehaviour
{
    public Text achivementText;

    public void UnLockAchivement(string title)
    {
        achivementText.text = "λμ κ³Όμ  ν΄μ : " + title;
    }
}

public class GameManager : MonoBehaviour
{
    public void OnPlayerDead()
    {
        Invoke("Restart", 5f);
    }

    private void Restart()
    {
        SceneManager.LoadScene(0);
    }
}

public class PlayerHealth : MonoBehaviour
{
    public UIManager uiManager;
    public AchivementSystem achivementSystem;
    public GameManager gameManager;

    private void Dead()
    {
        uiManager.OnPlayerDead();
        achivementSystem.UnLockAchivement("λ΄ν΄μ λ²μΉ");
        gameManager.OnPlayerDead();

        Destroy(gameObject);
    }
    
    void OnTriggerEnter(Collider other)
    {
        Dead();
    }
}
```

<br>

λ¬Όλ‘  κ΅¬νμλ λ¬Έμ κ° μμ κ²μ΄λ€. νμ§λ§ μμ κ°μ μν©μμ μ¬λ§ μ μ΄λ²€νΈλ₯Ό μΆκ°ν΄μΌ νλ€λ©΄?<br>
-> β­ **μ΄λ²€νΈλ₯Ό μΆκ°ν  λλ§λ€ μ΄λ²€νΈμ κ΄λ ¨λ κ°μ²΄λ€μ μ°κ²°μμΌμ£Όμ΄μΌ νλ©°, λ©μλλ₯Ό λ±λ‘ν΄μ£Όμ΄μΌ νλ€.** β­<br>
μ΄λ νκ°? β­β­ **μ»€νλ§μ΄ λ§μμ§κ³ , μ½λκ° λ³΅μ‘ν΄μ§λ©° μ€νκ²ν° μ½λκ° λκΈ° μ½μ** β­β­ μ΄λΌλ κ²μ μ μ μμ κ²μ΄λ€.<br>
κ³ λ‘ μ΄λ¬ν μν©μμλ μ΄λ²€νΈ κ΄λ ¨ κΈ°λ₯λ€μ μ¬μ©νκΈΈ μ κ·Ή κΆμ₯νλ€.<br>

μ΄λ²€νΈλ μμμ μ€λͺνλ―μ΄, μ¬κ±΄ λ°λμ΄ λλ©΄ κ·Έ μ¬κ±΄μ λ±λ‘μ ν΄λμ κΈ°λ₯λ€μ΄ μλμΌλ‘ λ°λλλ κΈ°λ₯μ΄λ€.<br> 
β­ **νΉμ΄ν κ²μ΄λΌλ©΄ μ΄λ²€νΈλ₯Ό λ°λ μν€λ μΈ‘λ κΈ°λ₯μ λ±λ‘ ν΄λ¨λ μΈ‘λ μλ‘μκ² κ΄μ¬μ΄ μλ€.**<br>
μ΄λ²€νΈμ κΈ°λ₯μ λ±λ‘μν€λ μΈ‘μ κΈ°λ₯μ λ±λ‘ν΄λκ³  κ·Έκ² μ΄λ»κ² λ°λλκ³  μΈμ  λ°λ μν€λμ§ μ ν μ κ²½μ°μ§ μλλ€.<br>
λ§μ°¬κ°μ§λ‘ μ΄λ²€νΈλ₯Ό λ°λ μν€λ μΈ‘λ μκΈ° μ΄λ²€νΈμ μ΄λ€ κΈ°λ₯μ΄ λ±λ‘λμ΄ μλμ§ μ κ²½μ μ μ°κ³  λ°λμν¨λ€.<br>
β­β­ **λ°λΌμ μ»€νλ§μ μ€μΌ μ μμ΄ μ½λκ° κ°λ¨ν΄μ§κ³  μΆν μ μ§λ³΄μμλ μ λ¦¬ν΄μ§λ λ°©μμ΄λΌλ κ²μ μ μ μλ€.** β­β­<br>


π <br>
```c#
public class PlayerHealth : MonoBehaviour
{
    public UnityEvent onPlayerDead;

    private void Dead()
    {
        onPlayerDead.Invoke(); //μ½λ°±ν¨μλ€μ μΈμ€νν° μμμ λ±λ‘νμ.

        Destroy(gameObject);
    }
    
    void OnTriggerEnter(Collider other)
    {
        Dead();
    }
}
```

<br>
<br>

## π Unity Event
β­ **Unity Eventλ delegateμ eventμ κΈ°λ₯μ μ¬μ©νκΈ° μ½λλ‘ μ λν°μμ λ©νν΄ μ κ³΅νλ κ²μ μλ―Ένλ€.** β­<br>
μ¦ λ€μκ³Ό κ°μ κ³Όμ λ€μ κ°λ¨ν λ°©μμΌλ‘ μ¬μ©ν  μ μλλ‘ λλ Unity μ κ³΅ κΈ°λ₯μ΄λ€.<br>
1. Publisher(κ²μμ)μ Subscriber(κ΅¬λμ)μ κ΄κ³λ₯Ό μ€μ .<br>
2. Delegateκ° νμ©νλ λ©μλμ μνμ μ μ.<br>
3. λλ¦¬μ νμμ μ°Έμ‘° λ³μλ₯Ό λ§λ  λ€ event μμ½μ΄λ₯Ό λΆμ¬ μ΄λ²€νΈ κ°μ²΄λ‘ μ¬μ©.<br>
4. κ΅¬λμ ν΄λμ€μμ κ²μμμ μ΄λ²€νΈ κ°μ²΄μ Event Handlerλ₯Ό λ±λ‘.<br>

(λνμ μΈ μ¬μ© μλ‘λ Buttonμ onClickμ΄ UnityEventμ Instance μ΄λ€.)<br>
(μ¬μ€ UnityEventλ λ―Έλ¦¬ κ΅¬ννμ¬ λ΄μ₯λ **Observer Pattern** μΌλ‘ λ³΄λ κ²μ΄ λ§λ€κ³  νλ€.)<br>
<br>
<br>

π **UnityEventμ μ½λ°±μ λ±λ‘νλ λ°©μμ λ€μκ³Ό κ°λ€.**<br>
1. μΈμ€νν°μμ μ§μ  λ£μ΄μ£Όλ λ°©μ<br>
2. scriptμμ μ½λλ‘ λ£μ΄μ£Όλ λ°©μ<br>
<br>

π λ¬Όλ‘  1λ² λ°©μμ΄ νΈλ¦¬νκ² μ§λ§, β­ **1λ² λ°©μμ λ°νμ μ μ½λ°±λ€μ μμ  ν  μ μλ€.** β­<br>
1λ² λ°©μμΌλ‘ λ±λ‘<br>
![μ λν°μ΄λ²€νΈμΈμ€νν°](https://user-images.githubusercontent.com/43705434/125920728-088b2537-bc61-49d6-b755-fef5588f382b.PNG)<br>

<br>
<br>

π β­β­ **2λ² λ°©μμ μ¬μ©νλ©΄ μ’ λ λ³΅μ‘νκ² μ§λ§, μμκ³Ό μ€ν¬λ¦½νΈ κ° λ²μ  μΆ©λμ νΌν  μ μμΌλ©°<br>
λ°νμ μ μμ μ΄ κ°λ₯νλ€λ μ₯μ μ΄ μμ΄ λλΆλΆ 2λ² λ°©μμ μ νΈνλ€.** β­β­<br>
π 2λ² λ°©μμΌλ‘ λ±λ‘<br>
```c#
using UnityEngine;
using System.Collections;
using UnityEngine.Events;
using UnityEngine.UI;
 
public class UnityEventTest : MonoBehaviour {
   public UnityEvent[] _event = new UnityEvent[8];
   int state = 0;
  
   void Start ()
   {
       //create unityaction delegate
       UnityAction []action = new UnityAction[8];
       action[0] = new UnityAction(FuncA);
       action[1] = new UnityAction(FuncB);
       action[2] = new UnityAction(FuncC);
       action[3] = new UnityAction(FuncD);
       action[4] = new UnityAction(FuncE);
       action[5] = new UnityAction(FuncF);
       action[6] = new UnityAction(FuncG);
     
       action[7] = new UnityAction(FuncA);
       action[7] += new UnityAction(FuncB);
       action[7] += new UnityAction(FuncC);
       action[7] += new UnityAction(FuncD);
       action[7] += new UnityAction(FuncE);
       action[7] += new UnityAction(FuncF);
       action[7] += new UnityAction(FuncG);
 
       //register
       for(int i=0; i<8; i++)
       {
           _event[i] = new UnityEvent();
           _event[i].AddListener (action[i]);
       }
   }
   
   void Update ()
   {
       if (Input.GetKeyDown (KeyCode.A))
       {
           _event[state].Invoke ();
           state = (++state>7) ? 0 : state;
       }
   }
 
   public void FuncA() {     Debug.Log("Air");    }
   public void FuncB() {     Debug.Log("Baby");   }
   public void FuncC() {     Debug.Log("Cat");    }
   public void FuncD() {     Debug.Log("Do");     }
   public void FuncE() {     Debug.Log("Ear");    }
   public void FuncF() {     Debug.Log("Fly");    }
   public void FuncG() {     Debug.Log("Good");   }
}
```

<br>
<br>

π UnityEventλ μΈμλ‘ **UnityAction** μ λ°μΌλ©° (μ λν°μμ μ κ³΅νλ Delegate)<br>
β­ **μ΅λ 4κ°μ μΈμ**λ₯Ό κ°λ UnityActionμ λ°μ μ μλ€.<br>
-> μ΄λ μΈμλ₯Ό κ°λ UnityActionμ λ°μΌλ €λ©΄ μλμ κ°μ΄ β­ **Generic ν΄λμ€**λ₯Ό μμλ°μ μ΄μ©ν΄μΌ νλ€.<br>
λν μ λ€λ¦­ ν΄λμ€λ₯Ό μμλ°μ UnityEventλ μΈμ€νν° μμ λΈμΆλμ§ μμΌλ―λ‘ **μ§λ ¬ν** ν΄μ£Όμ΄μΌ νλ€.<br>

π <br>
```c#
public  class MyEvent: UnityEvent <int> {}
 
public class UnityEventTest : MonoBehaviour {
   public MyEvent[] _event = new MyEvent[8];
   int state = 0;
  
   void Start ()
   {
       //create unityaction delegate
       UnityAction<int> []action = new UnityAction<int>[8];
       action[0] = new UnityAction<int>(FuncA);
       action[1] = new UnityAction<int>(FuncB);
       action[2] = new UnityAction<int>(FuncC);
       action[3] = new UnityAction<int>(FuncD);
       action[4] = new UnityAction<int>(FuncE);
       action[5] = new UnityAction<int>(FuncF);
       action[6] = new UnityAction<int>(FuncG);
     
       action[7] = new UnityAction<int>(FuncA);
       action[7] += new UnityAction<int>(FuncB);
       action[7] +=new UnityAction<int>( FuncC);
       action[7] += new UnityAction<int>(FuncD);
       action[7] += new UnityAction<int>(FuncE);
       action[7] += new UnityAction<int>(FuncF);
       action[7] += new UnityAction<int>(FuncG);
 
       //register
       for(int i=0; i<8; i++)
       {
           _event[i] = new MyEvent();
           _event[i].AddListener (action[i]);
       }
   }
 
   void Update ()
   {
       if (Input.GetKeyDown (KeyCode.A))
       {
           _event[state].Invoke (100);
           state = (++state>7) ? 0 : state;
       }
   }
 
   public void FuncA(int cnt) {     Debug.Log("Air:" + cnt);    }
   public void FuncB(int cnt) {     Debug.Log("Baby:" + cnt);   }
   public void FuncC(int cnt) {     Debug.Log("Cat:" + cnt);    }
   public void FuncD(int cnt) {     Debug.Log("Do:" + cnt);     }
   public void FuncE(int cnt) {     Debug.Log("Ear:" + cnt);    }
   public void FuncF(int cnt) {     Debug.Log("Fly:" + cnt);    }
   public void FuncG(int cnt) {     Debug.Log("Good:" + cnt);   }
}
```
    
<br>
<br>
  
## π UnityAction
μ¬μ€ UnityActionμ β­ **UnityEventμ μ νν μΌμΉνλ κΈ°λ₯μ μ κ³΅ ν  μ μλ€.**<br>
νμ§λ§ μ§λ ¬νλμ΄μμ§ μκΈ° λλ¬Έμ, Public μΌλ‘ μ μΈν΄λ **μΈμ€νν° μμ λΈμΆλμ§ μλλ€.**<br>
λν += , -= λ₯Ό ν΅ν΄μλ§ λ©μλλ₯Ό μΆκ°/μ­μ  ν  μ μλ€.<br>
UnityEventμ μ¬λ¬ λ©μλλ₯Ό μ½λ°±μΌλ‘ μΆκ°ν΄μΌ ν  λ, UnityActionμ μ μΈνμ¬ <br>
ν¨μλ€μ UnityActionμ λ±λ‘νκ³  ν΄λΉ UnityActionμ UnityEventμ λ±λ‘νλ λ°©μμΌλ‘λ μ¬μ©νλ€.<br>

UnityEventλ **μΈμ€νν°μ μ§λ ¬νλμ΄ λΈμΆ**λκ² νκΈ° μν΄μ μ¬μ©νλ κ²μ΄λ€.<br>
λ§μ½ μΈμ€νν°μ λΈμΆν  νμκ° μλ€λ©΄ **UnityActionλ§ μ¬μ©ν΄λ μΆ©λΆνλ€.**<br>
UnityActionμ νλ² κ±°μ³μ UnityEventλ‘ λ£μ μ΄μ λ μ§λ ¬νλ₯Ό μν΄μ<br>
μ½λμμ μ§μ  λ±λ‘ν΄μ€ λ μ¬μ©λλ AddListener ν¨μμ κΈ°λ³Έ μΈμμ<br>
νμμ΄ UnityActionμ΄κΈ° λλ¬Έμ΄λ€. **κ·Έλ₯ λ©μλλ₯Ό κ³§λ°λ‘ λ£μ΄λ λ¬Έμ λ μλ€.**<br>
<br>
<br>
