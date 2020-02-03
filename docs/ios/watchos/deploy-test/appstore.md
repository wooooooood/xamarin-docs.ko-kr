---
title: 앱 스토어에 watchOS Apps 배포
description: 이 문서에서는 Xamarin을 사용 하 여 빌드한 watchOS apps를 앱 스토어에 배포 하는 방법을 설명 합니다. 배포 프로 비전 프로필 및 iTunes Connect에 대해 살펴보고 몇 가지 문제 해결 팁도 제공 합니다.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: a622684461bfe2e4a57b910288ee1f9afb54c694
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725124"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>앱 스토어에 watchOS Apps 배포

> [!IMPORTANT]
> [Apple의 감시 키트 제출 가이드](https://developer.apple.com/app-store/watch/)를 검토 하 고, 발생할 수 있는 문제에 대 한 [문제 해결](#troubleshooting) 섹션을 참조 하세요.

- 다음이 있는지 확인 합니다.
  - 프로젝트에 대해 생성 되는 [**배포 프로 비전 프로필**](#provisioning) 입니다.
  - IOS 부모 앱에 대 한 **배포 대상** (`MinimumOSVersion`)이 **8.2** 이전 (8.3은 지원 되지 않음)로 설정 되어 있습니다.

- [**ITunes Connect**](#iTunes_Connect)에서:

  - IOS 앱 항목을 만들거나 기존 앱에 **새 버전** 을 추가 합니다.
  - 조사식 아이콘 및 스크린 샷을 추가 합니다.

- 그런 다음 [Mac용 Visual Studio](#xamarin_studio) (Visual Studio는 현재 지원 되지 않음):

  - IOS 앱을 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다.
  - **앱 스토어** 구성으로 변경 합니다.
  - **보관** 기능을 사용 하 여 응용 프로그램 보관 파일을 만듭니다.

- 마지막으로 [Xcode 6.2 +](#xcode) 로 전환 합니다.

  - **창 > 구성 도우미** 로 이동 하 여 **보관**을 선택 합니다.
  - 목록에서 응용 프로그램 및 보관을 선택 합니다.
  - 생략할 **유효성 검사** ... 보관 파일입니다.
  - **제출** ... 검토 및 승인을 위해 iTunes Connect에 업로드 하는 단계를 수행 합니다.

아래 항목과 관련 된 특정 팁을 읽으십시오. 문제가 있는 경우 [문제 해결](#troubleshooting) 섹션을 참조 하세요.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>배포 프로 비전 프로필

앱 스토어 배포를 빌드하려면 솔루션의 각 앱 ID에 대 한 **배포 프로 비전 프로필** 을 만들어야 합니다.

와일드 카드 앱 ID가 있는 경우 *프로 비전 프로필은 하나만 필요 합니다*. 하지만 각 프로젝트에 대해 별도의 앱 ID가 있는 경우 각 앱 ID에 대 한 프로 비전 프로필이 필요 합니다.

![](appstore-images/provisioningprofile-distribution-sml.png "The App Store Distribution profile")

세 프로필을 모두 만들면 목록에 표시 됩니다. 각 항목을 두 번 클릭 하 여 다운로드 하 고 설치 해야 합니다.

![](appstore-images/provisioningprofiles-sml.png "The list of available profiles")

**빌드 > IOS 번들 서명** 화면을 선택 하 고 **Appstore | iPhone** 구성을 선택 하 여 **프로젝트 옵션** 에서 프로 비전 프로필을 확인할 수 있습니다.

**프로 비전 프로필** 목록에 일치 하는 모든 프로필이 표시 됩니다 .이 드롭다운 목록에서 사용자가 만든 일치 하는 프로필을 확인 해야 합니다.

![](appstore-images/options-selectprofile-sml.png "The iOS Bundle Signing dialog")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

특히 다음과 같이 [앱 배포 개요](~/ios/deploy-test/app-distribution/index.md)를 따르세요.

- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

ITunes Connect에서 앱을 구성 하는 경우 보기 아이콘과 스크린샷 추가를 잊지 마세요.

![](appstore-images/itunesconnect-watch-sml.png "The Watch icon and screenshots in iTunes Connect")

아이콘 파일은 1024x1024 픽셀 이어야 하며 표시 될 때이에 대 한 원형 마스크가 적용 됩니다. 아이콘에 알파 채널이 없어야 합니다.

스크린샷을 하나 이상 필요 합니다. 최대 5 개까지 제출할 수 있습니다.
312x390 픽셀 이어야 하 고 작동 중인 시청 앱을 보여 줍니다.
이 크기에서 스크린샷을 사용할 수 있습니다.

<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

1. IOS 앱이 시작 프로젝트 인지 확인 합니다. 그렇지 않으면 마우스 오른쪽 단추를 클릭 하 여 설정 합니다.

   ![](appstore-images/xs-startup.png "Setting the startup project")

2. **Appstore** 빌드 구성을 선택 합니다.

   ![](appstore-images/xs-appstore.png "The AppStore build configuration")

3. **빌드 > 보관** 메뉴 항목을 선택 하 여 보관 프로세스를 시작 합니다.

   ![](appstore-images/xs-archive.png "The Build menu")

**> 보관 보기 ...** 메뉴 항목을 선택 하 여 이전에 만든 보관 파일을 볼 수도 있습니다.

  ![](appstore-images/xs-archives-sml.png "The Archives view")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode는 Mac용 Visual Studio에서 만든 보관 파일을 자동으로 표시 합니다.

1. Xcode을 시작 하 고 **창 > 구성 도우미**를 선택 합니다.

   ![](appstore-images/xc-organizer.png "The Window menu")

2. **보관** 탭으로 전환 하 고 Mac용 Visual Studio를 사용 하 여 만든 보관 파일을 선택 합니다.

   ![](appstore-images/xc-archives.png "The Archives tab")

3. 필요에 따라 파일의 **유효성을 검사** 하 고, **제출 ...** 을 선택 하 여 앱을 iTunes Connect에 업로드 합니다.

4. 개발 팀 (둘 이상에 속하는 경우)을 선택 하 고 제출을 확인 합니다.

   ![](appstore-images/xc-submit1.png "The development team section")

5. ITunes Connect를 다시 방문 하 여 업로드 된 이진 파일을 확인 합니다. 앱의 구성 페이지로 이동 하 고 맨 위 메뉴에서 **시험판** 을 선택 하 여 **빌드** 목록을 표시 합니다.

   [![](appstore-images/itc-prerelease-sml.png "The apps configuration page in iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

그런 다음 **버전** 페이지에서 승인을 위해 앱을 제출할 수 있습니다. 자세한 내용은 [iOS 앱 배포 개요](~/ios/deploy-test/app-distribution/index.md) 를 참조 하세요.

## <a name="troubleshooting"></a>문제 해결

앱 스토어에 제출 하는 동안 발생할 수 있는 몇 가지 오류 및 문제를 해결 하기 위해 수행할 수 있는 단계는 다음과 같습니다.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>보관 메뉴 옵션은 Mac용 Visual Studio 표시 되지 않습니다.

[위의 단계](#xamarin_studio) 를 수행 하 여 보관을 위한 솔루션을 구성 합니다. 시작 프로젝트를 올바르게 설정할 수 없으면 시작 프로젝트를 변경 하기 전에 빌드 구성이 먼저 디버그 또는 릴리스로 설정 되었는지 확인 합니다. 그런 다음 빌드 구성을 **Appstore**로 다시 설정 합니다.

### <a name="invalid-icon"></a>잘못됨 아이콘

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

아이콘에서 [알파 채널을 제거 하는 방법에 대 한 지침](~/ios/watchos/troubleshooting.md) 을 따르세요.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion 불일치

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

솔루션의 모든 프로젝트 (iOS 앱, 조사식 확장 및 조사식 앱)는 동일한 버전 번호를 사용 해야 합니다. 버전 번호가 정확 하 게 일치 하도록 각 **info.plist** 파일을 편집 합니다.

### <a name="missing-icons"></a>누락 된 아이콘

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

[아이콘](~/ios/watchos/app-fundamentals/icons.md) 사용의 지침에 따라 모든 필수 이미지를 Watch 앱 프로젝트에 추가 합니다.

### <a name="missing-icon"></a>누락 된 아이콘

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

최신 버전의 Mac용 Visual Studio 있는지 확인 하 고 **appicons.appiconset** 에 전체 이미지 집합이 포함 되어 있는지 확인 합니다. 이 오류가 계속 표시 되 면 콘텐츠의 원본을 확인 하 여 필요한 모든 이미지에 대 한 항목이 포함 되어 있는지 확인 **합니다.** 또는 최신 버전의 Xamarin을 사용 하 고 있는지 확인 한 후 **appicons.appiconset**을 삭제 하 고 다시 만듭니다.

> [!IMPORTANT]
> Mac용 Visual Studio의 조사식 아이콘 지원에는 알려진 버그가 있습니다. **29x29@3x** 이미지에는 88x88 픽셀 이미지 (픽셀 87x87)가 필요 합니다.

Mac용 Visual Studio에서이 문제를 해결할 수 없습니다. Xcode에서 이미지 자산을 편집 하거나 **콘텐츠. json** 파일을 수동으로 편집 합니다.

### <a name="invalid-watchkit-support"></a>잘못 된 WatchKit 지원

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

이 메시지는 유효성 검사 및 전송 중에 표시 되거나 성공적으로 업로드 된 후 iTunes Connect의 자동화 된 전자 메일에 표시 될 수 있습니다.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> 앱을 Mac용 Visual Studio에 **보관** 한 다음 Xcode 6.2 +로 전환 하 여 유효성을 검사 하 고 iTunes Connect에 업로드 해야 합니다.

안정적인 Xamarin 채널 및 Xcode 6.2 +를 사용 합니다.

### <a name="invalid-provisioning-profile"></a>잘못 된 프로 비전 프로필

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

모든 3 개의 프로젝트에 대 한 **배포 프로 비전 프로필** 은 iOS 앱, 시청 확장 및 조사식 앱 (명시적 (3 개 프로필) 또는 단일 와일드 카드 프로필을 통해)에 제공 되어야 합니다. 프로 비전 프로필이 iOS 개발자 센터에 존재 하 고 다운로드 하 여 Mac에 추가 했는지 확인 합니다.

### <a name="invalid-code-signing-entitlements"></a>잘못 된 코드 서명 자격

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Apple 개발자 센터에서 프로 비전 프로필을 올바르게 설정 하 고 다운로드 하 여 설치 했는지 확인 합니다. 또한 각 프로젝트에 대 한 Mac용 Visual Studio의 속성 창에서 설정 되었는지 확인 합니다.

### <a name="invalid-architecture"></a>잘못 된 아키텍처

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Watch 앱 [Unified API (64 비트)](~/cross-platform/macios/unified/index.md) xamarin.ios 앱만 추가할 수 있습니다.
IOS 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션 > 빌드 > Ios 빌드 > 고급 탭** 으로 이동한 다음 Appstore iPhone 구성에 대해 **지원 되는 아키텍처** 에 **ARM64** (예: **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>이 번들은 잘못 되었습니다.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

부모 iOS 응용 프로그램에는 ' 8.2 '이 하 여야 합니다.

### <a name="non-public-api-usage"></a>공용이 아닌 API 사용

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

최신 버전의 Xcode 및 Xamarin 도구를 사용 하 고 있는지 확인 합니다.
코드에서 public이 아닌 Api에 액세스 해서는 안 됩니다.

### <a name="build-error-mt5309"></a>빌드 오류 MT5309

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

이 오류는 **Xcode**에서 Xcode 설치의 이름을 변경한 결과일 수 있습니다. 예를 들어 설치의 이름을 **XCode 6.2. app**으로 바꾸면이 오류가 발생 합니다.

## <a name="related-links"></a>관련 링크

- [Apple WatchKit 제출 가이드](https://developer.apple.com/app-store/watch/)
