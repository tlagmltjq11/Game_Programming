## Generic Singleton Code
아직 Thread Safe는 적용하지 않은 상태.<br>
<br>

where T : SingletonMonoBehaviour<T> 부분에 대해서 헷갈린다면 해당 [링크](https://cafe.naver.com/unityhub/123621)에서 답변들 복기.<br>
<br>
<br>

```c#
using UnityEngine;

public class SingletonMonoBehaviour<T> : MonoBehaviour where T : SingletonMonoBehaviour<T>
{
    #region Field & Property
    private static bool m_ShuttingDown = false; //Destroy 여부 확인용 
    public bool m_DontDestroyOnLoad; //DontDestroyOnLoad 적용 여부
    private static T m_Instance = null;

    public static T Instance
    {
        get
        {
            /* 게임 종료 명령 후 타이밍에 따라 해당 매니저 클래스가
            호출 되는 상황을 막기 위해 m_ShuttingDown 변수를 이용, null 을 리턴한다.*/
            if (m_ShuttingDown)
            { 
                Debug.Log("[Singleton] Instance '" + typeof(T) + "' already destroyed. Returning null.");
                return null; 
            }

            //Awake가 호출되지 않은 상황 즉 씬에 컴포넌트로서 부착되지 않은 상황이다.
            if (m_Instance == null)
            {
                //인스턴스 존재 여부 확인
                m_Instance = GameObject.FindObjectOfType(typeof(T)) as T;

                if (m_Instance == null) //인스턴스가 존재하지 않을 경우
                {
                    //새로운 게임오브젝트를 만들어서 싱글톤 Attach
                    m_Instance = new GameObject("SingleTon Of " + typeof(T).ToString(), typeof(T)).GetComponent<T>();

                    if (m_Instance == null) //Creation Failure
                    {
                        Debug.LogError("Problem during the creation of " + typeof(T).ToString());
                    }
                }
            }

            return m_Instance;
        }
    }
    #endregion

    #region Unity Methods
    void Awake()
    {
        if (m_Instance == null)
        {
            m_Instance = this as T; //m_instance를 자신으로 설정
            OnAwake();

            //각 매니저마다 지정 후 사용
            if (m_DontDestroyOnLoad)
            {
                DontDestroyOnLoad(this);
            }
        }
        else
        {
            Destroy(gameObject);
        }
    }

    void Start()
    {
        if (m_Instance == (T)this)
        {
            OnStart();
        }
    }

    void OnApplicationQuit() 
    { 
        m_ShuttingDown = true;
    }

    void OnDestroy() 
    { 
        m_ShuttingDown = true;
    }
    #endregion

    #region Protected Virtual Methods
    protected virtual void OnAwake() { }
    protected virtual void OnStart() { }
    #endregion
}
```
<br>
<br>

## 참조링크
https://velog.io/@sonohoshi/4.-%EC%8B%B1%EA%B8%80%ED%86%A4-%ED%8C%A8%ED%84%B4-with-Unity <br>
https://skuld2000.tistory.com/26 <br>
