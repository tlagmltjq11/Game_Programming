## 🔔 CompareTag
유니티 게임 오브젝트의 tag를 체크하여, tag에 따라 다른 동작을 해주려고 할 때<br>
tag를 체크하는 코드를 다음과 같이 작성하면 경고 메세지가 뜬다.<br>

```c#
if (gameObject.tag == "MyTag")
{
    // do something...
}
```
<br>

⭐ **이유는 == 로 비교하는 것은 비효율적이기 때문에, CompareTag()를 사용하라는 것이다.**<br>

```c#
if (gameObject.CompareTag("MyTag"))
{
    // do something...
}
```
<br>

👉 무엇이 둘의 효율 차이를 낼까?<br>
우선 GameObejct.tag는 멤버변수가 아닌 프로퍼티라는 사실로부터 차이가 시작된다.<br>
⭐⭐ **프로퍼티의 getter가 실행되는 순간 string을 복사-생성한다. 즉 동적 할당을 실행하는 것이다.<br>
이는 곧, 메모리 할당 및 복사의 부하 + GC의 추가적인 일 처리로 이어지게 된다.** ⭐⭐<br>

👉 반면 CompareTag()는 이러한 과정없이 tag 비교가 가능하도록 구현되어 있다고 한다.<br>
-> 대략 27%의 성능 향상 효과를 볼 수 있다고 한다.<br>

👉 더불어 CompareTag() 는 인자로 전달받은 string 이 실제로 존재하는 태그인지 확인하는 동작까지 포함한다.<br>
-> ⭐ **안정성** 면에서도 더 우세하다.<br>
<br>
<br>
