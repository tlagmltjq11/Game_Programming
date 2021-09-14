👉 **GetComponent(), Find() 메소드 사용 시 캐싱해서 사용하기**<br>
GetComponent, Find, FindObjectOfType 등의 메소드는 자주 호출될 경우 성능에 악영향을 끼친다.<br>
-> 오브젝트 및 컴포넌트들을 전부 탐색하므로..<br>
따라서 객체 참조가 필요할 때마다 **Update에서 Get, Find 메소드들을 호출하는 방식은 지양하고,<br>
최대한 Awake, Start 메소드에서 Get, Find 메소드들을 통해 객체들을 필드에 캐싱하여 사용해야 한다.**<br>

예시<br>
```c#
private void Update()
{
    GetComponent<Rigidbody>().AddForce(Vector3.right);
}
```
<br>
위와 같은 코드가 있다면, 아래처럼 바꾼다.<br>

```c#
private Rigidbody rb;

private void Awake()
{
    rb = GetComponent<Rigidbody>();
}

private void Update()
{
    rb.AddForce(Vector3.right);
}
```
<br>
<br>

👉 **GetComponent() 대신 TryGetComponent() 사용하기**<br>
https://medium.com/chenjd-xyz/unity-tip-use-trygetcomponent-instead-of-getcomponent-to-avoid-memory-allocation-in-the-editor-fe0c3121daf6<br>
<br>

GetComponent() 메소드는 할당에 성공하거나 실패해도 언제나 GC Allocation이 발생한다.<br>
하지만 **TryGetComponent()를 이용하면 GC 걱정 없이 깔끔하게 사용할 수 있으며,<br>
할당 성공 여부를 bool 타입으로 리턴받을 수 있다.**<br>
<br>
<br>

👉 **Object.name, GameObject.tag 사용하지 않기**<br>
게임 오브젝트의 이름을 참조해야 할 때, .name 프로퍼티를 호출한다.<br>
그리고 태그 비교를 해야 할 때, .tag 프로퍼티를 호출해서 ==, .Equals() 등으로 비교한다.<br>
그런데 이런 호출 하나 하나가 전부 가비지를 한 개씩 생성한다.<br>
.name으로 프로퍼티 Getter를 호출할 때마다 가비지 한 개,<br>
.tag로 프로퍼티 Getter를 호출할 때마다 가비지 한 개.<br>
예전부터 있었던 문제이고, 2021.1.16f1 버전에서도 직접 테스트해본 결과 동일했다.<br>

**(추가로 모든 비교문에서 .equals()를 사용하도록 하자.<br>
== 구문으로 사용하면 임시적인 메모리가 나오게 되며 가비지 컬렉션의 먹이를 준다.)**<br>
<br>

UnityEngine.Object.name 프로퍼티를 살펴보면<br>

```c#
public string name
{
    get
    {
        return GetName(this);
    }
    set
    {
        SetName(this, value);
    }
}

[MethodImpl(MethodImplOptions.InternalCall)]
[FreeFunction("UnityEngineObjectBindings::GetName")]
private static extern string GetName(Object obj);
```
<br>

이런 식으로 구현되어 있다.<br>
**결국 오브젝트의 이름을 갖고 있는건 유니티 네이티브 영역이고,<br>
이를 참조하려고 할 때마다 문자열을 새롭게 힙에 할당해 오기 때문에 가비지가 발생하는 것이다.**<br>

태그도 비슷한 방식으로 되어 있지만, 그래도 해결 방법이 있다.<br>
**gameObject.tag == "Player" 대신에<br>
gameObject.CompareTag("Player")처럼<br>
GameObject.CompareTag(string), Component.CompareTag(string) 메소드를 사용하면 가비지 생성을 방지할 수 있다.**<br>

**결국 name 프로퍼티만 주의해서 사용하면 된다.**<br>
“절대로 쓰면 안된다”, 이런 것이 아니라<br>
성능에 별 영향이 없는 것 같거나 다른 방안이 없으면 그냥 사용하고,<br>
성능에 민감하면 다른 방법을 찾으면 된다.<br>
<br>
<br>

