---
title: Xamarin으로 빌드된 tvOS apps 문제 해결
description: 이 문서에서는 Xamarin으로 빌드된 tvOS 앱을 개발 하는 동안 문제를 해결 하는 데 도움이 되는 다양 한 팁을 제공 합니다. 알려진 문제 및 특정 오류에 대해 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 11ac6289b7d2f278f534f5a65679754d212b5067
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030526"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Xamarin으로 빌드된 tvOS apps 문제 해결

_이 문서에서는 Xamarin의 tvOS 지원으로 작업 하는 동안 발생할 수 있는 문제에 대해 설명 합니다._

<a name="Known-Issues" />

## <a name="known-issues"></a>알려진 문제점

현재 Xamarin tvOS 지원 릴리스에는 다음과 같은 알려진 문제가 있습니다.

- Mono **프레임 워크** – Mono 4.3 ProtectedData는 mono 4.2에서 데이터 암호를 해독 하지 못합니다. 따라서 NuGet 패키지는 보호 된 NuGet 소스가 구성 될 때 `Data unprotection failed` 오류로 인해 복원 되지 않습니다.
  - **해결 방법** – Mac용 Visual Studio에서 암호 인증을 사용 하는 모든 NuGet 패키지 소스를 다시 추가 해야 패키지 복원을 다시 시도할 수 있습니다.
- **Mac용 Visual Studio w/ F# 추가 기능** – Windows에서 Android 템플릿을 만들 F# 때 발생 하는 오류입니다. 이는 Mac에서 제대로 작동 해야 합니다.
- **Xamarin.ios** – 대상 프레임 워크가 `Unsupported`로 설정 된 xamarin.ios 통합 템플릿 프로젝트를 실행 하는 경우 팝업 `Could not connect to the debugger` 표시 될 수 있습니다.
  - **잠재적 해결 방법** – 안정적인 채널에서 사용할 수 있는 Mono 프레임 워크 버전을 다운 그레이드 합니다.
- Xamarin **Visual studio & xamarin.ios** – Visual Studio에서 WatchKit 응용 프로그램을 배포 하는 경우 오류 `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` 표시 될 수 있습니다.

