페이스북 로그인 설정 및 API 에 대해 설명합니다.



## Firebase 페이스북 로그인 설정

* Authentication - SignIn method 를 통해 Facebook 사용 설정 활성화.
* Facebook Developer Console 에서 SHA-1 지문을 통해 앱 ID 와 비밀번호를 발급받아 입력합니다.



![facebook login setup](https://user-images.githubusercontent.com/72954886/96245921-de356980-0fe2-11eb-8f84-2cbc563e29ac.png)





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
        
        resValue("string", "facebook_app_id", "123456") //added
    }
    ...
```





## Implement Login with Facebook

Now, you can implement Login with Facebook in the scene where the GamePubSDK (GameObject) exists. For example:

```
public class MainController : MonoBehaviour
{    
    public void OnClickFacebookLogin()
    {        
        GamePubSDK.Instance.Login(PubLoginType.FACEBOOK, result =>
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