👉 **비어있는 유니티 이벤트 메소드 방치하지 않기**<br>
Awake(), Update() 등의 유니티 기본 이벤트 메소드는 매직 메소드라고도 불리며,<br>
**스크립트 내에 작성되어 있는 것만으로도 호출되어 성능을 소모한다.**<br>
따라서 내용이 비어있는 유니티 기본 이벤트 메소드는 아예 **지워야 한다.**<br>
<br>
<br>

👉 **StartCoroutine() 자주 호출하지 않기**<br>
**StartCoroutine() 메소드는 Coroutine 타입의 객체를 리턴하므로 GC의 먹이가 된다.**<br>
따라서 짧은 주기로 코루틴을 자주 실행해야 하는 경우, UniTask, UniRx 등으로 대체하는 것이 좋다.<br>
UniTask는 Cancellation 관리가 번거로우므로, UniRx의 MicroCoroutine을 활용하면 좋다.<br>
**그리고 매 프레임 실행되는 코루틴의 경우(yield return null), 대신 Update를 사용하는 것이 성능상 효율적이다.**<br>
그런데 UniRx의 MicroCoroutine을 사용한다면 매 프레임 실행해도 된다.<br>
<br>
<br>

👉 **코루틴의 yield 캐싱하기**<br>

```c#
private IEnumerator SomeCoroutine()
{
    while(true)
    {
        // ...
        yield return new WaitForSeconds(0.01f);
    }
}
```
<br>

**코루틴에서는 WaitForSeconds() 등의 객체를 yield return으로 사용한다.<br>
그런데 위처럼 항상 new로 생성할 경우, 모조리 GC.Collect()의 대상이 된다.<br>
따라서 아래처럼 캐싱하여 사용하는 것이 좋다.**<br>

```c#
private IEnumerator SomeCoroutine()
{
    var wfs = new WaitForSeconds(0.01f);
    while(true)
    {
        // ...
        yield return wfs;
    }
}
```
<br>
<br>

👉 **참조 캐싱하기**<br>
위의 ‘메소드 호출 줄이기’와 같은 맥락이다.<br>
이것은 주로 **프로퍼티 호출에 해당한다.**<br>
예를 들어,<br>

```c#
void Update()
{
    _ = Camera.main.gameObject;
    _ = Camera.main.transform.forward;
    _ = targetObject.transform.parent.gameObject.activeInHierarchy;
    _ += Time.deltaTime;
}
```
<br>

이런 경우이다.<br>
프로퍼티는 필드가 아니다. 필드처럼, 혹은 메소드처럼 사용할 수 있는 참조이다.<br>
그리고 메소드 호출만큼의 오버헤드가 발생한다.<br>
심지어 위처럼 대상.프로퍼티.프로퍼티.프로퍼티.값 이렇게 이어지는 경우에는<br>
호출하는 모든 프로퍼티가 각각 오버헤드로 이어진다.<br>

따라서 이를 자주 호출해야 하는 경우에는 미리 참조로 **캐싱**해두는 것이 좋다.<br>
예전보다 나아졌긴 하지만 심지어 Camera.main도 필드 참조보다 훨씬 비싸다.<br>
메인 카메라로 등록된 참조를 바로 가져오는 친절한 친구가 아니라,<br>
내부적으로 FindMainCamera() 메소드 호출을 통해 “MainCamera” 태그가 붙은 카메라들 중에<br>
현재 렌더링을 담당하고 있는 카메라를 가져오는 프로퍼티다.<br>
<br>

그래서 이렇게,<br>

```c#
private GameObject _mainCamObject;
private Transform  _mainCamTransform;
private GameObject _targetParentObject;
private float      _deltaTime;

void Start()
{
    _mainCamObject = Camera.main.gameObject;
    _mainCamTransform = Camera.main.transform;
    _targetParentObject = targetObject.transform.parent.gameObject;
}

void Update()
{
    _deltaTime = Time.deltaTime;

    // ...

    // Usages
    _ = _mainCamObject;
    _ = _mainCamTransform.forward;
    _ = _targetParentObject.activeInHierarchy;
}
```
<br>

