## Pod 라이브러리 만들기

cocoapods 에서 제공되는 템플릿 명령어다. 이미 개발된 라이브러리 프로젝트가 있다면 굳이 이걸 쓰지 않아도 배포할 수 있다.

```
$ pod lib create pod_name
```

다른 포스팅에서 많이 설명하기 때문에 생략한다.(몇가지 질문항목들이 나오고 작성하면 완료된다.)

* 사용할 플랫폼 선택 : iOS / macOS
* 사용할 언어 : Swift / ObjC (어차피 언어 둘다 사용이 가능하기 때문에 크게 중요하지 않아보임.)
* 데모 어플리케이션 포함 여부 : Yes / No
* 어떤 테스팅 프레임워크 사용 할건지 : Quick / None
* view 기반 테스팅 할 건지 여부 : Yes / No



![readme view](https://user-images.githubusercontent.com/20632507/103997286-40ff6100-51de-11eb-893a-0674c988098f.png)

라이브러리 생성시 같이 생성되는 readme 파일을 열었을때 화면이다.

build, pod, license, platform 뱃지 모두 아직 배포가 안되어서 not found 상태이다.

* build : travis 같은 ci 툴과 연동해서 url 를 넣어주면 passing 으로 바뀐다.
* pod : 이건 public 배포후 나오는 pod repo 주소를 넣어주면 버전명이 표기된다.(뱃지 url은 README.md 참고)
* license : 마찬가지 배포시 포함되는 라이센스파일 주소를 넣어주면 된다.
* platform : 배포시 제공하는 플랫폼이 표기된다.(뱃지 url은 README.md 참고)





## PodSpec 파일 생성 및 편집하기

spec 파일만 따로 만드는 방법이 있다. 사실 이 파일만 있다면 배포가 가능하다.

```
$ pod spec create pod_name
```

.podspec 이 로컬에 생성 될 것이다.

```
Pod::Spec.new do |s|
  s.name                = "git-public-pod-public"
  s.version             = "0.1.5"
  s.summary             = "The SDK for iOS Swift provides a modern way of implementing APIs."

  s.description         = <<-DESC
                          Developed in Swift, the SDK for iOS Swift provides a modern way of implementing
                          LINE APIs. The features included in this SDK will help you develop an iOS app with
                          engaging and personalized user experience.
                          DESC

  s.homepage            = "https://github.com/james-chun-dev/git-public-pod-public"
  s.license             = "Apache License, Version 2.0"

  s.author              = "James"
  s.platform            = :ios, "13.0"
  
  s.module_name         = "SDK1"
  s.source              = { :git => "https://github.com/james-chun-dev/git-public-pod-public.git", :tag => "#{s.version}" }

  s.source_files        = 'SDK1/SDK1/SDK1.h'
  
end
```

platform 과 source_files 만 잘 신경써주면 된다.

* platform: Podfile 상단에 있는 ios 버전 허용범위

* source_files: sdk내에 포함될 소스 및 리소스(번들)

  

만약 문제가 있더라도 편집된 .podspec 파일이 유효한지 체크해주는 명령어가 있다.

```
$ pod spec lint
```

했더니 이런 로그가 나왔다.

```
- ERROR | [iOS] File Patterns: The git-public-pod-public (0.1.2) spec is empty (no source files, resources, resource_bundles, preserve paths, vendored_libraries, vendored_frameworks, dependencies, nor subspecs).
- WARN  | [iOS] license: Unable to find a license file
- NOTE  | [iOS] xcodebuild:  warning: Skipping code signing because the target does not have an Info.plist file and one is not being generated automatically. (in target 'App' from project 'App')
```

에러로그는 소스파일 셋팅을 안해줘서 였고 라이센스워닝은 파일이 없었고 NOTE 이 내용은 무슨 문제인지 파악하지 못했다.

(아시는 분 계신다면 답글부탁드림.)



## Git Repo 만들고 배포하기(public, private 동일)

위에 `pod spec lint` 에 성공하려면 git repo 주소가 존재해야하고 tag가 걸려있어야 한다.

git 사용법에 대해서는 생략하겠다.

![tag](https://user-images.githubusercontent.com/20632507/104142991-25c06b80-5401-11eb-826f-a95c7cfe81ef.png)

tag 버전과 sdk 버전이 일치해야 한다.( podspec 파일내 version)



## Git(public) Pod(public) 배포하기

git repo 도 public, private 이 존재하고 pod repo 도 public, private 존재한다.

우선 git과 cocoapod 개념을 이해하고 진행하는 것이 좋다.

테스트케이스는 git repo 와 pod repo 도 public 인 상황을 진행한다.



pod trunk 에 등록하는 세션요청 명령어. 재배포시에는 안해줘도 된다.

```
$ pod trunk register chc3484@gmail.com jamesChun
```

이렇게 이메일주소와 이름을 입력후 명령어를 실행하면 이메일주소로 확인메일이 발송된다.



![email](https://user-images.githubusercontent.com/20632507/104144508-cb2a0e00-5406-11eb-9a39-c981391a74c7.png)

링크를 클릭해주면 완료되고 다음 명령어로 업로드를 진행하면 된다.

```
$ pod trunk push pod_name.podspec
```

![success](https://user-images.githubusercontent.com/20632507/104155710-181fdb80-542b-11eb-9bfe-0b54e46ef576.png)

업로드 성공하면 이런 로그가 나온다. 

#### Example Repository

* https://github.com/james-chun-dev/git-public-pod-public
* https://cocoapods.org/pods/git-public-pod-public



#### 재배포시

trunk 에 등록된 세션정보 확인후 push.

```
$ pod trunk me
$ pod trunk push
```





## Git(private) Pod(private) 배포하기

안드로이드와는 다르게 라이브러리 배포방식이 public 과 private 이 다르다.

테스트케이스는 git repo 와 pod repo 가 private인 상황을 진행한다.



### Git repo 만들기

git 주소가 2개가 필요하다.

* git lib repo (라이브러리 프로젝트)
* git specs repo (public 에서 사용한 cocoapod repo 역할을 할 git spec파일 저장소)

우선 위에 설명되있는 라이브러리 git repo 가 있다는 전제하에 설명한다. 물론 podspec 파일도 셋팅되어있어야 한다.

빈 레퍼지토리 https://github.com/james-chun-dev/specs.git 하나를 private 으로 만들었다.



그리고 pod repo 에 add 해준다.

```
$ pod repo add git-private-pod-private https://github.com/james-chun-dev/specs.git
```

![repo dir](https://user-images.githubusercontent.com/20632507/104162444-f6792100-5437-11eb-8e9d-20f152268d67.png)

정상적으로 완료되면 위와 같이 /Users/.cocoapods/repos 에 폴더가 생성된다.

(README.md 는 git repo 만들때 임의로 생성한 파일)



.podspec 파일을 위에서 설명했듯이 편집해주고 push 명령어를 실행한다.

```
$ pod repo push git-private-pod-private git-private-pod-private.podspec
```

![log](https://user-images.githubusercontent.com/20632507/104159737-3689d500-5433-11eb-9355-18d443a0e52e.png)

그랬더니 에러가 나네...음 tag 를 안걸어서 나는 에러였다. git 라이브러리 프로젝트 repo 에다 tag 를 걸어주고 다시 했더니



![push](https://user-images.githubusercontent.com/20632507/104159753-3be71f80-5433-11eb-90f6-3919c6f9be70.png)

성공!



아까 등록한 https://github.com/james-chun-dev/specs.git 에 가보니 podspec 폴더에 태그가 걸려서 올라와있다.

![specs](https://user-images.githubusercontent.com/20632507/104159774-45708780-5433-11eb-90f3-cb9efa266ca5.png)

이렇게 되면 성공한거다. 



### Pod file 사용법(private)

pod file 에서의 사용법은 약간 다른데 어느정도 예상된 방법이다.

이 내용은 찾아보면 생각보다 많이 나온다ㅎㅎ

```
pod 'git-private-pod-private', :git => 'https://github.com/james-chun-dev/git-private-pod-private'
```

이렇게 해도 되고 pod file 상단에 spec git repo 를 넣어주고

```
source 'https://github.com/james-chun-dev/specs.git'

pod 'git-private-pod-private'
```

이렇게 해도 된다. 아마도 여러 private 라이브러리용 spec git 으로 활용해도 괜찮을듯 싶다. 새로 spec 용 git 을 파지않고!!



## Git(private) Pod(public) 배포하기

사실 이 테스트케이스가 궁금했다.

git repo 는 private 이고 pod repo 는 public 으로 둬서 소스는 비공개인 대신 라이브러리는 public 하게 제공하고 싶었기 때문이다. **역시나 안드로이드 jcenter 와 마찬가지로 되지 않는다가 결론이다.**

git repo 와 podspec 을 모두 셋팅한 후 

```
$ pod spec lint
```

했더니

![warn](https://user-images.githubusercontent.com/20632507/104255887-6d0c3200-54bd-11eb-9219-5cec9b67fe50.png)

git 주소가 private 라 찾을수 없단다. jcenter 처럼 404 났겠지...

원리를 알기에 안될걸 알면서도 확인해보고 싶었다.



## Podfile 전체내용

```
source 'https://github.com/james-chun-dev/specs.git'
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '14.3'

workspace 'frameworkProj'
project 'frameworkTEST/frameworkTEST.xcodeproj'
project 'frameworkTESTAPP/frameworkTESTAPP.xcodeproj'

target 'frameworkTEST' do

	project 'frameworkTEST/frameworkTEST.xcodeproj'
	pod 'AFNetworking', '~> 3.0'
	pod 'PopupDialog', '~> 1.1'

end

target 'frameworkTESTAPP' do

	project 'frameworkTESTAPP/frameworkTESTAPP.xcodeproj'
	pod 'MBProgressHUD', '~> 1.2.0'
	pod 'git-public-pod-public', '~> 0.1'
	pod 'git-private-pod-private'
end
```

![test](https://user-images.githubusercontent.com/20632507/104257054-e0af3e80-54bf-11eb-8ffb-6c214d7448e8.png)

테스트해보았더니 문제없이 받아진다.