## 🔔 Main Concept
**Event 혹은 Callback이란 특정 이벤트가 발생했을 때, ⭐ 등록해둔 모든 메소드를이 호출되는 것을 의미한다.**<br>
이러한 개념은 다음과 같은 상황에서 요긴하게 사용될 수 있다.<br>

상황- 플레이어가 죽었을 때, 처리해줘야 하는 사항들이 많은 경우 (Text, Reset, Achivement...)<br>
위와 같은 상황을 구현할 때, Event 개념을 사용하지 않게되면 다음과 같아진다.<br>

📜 <br>
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
        achivementText.text = "도전과제 해제: " + title;
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
        achivementSystem.UnLockAchivement("뉴턴의 법칙");
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

물론 구현에는 문제가 없을 것이다. 하지만 위와 같은 상황에서 사망 시 이벤트를 추가해야 한다면?<br>
-> ⭐ **이벤트를 추가할 때마다 이벤트와 관련된 객체들을 연결시켜주어야 하며, 메소드를 등록해주어야 한다.** ⭐<br>
어떠한가? ⭐⭐ **커플링이 많아지고, 코드가 복잡해지며 스파게티 코드가 되기 쉽상** ⭐⭐ 이라는 것을 알 수 있을 것이다.<br>
고로 이러한 상황에서는 이벤트 관련 기능들을 사용하길 적극 권장한다.<br>

이벤트는 위에서 설명했듯이, 사건 발동이 되면 그 사건에 등록을 해놓은 기능들이 자동으로 발동되는 기능이다.<br> 
⭐ **특이한 것이라면 이벤트를 발동 시키는 측도 기능을 등록 해놨던 측도 서로에게 관심이 없다.**<br>
이벤트에 기능을 등록시키는 측은 기능을 등록해놓고 그게 어떻게 발동되고 언제 발동 시키는지 전혀 신경쓰지 않는다.<br>
마찬가지로 이벤트를 발동 시키는 측도 자기 이벤트에 어떤 기능이 등록되어 있는지 신경을 안 쓰고 발동시킨다.<br>
⭐⭐ **따라서 커플링을 줄일 수 있어 코드가 간단해지고 추후 유지보수에도 유리해지는 방식이라는 것을 알 수 있다.** ⭐⭐<br>


📜 <br>
```c#
public class PlayerHealth : MonoBehaviour
{
    public UnityEvent onPlayerDead;

    private void Dead()
    {
        onPlayerDead.Invoke(); //콜백함수들은 인스펙터 상에서 등록했음.

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

## 🔔 Unity Event
⭐ **Unity Event란 delegate와 event의 기능을 사용하기 쉽도록 유니티에서 랩핑해 제공하는 것을 의미한다.** ⭐<br>
즉 다음과 같은 과정들을 간단한 방식으로 사용할 수 있도록 돕는 Unity 제공 기능이다.<br>
1. Publisher(게시자)와 Subscriber(구독자)의 관계를 설정.<br>
2. Delegate가 허용하는 메서드의 원형을 정의.<br>
3. 대리자 형식의 참조 변수를 만든 뒤 event 예약어를 붙여 이벤트 객체로 사용.<br>
4. 구독자 클래스에서 게시자의 이벤트 객체에 Event Handler를 등록.<br>

(대표적인 사용 예로는 Button의 onClick이 UnityEvent의 Instance 이다.)<br>
(사실 UnityEvent는 미리 구현하여 내장된 **Observer Pattern** 으로 보는 것이 맞다고 한다.)<br>
<br>
<br>

👉 **UnityEvent에 콜백을 등록하는 방식은 다음과 같다.**<br>
1. 인스펙터에서 직접 넣어주는 방식<br>
2. script에서 코드로 넣어주는 방식<br>
<br>

👉 물론 1번 방식이 편리하겠지만, ⭐ **1번 방식은 런타임 시 콜백들을 수정 할 수 없다.** ⭐<br>
1번 방식으로 등록<br>
![유니티이벤트인스펙터](https://user-images.githubusercontent.com/43705434/125920728-088b2537-bc61-49d6-b755-fef5588f382b.PNG)<br>

<br>
<br>

👉 ⭐⭐ **2번 방식을 사용하면 좀 더 복잡하겠지만, 에셋과 스크립트 간 버전 충돌을 피할 수 있으며<br>
런타임 시 수정이 가능하다는 장점이 있어 대부분 2번 방식을 선호한다.** ⭐⭐<br>
📜 2번 방식으로 등록<br>
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

👉 UnityEvent는 인자로 **UnityAction** 을 받으며 (유니티에서 제공하는 Delegate)<br>
⭐ **최대 4개의 인자**를 갖는 UnityAction을 받을 수 있다.<br>
-> 이때 인자를 갖는 UnityAction을 받으려면 아래와 같이 ⭐ **Generic 클래스**를 상속받아 이용해야 한다.<br>
또한 제네릭 클래스를 상속받은 UnityEvent는 인스펙터 상에 노출되지 않으므로 **직렬화** 해주어야 한다.<br>

📜 <br>
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
  
## 🔔 UnityAction
사실 UnityAction은 ⭐ **UnityEvent와 정확히 일치하는 기능을 제공 할 수 있다.**<br>
하지만 직렬화되어있지 않기 때문에, Public 으로 선언해도 **인스펙터 상에 노출되지 않는다.**<br>
또한 += , -= 를 통해서만 메서드를 추가/삭제 할 수 있다.<br>
UnityEvent에 여러 메서드를 콜백으로 추가해야 할 때, UnityAction을 선언하여 <br>
함수들을 UnityAction에 등록하고 해당 UnityAction을 UnityEvent에 등록하는 방식으로도 사용한다.<br>

UnityEvent는 **인스펙터에 직렬화되어 노출**되게 하기 위해서 사용하는 것이다.<br>
만약 인스펙터에 노출할 필요가 없다면 **UnityAction만 사용해도 충분하다.**<br>
UnityAction을 한번 거쳐서 UnityEvent로 넣은 이유는 직렬화를 위해서<br>
코드에서 직접 등록해줄 때 사용되는 AddListener 함수의 기본 인자의<br>
타입이 UnityAction이기 때문이다. **그냥 메서드를 곧바로 넣어도 문제는 없다.**<br>
<br>
<br>
