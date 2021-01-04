## SetupSDK

[설명]

SDK 를 사용할 수 있는 상태로 만드는 초기화 함수.

정상적으로 초기화가 된다면 인앱상품리스트를 콜백으로 반환해준다.

[정의]

```c#
public void SetupSDK(string deviceUID, // 디바이스 아이디
                     Action<Result<PubSetupSDKResult>> action)
```

[예제]

```
GamePubSDK.Instance.SetupSDK(SystemInfo.deviceUniqueIdentifier, result =>
{
   result.Match(
         value => {
            foreach(PubInAppProduct item in value.InAppProducts)
            {
               Debug.Log(item.productID);               
            }                    
         },
         error => {            
            Debug.Log(error.ResponseCode);
            Debug.Log(error.Message);
            Debug.Log(error.ErrorCode);
         });
});
```



## Login

[설명]

로그인 함수.

성공시에는 로그인한 해당 소셜계정의 정보를 콜백으로 반환해준다.

[정의]

```c#
public void Login(PubLoginType loginType, // 로그인타입(구글,페북,애플,게스트)
                  PubAccountServiceType serviceType, // 서비스타입(로그인,계정전환)
                  Action<Result<PubLoginResult>> action)
```

[예제]

```
GamePubSDK.Instance.Login(PubLoginType.GOOGLE,
            PubAccountServiceType.ACCOUNT_LOGIN, result =>
{
   result.Match(
         value => {                                
            Debug.Log(value.UserProfile.DisplayName);
            Debug.Log(value.UserProfile.UniqueId);
            Debug.Log(value.UserProfile.ChannelId);
            Debug.Log(value.UserProfile.Email);
         },
         error => {                    
            Debug.Log(error.ResponseCode);
            Debug.Log(error.Message);
            Debug.Log(error.ErrorCode);
         });
});
```



## Logout

[설명]

로그아웃 함수.



[예제]

```
GamePubSDK.Instance.Logout(result =>
{
   result.Match(
         value =>
         {                    
            Debug.Log(value.Message);
         },
         error =>
         {
            Debug.Log(error.ResponseCode);
            Debug.Log(error.Message);
            Debug.Log(error.ErrorCode);
         });
});
```



## OpenPolicyLink

[설명]

약관사이트를 웹뷰형태로 열어주는 함수.

[정의]

```c#
public void OpenPolicyLink(PubPolicyType policyType) // 정책타입(개인정보처리방침, 환불, 서비스)
```

[예제]

```
GamePubSDK.Instance.OpenPolicyLink(PubPolicyType.PRIVACY);
```



## ImageBanner

[설명]

어드민 콘솔에 등록된 이미지를 웹뷰형태로 Display.



[예제]

```
GamePubSDK.Instance.ImageBanner();
```



## InAppPurchase

[설명]

인앱결제 함수.

[정의]

```c#
public void InAppPurchase(string pid, // 인앱상품 아이디
                          Action<Result<PubPurchaseData>> action)
```

[예제]

```
GamePubSDK.Instance.InAppPurchase("gamepub_1000", result =>
{
   result.Match(
         value => {
            //value.IntDate
            //value.PlayerId
            //value.ProductId
            //value.ServerId
            //value.Status
            Debug.Log(value.Status);
         },
         error => {
            Debug.Log(error.ResponseCode);
            Debug.Log(error.Message);
            Debug.Log(error.ErrorCode);
         });
});
```



## OpenHelpURL

[설명]

고객센터 페이지를 웹뷰형태로 열어주는 함수.



[예제]

```
GamePubSDK.Instance.OpenHelpURL(result =>
{
   result.Match(
         value => {
            Debug.Log(value.Message);
         },
         error => {
            Debug.Log(error.ResponseCode);
            Debug.Log(error.Message);
            Debug.Log(error.ErrorCode);
         });
});
```





## ResponseCode

| code | Description       |
| ---- | ----------------- |
| 0    | 성공              |
| 1    | 유저가 취소했을때 |
| 2    | 네트워크 에러     |
| 3    | 서버 에러         |
| 4    | 인증 에러         |
| 5    | 내부 에러         |
| 6    | 결제 에러         |



## ErrorCode

| code | Message            | Description           |
| ---- | ------------------ | --------------------- |
| 204  | NoContent          |                       |
| 400  | BadParameter       | 파라미터가 부정확하다 |
| 401  | Unauthorized       | 인증 실패             |
| 404  | NotFound           |                       |
| 412  | PreconditionFailed | 유저 접속 제한        |
| 502  | ServiceUnavailable | 서버 에러             |
| 503  | Bad Gateway        | 서버 점검             |

