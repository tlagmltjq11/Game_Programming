## 🔔 Serialization

![직렬화](https://user-images.githubusercontent.com/43705434/129681875-90cc5ca8-836c-43e2-96ad-40fdf0ecc544.PNG)<br>
<br>

의미 자체는 **특정 객체(데이터)를 바이트의 나열(Stream) 혹은 스트링으로 바꿔서 파일이나 네트워크통신으로 Stream 가능하게 해주는 것이다.**<br>
쉽게는 객체의 상태를 저장하기 위해서 객체를 Byte stream 으로 변환하는 것을 의미한다고 알면 된다.<br>
**-> 바이너리 스트림 or 스트링 형태로 변환했기 때문에 데이터베이스에 저장/읽기, 네트워크로 송신/수신이 간편해진다.** ⭐⭐<br>
반대로 직렬화된 데이터를 원래 객체(데이터)로 바꿔주는 것은 Deserialize(디시리얼라이즈, 역직렬화) 라고 한다.<br>
결국 바이너리 데이터와 객체 데이터 사이의 인코딩과 디코딩인 것이다.<br>

에셋을 읽고 쓰는 과정, 인스펙터 윈도우, 프리팹, 리소스 폴더 등 직렬화는 여러곳에서 사용되고 있다.<br>
-> 유니티에서 사용하는 프리팹은 게임오브젝트 한개 혹은 여러개를 시리얼라이즈 한 것이다.<br>
<br>
<br>

## 🔔 Serialization는 왜 하는가?
1. 네트워크로 송/수신하기 위해 사용한다. ⭐<br>
2. 데이터베이스나 파일에 저장하여 보관하기 위해 사용한다. ⭐<br>
3. 정보은닉을 지키면서 인스펙터 창을 통해 조작하기위한 용도로도 사용한다. ⭐<br>

**2번 설명**<br>
객체를 다른 컴퓨터나 프로그램이 종료되고도 남겨놓거나 재사용하고 싶다면 어떻게 해야할까?<br>
해당 객체에 관련된 코드가 있어야 할것이다. 하지만 코드만 있다고 끝인가? 아니다 객체에 대한 정보가 있어야 한다.<br>
하나의 컴퓨터라면 있을수도 있다. 하지만 다른 컴퓨터나 프로그램이 종료됬다면? 객체에 관한 정보는 사라지지 않았을까?<br>
객체를 있던 그대로 사용하거나 전송하기에는 무리가 있을것이다. 이때 바이트스트림으로 남겨놓거나 변환한다면 이 문제가 해결된다.<br>
한마디로 직렬화를 하는 이유는, **이용하던(이용할) 데이터를 ‘복원’ 하기 위한 과정인것이다.**<br>
<br>
<br>

## 🔔 Unity의 Serialization 조건
자바의 직렬화, 네트워크 직렬화 등등 각자의 환경에 직렬화는 직렬화 가능한 조건이 있다. 유니티도 마찬가지다.<br>
**클래스 내의 필드가 직렬화되도록 하려면 다음의 조건을 만족해야한다.** ⭐<br>

👉 public이거나 [SerializeField] 속성이 있어야한다.<br>
-> 유니티는 기본적으로 Public 데이터만 직렬화한다.<br>
-> Private은 SerializeField를 사용해야 직렬화가 가능하다.<br>
👉 static이 아니어야한다.<br>
👉 const가 아니어야한다.<br>
-> **정적 또는 const 멤버는 논리적으로 인스턴스의 일부가 아니라 전체 클래스의 일부이기 때문에 불가능하다고 한다.**<br>
-> 하지만 static이 프로퍼티라면 직렬화가 가능하다고 함.<br>
👉 readonly가 아니어야한다.<br>
👉 직렬화가 가능한 필드타입이어야한다. (단순 필드 타입만 가능하다.)<br>

마지막 조건을 보면 모든 타입이 다 직렬화가 가능하지 않다는 것을 알 수 있다.<br>
**직렬화가 가능한 단순 필드 타입은 다음을 얘기한다.** ⭐<br>

👉 [Serializable] 속성이 있는 비 추상, 일반 클래스<br>
👉 [Serializable] 속성이 있는 커스텀 구조체<br>
👉 UnityEngine.Object에서 파생된 오브젝트<br>
👉 프리미티브 데이터 타입(int, float, double, bool. string 등)<br>
👉 열거형 타입<br>
👉 특정 Unity 타입 : Vector2, Vector3, Vector4, Rect, Quaternion, Matrix4x4,<br>
Color, Color32, LayerMask, AnimationCurve, Gradient, RectOffset, GUIStyle<br>
<br>

직렬화가 지원되지 않는 타입은 **iserializationcallbackreceiver ⭐ 인터페이스를 상속받아 직렬화가 가능한<br>
필드타입으로 수정하여 사용할 수 있다.**<br>
<br>

**클래스 직렬화를 통해 인스펙터에 노출되는 예시**<br>
![클래스직렬화1](https://user-images.githubusercontent.com/43705434/129681870-2d38517a-f077-45ec-abef-bbf829c266e1.PNG)<br>
![클래스직렬화2](https://user-images.githubusercontent.com/43705434/129681879-a0f8380a-4336-461b-9616-db6613b5198c.PNG)<br>
<br>
<br>

## 🔔 예시 - Serializer 구현
```c#
/*
 * 스트링으로 시리얼라이즈 & 디시리얼라이즈
 * 바이트배열로 시리얼라이즈 & 디시리얼라이즈
*/
 
using System;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;
 
public class Serializer
{
    #region serialize
 
    //오브젝트 시리얼라이즈 후 결과 값을 스트링으로 변환하여 반환
    public static string ObjectToStringSerialize(object obj)
    {
        using (var memoryStream = new MemoryStream())
        {
            BinaryFormatter bf = new BinaryFormatter();
            bf.Serialize(memoryStream, obj);
            memoryStream.Flush();
            memoryStream.Position = 0;
 
            return Convert.ToBase64String(memoryStream.ToArray());
        }
    }
 
    //오브젝트를 시리얼라이즈 후 바이트 배열 형태로 반환
    public static byte[] ObjectToByteArraySerialize(object obj)
    {
        using (var memoryStream = new MemoryStream())
        {
            BinaryFormatter bf = new BinaryFormatter();
            bf.Serialize(memoryStream, obj);
            memoryStream.Flush();
            memoryStream.Position = 0;
 
            return memoryStream.ToArray();
        }
    }
 
    #endregion
 
    #region Deserialize
 
    //스트링 타입의 시리얼라이즈된 데이타를 디시리얼라이즈 후 해당 타입으로 변환하여 반환
    public static T Deserialize<T>(string xmlText)
    {
        if (xmlText != null && xmlText != String.Empty)
        {
            byte[] b = Convert.FromBase64String(xmlText);
            using (var stream = new MemoryStream(b))
            {
                var formatter = new BinaryFormatter();
                stream.Seek(0, SeekOrigin.Begin);
                return (T)formatter.Deserialize(stream);
            }
        }
        else
        {
            return default(T);
        }
    }
 
    //바이트 배열 형태의 시리얼라이즈된 데이타를 디시리얼라이즈 후 해당 타입으로 변환하여 반환
    public static T Deserialize<T>(byte[] byteData)
    {
        using (var stream = new MemoryStream(byteData))
        {
            var formatter = new BinaryFormatter();
            stream.Seek(0, SeekOrigin.Begin);
            return (T)formatter.Deserialize(stream);
        }
    }
 
    #endregion
}
```
<br>

게임 중 생성된 인스턴스를 생성하고 게임이 진행 되면서 인스턴스가 가지는 값들이 변경되었다.<br>
그리고 이 상태를 그대로 저장하기위해 또는 서버로 전송하기 위해 시리얼라이즈가 필요한 상황이다.<br>
이때 인스턴스를 파라미터로 시리얼라이저에게 전달한다.<br>
**시리얼라이저는 전달받은 오브젝트를 시리얼라이즈 후 스트링 또는 바이트 배열 형태로 반환한다.** ⭐<br>
반환받은 데이타를 저장또는 서버로 전달이 가능하다.<br>
인스턴스를 시리얼라이즈하기 위해서는 **바이너리 포매터(BinaryFormatter)와 메모리 스트림(MemoryStream)이 필요하다.** ⭐<br>
**시리얼라이즈 기능은 바이너리 포매터에 포함되어있다.** 이것으로 시리얼라이즈 할 때 파라미터를 인스턴스와 메모리 스트림을 전달하면<br>
메모리 스트림에 인스턴스가 직렬화되어 들어간다. 여기까지의 과정은 똑같고, **메모리 스트림을 어떻게 가공하느냐에 따라 반환 값이 달라진다.<br>
ToArray()를 사용하면 바이트 배열형태로 반환하고 ToArray() 한 값을 ToBase64String()로 컨버팅하게 되면 스트링 형태로 반환하게된다.**<br>
디시리얼라이즈는 이과정을 거꾸로한다고 생각하면 된다. 스트링 타입은 바이트 배열로 변환하고 바이트 배열은 메모리 스트림으로 들어간다.<br>
이 메모리 스트림의 바이너리 포매터의 디시리얼라이즈 기능을 사용해 디시리얼라이즈한다. 최종적으로 해당 타입으로 형변환 하여 반환하게 된다.<br>
<br>

![직렬화예시1](https://user-images.githubusercontent.com/43705434/129681886-2c0c536c-b116-4cfb-b932-694a2e07b167.PNG)<br>
<br>
  
시리얼라이즈된 데이터를 에디터로 열어보면 위와같이 보인다.<br>
해당 데이타를 저장이나 전송할 때 사용하게된다. 그전에 MD5나 SHA-1 등의 암호화를 거치기를 권장한다.<br>

※ 주의 'base64String()'로 컨버팅 하지않고 그냥 ToString()을 하게되면 다른 플랫폼으로 전달 시 전달 된 곳에서 인식하지 못하는 경우도 있다.<br>
반드시 base64 컨버팅하는 것을 추천한다. ⭐<br>
<br>
<br>


## 🔔 예시 - Serializer 사용

![직렬화예시2](https://user-images.githubusercontent.com/43705434/129681888-e72dca80-f438-4337-be07-dd9139c5a9e0.PNG)<br>
<br>

게임 정보를 담당하는 클래스를 만들고 이것을 인스턴스화한다. UI에서 입력된 값들로 정보를 갱신한다.<br>
저장버튼을 누르면 해당 인스턴스가 시리얼라이즈 되어 스트링형태로 저장된다.<br>
로드 버튼을 누르면 반대의 과정이 일어난다.<br>
스트링형태로 저장된 데이타를 디시리얼라이즈 과정을 거치고 최종적으로 게임 정보 인스턴스를 반환한다.<br>
반환된 인스턴스의 정보들을 UI에 표시한다<br>
<br>
  
```c#  
/*
 * UI 모음
 * 이름 입력, 표시
 * 리벨 입력, 표시
*/
public class UIview : MonoBehaviour
{
    public Text textName;   //이름
    public Text textLevel;  //레벨
 
    public Text inputName;    //입력필드의 이름 값
    public Text inputLevel;   //입력필드의 레벨 값
}
```
화면에 표시되는 UI를 모아둠<br>
<br>

```c#
/*
 * 게임 초기화
 * 게임 저장, 게임 로드 버튼 클릭 처리
*/
public class Controller : MonoBehaviour
{
    public UIview uiView;       //UIview 컴포넌트 캐싱
    private SaveLoad saveLoad;  //저장/불러오기 객체
    private GameInfo gameInfo;  //게임 정보 객체
 
    void Start()
    {
        InitGame();
    }
 
    //게임 초기화
    private void InitGame()
    {
        //저장/불러오기 객체 초기화
        saveLoad = new SaveLoad();
        //게임 정보 객체 초기화
        gameInfo = new GameInfo("teddy", 1);
 
        //초기화한 게임 정보를 화면에 정보 표시
        uiView.textName.text = gameInfo.GetUserName();
        uiView.textLevel.text = gameInfo.GetUserLevel().ToString();
    }
 
    //저장 버튼 클릭
    public void OnclickSaveButton()
    {
        //입력한 정보를 화면에 표시
        //uiView.textName.text = uiView.inputName.text;
        //uiView.textLevel.text = uiView.inputLevel.text;
 
        //게임 정보 입력
        gameInfo.SetUserName(uiView.inputName.text);
        gameInfo.SetUserLevel(Convert.ToInt32(uiView.inputLevel.text));
 
        //입력한 정보를 저장
        saveLoad.SaveGameInfo(gameInfo);
    }
 
    //불러오기 버튼 클릭
    public void OnclickLoadButton()
    {
        //저장된 게임 정보 로드하여 게임 정보 객체에 할당
        gameInfo = saveLoad.LoadGameInfo();
 
        //불러온 게임 정보를 화면에 표시
        uiView.textName.text = gameInfo.GetUserName();
        uiView.textLevel.text = gameInfo.GetUserLevel().ToString();
    }
}
```
게임이 시작되고 정보들을 초기화한다. UI 버튼에 대한 입력을 받아서 각각의 기능을 처리한다.<br>
<br>

```c#
/*
 * 저장할 게임 정보 클래스
*/
[Serializable]
public class GameInfo 
{
    private string userName;    //유저 이름
    private int userLevel;      //유저 레벨
 
    public GameInfo(string userName, int userLevel)
    {
        this.userName = userName;
        this.userLevel = userLevel;
    }
 
    //유저이름 가저오기
    public string GetUserName()
    {
        return this.userName;
    }
 
    //유저 레벨 가져오기
    public int GetUserLevel()
    {
        return this.userLevel;
    }
 
    //유저 이름 할당
    public void SetUserName(string userName)
    {
        this.userName = userName;
    }
 
    //유저 레벨 할당
    public void SetUserLevel(int userLevel)
    {
        this.userLevel = userLevel;
    }
}
```
게임에서 사용되는 정보들을 담은 클래스이다. 이 클래스에는 [Serializable] 이 붙어있어 시리얼라이즈가 가능하다<br>
<br>

```c#
/*
 * 게임을 저장하고 로드합니다.
*/
public class SaveLoad
{
    //저장/로드 할 때 필요한 키 값
    private readonly string SAVEKEY = "GAMEINFO_SAVE_KEY";
 
    //게임 정보 저장하기
    public void SaveGameInfo(GameInfo gi)
    {
        //객체를 스트링형태로 변환
        string saveData = Serializer.ObjectToStringSerialize(gi);
        //변환된 스트링 값을 PlayerPrefs에 저장
        PlayerPrefs.SetString(SAVEKEY, saveData);
    }
 
    //게임 정보 불러오기
    public GameInfo LoadGameInfo()
    {
        //PlayerPrefs에 저정된 스트링 형태의 게임 정보 가져오기
        string laodData = PlayerPrefs.GetString(SAVEKEY, string.Empty);
        //스트링형태의 정보를 게임 정보 타입으로 디시리얼라이즈 후 반환
        return Serializer.Deserialize<gameinfo>(laodData);
    }
}
```
게임 정보를 담은 인스턴스를 시리얼라이즈 요청하고 반환된 값을 프리퍼런스에 저장한다.<br>
반대로 불러올 때에는 디시리얼라이즈 과정을 거쳐 게임 정보 인스턴스를 반환한다.<br>
<br>
<br>

## 🔔 관련 Attribute
👉 Serializable : 하위 목록들을 직렬화 시킨다.<br>
👉 NonSerialized : 직렬화 대상에서 제외한다. -> 제외됐기에 인스펙터 창에서도 숨겨진다.<br>
👉 SerializeField : Public이 아닌 필드를 직렬화 시킨다.(바로 아래만)<br>
👉 HideInInspector : 직렬화 여부와 관계없이 필드를 인스펙터 창에서 숨긴다.<br>

**HideInInspector vs NonSerialized**<br>
HideInInspector는 인스펙터에서 수정된 적이 있다면 해당 값을 유지하지만<br>
NonSerialized는 선언과 동시에 디폴트 값으로 설정된다.<br>
-> 애초에 NonSerialized는 직렬화 대상에서 제외하는 기능이기 때문에 자동으로 인스펙터에서 숨겨지는 것.<br>
-> HideInInspector는 말그대로 직렬화 여부와 관계없이 인스펙터 창에서 숨기는 용도!<br>
**둘은 엄연히 기능이 다르다는 점 주의.** ⭐<br>
<br>
<br>

## 🔔 Marshalling
**마샬링이란 이기종간의 통신을 위해서 서로간의 데이터 형식을 맞추는 것을 의미한다.**<br>
예를들면 Little Endian / Big Endian 형태의 이기종에서 통신을 하기위해 데이터 형식을 맞추는 것을 의미한다.<br>

**저장 또는 전송에 적합한 다른 데이터 형식으로 변환하는 과정이다.<br>
서버에 넘겨지는 인자, 리턴 값들을 프로그래밍 인터페이스에 맞도록 데이터를 조직화하고,<br>
미리 정해진 다른 형식으로 변환하는것 이것이 프로그래밍에서의 마샬링이다.**<br>
이는 XML로의 마샬링, Byte 스트림으로의 마샬링 등 다양한 방법이 있는데 이처럼 데이터 교환시<br>
어떠한 정해진 표준에 맞게 해당 데이터를 가공하는 것이 마샬링이라면,<br>
**가공된 데이터를 원격지에서 사용하기위한 과정을 언마샬링 이라고 한다.**<br>
<br>

마살링도 참조 마샬링과 값 마샬링으로 구분할 수 있다.<br>

* 👉 참조 마샬링(MBR): MarshalByRefObject 를 상속<br>
객체의 메모리를 통째로 저장한 후 다른 머신에서 객체를 복원해서 사용하는 기술<br>

* 👉 값 마샬링(MBV): Serializable Attribute를 지정하거나 ISerializable 인터페이스를 구현<br>
값 마샬링이란 객체를 핸들하기 위한 정보만을 묶어서 넘긴 후 그 정보를 이용해서 원격으로 객체를 핸들하는 기술<br>
-> **직렬화는 값 마샬링의 한 종류이다!**<br>
<br>
<br>

## 🔔 Marshalling 예시
마샬링이란 어떤 언어(C#)로 작성된 프로그램의 출력 매개변수들(Object, Struct, Data 등)을<br>
다른 언어(C++)로 작성된 프로그램의 입력으로 전달해야 하는 경우에 필요하다.<br>

ex) C++ 로 작성된 프로그램에서 객체를 저장하거나 전송할 수 있는 형태로 묶어(마샬링) 다른 환경인 C#(닷넷)으로 전달하여<br>
해당 C# 프로그램에서 마샬링으로 전달된 데이터를 다시 복원(언마샬링)하여 사용하는것이다.<br>
예를 들면, function setWindowText()를 보자. 이 함수는 LPCTSTR(LPCSTR = long pointer constant t_string = const tchar * ) 을 다루지만<br>
System.String을 사용하지 않는 함수이다. 또한 이 함수의 반환값은 BOOL이라는 타입으로 변환 성공여부를 반환한다.<br>
하지만 C# BOOL이 아닌 System.Boolean타입을 가지고 있기 때문에 이 함수를 사용할 수 없다.<br>
다른 환경에서도 사용하기 위해서는 data type format을 변경하기 보다는 타입명만 바꿔주면 해결이 된다.<br>
예를 들어, System.String str = “Hello”;<br>
이 부분을 System.Char의 array로 표현하면<br>
System.Char[] ch = str.ToCharArray();<br>
다음과 같이 나타낼 수 있을것이다.<br>
이 두개의 data는 다르다고 할 수 있을까?<br>
답은 아니다.<br>
둘다 같은 data 를 다루고 있고 str와 ch 변수 둘다 같은 방식으로 담기게 된다.<br>
C#에서는 System.Int32, Window API는 INT를 가지고있으므로 System.Int32에서 INT로 정렬하려고 한다면<br>
data type 만 변경하면 해결될 수 있다. (내용을 가공하거나 다른 형식으로 복사하여 변형시키는 등으로 해결하지 않아도 된다.)<br>
<br>
<br>

## 🔔 Marshalling과 직렬화
Marshalling 과 Serialization 은 원격 프로시저를 호출하는 것에서는 약간 유사하지만, 의도를 따지면 의미적으로 틀리다.<br>
Marshalling 을 하게 되면, 원격 프로시저를 호출하는 것에서 함수의 parameter 값들 return 값들을 전달할 수 있다.<br>
보통 Marshalling 은 여기저기에서 parameter 들을 얻는 반면, Serialization은 구조화된 데이터 를 byte stream 과 같은<br>
primitive 형식 혹은 그 반대로 복사를 하는 것을 의미힌다.<br>
이러한 의미에서 Serialization은 marshalling 의 pass-by-value 를 구현하는 것의 일종으로 볼 수 있다.<br>
<br>
<br>

## 🔔 참조링크
https://teddy.tistory.com/23 직렬화 예시 <br>
https://birthbefore.tistory.com/11 직렬화 예시 <br>
https://codinggom.github.io/Unity-%EC%A7%81%EB%A0%AC%ED%99%94/ <br>
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=pxkey&logNo=221307184650 <br>
https://devjaya.tistory.com/1 (마샬링) <br>
https://hwanine.github.io/network/Marshalling/ (마샬링) <br>
https://starblood.tistory.com/entry/Marshalling-vs-Serialization-마샬링-과-시리얼라이즈-의-차이 <br>
<br>
<br>