**자주 호출하는 프로퍼티, 참조들은 최대한 해당 타입 그대로 필드에 담아 사용하는 것이 좋다.**<br>
Time.deltaTime도 캐싱하는 것은 과하다고 생각할 수 있는데,<br>
Time.deltaTime 또한 내부 메소드 호출로 구현되어 있다.<br>
여러 군데, 수십 군데에서 Time.deltaTime을 그대로 항상 호출하여 사용하면<br>
그만큼의 메소드 호출 비용이 발생하는 것이니 항상 Update() 최상단에서 캐싱해서 사용하는 것이 좋다.<br>
-> **Usage 부분에서 최상단에서 캐싱한 deltaTime을 사용하라는 것임.**<br>
<br>
<br>

👉 **빌드 이후 Debug.Log() 사용하지 않기**
Debug의 메소드들은 에디터에서 디버깅을 위해 사용하지만, 빌드 이후에도 호출되어 성능을 많이 소모한다.<br>
따라서 아래처럼 Debug 클래스를 에디터 전용으로 래핑에서 사용할 경우, 이를 방지할 수 있다.<br>
-> [정리해둔 파일](https://github.com/tlagmltjq11/CS_and_Etc/blob/main/Debug.md)<br>

```c#
public static class Debug
{
    [Conditional("UNITY_EDITOR")]
    public static void Log(object message)
        => UnityEngine.Debug.Log(message);
}
```
<br>
<br>

👉 **Transform 변경은 한번에**
position, rotation, scale을 한 메소드 내에서 여러 번 변경할 경우, 그 때마다 트랜스폼의 변경이 이루어진다.<br>
그런데 트랜스폼이 **여러 자식 트랜스폼들을 갖고 있는 경우, 자식 트랜스폼도 함께 변경된다.**<br>
**따라서 벡터로 미리 담아두고 최종 계산 이후, 트랜스폼에 단 한 번만 변경을 지정하는 것이 좋다.**<br>
또한 position과 rotation을 모두 변경해야 하는 경우 SetPositionAndRotation() 메소드를 사용하는 것이 좋다.<br>
<br>
<br>

👉 **불필요하게 부모 자식 구조 늘리지 않기**
하이라키가 너무 복잡할 경우, 나름의 카테고리별로 빈 부모 오브젝트를 만들고<br>
자식 오브젝트를 넣어서 정리하는 경우가 많다.<br>
그런데 이것도 최소한으로 나누고, 불필요한 부모-자식 관계를 늘리지 않는 것이 좋다.<br>
**자식 게임오브젝트의 트랜스폼은 부모 트랜스폼에 종속적이므로<br>
부모 트랜스폼에 변경이 생기면 “모든” 자식 트랜스폼에 변경이 적용되기 때문에<br>
성능에 악영향을 끼칠 수 있다.**<br>

따라서 부모-자식 관계는 필요한 만큼 최소한으로 구성해야 한다.<br>
<br>
<br>

👉 **ScriptableObject 활용하기**<br>
게임 내에서 항상 공통으로 참조하는 변수를 사용하는 경우,<br>
각 객체의 필드로 사용하게 되면 동일한 데이터가 객체의 수만큼 메모리를 차지하게 된다.<br>
반면에 스크립터블 오브젝트로 만들고, 이를 필드로 참조하여 공유하게 되면<br>
**객체의 수에 관계 없이 동일 데이터는 단 하나만 존재하게 되어 메모리를 절약할 수 있다.**<br>

참고 : 경량 패턴(Flyweight Pattern)<br>
<br>
<br>

👉 **필요하지 않은 경우, 리턴하지 않기**<br>

```c#
private int SomeMethod() // 정수 타입을 리턴하는 메소드
{
    //...
    return 0;
}

private void Caller()
{
    SomeMethod(); // 메소드의 리턴값을 사용하지 않음
}
```
<br>

메소드를 호출하고 그 리턴값을 항상 사용하지는 않는 경우, 리턴값이 void인 메소드를 작성하는 것이 좋다.<br>
리턴하는 것만으로 항상 성능 소모가 발생하며, 심지어 참조형인 경우 GC의 먹이가 된다.<br>
(대표적으로 StartCoroutine())<br>
<br>
<br>

👉 **new로 생성하는 부분 최대한 줄이기**<br>
클래스 타입으로 생성한 객체는 힙에 할당되며,<br>
더이상 참조되지 않을 때 가비지 콜렉터에 의해 자동 수거된다.<br>
그런데 너무 잦은 GC의 수거는 성능에 악영향을 끼칠 수 있다.<br>
따라서 가능하면 한 번만 생성하고 이후에는 재사용 하는 방식을 사용하거나<br>
최대한 new로 생성하는 부분을 줄이는 것이 좋다.<br>

**이럴 활용하는 대표적인 방법으로 버퍼(Buffer)의 사용,<br>
메모리 풀링(Memory Pooling) 등이 있다.**<br>

버퍼의 사용은 어떤 기능 호출 시 매번 배열을 생성하여 가비지가 생기는 경우<br>
매 수행마다 배열을 생성하는 대신, 미리 하나의 큰 배열을 생성하고<br>
이 배열의 offset(index)을 이동하며 그곳에 할당하는 방식이다.<br>
이 방식을 사용하는 대표적인 예시로 StringBuilder가 있다.<br>

그리고 메모리 풀링은 동일한 타입의 많은 객체를 너무 자주 생성/파괴하는 경우<br>
이를 미리 필요한 만큼 생성해놓고 재사용하는 방식이다.<br>
유니티에서는 이 방식에서 파생된 오브젝트 풀링이 자주 사용된다.<br>
<br>
<br>

👉 **오브젝트 풀링 사용하기**<br>
게임오브젝트의 생성과 파괴는 성능의 소모가 작지 않다.<br>
따라서 생성과 파괴가 빈번하게 발생한다면(총알, 폭탄 등)<br>
**오브젝트 풀링을 통해 일정 개수의 게임오브젝트를 미리 생성하고<br>
활성화/비활성화하여 재사용하는 방식을 활용하는 것이 좋다.**<br>

https://rito15.github.io/posts/unity-object-pooling/<br>
<br>
<br>

👉 **구조체 사용하기**<br>
http://clarkkromenaker.com/post/csharp-structs/<br>
동일한 데이터를 하나는 구조체, 하나는 클래스로 작성할 경우<br>
클래스는 참조를 위해 8~24 바이트의 추가적인 메모리를 필요로 한다.<br>
**따라서 데이터 클래스는 구조체로 작성하는 것이 좋으며, GC의 먹이가 되지 않는다는 장점도 있다.**<br>
그리고 구조체는 크기에 관계없이 항상 스택에, 클래스는 힙에 할당된다.<br>
16kB를 초과한다고 해서 구조체가 힙에 할당된다는 루머를 본적이 있는데, 사실이 아니다.<br>
https://www.sysnet.pe.kr/2/0/12619 <br>
-> **해당 링크에서 구조체가 16kb를 넘거나, 필드로 객체를 갖고있더라도 무조건 스택에 할당된다고 설명한다.**<br>
<br>

**스택과 힙, Value Type과 Reference Type**<br>
스택에 할당된다는 것, 힙에 할당된다는 것은 무엇일까?<br>
스택은 지역 메모리, 힙은 동적 메모리라고 할 수 있다.<br>
모든 Value Type(int, float, … , struct) 지역 변수는 스택에 저장된다.<br>
그래서 해당 변수가 생성된 메소드 블록을 벗어나면 메모리에서 해제된다.<br>
Value Type의 변수를 메소드에서 반환했을 때, 혹은 매개변수로 전달할 때<br>
“값이 복제된다”라고 하는 이유가 바로 여기에 있다.<br>
스택 메모리에 할당되었기 때문에 해당 영역(블록)을 벗어나면 사라진다.<br>
그런데 어쨌든 이 값을 반환값이나 매개변수로 전달은 해야 하므로<br>
기존 영역을 벗어나 새로운 영역에 도달했을 때, 그 영역에서 새롭게 할당하는 것이다.<br>
이것이 바로 Value Type의 전달 방식이다.<br>
<br>

반면에 Reference Type(Class Type)의 객체는 스택이 아닌 힙에 저장된다.<br>
정확히 말하자면, 객체는 힙에 할당되고 이 힙의 시작 주소를 가리키는 변수가 스택에 할당된다.<br>
그래서 영역(블록)을 벗어났을 때 이 스택의 변수는 사라지더라도<br>
힙에 저장된 객체 자체는 사라지지 않는다.<br>
그런데 C#에서는 힙에 저장된 이 객체가 더이상 그 어떤 변수에게서도 참조되지 않을 때,<br>
조금 명확한 용어를 사용하자면 참조 카운트가 0이 됐을 때<br>
가비지 콜렉터(GC)에 의해 자동으로 메모리에서 해제된다.<br>
그리고 이 자동 메모리 수거가 발생할 때의 오버헤드가 문제될 수 있기 때문에<br>
C#에서 다들 GC Call을 최대한 줄이려고 노력하는 것이다.<br>
<br>
<br>

**클래스 타입과 구조체 타입의 선택**
그렇다고 모든 타입을 클래스가 아닌 구조체로 만든다?<br>
이것은 또다른 문제가 된다.<br>
구조체는 영역을 벗어나 전달될 때마다 복제된다.<br>
만약 클래스 타입이었다면 단순히 참조 변수만 복제되고,<br>
객체는 오버헤드 없이 전달될텐데<br>
구조체라서 통째로 복제된다.<br>

그래서 일정 크기 이상의 구조체는 차라리 클래스로 만들고,<br>
아니면 복제/메모리 해제가 자주 발생할 수 있는지 여부에 따라<br>
구조체/클래스 중 선택하고..<br>
다양한 상황에 다양한 요소들을 고려하여 적절하게 선택해야 한다.<br>
이는 정답이 없는 문제다.<br>

**그렇다면 어떻게 결정할 수 있을까?**
경험 많은 프로그래머가 아니라면, 원칙을 세워두면 좋다.<br>
**생성/해제가 자주 일어나면 구조체로 만든다.<br>
생성/해제보다 전달(매개변수, 리턴)이 훨씬 자주 일어나면 클래스로 만들거나 매개변수 한정자 in을 사용한다.<br>
간단한 원칙은 위와 같다.**<br>
<br>

**클래스를 사용해야하는 경우**<br>
1. **상속**이 필요한 경우<br>
2. **메모리를 많이** 차지하는 경우<br>
3. **복잡**한 객체를 정의하는 경우<br>
<br>

**구조체를 사용해야하는 경우**<br>
1. **짧게** 생성되고 없어지는 경우<br>
2. **단순**하고 **메모리를 적게** 차지하는 경우<br>
3. 클래스를 사용할 필요가 없는 경우<br>
ex) position, rotation 등과 같은 데이터 셋..<br>
<br>

