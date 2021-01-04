게스트 로그인 설정 및 API 에 대해 설명합니다.



## Firebase 게스트 로그인 설정

* Authentication - SignIn method 를 통해 **익명** 사용 설정 활성화.

  





## Implement Login with Guest

Now, you can implement Login with Guest in the scene where the GamePubSDK (GameObject) exists. For example:

```
public class MainController : MonoBehaviour
{    
    public void OnClickGuestLogin()
    {        
        GamePubSDK.Instance.Login(PubLoginType.GUEST, result =>
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

