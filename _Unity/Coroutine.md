## 🔔 코루틴
(Unity는 기본적으로 싱글 스레드 시스템이기 때문에 코루틴을 적극 활용한다.)<br>
코루틴을 알기 전에 서브루틴이라는 것부터 확인해야 한다.<br>
⭐ **서브루틴이란 어떤 동작을 하기위해 함수가 호출이 되었고 그 함수의 동작이 끝나면<br>
자신을 호출하였던 메인루틴으로 돌아오는 경우를 말한다.**<br>

코루틴이란 간단하게 하나의 진입점과 하나의 탈출점이 있는 서브루틴과 다르게 ⭐ **다양한 진입점과 다양한 탈출점이 있는 루틴이라 생각하면 된다.**
즉, 메인 루틴에서 어떤 함수를 호출하고 그 함수가 동작 하다 중간에 멈추고 다시 메인 루틴의 동작을 하다 다시 멈춘 함수로 돌아와서 마저 작업을 하게 된다.
⭐⭐**마치 라운드 로빈 스케줄링처럼 여러 루틴들을 번갈아가며 수행하여 마치 병렬 수행인 것 처럼, 멀티 스레딩을 사용하는 것 처럼 보이게 해준다.** ⭐⭐<br>

👉 이러한 코루틴은 디버깅과 코드 분석이 어려워질 수 있으므로 명확한 상황에서만 사용하고 남발하면 안된다.<br>
<br>

## 🔔 코루틴을 사용하는 이유
Update 메소드는 매 프레임마다 호출되기에, 60fps라면 1초에 60번 호출된다.<br>
그런데 만약 5초동안 지연을 시켜야 하는 상황이 발생한다면 어떻게 할까?<br>
Update() 메소드를 5 * 60 = 300번 호출하면 될 것이다. 하지만 이러한 딜레이가 발생하는<br>
상황을 **모두 Update 구문에 정의하면 기기의 성능에 따라 프레임 드랍을 발생할 수 있고<br>
-> 짧은 시간안에 계속해서 많은 연산을 처리해야 하기 때문에 프레임 드랍이 발생할 수 있음** ⭐⭐<br>
프레임 드랍 발생 시 **정확한 처리가 되지 않을 것이다.**<br>
-> 코루틴을 사용하면 원하는 작업을 원하는 시간만큼 수행하는 것이 가능하기에 위와 같은 문제를 해결 할 수 있다.<br>
-> 프레임에 영향을 받지 않는 시간 기반의 서브루틴을 구동할 수 있게 되었고<br>
-> **병렬처리를 하는 것 처럼 보일 수 있게 된다.**<br>
<br>

Unity 프로그래밍에서 코루틴이 중요한 이유는 Unity는 코루틴을 협력형 멀티태스킹을<br>
구현하는 용도로 사용하기 때문이다. 협력형 멀티태스킹은 일종의 시분할 방식으로 여러 task가<br>
하나의 CPU를 나눠쓰는 방식인데, 선점형 멀티태스킹과 달리 운영체제의 개입 없이 각 task가 독점적으로<br>
CPU를 사용하고 사용이 끝나면 자발적으로 양보하는 방식이다. 이 방식의 장점은 리소스를 사용하는 도중에<br>
강제로 CPU를 뺏기는 일이 없으므로, 크리티컬 섹션을 보호하기 위한 락(lock)이나 세마포어(semaphore) 등<br>
동기화 수단이 필요 없다는 점이다.<br>
또한 멀티스레드는 버그 없는 코드를 작성하기 어렵기로 악명이 높다.<br>
유니티 자체가 비교적 쉬운 C#을 채택하고 싱글스레드를 지원하기 때문에 코루틴은 필수적이라고 할 수 있다.<br>
-> C#단에서 코루틴을 지원하지 않기 때문에 유니티에서는 c#의 이터레이터에 **스케쥴링 매커니즘**을 부여해<br>
-> 코루틴 기능을 구현해 지원한다.<br>
<br>

**함수의 상태를 저장/복원 하는 것이 가능하기에 (일시정지 처럼) 원하는 타이밍에 함수를 재가동 할 수 있다는 장점**<br>
서브루틴 생성 등등..<br>
<br>
<br>

## 🔔 Invoke vs Coroutine
Invoke도 코루틴과 유사하게 특정 시간 뒤 호출될 수 있으며 InvokeRepeating을 사용하면 거의 동일하게 사용이 가능하다.<br>
그렇다면 둘의 차이점은 무엇이 있을까?<br>

1. Invoke는 함수자체가 반환값, 매개변수가 없어야 한다는 제약이 존재한다.
2. 코루틴은 매개변수에 대한 제약이 없다.
3. ⭐ Invoke는  Game Object의 Active 상태에 상관없이 동작한다. ⭐
4. IsInvoking을 통해 인보크의 실행 여부를 알 수 있다.

-> ⭐⭐ **코루틴은 좀 더 복잡하고 유연한 코딩이 가능해서 InvokeRepeating으로 대체되지 않는다.** ⭐⭐<br>
<br>

## 🔔 Coroutine 실행
우선 코루틴은 IEnumerator 열거자를 반환하며 StartCoroutine을 직접 구현한다면 아래와 같을 것이다. ⭐<br>
http://theeye.pe.kr/archives/2725 참고<br>
```c#
void Start () {
	//StartCoroutine ("RunCoroutine")
	IEnumerator runCoroutine = RunCoroutine ();
	while (runCoroutine.MoveNext ()) {
		object result = runCoroutine.Current;

		if (result is WaitForSeconds) {
			// 원하는 초만큼 기다리는 로직 수행
			// 여기 예제에서는 1초만큼 기다리게 될 것임을 알 수 있음
		}
		else if ...
	}
}

IEnumerator RunCoroutine() {
	yield return new WaitForSeconds(1.0f);
	yield return new WaitForSeconds(1.0f);
	yield return new WaitForSeconds(1.0f);
}
```
<br>
<br>

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

## 🔔 코루틴 최적화
* yield return new로 오브젝트를 지속적으로 생성해 반환하면 가비지가 생긴다.<br>
-> **자주 사용하는 시간객체 등을 static으로 선언해두고 사용하던지 캐싱을 사용.** ⭐<br>

**캐싱 예제**<br>
```c#
IEnumerator TickEverySecond()
{
    var wait = new WaitForSeconds(1f); // Cache
    while(true)
    {
        Debug.Log("Tick"); // 1초마다 출력될 것
        yield return wait; // Reuse
    }
}
```
<br>

*  StartCoroutine 실행 시 생성되어 반환되는 코루틴 객체를 관리하자<br>
-> 코루틴을 남발했을 때 쌓이게되는 코루틴 클래스 객체를 무시못할 것<br>
-> https://m.blog.naver.com/dlwhdgur20/221016173139 링크를 참조해보면<br>
-> 직접 코루틴들을 관리하는 매니저 클래스를 생성해서 가비지를 줄이고 있다. ⭐<br>
<br>
<br>

## 참조링크
https://planek.tistory.com/36 <br>
https://kwangyulseo.com/2015/05/15/%EC%BD%94%EB%A3%A8%ED%8B%B4coroutine-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0/ <br>
https://ansohxxn.github.io/unity%20lesson%202/ch11/ <br>