설계 및 프로그래밍 진행 상황에 따라 고려하여 결정하면 된다.<br>
<br>
<br>

👉 **컬렉션 재사용하기**<br>
List를 메소드 내에서 반복적으로 할당하여 사용하는 경우가 많다.<br>

```c#
private void SomeMethod() // 여러 번 호출되는 메소드
{
    List<Transform> transformList = new List<Transform>();
    transformList.Add(...);
    // ... 
}
```
<br>

**습관적으로 사용하게 되는 방식이지만, 이렇게 되면 가비지 콜렉터의 호출이 반드시 발생한다.<br>
따라서 다음과 같이 변경하여 사용하는 것만으로 컬렉션에 할당되는 가비지를 줄일 수 있다.**<br>

```c#
transformList = new List<Transform>();

private void SomeMethod()
{
    transformList.Clear();
    transformList.Add(...);
    // ... 
}
```
<br>

실제로 이렇게 바꿨을 때, 아래처럼 가비지를 굉장히 줄인 경우가 있었다.<br>
<br>
<br>

**가비지 할당(GC.Alloc) 자체가 성능에 큰 영향을 미치지는 않지만,<br>
결국 가비지가 수집(GC.Collect)될 때 성능에 많은 영향을 끼치게 되므로 반드시 신경써야 하는 부분이다.**<br>
<br>
<br>

