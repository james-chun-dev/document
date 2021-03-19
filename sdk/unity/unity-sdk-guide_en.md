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

### Download Page

To get the latest GamePub SDK for Unity, download the `.unitypackage` file from our [Resources/Downloads](https://docs.igamepub.co.kr/android-sdk-guide/resources/downloads).

### Import into your project

With your Unity project open, double-click on the downloaded `.unitypackage` file. Import everything in the package, as seen here:

![import unity package](https://user-images.githubusercontent.com/72954886/96225015-64de4c80-0fcb-11eb-89a6-4f1bc4a5485b.png)

## Add GamePubSDK prefab to your scene

After importing the package, in your **Project** panel, you'll find a **GamePubSDK** prefab under `Assets/GamePubSDK/`. Drag it to the **Hierarchy** panel of the scene to which you want to add Login:

![add prefab](https://user-images.githubusercontent.com/72954886/96225043-70317800-0fcb-11eb-9b47-0a74f3126599.png)



## Update player settings



### Publishing Settings

* ##### mainTemplate.gradle

```
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    implementation 'com.gamepub.pubsdk:pubsdk:0.2.4'

    implementation 'com.google.code.gson:gson:2.8.5'
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.11"

**DEPS**}
```

* ##### launcherTemplate.gradle

```
dependencies {
    implementation project(':unityLibrary')
    implementation 'androidx.multidex:multidex:2.0.1'//added
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
    }
    ...
```

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

## 초기화

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

## 오류코드

## 로그인환경설정

## 로그인/로그아웃/탈퇴/검증

## 계정연동

## 결제


