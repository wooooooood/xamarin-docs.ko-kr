---
title: mtouch를 사용하여 Xamarin.iOS 앱을 번들로 묶기
description: 이 문서에서는 Xamarin.iOS 응용 프로그램을 번들로 묶고, 시뮬레이터에서 실행하고, 물리적 디바이스에 배포하는 데 필요한 많은 단계를 제공하는 도구인 mtouch를 설명합니다.
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: 870a9cb20ea962b3c1a342e7222c5e9322537dd1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109213"
---
# <a name="using-mtouch-to-bundle-xamarinios-apps"></a>mtouch를 사용하여 Xamarin.iOS 앱을 번들로 묶기

iPhone 응용 프로그램은 응용 프로그램 번들로 제공됩니다. 응용 프로그램 번들은 iPhone이 응용 프로그램에 대해 배우기 위해 사용하는 코드, 데이터, 구성 파일 및 매니페스트를 포함하는 `.app` 확장이 포함된 디렉터리 입니다.

.NET 실행 파일을 응용 프로그램으로 바꾸는 프로세스는 대부분 응용 프로그램을 번들로 바꾸는 데 필요한 여러 단계를 통합하는 도구인 `mtouch` 명령을 통해 이루어집니다. 이 도구는 시뮬레이터에서 응용 프로그램을 시작하고 실제 iPhone 또는 iPod Touch 디바이스에 소프트웨어를 배포할 때도 사용됩니다.

## <a name="detailed-instructions"></a>자세한 지침

mtouch 도구의 가능한 모든 사용법이 포함된 [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) 설명서 페이지를 확인하세요.

## <a name="installation"></a>설치

Mac에서 `mtouch`는 Xamarin.iOS와 함께 번들로 제공됩니다. 다음 디렉터리에서 찾을 수 있습니다.

**/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**

`mtouch`를 사용하기 편리하도록 만들려면 시스템의 `PATH` 환경 변수에 부모 디렉터리를 추가합니다.  

예를 들어, Bash에서 이를 수행하려면 **~/.bash_profile** 파일의 끝에 다음 줄을 추가합니다.

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> `mtouch`를 사용하려면 **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**을 가리키는 바로 가기 링크인 **/Developer/MonoTouch/usr/bin**을 사용하지 마세요. 이 바로 가기 링크는 **/Library/Frameworks/...** 에 설치되지 않은 이전 MonoTouch 릴리스와 호환성을 유지하기 위해서만 존재하며, 향후 릴리스에서는 사라집니다.

## <a name="building"></a>빌드

`mtouch` 명령은 세 가지 방법으로 코드를 컴파일할 수 있습니다.

-  시뮬레이터 테스트용 컴파일.
-  디바이스 배포용으로 컴파일.
-  실행 파일을 디바이스에 배포.


### <a name="building-for-the-simulator"></a>시뮬레이터용으로 빌드

시작할 때 가장 일반적으로 사용되는 시나리오는 시뮬레이터에서 응용 프로그램을 사용해 보는 것이며, 따라서 여러분은 `mtouch -sim` 명령을 사용하여 코드를 시뮬레이터 패키지로 컴파일하게 될 것입니다. 이 작업은 다음과 같이 수행합니다.

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>디바이스용으로 빌드

디바이스용 소프트웨어를 빌드하려면 `mtouch -dev` 옵션을 사용하여 응용 프로그램을 빌드해야 하며, 또한 응용 프로그램을 서명하는 데 사용되는 인증서의 이름을 제공해야 합니다. 다음은 디바이스용 응용 프로그램을 빌드하는 방법입니다.

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

이 예에서는 "iPhone Developer: Miguel de Icaza" 인증서를 사용하여 응용 프로그램을 서명합니다. 이 단계는 매우 중요하며, 이 단계를 거치지 않으면 물리적 디바이스에서 응용 프로그램 로드를 거부합니다.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>응용 프로그램 실행


### <a name="launching-on-the-simulator"></a>시뮬레이터에서 시작

응용 프로그램 번들이 있으면 시뮬레이터에서 시작하는 방법은 매우 간단합니다.

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

`--sdkroot` 플래그를 설정하지 않으면 xcode-select 경로가 기본값으로 사용되어 다음과 같은 경고가 표시됩니다.

> 예: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer 

위의 명령줄은 다음과 같은 출력을 생성합니다.

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



또한 디버깅에 도움이 되도록 표준 출력 및 표준 오류 파일의 로그를 보관할 것을 강력하게 권장합니다. `Console.WriteLine`의 출력은 `stdout`으로 이동하고, `Console.Error.WriteLine`의 출력 및 기타 런타임 오류 메시지는 `stderr`로 이동합니다.

이렇게 하려면 `--stdout` 및 `--stderr` 플래그를 사용합니다.

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

응용 프로그램이 실패할 경우 출력 및 오류를 살펴보고 문제를 진단할 수 있습니다.


### <a name="deploying-to-a-device"></a>디바이스에 배포

디바이스에 배포하려면 Apple의 [관리 디바이스](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) 문서에 설명된 대로 디바이스를 프로비전해야 합니다. 디바이스가 올바르게 프로비전되면 mtouch 명령을 사용하여 컴파일된 ".app"을 디바이스에 배포할 수 있습니다. 이 작업은 다음 명령을 사용하여 수행합니다.

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

`--sdkroot` 플래그를 설정하지 않으면 xcode-select 경로가 기본값으로 사용되어 다음과 같은 경고가 표시됩니다.

> 예: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer 

다음 단계는 일반적으로 Mac용 Visual Studio에서 수행됩니다.

## <a name="reference"></a>참조

다른 명령줄 옵션에 대한 자세한 내용은 [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) 설명서 페이지를 참조하세요.



## <a name="related-links"></a>관련 링크

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
