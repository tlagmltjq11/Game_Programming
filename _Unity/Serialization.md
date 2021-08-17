## 직렬화
의미 자체는 특정 객체(데이터)를 바이트의 나열(Stream) 혹은 스트링으로 바꿔서 파일이나 네트워크통신으로 Stream 가능하게 해주는 것이다. 반대로 직렬화된 바이트의 나열을 원래 객체(데이터)로 바꿔주는 것은 Deserialize(디시리얼라이즈, 역직렬화) 라고 한다. 결국 바이너리 데이터와 객체 데이터 사이의 인코딩과 디코딩인 것이다.
(나열시키는 방법은 binary가 될 수도 있고, json, xml 같은 text 파일이 될 수도 있다.)

유니티는 기본적으로 Public 데이터만 직렬화함. -> 시리얼라이즈 어트리뷰트로 private도 직렬화하는것

여기서 중요한 것이 Unity에서 C# 클래스를 기본적으로 만들게 되면 자동적으로 MonoBehaviour를 상속받게 되는데 이렇게 상속을 받게 된다면 인스펙터 테이블에 노출을 할 수 없게 된다.
아무것도 상속을 받지 않게되어야 (사실 UnityEngine.Object를 상속 받음) 인스펙터 테이블에 노출되게 된다.

하지만 구조상 public이어야 하지만 노출 시키면 안되는 것들이 몇가지 있게 된다.

이럴 경우 System.NonSerialized를 사용하게 된다.혹은 하이드인인스펙터?

에셋을 읽고 쓰는 과정, 인스펙터 윈도우, 프리팹, 리소스 폴더 등 직렬화는 여러곳에서 사용됨.
유니티에서 사용하는 프리팹은 게임오브젝트 한개 혹은 여러개를 시리얼라이즈 한 것이다.

Serialization는 왜 하는가?
1. 전송하기 위해
2. 데이터베이스나 파일에 저장하여 보관하기 위해
3. 유니티에서는 해당 직렬화를 정보은닉을 지키면서 인스펙터 창을 통해 조작가능하기위한 기능으로도 사용한다.
한단어로 이야기하자면 ‘영속성’이 아닐까 한다. 영속성이란 오래도록 계속 유지되는 성질을 뜻한다. 객체를 다른 컴퓨터나 프로그램이 종료되고도 남겨놓거나 재사용하고 싶다면 어떻게 해야할까? 해당 객체에 관련된 코드가 있어야 할것이다. 하지만 코드만 있다고 끝인가? 아니다 객체에 대한 정보가 있어야 한다. 하나의 컴퓨터라면 있을수도 있다. 하지만 다른 컴퓨터나 프로그램이 종료됬다면? 객체에 관한 정보는 사라지지 않았을까? 객체를 있던 그대로 사용하거나 전송하기에는 무리가 있을것이다. 이때 바이트스트림으로 남겨놓거나 변환한다면 이 문제가 해결된다. 한마디로 직렬화를 하는 이유는, 이용하던(이용할) 데이터를 ‘복원’ 하기 위한 과정인것이다.

Unity의 Serialization
자바의 직렬화, 네트워크 직렬화 등등 각자의 환경에 직렬화는 직렬화 가능한 조건이 있다. 유니티도 마찬가지다. 유니티에서 사용되는 클래스(스크립트에 작성된)를 직렬화를 하기 위해서는 다음 조건을 지켜야 한다.

‘public’ 또는 ‘[SerializeField]’ 속성을 가져야 한다.
class MyClass{ public int serializeNum1; // or [SerializeField] private int serializeNum2 }

‘[SerializeField]’ 속성을 가졌다면 구조체와 class도 가능
구조체의 경우 Unity 4.5버전부터 가능하며, class의 경우는 비추상 클래스여야 한다.
static, const, readonly가 아니여야 한다.
‘fieldtype’은 직렬화 할 수 있는 타입이어야 한다.
int, string, bool, float 등의 기본타입
단순 필드 타입의 배열과 List<T>도 직렬화가 가능하다.
UnityEngine.Object 에서 파생한 오브젝트의 참조

클래스 내의 필드가 직렬화되도록 하려면 다음의 조건을 만족해야한다.
public이거나 [SerializeField] 속성이 있어야한다.
static이 아니어야한다.
const가 아니어야한다.
readonly가 아니어야한다.
직렬화가 가능한 필드타입이어야한다. (단순 필드 타입만 가능하다.)

마지막 조건을 보면 모든 타입이 다 직렬화가 가능하지 않다는 것을 알 수 있다.
직렬화가 가능한 단순 필드 타입은 다음을 얘기한다.

[Serializable] 속성이 있는 비 추상, 일반 클래스
[Serializable] 속성이 있는 커스텀 구조체
UnityEngine.Object에서 파생된 오브젝트
프리미티브 데이터 타입(int, float, double, bool. string 등)
열거형 타입
특정 Unity 타입 : Vector2, Vector3, Vector4, Rect, Quaternion, Matrix4x4, Color, Color32, LayerMask, AnimationCurve, Gradient, RectOffset, GUIStyle

직렬화가 지원되지 않는 타입은 iserializationcallbackreceiver 인터페이스를 상속받아 직렬화가 가능한
필드타입으로 수정하여 사용할 수 있다.
  
  
https://teddy.tistory.com/23 직렬화 예시
https://birthbefore.tistory.com/11 직렬화를 사용해 세이브, 로드 하는 예시
https://codinggom.github.io/Unity-%EC%A7%81%EB%A0%AC%ED%99%94/
