# Universal SDK for Unity

Universal SDK is Login, InAppPurchase, Push, etc in Unity3d.

- [시작하기](#시작하기)
- [초기화](#초기화)
- [오류코드](#오류코드)
- [로그인 환경 설정](#로그인환경설정)
- [로그인/로그아웃/탈퇴/검증](#로그인/로그아웃/탈퇴/검증)
- [계정 연동](#계정연동)
- [결제](#결제)


## 시작하기

### SDK 다운로드

최신 버전의 GamePub SDK for Unity 를 받으려면 [Resources/Downloads](https://docs.igamepub.co.kr/unity-sdk-guide/resources/downloads) 에서 `.unitypackage` 파일을 다운로드하십시오.

### 프로젝트에 추가하기(import)

Unity 프로젝트를 연 상태에서 다운로드 한`.unitypackage` 파일을 두 번 클릭합니다. 여기에 표시된대로 패키지의 모든 항목을 가져옵니다:

![import unity package](https://user-images.githubusercontent.com/72954886/96225015-64de4c80-0fcb-11eb-89a6-4f1bc4a5485b.png)

### 유니티 씬에 GamePubSDK 프리펩 추가하기

패키지를 가져온 후 **Project** 패널에서 `Assets/GamePubSDK/` 아래에 **GamePubSDK** 프리펩을 찾을 수 있습니다. 로그인을 추가하려는 장면의 **Hierarchy** 패널로 드래그합니다:

![add prefab](https://user-images.githubusercontent.com/72954886/96225043-70317800-0fcb-11eb-9b47-0a74f3126599.png)

### 기본 환경 설정

#### Gradle 환경 설정 방법

* ##### mainTemplate.gradle

```
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    implementation 'com.gamepub.pubsdk:pubsdk:0.2.4' //added

    implementation 'com.google.code.gson:gson:2.8.5' //added
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.11" //added

**DEPS**}
```

* ##### launcherTemplate.gradle

```
dependencies {
    implementation project(':unityLibrary')
    implementation 'androidx.multidex:multidex:2.0.1' //added
    }

android {
    compileSdkVersion **APIVERSION**
    buildToolsVersion '**BUILDTOOLS**'

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    defaultConfig {
        minSdkVersion **MINSDKVERSION**
        targetSdkVersion **TARGETSDKVERSION**
        applicationId '**APPLICATIONID**'
        ndk {
            abiFilters **ABIFILTERS**
        }
        versionCode **VERSIONCODE**
        versionName '**VERSIONNAME**'

        multiDexEnabled true //added

        resValue("string", "facebook_app_id", "206076617381252") //added
        resValue("string", "google_web_client_id", "892923933661-1218sobcuhmmt93haj2j22dom4vdppgl.apps.googleusercontent.com") //added
    }
    ...
```

| 값                   | 설명                                                         |
| -------------------- | ------------------------------------------------------------ |
| facebook_app_id      | 페이스북에서 발급 받은 앱ID                                  |
| google_web_client_id | [Google API 콘솔](https://console.developers.google.com/apis/credentials)에서 프로젝트와 OAuth 2.0 항목을 선택하면 기존 프로젝트의 웹 클라이언트 ID를 확인할 수 있습니다. |


* ##### gradleTemplate.properties

```
android.useAndroidX=true
android.enableJetifier=true
```

* ##### baseProjectTemplate.gradle

```
allprojects {
    buildscript {
        repositories {**ARTIFACTORYREPOSITORY**
            google()
            jcenter()
            maven {
                url "https://dl.bintray.com/gamepub/gamepub-sdk"
            }
        }

        dependencies {            
            classpath 'com.android.tools.build:gradle:3.4.0'
            classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.3.11"
            classpath 'com.google.gms:google-services:4.3.0'
            **BUILD_SCRIPT_DEPS**
        }
    }

    repositories {**ARTIFACTORYREPOSITORY**
        google()
        jcenter()
        maven {
            url "https://dl.bintray.com/gamepub/gamepub-sdk"
        }
        flatDir {
            dirs "${project(':unityLibrary').projectDir}/libs"
        }
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

#### Firebase 안드로이드 앱 설정

* Android 앱 등록
* 업로드 키 인증서 등록 (APK 빌드 시 사용한 Keystore의 SHA-1)
* google-services.json 다운로드 (Firebase 로그인 설정 후 다운로드 해주세요.)

#### google-services.json 파일 변환

1. google-services.json 파일을 xml 형태로 변환합니다. 파일 변환은 [Convert google-services.json to values XML](https://dandar3.github.io/android/google-services-json-to-xml.html) 에서 지원합니다.
2. 변환된 google-services.xml 파일은 `Assets/Plugins/Android/res/values`  에 복사합니다.

## 초기화

### SetupSDK 구현부

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
## 오류코드
```
public class Error
{
    public int code {get; set;}
    public string message {get; set;}
}
```

## 로그인환경설정
### Firebase 구글 로그인 설정

* Authentication - SignIn method 를 통해 Google 사용 설정 활성화.
* [Google API 콘솔](https://console.developers.google.com/apis/credentials)에서 프로젝트와 OAuth 2.0 항목을 선택하면 기존 프로젝트의 웹 클라이언트 ID를 확인할 수 있습니다.

![firebase google login setup](https://user-images.githubusercontent.com/72954886/96237833-bc36e980-0fd8-11eb-925f-3de8a2e857b9.png)

### Firebase 페이스북 로그인 설정

* Authentication - SignIn method 를 통해 Facebook 사용 설정 활성화.
* Facebook Developer Console 에서 SHA-1 지문을 통해 앱 ID 와 비밀번호를 발급받아 입력합니다.

![facebook login setup](https://user-images.githubusercontent.com/72954886/96245921-de356980-0fe2-11eb-8f84-2cbc563e29ac.png)

### Firebase 게스트 로그인 설정

* Authentication - SignIn method 를 통해 **익명** 사용 설정 활성화.

### Android

launcherTemplate.gradle 설정

```groovy
dependencies {
    ...
    }

android {
    ...

    defaultConfig {
        ...
        
        resValue("string", "facebook_app_id", "123456") //added
        resValue("string", "google_web_client_id", "123456") //added
    }
    ...
```

### iOS

Preferences > GamePub SDK 탭을 통해서 설정.
- Facebook App ID 에 값을 입력합니다.
- 애플로그인을 활성화시키면 유니티 빌드시 Capability 설정까지 자동으로 설정됩니다.

![sdk settings](https://user-images.githubusercontent.com/72954886/111590319-4d6cef00-8809-11eb-8dce-0e9f1361377f.png)


## 로그인/로그아웃/탈퇴/검증

### 로그인 구현부

현재 GamePubSDK (GameObject) 가 있는 씬에서 로그인을 구현할 수 있습니다. 예를 들면:

```c#
public class MainController : MonoBehaviour
{    
    public void OnClickExampleLogin()
    {        
        GamePubSDK.Instance.Login(PubLoginType, result =>
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

LoginType 정의
```
public enum PubLoginType
{
    NONE,
    GOOGLE,
    FACEBOOK,
    GUEST,
    APPLE,
}
```
AccountServiceType 정의
```
public enum PubAccountServiceType
{
    NONE,
    ACCOUNT_LOGIN,      //로그인
    ACCOUNT_CONVERSION, //계정전환
    ACCOUNT_LINK,       //계정연동
}
```
UserInfo 정의

#### 자동로그인
```
code sample
```
#### 로그아웃 구현부
```
GamePubSDK.Ins.Logout(PubLoginType, result =>
{
    result.Match(
        value =>
        {
            UpdateRawSection(value);            
        },
        error =>
        {
            UpdateRawSection(error);
        });
});
```
#### 탈퇴
```
GamePubSDK.Ins.Secede(result =>
{
    result.Match(
        value =>
        {
            UpdateRawSection(value);            
        },
        error =>
        {
            UpdateRawSection(error);
        });
});
```
#### 검증
server to server 참고
## 계정연동

하나의 게임 계정에 복수 개의 소셜계정(구글/페이스북 등)을 연결/해제할 수 있는 기능입니다.(최소 연동 소셜 계정은 1가지입니다.)

>연동화면 UI는 개발사에서 구현해주세요.

### 연동
```
code sample
```
### 연동해제
```
code sample
```
## 결제
### 인앱 상품 조회
```
code sample
```
### 인앱 상품 결제
아래 함수 하나로 구글, 애플 결제가 가능합니다.
```
// productId : 스토어에 등록된 상품ID
// serverId  : 결제를 진행한 캐릭터의 서버아이디
// playerId  : 결제를 진행한 캐릭터의 캐릭터 아이디
// etc       : 결제를 진행한 캐릭터 기타 정보
GamePubSDK.Ins.InAppPurchase(
            "productId",
            "serverId",
            "playerId",
            "etc",
            result =>
        {
            result.Match(
                value => {
                    UpdateRawSection(value);
                },
                error => {
                    UpdateRawSection(error);
                });
        });
```
##### PurchaseInfo 정의
결제 성공 후 결제한 아이템의 정보입니다.
```
public class PubPurchaseData
{
    public int IntDate { get { return intDate; } }
    public string TransactionId { get { return transactionId; } }
    public int Status { get { return status; } }
    public string ServerId { get { return serverId; } }
    public string PlayerId { get { return playerId; } }
    public string ProductId { get { return productId; } }
}
```
### 결제 아이템 지급
server to server 참고

## 기타 API

### 쿠폰

>쿠폰을 입력 받는 UI는 개발사에서 구현해주세요.

#### 쿠폰 사용
```
// key       : 발급받은 쿠폰 키
// serverId  : 캐릭터의 서버아이디
// playerId  : 캐릭터의 캐릭터 아이디
// etc       : 캐릭터의 기타 정보
GamePubSDK.Ins.CouponUse(key, serverId, playerId, etc, result =>
{
    result.Match(value =>
    {
        Debug.Log(value.Code);
        Debug.Log(value.Msg);
    }, error =>
    {
        Debug.Log(error.Message);
    });
});
```

#### 아이템 지급

server to server 참고

### 사용자 정보 설정

사용자 언어, 푸시, 야간푸시, 광고푸시를 설정할 수 있습니다.
>on/off 설정하는 UI는 개발사에서 구현해주세요.

#### 푸시 설정
```
// languageCode : 언어 코드
// push         : 푸시
// pushNight    : 야간 푸시
// pushAd       : 광고 푸시
GamePubSDK.Ins.UserInfoUpdate(
    PubLanguageCode,
    push,
    pushNight,
    pushAd,
    result =>
    {
        result.Match(
            value =>
            {
                //value.AccountId
            },
            error =>
            {
                //error.Message
            });
    });
```
### 공지사항

>공지사항 UI는 개발사에서 구현해주세요.

```
GamePubSDK.Ins.OpenNotice(result =>
{
    result.Match(
        value =>
        {            
            for(int i=0; i<value.NoticeList.Length; i++)
            {                        
                //value.NoticeList[i].Title;
                //value.NoticeList[i].Contents;
            }
        },
        error =>
        {
            
        });
});
```

PubNoticeData 정의

```
public class PubNoticeData
{    
    public string Type { get { return type; } }
    public string Title { get { return title; } }
    public string Contents { get { return contents; } }
    public string ImageURL { get { return imageURL; } }
    public string Link { get { return link; } }
    public string StartDate { get { return startDate; } }
    public string EndDate { get { return endDate; } }
    public string RegDate { get { return regDate; } }
}
```

### 고객지원

게임펍 어드민에 설정된 고객센터 URL 로 이동됩니다.

```
GamePubSDK.Ins.OpenHelpURL(result =>
{
    result.Match(
        value => {
            
        },
        error => {
            
        });
});
```

### 약관동의

UI 제공 준비중

```
code sample
```

### 이용약관

이용약관 UI를 호출합니다.

>대시보드 - 약관 - 이용약관 설정 항목를 설정해주세요.

```
GamePubSDK.Ins.OpenPolicyLink(PubPolicyType.SERVICE);
```

### 개인정보취급방침
개인정보 취급방침 UI를 호출합니다.
>대시보드 - 약관 - 개인정보처리방침 설정 항목를 설정해주세요.
```
GamePubSDK.Ins.OpenPolicyLink(PubPolicyType.PRIVACY);
```

### 환불규정
환불규정 UI를 호출합니다.
>대시보드 - 약관 - 환불정책 설정 항목를 설정해주세요.
```
GamePubSDK.Ins.OpenPolicyLink(PubPolicyType.REFUND);
```

