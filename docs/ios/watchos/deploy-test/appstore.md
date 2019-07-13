---
title: WatchOS 앱을 앱 스토어에 배포
description: 이 문서에서는 앱 스토어에 Xamarin에 내장 된 watchOS 앱을 배포 하는 방법을 설명 합니다. 배포 프로 비전 프로필 및 iTunes Connect 사용 하 고 또한 몇 가지 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 58e3593dc09c76439a3e128e51f354c169d7e72e
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865970"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>WatchOS 앱을 앱 스토어에 배포

> [!IMPORTANT]
> 검토 해야 [Apple Watch 키트 제출 가이드](https://developer.apple.com/app-store/watch/), 참조 및 합니다 [문제 해결](#troubleshooting) 해야 하는 모든 문제에 대 한 섹션입니다.

- 했는지 확인 합니다.
  - [**배포 프로 비전 프로필** ](#provisioning) 프로젝트에 대해 생성 합니다.
  - 합니다 **배포 대상을** (`MinimumOSVersion`) iOS 앱 집합에 부모에 대 한 **8.2** 또는 이전 버전 (8.3 지원 되지 않습니다.).

- [ **iTunes Connect**](#iTunes_Connect):

  - IOS 앱 항목을 만듭니다 (또는 추가 **새 버전** 기존 앱에).
  - 조사식 아이콘 및 스크린 샷을 추가 합니다.

- 그런 다음 [Mac 용 Visual Studio](#xamarin_studio) (Visual Studio 현재 지원 되지 않습니다).

  - 선택한 iOS 앱을 마우스 오른쪽 단추로 클릭 **시작 프로젝트로 설정**합니다.
  - 변경 된 **앱 스토어** 구성 합니다.
  - 사용 합니다 **보관** 기능 응용 프로그램 보관 파일을 만듭니다.

- 마지막으로 전환할 [Xcode 6.2 +](#xcode)

  - 로 이동 합니다 **창 > 구성 도우미** 선택한 **보관**합니다.
  - 목록에서 응용 프로그램 및 보관을 선택 합니다.
  - (선택 사항) **의 유효성을 검사 하는 중...**  보관 합니다.
  - **전송...**  보관 및 iTunes를 업로드 하는 단계 검토 및 승인에 대 한 연결에 따라 합니다.

이러한 항목 아래에 관련 된 특정 팁을 읽어 보세요. 참조 된 [문제 해결](#troubleshooting) 문제가 있는 경우 섹션입니다.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>배포 프로 비전 프로필

만들어야 하는 앱 스토어 배포를 위한 구축 하는 **배포 프로 비전 프로필** 솔루션의 각 앱 ID에 대 한 합니다.

와일드 카드 앱 ID를 있다면 *하나만 프로 비전 프로필 해야*; 각 프로젝트에 대 한 별도 앱 ID가 있는 경우 각 앱 ID에 대 한 프로 비전 프로필을 해야 하지만:

![](appstore-images/provisioningprofile-distribution-sml.png "앱 스토어 배포 프로필")

세 개의 프로필 모두 만든 경우 목록에 나타납니다. 다운로드 하 고 (에 두 번 클릭)에서 각각 설치 해야 합니다.

![](appstore-images/provisioningprofiles-sml.png "사용할 수 있는 프로필 목록")

프로 비전 프로필을 확인할 수 있습니다는 **프로젝트 옵션** 를 선택 하 여 합니다 **빌드 > iOS 번들 서명** 화면을 선택 하는 **AppStore | iPhone** 구성입니다.

합니다 **프로 비전 프로필** 목록 일치 하는 모든 프로필에 표시 됩니다-사용자가 만든이 드롭다운 목록에서 일치 하는 프로필을 표시 합니다.

![](appstore-images/options-selectprofile-sml.png "IOS 번들 서명 대화 상자")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

수행 합니다 [앱 배포 개요](~/ios/deploy-test/app-distribution/index.md), 특히:

- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

ITunes Connect에서에서 앱을 구성할 때 조사식 아이콘 및 스크린샷을 추가할 것을 잊지 마세요.

![](appstore-images/itunesconnect-watch-sml.png "조사식 아이콘 및 iTunes Connect에서 스크린샷")

아이콘 파일 1024x1024 픽셀 이어야 하 고 표시 되 면 적용 하는 순환 마스크 해야 합니다. 아이콘에 알파 채널이 없어야 합니다.

스크린샷을 하나 이상 필요 하며 최대 5까지 제출할 수 있습니다.
312 x 390 픽셀 이어야 하며 실행 중인 Watch 앱을 보여 줍니다.
이 크기에 대 한 스크린샷을를 42 mm 조사식 시뮬레이터를 사용할 수 있습니다.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

1. IOS 앱 시작 프로젝트 인지 확인 합니다. 그렇지 않은 경우에 설정 하려면 마우스 오른쪽 단추로 클릭 합니다.

   ![](appstore-images/xs-startup.png "시작 프로젝트 설정")

2. 선택 된 **AppStore** 빌드 구성:

   ![](appstore-images/xs-appstore.png "앱 스토어 빌드 구성")

3. 선택 된 **빌드 > 보관** 보관 프로세스를 시작 하려면 메뉴 항목:

   ![](appstore-images/xs-archive.png "빌드 메뉴")

선택할 수도 있습니다는 **보기 > 보관 하는 중...**  메뉴 항목을 이전에 생성 된 보관 파일을 참조 하세요.

  ![](appstore-images/xs-archives-sml.png "보관 보기")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode에서 mac 용 Visual Studio에서 만든 보관을 자동으로 표시 됩니다.

1. Xcode를 시작 하 고 선택 **창 > 도우미**:

   ![](appstore-images/xc-organizer.png "창 메뉴")

2. 으로 전환 합니다 **보관** 탭 및 Mac 용 Visual Studio를 사용 하 여 만든 보관 선택:

   ![](appstore-images/xc-archives.png "보관 파일 탭")

3. 필요에 따라 **의 유효성을 검사 하는 중...**  보관 파일을 선택한 **제출 하는 중...**  iTunes Connect에 앱을 업로드 합니다.

4. (사용자가 속한 둘 이상의) 하는 경우 개발 팀을 선택 하 고 제출 확인.

   ![](appstore-images/xc-submit1.png "개발 팀 섹션")

5. ITunes Connect 업로드 된 바이너리를 다시 방문 하세요. 앱의 구성 페이지로 이동 및 선택 **시험판** 확인 하려면 위쪽 메뉴에서를 **빌드** 목록:

   [![](appstore-images/itc-prerelease-sml.png "ITunes Connect에서에서 앱 구성 페이지")](appstore-images/itc-prerelease.png#lightbox)

승인에 대 한 앱에 제출할 수 있습니다 합니다 **버전** 페이지입니다. 참조 된 [iOS 앱 배포 개요](~/ios/deploy-test/app-distribution/index.md) 자세한 내용은 합니다.


## <a name="troubleshooting"></a>문제 해결

앱 스토어 및 해결을 위해 수행할 수 있는 단계에 제출 하는 동안 발생할 수 있는 일부 오류는 다음과 같습니다.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>보관 파일 메뉴 옵션 Mac 용 Visual Studio에 표시 됩니다.

수행 합니다 [위의 단계](#xamarin_studio) 보관 솔루션을 구성 하 합니다. 시작 프로젝트를 올바르게 설정할 수 없습니다, 하는 경우 빌드 구성을 디버그 또는 릴리스 시작 프로젝트를 변경 하기 전에 먼저 설정할지 확인 합니다. 빌드 구성을 다시 설정 합니다 **AppStore**합니다.

### <a name="invalid-icon"></a>잘못됨 아이콘

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

수행 합니다 [알파 채널을 제거 하는 것에 대 한 지침](~/ios/watchos/troubleshooting.md) 아이콘에서.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion Mismatch

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

-IOS 앱, 조사식 확장 및 Watch 앱-솔루션의 모든 프로젝트는 동일한 버전 번호를 사용 해야 합니다. 각 편집 **Info.plist** 파일에 버전 번호를 정확 하 게 일치 합니다.

### <a name="missing-icons"></a>누락 된 아이콘

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

지침에 따라를 [아이콘을 사용 하 여 작업](~/ios/watchos/app-fundamentals/icons.md) Watch 앱 프로젝트에 필요한 모든 이미지를 추가 합니다.

### <a name="missing-icon"></a>누락 된 아이콘

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Mac에 최신 버전의 Visual Studio를 사용 했는지 확인 하 **AppIcon.appiconset** 이미지의 전체 집합을 포함 합니다. 이 오류가 계속 나타나면 원본을 보려면 합니다 **Contents.json** 에 필요한 모든 이미지에 대 한 항목이 포함 되어 있는지 확인 합니다. 또는 최신 버전의 Xamarin 사용 하는 확인 되 면 삭제 하 고 다시 만들어야 합니다 **AppIcon.appiconset**합니다.

> [!IMPORTANT]
> Mac의 조사식 아이콘 지원에 대 한 Visual Studio에서 알려진된 버그가 있습니다: 88 x 88 픽셀 이미지에 대 한 것으로 예상 합니다 **29x29@3x** 이미지 (87 x 87 픽셀 이어야 합니다).


Xcode의 이미지 자산을 편집 하거나-Mac 용 Visual Studio에서이 해결 하거나 수동으로 편집할 수 없습니다는 **Contents.json** 파일 (맞게 [이 샘플](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>잘못 된 WatchKit 지원

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

이 메시지가 나타날 수 있습니다 또는 자동화 된 전자 메일 유효성 검사 및 제출 하는 동안 iTunes Connect에서에서 명백 하 게 성공적으로 업로드 한 후입니다.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> 수행 해야 합니다 **보관** 앱 용 Visual Studio Mac 및 Xcode 6.2 + 다음 전환의 유효성을 검사 하 여 iTunes Connect에 업로드 합니다.


안정적인 Xamarin 채널과 Xcode 6.2 +를 사용 합니다.



### <a name="invalid-provisioning-profile"></a>잘못 된 프로 비전 프로필

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**배포 프로 비전 프로필** watch 앱 솔루션에 세 프로젝트를 모두 제공 해야 합니다: iOS 앱, 조사식 확장 및 Watch 앱-하거나 명시적으로 (세 개의 프로필) 또는 단일 와일드 카드 프로필을 통해. IOS 개발자 센터에서에서 프로 비전 프로필 및 다운로드 했으며 mac에 추가 확인

### <a name="invalid-code-signing-entitlements"></a>잘못 된 코드 서명 권한 부여

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

프로 비전 프로필은 Apple 개발자 센터에서 제대로 설정 및는 다운로드 하 고 설치를 확인 합니다. 또한 각 프로젝트에 대 한 Mac의 속성 창에 대 한 Visual Studio에서 설정 된 확인 합니다.

### <a name="invalid-architecture"></a>잘못 된 아키텍처

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Watch 앱만 추가할 수 있습니다 [Unified API (64 비트)](~/cross-platform/macios/unified/index.md) Xamarin.iOS 앱.
IOS 앱 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음로 이동 **옵션 > 빌드 > iOS 빌드 > 고급 탭** 있는지 확인 합니다 **지원 되는 아키텍처** AppStore iPhone 용 구성 포함 **ARM64** (예: **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>이 번들에 유효 하지 않습니다.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

상위 iOS 응용 프로그램에 '8.2' 이하로 설정 MinimumOSVersion 있어야 합니다.

### <a name="non-public-api-usage"></a>Public이 아닌 API 사용

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

최신 버전의 Xcode 및 Xamarin 도구를 사용 하는 것을 확인 합니다.
코드에서 public이 아닌 Api를 액세스 하지 해야 합니다.

### <a name="build-error-mt5309"></a>오류 MT5309 빌드

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

이 오류에 있는 이름이에서 Xcode 설치의 결과인 가능성이 **Xcode.app**합니다. 설치를 바꾸면이 오류는 발생 하는 예를 들어 **XCode 6.2.app**합니다.



## <a name="related-links"></a>관련 링크

- [Apple WatchKit 제출 가이드](https://developer.apple.com/app-store/watch/)
