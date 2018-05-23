---
title: 문제 해결
description: 이 문서에서는 Xamarin의 tvOS 지원 팀과 작업 하는 동안 발생할 수 있는 문제를 알고 있습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 86106fa5ca53e93ccffb4dd141914c01ab65a506
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>문제 해결

_이 문서에서는 Xamarin의 tvOS 지원 팀과 작업 하는 동안 발생할 수 있는 문제를 알고 있습니다._

<a name="Known-Issues" />

## <a name="known-issues"></a>알려진 문제

현재 버전의 Xamarin tvOS 지원에 다음과 같은 알려진된 문제가 있습니다.

- **모노 프레임 워크** – 모노 4.3 Cryptography.ProtectedData 모노 4.2에서 데이터를 암호 해독에 실패 합니다. NuGet 패키지 복원 오류가 발생 하 여 결과적으로, 못합니다 `Data unprotection failed` 보호 된 NuGet 원본이 구성 된 경우.
    - **해결 방법** – Visual Studio for Mac 있는 NuGet 패키지 소스가 다시 패키지를 복원 하기 전에 사용 하 여 암호 인증을 다시 추가 해야 합니다.
- **Mac 지 원하는 F #에서 추가 기능에 대 한 visual Studio** – Windows에서 F # Android 템플릿 작성 하는 동안 오류가 발생 했습니다. 이것은 여전히 제대로 작동할 mac
- **Xamarin.Mac** –로 설정 된 대상 프레임 워크 Xamarin.Mac 통합된 템플릿 프로젝트를 실행 하는 경우 `Unsupported`, 팝업 `Could not connect to the debugger` 나타날 수 있습니다.
    - **잠재적 해결 방법** – 안정적인 채널에서 사용할 수 있는 모노 프레임 워크 버전을 다운 그레이드 합니다.
- **Xamarin Visual Studio 및 Xamarin.iOS** – Visual studio에서는 오류 WatchKit 응용 프로그램을 배포 하는 경우 `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` 나타날 수 있습니다.

찾을 수 있습니다 하나 버그 보고서 [: Bugzilla](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)합니다.

## <a name="troubleshooting"></a>문제 해결

다음 섹션에서는 Xamarin.tvOS와 이러한 문제와 솔루션 tvOS 9를 사용할 때 발생할 수 있는 몇 가지 알려진된 문제를 나열 합니다.

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>잘못 된 실행 파일-실행 파일에 없는 bitcode

Apple TV 앱 스토어에 Xamarin.tvOS 응용 제출 하려고 시도할 때, 형태로 오류 메시지가 표시 될 수 있습니다 _"잘못 된 실행 파일-실행 파일에 없는 bitcode"_ 합니다.

이 문제를 해결 하려면 다음을 수행 합니다.

1. Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭에서 Xamarin.tvOS 프로젝트 파일에는 **솔루션 탐색기** 선택 **옵션**합니다.
2. 선택 **tvOS 빌드** 을 있는지 확인 하 고는 **릴리스** 구성: 

    [![](troubleshooting-images/ts01.png "TvOS 빌드 옵션을 선택 합니다.")](troubleshooting-images/ts01.png#lightbox)
3. 추가 `--bitcode=asmonly` 에 **추가 mtouch 인수** 필드를 클릭는 **확인** 단추입니다.
4. 응용 프로그램을 다시 작성은 **릴리스** 구성 합니다.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>확인 하 여 tvOS Bitcode를 포함 하는 응용 프로그램

Bitcode Xamarin.tvOS 응용 프로그램 빌드에 포함 되어 있는지를 확인 하려면 터미널 앱을 열고 다음을 입력 합니다.

```csharp
otool -l /path/to/your/tv.app/tv
```

출력에서 다음을 확인 합니다.

```csharp
Section
  sectname __bundle
   segname __LLVM
      addr 0x0000000100001000
      size 0x000000000000124f
    offset 4096
     align 2^0 (1)
    reloff 0
    nreloc 0
     flags 0x00000000
 reserved1 0
 reserved2 0
```

`addr` 및 `size` 이 달라질 수 있지만 다른 필드와 같아야 합니다.

다음 사항을 확인 해야 합니다 정적 제 3 자 (`.a`) 라이브러리를 사용 하는 tvOS 라이브러리 (하지 iOS 라이브러리)에 대해 작성 된 상태와 이러한 아울러 bitcode 정보입니다.

응용 프로그램 또는 유효한 bitcode를 포함 하는 라이브러리에 대 한는 `size` 1 보다 커야 합니다. 라이브러리 bitcode 표시자 사용할 하면서 유효한 bitcode 포함 되어 있지 않을 수 있습니다 몇 가지 경우가 있습니다. 예를 들어:

**잘못 된 Bitcode**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**유효한 Bitcode** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

에 차이가 `size` 나열 된 예제에서 두 개의 라이브러리 간에 위에서 실행 합니다. Bitcode 사용 하도록 설정 된 라이브러리를 Xcode 보관 빌드에서 생성 해야 (Xcode 설정 `ENABLE_BITCODE`)로이 크기 문제를 해결할 수 있습니다.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Arm64 조각을 포함 하는 앱 Info.plist에서 UIRequiredDeviceCapabilities 목록에 "arm64"가지고 있어야

게시에 대 한 Apple TV 앱 스토어에 앱을 제출할 때 형식에서 오류가 발생할 수 있습니다.

_"만 arm64 분할 영역을 포함 하는 앱 있어야"arm64"UIRequiredDeviceCapabilities Info.plist에서 목록에"_

이 경우 편집 프로그램 `Info.plist` 파일을 다음 키가 있는지 확인 하십시오.

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

릴리스에 대 한 응용 프로그램을 다시 컴파일하고 iTunes Connect 다시 제출 하십시오.

### <a name="task-mtouch-execution----failed"></a>작업 "MTouch" 실행-실패

(예: MonoGame) 3rd 파티 라이브러리를 사용 하는 일련의 오류 메시지에서 끝나는 긴와 릴리스 컴파일 실패 한 경우 `Task "MTouch" execution -- FAILED`를 추가 해 보십시오 `-gcc_flags="-framework OpenAL"` 하 여 **추가 터치 인수**:

[![](troubleshooting-images/mtouch01.png "작업 MTouch 실행")](troubleshooting-images/mtouch01.png#lightbox)

도 포함 해야 `--bitcode=asmonly` 에 **추가 터치 인수**, 링커 옵션으로 설정 **링크 모든** 하 고 새로 컴파일하는를 실행 합니다.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS 90471 오류가 발생 했습니다. 큰 아이콘 누락 되었습니다.

메시지가 한 형태로 "ITMS 90471 오류가 발생 했습니다. 다음 사항을 확인 하십시오 큰 아이콘 없거나 "는 Xamarin.tvOS 앱 릴리스의 경우, 사과 TV 앱 스토어에 제출 하는 동안.

1. 큰 아이콘 자산을 포함 했는지 확인 프로그램 `Assets.car` 를 사용 하 여 만든 파일에는 [앱 아이콘](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) 설명서입니다.
2. 포함 확인는 `Assets.car` 에서 파일의 [아이콘과 이미지 작업을](~/ios/tvos/app-fundamentals/icons-images.md) 최종 응용 프로그램 번들에 대 한 설명서입니다.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>잘못 된 번들 – 게임 컨트롤러를 지원 하는 응용 프로그램 지원 해야 원격 Apple TV

또는 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>잘못 된 번들 – GameController 프레임 워크를 사용 하 여 Apple TV 앱 앱 Info.plist의에 GCSupportedGameControllers 키를 포함 해야 합니다.

게임 컨트롤러 게임을 개선 하 고 게임에 대 한 생생한의 의미를 제공 데 사용할 수 있습니다. 사용자의 원격 인스턴스 및 컨트롤러 사이 전환 하지 않아도 되므로 표준 Apple TV 인터페이스를 제어 하도 사용할 수 있습니다.

Apple TV 앱 스토어에 게임 컨트롤러 지원 되는 Xamarin.tvOS 앱을 제출 하는 형식으로 오류 메시지가 수신 된 경우.


_"응용 프로그램 이름"에 대 한 최근 프로그램 배달 문제가 하나 이상 발견 했습니다. 배송에 성공 했지만 다음 배송에서 다음과 같은 문제를 해결 해 볼 수 있습니다._

_잘못 된 번들 – 게임 컨트롤러를 지원 하는 응용 프로그램 원격 Apple TV도 지원 해야 합니다._

또는 

_앱 Info.plist의에서 잘못 된 번들 – GameController 프레임 워크를 사용 하 여 Apple TV 앱 GCSupportedGameControllers 키를 포함 해야 합니다._

Siri 원격에 대 한 지원을 추가 하는 솔루션 (`GCMicroGamepad`) 응용 프로그램에 `Info.plist` 파일입니다. 마이크로 게임 컨트롤러 프로필 Siri 원격 대상에 Apple에서 추가 되었습니다. 예를 들어 다음 키를 포함 합니다.

```xml
<key>GCSupportedGameControllers</key>  
  <array>  
    <dict>  
      <key>ProfileName</key>  
      <string>ExtendedGamepad</string>  
    </dict>  
    <dict>  
      <key>ProfileName</key>  
      <string>MicroGamepad</string>  
    </dict>  
  </array>  
<key>GCSupportsControllerUserInteraction</key>  
<true/>
```

> [!IMPORTANT]
> Bluetooth 게임 컨트롤러는 최종 사용자가 만들 수 있는 별도로 구매, 앱을 구입 사용자를 강제할 수 없습니다. 앱에서 게임 컨트롤러를 지 원하는 경우 게임 파일이 Apple TV의 모든 사용자가 사용할 수 있도록 Siri 원격도 지원 해야 합니다.

자세한 내용은 참조 하십시오 우리의 [게임 컨트롤러 작업](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) 의 섹션 우리의 [Siri 원격 인스턴스 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서입니다.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Incompatible target framework: .NetPortable, Version=v4.5, Profile=Profile78

이식 가능한 클래스 라이브러리 (PCL) Xamarin.tvOS 프로젝트에 포함 하려고 할 때 형성 하는 메시지 발생할 수 있습니다.

_Incompatible target framework: .NetPortable, Version=v4.5, Profile=Profile78_

이 문제를 해결 하려면 라는 XML 파일을 추가 ` Xamarin.TVOS.xml` 다음 내용을 사용 하 여:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

다음 경로:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

참고 경로에 프로필 번호는 PCL 프로필 수를 일치 해야 합니다.

위치에이 파일을 성공적으로 Xamarin.tvOS 프로젝트에 PCL 파일을 추가할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
