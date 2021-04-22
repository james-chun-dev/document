# fastlane iOS 사용법

## Setup

일단 fastlane 을 설치하는 방법은 여러가지가 있다.

- bundler
- brew
- macOS 의 system ruby

하지만 공식문서를 보면 bundler 를 가장 권장하며 시스템 ruby 는 권장하지 않는다.

### Xcode command line tools

```
xcode-select --install
```

이건 공통사항이다. 버전 호환성 이슈가 있을 수 있다.

### Homebrew 로 fastlane Install

```
brew install fastlane
```

fastlane 은 루비기반이기때문에 루비설치가 필수다. brew 로 fastlane 을 설치하게 되면 알아서 루비까지 설치해준다.

### fastlane 초기화

```
fastlane init
```

프로젝트 디렉토리에서 이 명령어를 실행해주면 fastlane 스크립트가 생성된다.

만약 스크립트 작성후 fastlane 이 실행되지 않는다면 bundler 설치를 해줘야 할 것이다.

아마도 이 부분때문에 bundler 로 환경설정 하라고 권장하는 것 같다.

```
gem install bundler
```

### fastlane 으로 터미널 명령어 실행하기

### framework 빌드 및 배포하기

### git 명령어 사용하기