👉 **List 사용할 때 주의할 점**<br>
**만약 배열을 사용할 때, 처음에는 비어있는 배열을 생성하고<br>
새로운 값을 하나씩 넣을 때마다 크기가 조금 더 큰 배열을 만들고,<br>
기존의 배열을 통째로 복제하는 방식으로 사용한다고 생각해보자.**<br>
이건 너무 비효율적이라고 생각할 수 있다.<br>

C#의 가변 배열인 List(T)는 내부적으로 배열로 구현되어 있다.<br>
그리고 내부 구현은 정말로 위에서 설명한 그대로 되어있다.<br>
new List(T)()로 생성하면 크기가 0인 배열을 생성한다.<br>
그리고 .Add()를 통해 요소를 하나씩 추가할 때마다 처음에는 크기가 4인 배열을 생성하고<br>
배열이 가득찰 때마다 현재 배열의 두 배 크기의 새로운 배열을 생성하며,<br>
기존의 배열을 그대로 복제해온 뒤 마지막 위치에 새로운 요소를 집어넣는 방식이다.<br>

**그래서 리스트를 생성할 때, 사용될 영역의 크기를 미리 알고 있다면<br>
new List(T)(100) 처럼 개수를 미리 지정하는 것이 좋다.<br>
그러면 내부적으로 그만큼의 크기를 갖는 배열을 미리 할당하여,<br>
.Add()를 하더라도 기존의 배열 전체를 통째로 복제하는 것을 방지할 수 있다.<br>
이미 생성된 리스트라면 .Capacity 프로퍼티에 크기 값을 넣어주면 된다.**<br>
-> 애초에 LinkedList를 쓰는 것도 방법이 될 수 있을 것 같다.<br>
<br>
<br>

