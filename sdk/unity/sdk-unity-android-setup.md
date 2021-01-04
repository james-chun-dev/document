Firebase 및 GamePub SDK for Unity 초기화에 대해 설명합니다.



## Firebase 안드로이드 앱 설정

* Android 앱 등록
* 업로드 및 앱서명 키 인증서 등록
* google-services.json 다운로드(Firebase 로그인 설정 후 다운로드 해주세요.)



![firebase register](https://user-images.githubusercontent.com/72954886/96234014-d3270d00-0fd3-11eb-977f-2391dfbfaed9.png)





## Unity 프로젝트 SDK 초기화

유니티프로젝트내에서 sdk 를 사용하기 위해 초기화를 진행합니다.



### google-services.json 파일 변환

1. google-services.json 파일을 xml 형태로 변환합니다. 파일 변환은 [Convert google-services.json to values XML](https://dandar3.github.io/android/google-services-json-to-xml.html) 에서 지원합니다.
2. 변환된 google-services.xml 파일은 `Assets/Plugins/Android/res/values`  에 복사합니다.



## Implement SetupSDK

위 설정이 완료되면 실행시 자동으로 SDK 초기화가 진행됩니다.

```
public class GamePubSDK : MonoBehaviour
    {
        static GamePubSDK instance;

        void Awake()
        {
            if (instance == null)
            {
                instance = this;
            }
            else if (instance != this)
            {
                Destroy(gameObject);
            }
            DontDestroyOnLoad(gameObject);
            SetupSDK();
        }

        public static GamePubSDK Instance
        {
            get
            {
                if (instance == null)
                {
                    GameObject go = new GameObject("GamePubSDK");
                    instance = go.AddComponent<GamePubSDK>();
                }
                return instance;
            }
        }

        void SetupSDK()
        {
            NativeInterface.SetupSDK(Guid.NewGuid().ToString());
        }
        ...
```



