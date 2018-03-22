---
title: "앱 스토어에 배포"
description: "앱 스토어에 Watch 앱 배포"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c5b89570fdd3df80d39c6621fcd12a23babed9ee
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="deploying-to-the-app-store"></a>앱 스토어에 배포

> [!IMPORTANT]
> 검토 해야 [Apple Watch 키트 제출 가이드](https://developer.apple.com/app-store/watch/), 참조 및는 [문제 해결](#Troubleshooting) 섹션의 모든 문제를 할 수 있습니다.

- 확인 해야 합니다.
  - [**배포 프로 비전 프로필** ](#provisioning) 프로젝트에 대해 생성 됩니다.
  - **배포 대상** (`MinimumOSVersion`) iOS 부모로 응용 프로그램 설정에 대 한 **8.2** 또는 이전 버전 (8.3 지원 되지 않음).

- [ **iTunes Connect**](#iTunes_Connect):

  - IOS 응용 프로그램 항목을 만듭니다 (또는 추가 **새 버전** 를 기존 응용 프로그램).
  - 조사식 아이콘와 스크린 샷을 추가 합니다.

- 그런 다음 [Mac 용 Visual Studio](#xamarin_studio) (Visual Studio는 현재 지원 되지 않습니다).

  - IOS 응용 프로그램을 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**합니다.
  - 변경 된 **앱 스토어** 구성 합니다.
  - 사용 하 여는 **보관** 기능 응용 프로그램 보관 파일을 만듭니다.

- 마지막으로 전환할 [Xcode 6.2 +](#xcode)

  - 이동 하는 **창 > 도우미** 선택 **보관**합니다.
  - 목록에서 응용 프로그램 및 보관 파일을 선택 합니다.
  - (선택 사항) **유효성 검사 중...**  보관 합니다.
  - **전송...**  보관 및 검토 및 승인에 대 한 연결 iTunes에 업로드 하는 단계에 따라 합니다.

이 항목 아래에 관련 된 특정 팁 읽어 보세요. 참조는 [문제 해결](#Troubleshooting) 문제가 있는 경우 섹션.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>프로 비전 프로필 배포

만들어야 하는 스토어 앱 배포에 대 한 빌드는 **배포 프로 비전 프로필** 각 응용 프로그램 ID 솔루션에 대 한 합니다.

응용 프로그램 ID, 와일드 카드 있다면 *프로 비전 프로필은 하나만 있으면 됩니다*; 하지만 각 프로젝트에 대 한 별도 응용 프로그램 ID가 있는 경우 각 응용 프로그램 ID에 대 한 프로 비전 프로필을 해야 합니다.

![](appstore-images/provisioningprofile-distribution-sml.png "앱 스토어 분포 프로필")

모든 세 개의 프로필을 만든 후 목록에 나타납니다. 다운로드 하 고 (에 두 번 클릭) 하 여 각 하나를 설치 해야 합니다.

![](appstore-images/provisioningprofiles-sml.png "사용할 수 있는 프로필 목록")

프로 비전 프로필을 확인할 수 있습니다는 **프로젝트 옵션** 선택 하 여는 **빌드 > iOS 번들 서명** 화면을 선택 하 고 **AppStore | iPhone** 구성입니다.

**프로 비전 프로필** 모든 일치 하는 프로필 목록에 표시 됩니다-직접 만든이 드롭 다운 목록에서 일치 하는 프로필 표시 되어야 합니다.

![](appstore-images/options-selectprofile-sml.png "IOS 번들 서명 대화 상자")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

에 따라는 [응용 프로그램 배포 개요](~/ios/deploy-test/app-distribution/index.md), 특히:

- [ITunes Connect에서에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

조사식 아이콘 및 스크린 샷을 추가 하려면 iTunes Connect에서에서 앱을 구성할 때 반드시:

![](appstore-images/itunesconnect-watch-sml.png "조사식 아이콘 및 iTunes Connect에서에서 스크린 샷")

아이콘 파일 1024 x 1024 픽셀 이어야 하 고 표시 될 때 적용 된 원형 마스크 해야 합니다. 알파 채널 아이콘 가질 수 없습니다.

적어도 하나의 스크린 샷이 필요, 최대 5 개의 제출할 수 있습니다.
312 x 390 픽셀 수를 Watch 앱 실행에서을 보여 줍니다.
이 크기에 대 한 스크린샷을 42 mm 조사식 시뮬레이터를 사용할 수 있습니다.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. IOS 앱 시작 프로젝트 인지 확인 합니다. 그렇지 않으면 마우스 단추를 설정 합니다.

  ![](appstore-images/xs-startup.png "시작 프로젝트 설정")

2. 선택 된 **AppStore** 빌드 구성:

  ![](appstore-images/xs-appstore.png "AppStore 빌드 구성")

3. 선택 된 **빌드 > 보관** 보관 프로세스를 시작 하려면 메뉴 항목:

  ![](appstore-images/xs-archive.png "빌드 메뉴")

선택할 수도 있습니다는 **보기 > 아카이브...**  메뉴 항목을 이전에 생성 된 보관 파일을 참조 하십시오.

  ![](appstore-images/xs-archives-sml.png "보관 파일 보기")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode는 Mac.에 대 한 Visual Studio에서 만든 보관 파일을 자동으로 표시 됩니다.

1. Xcode를 시작 하 고 선택 **창 > 도우미**:

  ![](appstore-images/xc-organizer.png "창 메뉴")

2. 전환 하는 **보관** 탭을 선택한 Mac 용 Visual Studio를 사용 하 여 만든 보관:

  ![](appstore-images/xc-archives.png "보관 탭")

3. 필요에 따라 **유효성 검사 중...**  , 보관 저장소를 눌러 **전송 중...**  iTunes Connect에 앱을 업로드할 수 있습니다.

4. (사용자가 속한 둘 이상의) 하는 경우 개발 팀을 선택 합니다. 제출을 확인 합니다.

  ![](appstore-images/xc-submit1.png "개발 팀 섹션")

5. ITunes Connect 업로드 된 바이너리를 다시 방문 합니다. 응용 프로그램의 구성 페이지로 이동 하 고 선택 **시험판** 참조 하 고 상단 메뉴에서는 **빌드** 목록:

  [![](appstore-images/itc-prerelease-sml.png "ITunes Connect에서에서 응용 프로그램 구성 페이지")](appstore-images/itc-prerelease.png#lightbox)

다음 승인을 받기 위해 응용 프로그램을 제출할 수 있습니다는 **버전** 페이지. 참조는 [iOS 응용 프로그램 배포 개요](~/ios/deploy-test/app-distribution/index.md) 자세한 정보에 대 한 합니다.


## <a name="troubleshooting"></a>문제 해결

다음은 몇 가지 오류 수정 위해 수행할 수 있는 단계 및 앱 스토어에 제출 하는 동안 발생할 수 있습니다.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>보관 메뉴 옵션 Mac 용 Visual Studio에 표시 됩니다.

에 따라는 [위의 단계](#xamarin_studio) 보관 하기 위한 솔루션을 구성 하려면. 시작 프로젝트를 올바르게 설정할 수 없습니다, 빌드 구성이 디버그 또는 릴리스 시작 프로젝트를 변경 하기 전에 설정 먼저이 확인 합니다. 빌드 구성을 다시 설정 합니다 **AppStore**합니다.

### <a name="invalid-icon"></a>잘못됨 아이콘

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcons27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

에 따라는 [알파 채널을 제거 하기 위한 지침](~/ios/watchos/troubleshooting.md) 아이콘이에서.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion 일치 하지 않습니다.

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

-IOS 앱, 조사식 확장 및 Watch 앱-솔루션의 모든 프로젝트는 동일한 버전 번호를 사용 해야 합니다. 각 편집 **Info.plist** 파일 버전 번호를 정확 하 게 일치 합니다.

### <a name="missing-icons"></a>누락 된 아이콘

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

지침에 따라는 [아이콘 작업](~/ios/watchos/app-fundamentals/icons.md) Watch 앱 프로젝트에 필요한 모든 이미지를 추가 하려면.

### <a name="missing-icon"></a>누락 된 아이콘

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Mac 및에 대 한 최신 버전의 Visual Studio를 사용 해야 하면 **AppIcons.appiconset** 이미지의 전체 집합을 포함 합니다. 이 오류를 표시 여전히 되는 경우 원본을 보려면는 **Contents.json** 에 필요한 모든 이미지에 대 한 항목이 포함 되어 있는지 확인 합니다. Xamarin의 최신 버전을 사용 하는 올바른지 확인 했으면 삭제 하 고 다시 만드는 또는 **AppIcons.appiconset**합니다.

> [!IMPORTANT]
> Mac의 조사식 아이콘 지원에 대 한 Visual Studio에서 알려진된 버그가 있습니다: 88 x 88 픽셀의 이미지를 가정은  **29x29@3x**  이미지 (87 x 87 픽셀 이어야 함).


이 문제는 해결 Visual Studio에서 Mac-중 하나가 편집 Xcode에서 이미지 자산 수 없거나 수동으로 편집 하는 **Contents.json** 파일 (일치 하도록 [이 샘플](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>잘못 된 WatchKit 지원

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

이 메시지가 나타날 수 있습니다 또는 자동화 된 전자 메일 유효성 검사 및 제출 하는 동안 iTunes Connect에서에서 분명히 성공적으로 업로드 한 후 합니다.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> 수행 해야 **보관** Mac 한 다음 전환할 Xcode 6.2 + iTunes Connect에 업로드 하 고 확인할 수에 대 한 Visual Studio에서 응용 프로그램입니다.


안정적인 Xamarin 채널과 Xcode 6.2 +를 사용 합니다.



### <a name="invalid-provisioning-profile"></a>잘못 된 프로비저닝 프로필

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**배포 프로 비전 프로필** 조사식 응용 프로그램 솔루션의 모든 세 프로젝트에 대해 제공 해야 합니다: iOS 응용 프로그램, 조사식 확장 및 Watch 앱-하거나 명시적으로 (3 개 프로필) 또는 단일 와일드 카드 프로필을 통해. IOS 개발자 센터에에서 프로 비전 프로필을 다운로드 하 고 mac에 추가 했는지 확인 하십시오.

### <a name="invalid-code-signing-entitlements"></a>잘못 된 코드 자격 서명

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

사용자 프로 비전 프로필은 Apple 개발자 센터에 올바르게 설치 및 다운로드 하 고 설치 했는지 확인 합니다. 또한 각 프로젝트에 대 한 Mac의 속성 창에 대 한 Visual Studio에서 설정 확인 합니다.

### <a name="invalid-architecture"></a>잘못 된 아키텍처

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

조사식 앱만 추가할 수 있습니다 [통합 API (64 비트)](~/cross-platform/macios/unified/index.md) Xamarin.iOS 앱.
IOS 앱 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음로 이동 **옵션 > 빌드 > iOS 빌드 > 고급 탭** 되어 있는지 확인 하 고는 **지원 아키텍처** AppStore iPhone에 대 한 구성에 포함 된 **ARM64** 않음 (예: **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>이 번들 올바르지 않습니다.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

부모 iOS 응용 프로그램에 '8.2' 이하로 설정 MinimumOSVersion 있어야 합니다.

### <a name="non-public-api-usage"></a>Public이 아닌 API 사용법입니다.

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

최신 버전의 Xcode 및 xamarin이 설치의 도구를 사용 하는 것을 확인 합니다.
코드에서 public이 아닌 Api를 액세스 하지 않아야 합니다.

### <a name="build-error-mt5309"></a>오류 MT5309 빌드

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

이 오류는에 있는 이름을 바꿀에서 Xcode 설치의 결과 가능성이 **Xcode.app**합니다. 프로그램 설치를 바꾸면이 오류는 발생 하는 예를 들어, **XCode 6.2.app**합니다.



## <a name="related-links"></a>관련 링크

- [Apple WatchKit 제출 가이드](https://developer.apple.com/app-store/watch/)
