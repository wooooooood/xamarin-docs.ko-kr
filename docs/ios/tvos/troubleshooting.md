---
title: Xamarin에 내장 된 tvOS 앱 문제 해결
description: 이 문서에서는 Xamarin에 내장 된 tvOS 앱을 개발 하는 동안 문제를 해결 하는 데 도움이 되는 다양 한 팁을 제공 합니다. 알려진된 문제 및 특정 오류를 설명합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 45c57aa6d6308697d9bc581bf8d1691f3b29a9e5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120582"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Xamarin에 내장 된 tvOS 앱 문제 해결

_이 문서에서는 Xamarin의 tvOS 지원을 사용 하는 동안 발생할 수 있는 문제 인지 알고 있어야 합니다._

<a name="Known-Issues" />

## <a name="known-issues"></a>알려진 문제

현재 릴리스의 Xamarin의 tvOS 지원에 다음과 같은 알려진된 문제가 있습니다.

- **Mono 프레임 워크** – Mono 4.3 Cryptography.ProtectedData Mono 4.2에서 데이터를 해독 하지 못했습니다. NuGet 패키지 복원 오류와 함께 실패 결과적으로, `Data unprotection failed` 보호 NuGet 소스를 구성 된 경우.
    - **해결 방법** – 모든 NuGet 패키지는 원본 패키지를 복원 하려면 다시 시도 하기 전에 사용 하 여 암호 인증을 다시 추가 해야 하는 Mac 용 Visual Studio에서.
- **포함 하는 Mac 용 visual Studio F# add-in** – 만들 때 오류는 F# Windows에서 Android 템플릿. 이 여전히 제대로 작동할 mac에 있습니다.
- **Xamarin.Mac** –로 대상 프레임 워크를 사용 하 여 Xamarin.Mac 통합된 템플릿 프로젝트를 실행 하는 경우 `Unsupported`, 팝업 `Could not connect to the debugger` 나타날 수 있습니다.
    - **잠재적 해결 방법** – 안정 채널에서 사용할 수 있는 Mono 프레임 워크 버전을 다운 그레이드 합니다.
- **Xamarin Visual Studio 및 Xamarin.iOS** – Visual studio에서는 오류 WatchKit 응용 프로그램을 배포 하는 경우 `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` 나타날 수 있습니다.

하면 모든 버그 보고서를 확인 하세요 [Bugzilla](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)합니다.

## <a name="troubleshooting"></a>문제 해결

다음 섹션에서는 Xamarin.tvOS와 이러한 문제에 솔루션을 사용 하 여 tvOS 9를 사용 하는 경우 발생할 수 있는 몇 가지 알려진된 문제를 나열 합니다.

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>잘못 된 실행 파일-실행 파일 bitcode를 포함 하지 않습니다.

Xamarin.tvOS 앱을 Apple TV App Store에 제출 하려고 시도할 때, 오류 메시지가 형태로 발생할 _"잘못 된 실행 파일-실행 파일에 없는 bitcode"_ 합니다.

이 문제를 해결 하려면 다음을 수행 합니다.

1. Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 Xamarin.tvOS 프로젝트 파일에는 **솔루션 탐색기** 선택한 **옵션**합니다.
2. 선택 **tvOS 빌드** 되어 있는지 확인 합니다 **릴리스** 구성: 

    [![](troubleshooting-images/ts01.png "TvOS 빌드 옵션을 선택 합니다.")](troubleshooting-images/ts01.png#lightbox)
3. 추가 `--bitcode=asmonly` 에 **추가 mtouch 인수** 필드를 클릭 합니다 **확인** 단추입니다.
4. 앱을 다시 작성 합니다 **릴리스** 구성 합니다.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>확인 하 여 tvOS Bitcode를 포함 하는 앱

Bitcode Xamarin.tvOS 앱 빌드에 포함 되어 있는지를 확인 하려면 터미널 앱을 열고 다음을 입력 합니다.

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

`addr` 및 `size` 다 수는 있지만 다른 필드와 같아야 합니다.

있는지 확인 해야 합니다. 정적 제 3 자 (`.a`) tvOS 라이브러리 (iOS 라이브러리가 아닌)에 대해 빌드된 라이브러리를 사용 하는 시스템과 또한 bitcode 정보입니다.

앱에 유효한 bitcode를 포함 하는 라이브러리는 `size` 1 보다 커야 합니다. 라이브러리 bitcode 표시자 사용할 수 있지만 유효한 bitcode를 포함 하지 있는 경우도 있습니다. 예를 들어:

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

에 차이가 `size` 위에 나열 된 예제에서 두 라이브러리 간에 실행 합니다. 사용 하도록 설정 하는 bitcode를 사용 하 여 라이브러리를 Xcode 보관 빌드에서 생성 해야 합니다 (Xcode 설정을 `ENABLE_BITCODE`)이 크기 문제 솔루션으로 합니다.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Arm64 조각을 포함 하는 앱 Info.plist의 UIRequiredDeviceCapabilities 목록의 "arm64" 있어야

앱이 게시에 대 한 Apple TV App Store에 제출할 때 오류가 발생할 수 있습니다는 형식:

_"만 arm64 조각을 포함 하는 앱도 있어야"arm64"UIRequiredDeviceCapabilities Info.plist에서 목록에"_

이 경우 편집에 `Info.plist` 파일과 다음 키에 있는지 확인 합니다.

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

릴리스용 앱을 다시 컴파일하고 iTunes Connect에 다시 제출 하십시오.

### <a name="task-mtouch-execution----failed"></a>"MTouch" 실행을 실패 한 작업

(예: MonoGame) 타사 라이브러리를 사용 하는 일련의 종료 오류 메시지의 긴를 사용 하 여 프로그램 릴리스 컴파일 실패 한 경우 `Task "MTouch" execution -- FAILED`를 추가 해 보세요 `-gcc_flags="-framework OpenAL"` 에 사용자 **추가 터치 인수**:

[![](troubleshooting-images/mtouch01.png "MTouch 실행 작업")](troubleshooting-images/mtouch01.png#lightbox)

포함 해야 `--bitcode=asmonly` 에 **추가 터치 인수**, 링커 옵션 설정 **링크 모든** 새로 컴파일하는 수행 합니다.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS 90471 오류가 발생 했습니다. 큰 아이콘 누락 되었습니다.

메시지가 형태로 "ITMS 90471 오류가 발생 했습니다. 다음을 확인 하세요. 큰 아이콘 없습니다"Xamarin.tvOS 릴리스의 경우 Apple TV App Store 앱을 제출 하는 동안.

1. 큰 아이콘 자산을 포함 했는지 확인 하 `Assets.car` 사용 하 여 만든 파일을 [앱 아이콘](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) 설명서.
2. 포함 되어 있는지 확인 합니다는 `Assets.car` 에서 파일을 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 에 최종 응용 프로그램 번들에 대 한 설명서입니다.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>잘못 된 번들 – 게임 컨트롤러를 지 원하는 앱도 지원 해야 원격 Apple TV

또는 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>잘못 된 번들 – GameController 프레임 워크를 사용 하 여 Apple TV 앱에서 앱의 Info.plist GCSupportedGameControllers 키를 포함 해야 합니다.

게임 플레이 높이고 게임에서 집중 교육의 의미를 제공 게임 컨트롤러를 사용할 수 있습니다. 원격 인스턴스 및 컨트롤러 사이 전환할 사용자 포함 되지 않도록 표준 Apple TV 인터페이스를 제어 하도 사용 수 있습니다.

Xamarin.tvOS Apple TV 앱 스토어에 게임 컨트롤러 지원 앱을 제출 하는 형식으로 오류 메시지가 발생 하는 경우:


_하나 이상의 "응용 프로그램 이름"에 대 한 최근 업데이트에 문제가 발견 되었습니다. 전송에 성공 했지만 다음 전송에서 다음과 같은 문제를 해결 하려고 할 수 있습니다._

_잘못 된 번들 – 게임 컨트롤러를 지 원하는 앱 원격 Apple TV 지원 해야 합니다._

또는 

_잘못 된 번들 – GameController 프레임 워크를 사용 하 여 Apple TV 앱은 앱의 Info.plist에 GCSupportedGameControllers 키를 포함 해야 합니다._

Siri 원격에 대 한 지원을 추가 하는 솔루션 (`GCMicroGamepad`) 앱의 `Info.plist` 파일입니다. Siri 원격 대상에 Apple에서 마이크로 게임 컨트롤러 프로필 추가 되었습니다. 예를 들어, 다음 키를 포함 합니다.

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
> Bluetooth 게임 컨트롤러는 최종 사용자가 만들 수 있는 선택적 구매, 앱 사용자가 구매를 강제할 수 없습니다. 게임 컨트롤러를 지 원하는 앱도 지원 해야 Siri 원격 게임을 모든 Apple TV 사용자가 사용할 수 있도록 합니다.

자세한 내용은 참조 하십시오 우리의 [게임 컨트롤러와 작업](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) 섹션 우리의 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>호환 되지 않는 대상 프레임 워크:입니다. NetPortable, 버전 = v4.5 프로필 Profile78 =

Xamarin.tvOS 프로젝트에는 이식 가능한 클래스 라이브러리 (PCL)를 포함 하려고 할 때 얻게 메시지 구성 하기 위해:

_호환 되지 않는 대상 프레임 워크:입니다. NetPortable, 버전 = v4.5 프로필 Profile78 =_

이 문제를 해결 하기 위해 라는 XML 파일을 추가 ` Xamarin.TVOS.xml` 다음과 같은 내용으로:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

경로:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

참고 경로에 프로필 번호 PCL 프로필 수와 일치 해야 합니다.

바로이 파일을 사용 하 여 PCL 파일 Xamarin.tvOS 프로젝트를 성공적으로 추가할 수 있어야 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
