애플 로그인 설정 및 API 에 대해 설명합니다.



## Firebase 애플 로그인 설정

* Firebase 프로젝트에 iOS, Android 모두 앱 등록이 되어 있어야 합니다.
* Apple Developer Program에 가입
* Firebase > Authentication > Sign-in method > Apple을 로그인 제공업체로 사용 설정
* Apple Developer 사이트에서 Apple로 [로그인 구성](https://help.apple.com/developer-account/#/dev1c0e25352)
  1. [Certificates, Identifiers & Profiles(인증서, 식별자 및 프로파일)(영문)](https://developer.apple.com/account/resources)에서 사이드바의 Identifiers(식별자)를 선택한 다음 왼쪽 상단 모서리에 있는 Add(추가) 버튼(+)을 클릭합니다.
  2. Service ID(서비스 ID)를 선택한 다음 Continue(계속)를 클릭합니다.
  3. Services ID(서비스 ID) 설명을 입력하고 고유 식별자를 제공합니다.
  4. Services ID(서비스 ID)를 등록하고 이를 목록에서 선택하여 구성을 시작합니다.
  5. Sign in with Apple(Apple로 로그인)을 선택하고 Configure(구성)를 클릭합니다.
  6. 모달이 나타나면 Primary App IDs(기본 App ID) 목록에서 웹사이트와 관련된 앱을 선택합니다.
  7. Website URLs(웹 사이트 URL) 아래에서 도메인, 하위 도메인 또는 반환 URL을 쉼표로 구분된 목록으로 제공합니다. 최소한 한 개의 도메인 또는 하위 도메인을 제공해야 합니다.
  8. Done(완료)을 클릭하여 구성을 저장합니다.
  9. Continue(계속)를 클릭합니다.
  10. 구성을 검토한 다음 Save(저장)를 클릭합니다.

* Apple Developer 사이트에서 [Apple Login Key 생성하기](https://help.apple.com/developer-account/#/dev77c875b7e)(Service ID 생성과 설정이 완료된 후)
  1. [Certificates, Identifiers & Profiles(인증서, 식별자 및 프로파일)(영문)](https://developer.apple.com/account/resources)에서 사이드바의 Keys를 선택한 다음 왼쪽 상단 모서리에 있는 Add(추가) 버튼(+)을 클릭합니다.
  2. Key Name을 입력하고 Sign in with Apple 선택하고 Configure(구성)를 클릭합니다.
  3. 모달이 나타나면 Primary App IDs(기본 App ID) 목록에서 웹사이트와 관련된 앱을 선택합니다.
  4. Save(저장)를 클릭하여 구성을 저장합니다.
  5. Continue(계속)를 클릭합니다.
  6. Register를 클릭하면 Download 버튼이 활성화 되고 클릭하면 .p8 확장자의 파일이 로컬에 저장됩니다.
  7. auth_key.p8 파일 다운로드는 1회성으로 저장되며 다시 다운받을 수 없습니다.
  8. 저장된 p8 파일을 에디터로 열게 되면 비공개 키가 나옵니다.

* Firebase Apple 로그인 설정 구성정보 저장
  1. 서비스 ID (Apple Developer 사이트에서 생성한 서비스ID)
  2. Apple 팀ID (Apple Developer 사이트 > View Membership > TeamID 값 복사)
  3. 키 ID (비공개 키 생성시 발급되는 Key ID)
  4. 비공개 키 (아래 이미지 참고)



![firebase project](https://user-images.githubusercontent.com/72954886/101318408-a42a6a80-38a3-11eb-8e2e-f8ee257634f3.png)



![apple developer](https://user-images.githubusercontent.com/72954886/101319035-9de8be00-38a4-11eb-85cc-910c28f233c9.png)



Firebase Project 에 등록된 앱 identifier 와 apple developer 콘솔에 등록된 앱 identifier 가 일치해야 합니다.



### 서비스아이디 만들기

![service id generate](https://user-images.githubusercontent.com/72954886/101321354-91fefb00-38a8-11eb-9894-45aa641b09ba.png)



![service id config](https://user-images.githubusercontent.com/72954886/101322397-4cdbc880-38aa-11eb-8107-2b73d95be58a.png)



![web auth config](https://user-images.githubusercontent.com/72954886/101322410-51a07c80-38aa-11eb-9230-f2635b70557f.png)





### 비공개키 만들기

![new key](https://user-images.githubusercontent.com/72954886/101326007-fec9c380-38af-11eb-80e4-4ad492fe5417.png)



![configure key](https://user-images.githubusercontent.com/72954886/101326027-08ebc200-38b0-11eb-9c34-36b8dff94c64.png)



![register](https://user-images.githubusercontent.com/72954886/101326054-11dc9380-38b0-11eb-8728-29b03eef47c9.png)



![download](https://user-images.githubusercontent.com/72954886/101326069-17d27480-38b0-11eb-82b6-5fff9ecf7b43.png)

### Firebase Apple Login 설정 구성

![firebase apple login cofing](https://user-images.githubusercontent.com/72954886/101326559-cbd3ff80-38b0-11eb-8012-010749bc8637.png)





## Implement Login with Apple

Now, you can implement Login with Apple in the scene where the GamePubSDK (GameObject) exists. For example:

```
public class MainController : MonoBehaviour
{    
    public void OnClickAppleLogin()
    {        
        GamePubSDK.Instance.Login(PubLoginType.APPLE, result =>
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