👉 **StringBuilder 사용하기**<br>
스트링의 연결(a + b)이 자주 발생하는 경우, StringBuilder.Append()를 활용하는 것이 좋다.<br>

```c#
string a = "a1";
a = "a2";
```
<br>

스트링에 상수 문자열을 초기화 하는 것은 GC를 호출하지 않지만,<br>

```c#
string strA = "a";
string strB = "b";

strA = "a" + "b";
strA = strA + strB;
```
<br>

스트링끼리 연결하는 경우에는 가비지를 생성한다.<br>
-> **"ab"와 "abb" 라는 문자열이 힙에 새로 할당**<br>
<br>

```c#
StringBuilder sb = new StringBuilder("");
sb.Append("a");
sb.Append("b");
```
<br>

따라서 이렇게 StringBuilder를 사용하는 것이 좋다.<br>
<br>
<br>


👉 **LINQ 사용 시 주의하기**<br>
https://medium.com/swlh/is-using-linq-in-c-bad-for-performance-318a1e71a732<br>
https://www.jacksondunstan.com/articles/4840<br>
LINQ는 개발자에게 굉장한 편의성을 제공해주며, 대개 성능이 크게 나쁘지는 않다.<br>
**하지만 정확히 이해하고 사용하지 않는다면 불필요한 성능 및 메모리 소모가 발생할 수 있으므로,<br>
잘 판단해서 사용해야 한다.**<br>

