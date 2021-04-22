## golang 설치

[공식사이트](https://golang.org/doc/install) 를 통해 설치합니다.

## 환경변수

윈도우든 맥이든 환경변수를 설정해야 합니다.

* GOROOT : /usr/local/go
* GOPATH : workspace 에 해당하므로 임의로 정하면 됩니다.
* GOBIN : $GOPATH/bin

특히 GOROOT, GOPATH 가 제대로 설정되있어야 mod 명령어가 잘 작동됩니다.

환경변수 설정은 각자 알아서 본인취향대로 하면 됩니다.

## mod 사용법

* go mod init

  vendor.json 파일을 읽고 mod 파일을 생성하여 내부 프로젝트에서 사용되고 있는 의존성 소스에 대해 내려받고 관리해준다.

* go mod vendor

  사용해보지 않음

* go mod tidy

* go mod verify

## 참고

* [Go Modules 살펴보기](https://velog.io/@kimmachinegun/Go-Go-Modules-살펴보기-7cjn4soifk#go111module)