[GitHub](https://github.com/xamarin/xamarin-macios/issues/new)에서 찾은 버그를 보고 하세요.

## <a name="troubleshooting"></a>문제 해결

다음 섹션에는 tvOS 및 해당 문제에 대 한 해결 방법으로 tvOS 9를 사용 하는 경우 발생할 수 있는 몇 가지 알려진 문제가 나와 있습니다.

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>잘못 된 실행 파일-실행 파일에 bitcode가 포함 되어 있지 않습니다.

Apple TV 앱 스토어에 tvOS 앱을 제출 하는 동안 _"잘못 된 실행 파일-실행 파일에 bitcode가 포함 되어 있지 않습니다." 라는_오류 메시지가 나타날 수 있습니다.

이 문제를 해결 하려면 다음을 수행 합니다.

1. Mac용 Visual Studio에서 **솔루션 탐색기** 의 TvOS 프로젝트 파일을 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다.
2. **TvOS 빌드** 를 선택 하 고 **릴리스** 구성을 확인 합니다. 

    [![](troubleshooting-images/ts01.png "Select tvOS Build options")](troubleshooting-images/ts01.png#lightbox)
3. **추가 mtouch 인수** 필드에 `--bitcode=asmonly`을 추가 하 고 **확인** 단추를 클릭 합니다.
4. **릴리스** 구성에서 앱을 다시 빌드합니다.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>TvOS 앱에 Bitcode가 포함 되어 있는지 확인 하는 중

TvOS 앱 빌드에 Bitcode가 포함 되어 있는지 확인 하려면 터미널 앱을 열고 다음을 입력 합니다.

```csharp
otool -l /path/to/your/tv.app/tv
```

출력에서 다음을 찾습니다.

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

`addr` 및 `size`은 다르지만 다른 필드는 동일 해야 합니다.

사용 중인 모든 타사 정적 (`.a`) 라이브러리가 tvOS 라이브러리 (iOS 라이브러리 아님)에 대해 빌드 되었으며 bitcode 정보도 포함 하 고 있는지 확인 해야 합니다.

유효한 bitcode를 포함 하는 앱 또는 라이브러리의 경우 `size`가 1 보다 큽니다. 라이브러리에 bitcode 마커가 있지만 유효한 bitcode를 포함 하지 않는 경우가 있습니다. 예를 들면,

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

나열 된 예제에서 두 라이브러리 간의 `size` 차이점은 위에서 실행 됩니다. 이 크기 문제에 대 한 해결 방법으로 bitcode enabled (Xcode setting `ENABLE_BITCODE`)를 사용 하는 Xcode archive 빌드에서 라이브러리를 생성 해야 합니다.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Arm64 조각만 포함 된 앱에는 info.plist의 UIRequiredDeviceCapabilities 목록에 "arm64"도 있어야 합니다.

게시를 위해 Apple TV 앱 스토어에 앱을 제출 하는 경우 다음과 같은 형식으로 오류가 발생할 수 있습니다.

_"Arm64 조각만 포함 된 앱은 info.plist의 UIRequiredDeviceCapabilities 목록에도" arm64 "가 있어야 합니다._

이 문제가 발생 하는 경우 `Info.plist` 파일을 편집 하 고 다음 키가 있는지 확인 합니다.

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
  <string>arm64</string>
</array>
```

릴리스를 위해 앱을 다시 컴파일하고 iTunes Connect에 다시 제출 합니다.

### <a name="task-mtouch-execution----failed"></a>작업 "MTouch" 실행--실패

타사 라이브러리 (예: MonoGame)를 사용 중이 고 `Task "MTouch" execution -- FAILED`로 끝나는 긴 일련의 오류 메시지가 포함 된 릴리스 컴파일이 실패 한 경우 **추가 touch 인수**에 `-gcc_flags="-framework OpenAL"`를 추가 해 보세요.

[![](troubleshooting-images/mtouch01.png "Task MTouch execution")](troubleshooting-images/mtouch01.png#lightbox)

**추가 터치 인수**에 `--bitcode=asmonly`을 포함 하 고, 링커 옵션을 **모두 연결** 로 설정 하 고, 완전 한 컴파일을 수행 합니다.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS-90471 오류입니다. 크게 아이콘이 없습니다.

"ITMS-90471 오류" 형식의 메시지를 가져옵니다. TvOS 앱을 릴리스에 대 한 Apple TV 앱 스토어에 제출 하는 동안 "커다란 아이콘이 없습니다." 다음을 확인 하세요.

1. [앱 아이콘](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) 설명서를 사용 하 여 만든 `Assets.car` 파일에 커다란 아이콘 자산을 포함 했는지 확인 합니다.
2. 최종 응용 프로그램 번들의 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서에서 `Assets.car` 파일을 포함 했는지 확인 합니다.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>잘못 된 번들 – 게임 컨트롤러를 지 원하는 앱은 Apple TV 원격도 지원 해야 합니다.

or 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>잘못 된 번들 – GameController framework를 사용 하는 Apple TV 앱은 앱의 Info. info.plist에 GCSupportedGameControllers 키를 포함 해야 합니다.

게임 컨트롤러를 사용 하 여 게임 플레이를 개선 하 고 게임에서 집중 교육의 의미를 제공할 수 있습니다. 또한 사용자가 원격 및 컨트롤러 간에 전환할 필요가 없도록 표준 Apple TV 인터페이스를 제어 하는 데 사용할 수 있습니다.

게임 컨트롤러 지원이 포함 된 tvOS 앱을 Apple TV 앱 스토어에 제출 하는 경우 다음과 같은 형식으로 오류 메시지를 가져옵니다.

_"앱 이름"에 대 한 최근 배달에서 하나 이상의 문제를 발견 했습니다. 배달에 성공 했지만 다음 배달 시 다음 문제를 해결할 수 있습니다._

_잘못 된 번들 – 게임 컨트롤러를 지 원하는 앱은 Apple TV 원격도 지원 해야 합니다._

or 

_잘못 된 번들 – GameController framework를 사용 하는 Apple TV 앱은 앱의 info.plist에 GCSupportedGameControllers 키를 포함 해야 합니다._

이 솔루션은 Siri 원격 (`GCMicroGamepad`)에 대 한 지원을 앱의 `Info.plist` 파일에 추가 하는 것입니다. Siri 원격을 대상으로 하는 마이크로 게임 컨트롤러 프로필이 Apple에서 추가 되었습니다. 예를 들어 다음 키를 포함 합니다.

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
> Bluetooth 게임 컨트롤러는 최종 사용자가 수행할 수 있는 선택적 구매 이며 앱이 사용자를 강제로 구매할 수 없습니다. 앱이 게임 컨트롤러를 지 원하는 경우 모든 Apple TV 사용자가 게임을 사용할 수 있도록 Siri 원격도 지원 해야 합니다.

자세한 내용은 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서의 [게임 컨트롤러 작업](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) 섹션을 참조 하세요.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>호환 되지 않는 대상 프레임 워크:. NetPortable 가능, 버전 = v 4.5, 프로필 = Profile78

TvOS 프로젝트에 PCL (이식 가능한 클래스 라이브러리)을 포함 하려고 하면 다음과 같은 메시지가 표시 될 수 있습니다.

_호환 되지 않는 대상 프레임 워크:. NetPortable 가능, 버전 = v 4.5, 프로필 = Profile78_

이 문제를 해결 하려면 다음 내용이 포함 된 `Xamarin.TVOS.xml` 이라는 XML 파일을 추가 합니다.

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

다음 경로로:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

경로의 프로필 번호는 PCL의 프로필 번호와 일치 해야 합니다.

이 파일이 준비 되 면 tvOS 프로젝트에 PCL 파일을 성공적으로 추가할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
