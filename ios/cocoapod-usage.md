## Cocoapod 설치

CocoaPods를 설치하는 방법은 다음과 같다. 

1. macOS 에는 Ruby 가 기본 내장되어 있다. 다음 명령어면 설치가 완료된다.

   ```
   $ sudo gem install cocoapods
   ```

2. 생성 명령어

   새로운 pod 파일을 생성해준다.

   ```
   $ pod init
   ```

3. 인스톨 명령어

   셋팅되어 있는 pod 파일이 있다면 해당경로에서 이 명령어만 쳐주면 알아서 라이브러리를 인스톨해준다.

   (만약 빌드시 의존도에 문제가 생겼다면 pod파일을 제외한 pod 폴더 및 lock 파일을 삭제하고 이 명령어를 쳐준다면 해결될 수도 있다.)

   ```
   $ pod install
   ```

4. 업데이트

   ```
   $ pod update
   ```

   

## Podfiles 파일 설정

Podfiles는 CocoaPods의 설정 파일이다. `pod init`을 통해서 파일을 생성할 수 있다.

```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '9.0'

target 'SampleSDK' do
    project 'SampleSDK/SampleSDK.xcodeproj'
    pod 'AFNetworking', '~> 3.0'
end
```

* pod: 사용하는 오픈소스 라이브러리 이름. 특정 버전, 브랜치, 경로, 저장소, 특정 커밋, 특정spec URL 등으로 원하는 시점의 소스 코드를 정의할 수 있다.
* target: 프로젝트의 특정 타겟에서만 반영되는 추가 설정. 주로 테스트 타겟의 경우에 테스트용 라이브러리 설정에 사용된다.
* xcodeproj: 프로젝트 이름과 워크스페이스 이름이 다른 경우 설정 옵션.


