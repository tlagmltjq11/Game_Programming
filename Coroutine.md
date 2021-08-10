## 🔔 코루틴
(Unity는 기본적으로 싱글 스레드 시스템이기 때문에 코루틴을 적극 활용한다.)<br>
코루틴을 알기 전에 서브루틴이라는 것부터 확인해야 한다.<br>
⭐ **서브루틴이란 어떤 동작을 하기위해 함수가 호출이 되었고 그 함수의 동작이 끝나면 자신을 호출하였던 메인루틴으로 돌아오는 경우를 말한다.**<br>

코루틴이란 간단하게 하나의 진입점과 하나의 탈출점이 있는 서브루틴과 다르게 ⭐ **다양한 진입점과 다양한 탈출점이 있는 루틴이라 생각하면 된다.**
즉, 메인 루틴에서 어떤 함수를 호출하고 그 함수가 동작 하다 중간에 멈추고 다시 메인 루틴의 동작을 하다 다시 멈춘 함수로 돌아와서 마저 작업을 하게 된다.
⭐⭐**마치 라운드 로빈 스케줄링처럼 여러 루틴들을 번갈아가며 수행하여 마치 병렬 수행인 것 처럼, 멀티 스레딩을 사용하는 것 처럼 보이게 해준다.** ⭐⭐<br>

👉 이러한 코루틴은 디버깅과 코드 분석이 어려워질 수 있으므로 명확한 상황에서만 사용하고 남발하면 안된다.<br>
<br>

👉 **Invoke vs Coroutine**<br>
Invoke도 코루틴과 유사하게 특정 시간 뒤 호출될 수 있으며 InvokeRepeating을 사용하면 거의 동일하게 사용이 가능하다.<br>
그렇다면 둘의 차이점은 무엇이 있을까?<br>

1. Invoke는 함수자체가 반환값, 매개변수가 없어야 한다는 제약이 존재한다.
2. 코루틴은 매개변수에 대한 제약이 없다.
3. ⭐ Invoke는  Game Object의 Active 상태에 상관없이 동작한다. ⭐
4. IsInvoking을 통해 인보크의 실행 여부를 알 수 있다.

-> ⭐⭐ **코루틴은 좀 더 복잡하고 유연한 코딩이 가능해서 InvokeRepeating으로 대체되지 않는다.** ⭐⭐<br>
<br>

👉 **Coroutine 실행 법 차이**<br>
코루틴을 실행할때에는 코루틴의 이름(String)으로 실행하거나, 함수 실행하듯이 실행하는 방식이 존재하는데<br>
이 두 방식은 차이를 보인다.<br>

```c#
void Start()
{
  StartCoroutine(Test());
}

IEnumerator Test()
{
  while(true)
  {
    yield return null;
  }
}
```
<br>

위처럼 함수 실행하듯이 코루틴을 실행하게 되면, StopCoroutine(Test());와 같이 ⭐ **해당** 코루틴을 중단 할 수 없다.<br>
이는 맨 처음 코루틴을 실행하는 부분에서 반환하는 ⭐ **IEnumerator 변수를 이용해야 중단이 가능하다.**<br>
예시 코드는 아래와 같다.<br>

```c#
IEnumerator enumerator;

void Start()
{
  enumerator = Test(); //반환
  StartCoroutine(enumerator); //실행
}

void Update()
{
  if(Input.GetKey(KeyCode.Space))
  {
    StopCoroutine(enumerator); //이런 식으로 중단이 가능하다.
  }
}

IEnumerator Test()
{
  while(true)
  {
    yield return null;
  }
}
```
<br>

코루틴의 이름으로 실행시키는 경우 **특별한 과정없이 곧바로 중단** 시킬 수 있다.<br>
하지만 유의할 점은 위처럼 IEnumerator 변수를 이용하거나, 함수 실행하듯이 실행한 코루틴을 이름만으로 중단 할 수 없다는 것.<br>
예시 코드는 아래와 같다.<br>

```c#
void Start()
{
  StartCoroutine("Test"); //실행
}

void Update()
{
  if(Input.GetKey(KeyCode.Space))
  {
    StopCoroutine("Test"); //이런 식으로 중단이 가능하다.
  }
}

IEnumerator Test()
{
  while(true)
  {
    yield return null;
  }
}
```
<br>

⭐⭐ **가장 큰 차이점은 매개변수가 존재할 때 이다.<br>
매개변수가 1개 존재할 때에는 두 방식 모두 사용이 가능하다. (코루틴의 이름을 사용하는 경우 박싱이 일어나 성능저하)<br>
하지만 매개변수가 2개 이상인 경우에는 코루틴의 이름을 이용하여 호출하는 것이 불가능하다.<br>
-> 구조체나 클래스를 이용하면 가능은 하겠지만 굳이?** ⭐⭐<br>
<br>
<br>
