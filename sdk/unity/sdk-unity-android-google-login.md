구글 로그인 설정 및 API 에 대해 설명합니다.



## Firebase 구글 로그인 설정

* Authentication - SignIn method 를 통해 Google 사용 설정 활성화.
* [Google API 콘솔](https://console.developers.google.com/apis/credentials)에서 프로젝트와 OAuth 2.0 항목을 선택하면 기존 프로젝트의 웹 클라이언트 ID를 확인할 수 있습니다.



![firebase google login setup](https://user-images.githubusercontent.com/72954886/96237833-bc36e980-0fd8-11eb-925f-3de8a2e857b9.png)





## Unity Project 설정

launcherTemplate.gradle 파일에 Firebase 에서 설정한 웹 클라이언트 ID 를 아래와 같이 설정합니다.



```groovy
dependencies {
    ...
    }

android {
    ...

    defaultConfig {
        ...
        
        resValue("string", "google_web_client_id", "123456") //added
    }
    ...
```





## Implement Login with Google

Now, you can implement Login with Google in the scene where the GamePubSDK (GameObject) exists. For example:

```c#
public class MainController : MonoBehaviour
{    
    public void OnClickGoogleLogin()
    {        
        GamePubSDK.Instance.Login(PubLoginType.GOOGLE, result =>
        {
            result.Match(
                value => {
                    //Debug.Log("Login OK. User display name : " + value.UserProfile.DisplayName);
                },
                error => {
                    //Debug.Log("Login failed : " + error.Message);
                });
        });
    }
    ...
```