LINQ의 대부분의 연산자는 중간 버퍼(일종의 배열)를 생성하고, 이는 모조리 가비지가 된다.<br>
**그러니까 LINQ 연산자 한두개쯤 사용해서 가능한 것을<br>
가독성이나 모종의 이유로 여러 번 나누어 처리하게 되면 모두 불필요한 성능 저하의 원인이 된다.<br>
성능에 민감하거나 매프레임 호출되는 부분에서 LINQ를 사용하고 있었다면,<br>
더 나은 방안은 없을지 다시 한 번 생각해보는 것이 좋다.**<br>
<br>
<br>

👉 **박싱, 언박싱 피하기**
박싱(Boxing)은 값 타입이 참조 타입으로 암시적, 또는 명시적으로 캐스팅되는 것을 의미한다.<br>
언박싱(Unboxing)은 참조 타입이 값 타입으로 명시적으로 캐스팅되는 것을 의미한다.<br>

**이 때 중요한 것은, 박싱과 언박싱이 단순 할당보다 성능이 매우 나쁘다는 것과<br>
참조 타입은 힙에 할당되어 GC의 먹이가 될 수 있다는 점이다.**<br>

```c#
public static class Debug
{
    public static void Log(object msg) { /* ... */ }
}
```
<br>

유니티 엔진을 사용하면서 가장 친숙한 메소드인 Debug.Log()이다.<br>
그리고 항상 박싱이 발생하는 대표적인 예시라고 할 수 있다.<br>
**파라미터의 타입이 object이기 때문에, 어떤 값 타입을 넣어도 참조타입인 object 타입으로 박싱된다.**<br>
이렇게 메소드 매개변수로 object 타입을 사용하거나 박싱을 유도하는 방식은 최대한 지양해야 한다.<br>
따라서 이를 피할 수 있는 대표적인 방법으로, 제네릭이 있다.<br>
**제네릭은 객체 생성 또는 메소드 호출 시 제네릭 타입이 하나의<br>
타입으로 고정되기 때문에, 박싱과 언박싱을 피할 수 있다.**<br>
<br>

**참고 : 해결된 박싱 이슈**<br>
**foreach 루프 박싱 이슈**<br>
foreach를 사용할 경우 매번 24byte의 추가적인 가비지가 발생한다는 이슈<br>
현재 버전에서는 해결되었다고 한다.<br>
**Dictionary의 키로 Enum을 사용할 경우 박싱 이슈**<br>
역시 .Net 4.x 버전에서 해결되었다고 한다.<br>
<br>
<br>

👉 **비싼 수학 계산 피하기**<br>

**나눗셈 대신 곱셈**<br>
나눗셈이 곱셈보다 느리다는 것은 흔히 알려져 있는 사실이다.<br>
그런데 곱셈을 해도 되는 부분에 나눗셈을 사용하는 코드가 의외로 자주 보인다.<br>
**1.0f / 2.0f 보다 1.0f * 0.5f가 빠르다. 꼭 기억하자.**<br>
<br>

**Magnitude 보다 sqrMagnitude를 사용하자 (제곱근 계산 X)**<br>
굳이 제곱근 까지 완벽하게 구해야하는 상황이 아니라면<br>
sqrMagnitude를 사용하도록 하자.<br>
<br>

**System.Math.Abs vs UnityEngine.Mathf.Abs vs 삼항 연산자**
// 삼항 연산자를 이용한 Abs<br>
(x >= 0) ? x : -x;<br>
삼항 연산자가 압도적으로 빠르다.<br>

닷넷 콘솔 디버그 빌드에서는 Math.Abs보다 삼항 연산자가 두 배 정도 빨랐고,<br>
릴리즈 빌드에서는 여섯 배 정도 빨랐다.<br>
심지어 삼항 연산자를 함수화 해도 Math.Abs(), Mathf.Abs()보다 빨랐다.<br>

그리고 유니티 엔진에서도 비슷한 결과를 얻을 수 있었다.<br>
가독성을 위해서라면 메소드를 사용할 수 있겠으나,<br>
**작은 성능에도 아주 민감하다면 삼항 연산자를 사용하는 것이 좋다.**<br>
<br>

**System.Math는 웬만해서 UnityEngine.Mathf보다 빠르다.**<br>
<br>
<br>
