GamePub SDK for Unity 사용 환경 및 초기 설정에 대해 설명합니다.

## Unity 지원 버전

* ##### 2019.4.x lts

* ##### 현재 하위버전은 지원하지 않습니다.



## Getting the SDK



### Download Page

To get the latest GamePub SDK for Unity, download the `.unitypackage` file from our [Resources/Downloads](https://docs.igamepub.co.kr/android-sdk-guide/resources/downloads).



### Import into your project

With your Unity project open, double-click on the downloaded `.unitypackage` file. Import everything in the package, as seen here:

![import unity package](https://user-images.githubusercontent.com/72954886/96225015-64de4c80-0fcb-11eb-89a6-4f1bc4a5485b.png)

## Add GamePubSDK prefab to your scene

After importing the package, in your **Project** panel, you'll find a **GamePubSDK** prefab under `Assets/GamePubSDK/`. Drag it to the **Hierarchy** panel of the scene to which you want to add GamePub Login:

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



