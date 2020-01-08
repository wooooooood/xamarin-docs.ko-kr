---
title: 추가 iOS 9 프레임 워크 변경 내용
description: 이 문서에서는 iOS 9에 도입 된 추가 프레임 워크 변경 내용에 대해 설명 합니다. AVFoundation, Avfoundation 및 CloudKit에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: d9d47e750580bb9e4a0f4a2283cbd9e8c6a44c93
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489091"
---
# <a name="additional-ios-9-frameworks-changes"></a>추가 iOS 9 프레임 워크 변경 내용

_이 문서에서는 iOS 9의 기존 프레임 워크에 대 한 추가, 사소한 변경 또는 향상 된 기능을 설명 합니다._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 Logo")](additional-framework-changes-images/ios9.png#lightbox)

IOS에 대 한 주요 변경 내용 외에도 Apple은 iOS 9의 여러 기존 프레임 워크에 대 한 수정 및 개선을 수행 했습니다.

## <a name="avfoundation-framework-additions"></a>AVFoundation 프레임 워크 추가

AVFoundation 프레임 워크에서 이제 [AVSpeechSynthesisVoice](xref:AVFoundation.AVSpeechSynthesisVoice) 클래스를 사용 하 여 언어 외에 식별자로 음성을 지정할 수 있습니다.

예를 들어 다음 코드는 사용 가능한 모든 음성 목록을 가져옵니다.

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

그런 다음 [AVSpeachUtterance](xref:AVFoundation.AVSpeechUtterance) 클래스 인스턴스의 `Voice` 속성으로 설정 하 여 목록에서 음성 중 하나를 사용할 수 있습니다.

이제 [Avqueueplayer](xref:AVFoundation.AVQueuePlayer) 클래스는 큐에 인터넷 스트림과 파일 기반 미디어를 혼합 하 여 지원 합니다. 이전 버전에서는 동일한 유형의 미디어만 큐에 대기 시킬 수 있었습니다.

자세한 내용은 Apple의 [AVSpeechSynthesisVoice 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)를 참조 하세요.

## <a name="avkit-framework-additions"></a>AVKit 프레임 워크 추가

새 PIP (그림-사진) 기능을 사용 하기 위해 AVKit 프레임 워크에는 새로운 `AVPictureInPictureController` 및 [AVPlayerViewController](xref:AVKit.AVPlayerViewController) 클래스가 포함 되어 있습니다.

- **Avpip Inpip controller** -이 클래스는 iOS 9 앱이 iPad의 크기 조정 가능한 부동 PIP 창에서 비디오 재생을 시작 하는 사용자에 게 응답할 수 있도록 합니다.
- **AVPlayerViewController** -iPad에서 크기 조정 가능한 부동 PIP 창에 비디오를 표시 하는 데 사용 되는 `AVPlayer` 컨트롤러를 관리 합니다.

자세한 내용은 [iPad 용 멀티태스킹](~/ios/platform/introduction-to-ios9/index.md#multitasking) 설명서 및 Apple의 [Avpictureinpicturecontroller 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) 및 [AVPlayerViewController 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController)를 참조 하세요.

## <a name="introducing-cloudkit-web-services"></a>CloudKit 웹 서비스 소개

CloudKit 프레임 워크는 iCloud에 액세스 하는 응용 프로그램 개발을 간소화 합니다. 여기에는 응용 프로그램 데이터를 검색 하 고 응용 프로그램 정보를 안전 하 게 저장할 수 있는 권한이 포함 됩니다. 이 키트는 개인 정보를 공유 하지 않고 iCloud Id로 응용 프로그램에 대 한 액세스를 허용 하 여 사용자에 게 익명의 계층을 제공 합니다.

새 _Cloudkit 웹 서비스_ 프레임 워크는 xamarin.ios 앱과 동일한 cloudkit 기반 데이터 및 콘텐츠에 대 한 액세스를 제공 하기 위해 웹 사이트에 통합 될 수 있는 JavaScript 라이브러리 (CLOUDKIT JS)를 제공 합니다.

> [!IMPORTANT]
> CloudKit JS를 사용 하 여 CloudKit 데이터베이스에서 콘텐츠를 액세스, 표시 또는 업데이트 하려면 먼저 해당 데이터베이스의 스키마를 정의 해야 합니다.

자세한 내용은 다음 문서를 참조 하세요.

- [Cloudkit 소개](~/ios/data-cloud/intro-to-cloudkit.md) -xamarin.ios 앱에서 cloudkit를 사용 하는 방법을 소개 합니다.
- [Cloudkit 빠른 시작](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -Apple의 cloudkit 소개.
- [CLOUDKIT Js 참조](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -Apple의 cloudkit js 설명서.
- [Cloudkit 웹 서비스 참조](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -cloudkit에 대 한 HTTP 인터페이스를 설명 하는 Apple의 참조입니다.
- Cloudkit 카탈로그: cloudkit 및 CloudKit를 사용 하는 Apple 샘플 앱 및 cloudkit [(Cocoa 및 JavaScript)에 대 한 소개](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) 입니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="foundation-framework-additions"></a>Foundation Framework 추가 기능

Apple에는 iOS 9의 Foundation framework에 대 한 다음과 같은 변경 내용이 포함 되어 있습니다.

### <a name="changes-to-nsbundle"></a>NSBundle에 대 한 변경 내용

IOS 9에 대 한 [Nsbundle](xref:Foundation.NSBundle) 클래스가 다음과 같이 변경 되었습니다.

- `GetPreservationPriorityForTag (NSString tag)`-지정 된 태그를 사용 하 여 리소스에 대 한 현재 유지 우선 순위를 가져옵니다. 유효한 값은 `1.0``0.0` 범위에 있으며 우선 순위가 가장 낮은 리소스는 먼저 제거 됩니다.
- `SetPreservationPriorityForTag (double priority, NSSet tags)`-지정 된 태그를 사용 하 여 리소스에 대 한 현재 유지 우선 순위를 설정 합니다. 유효한 값은 `1.0``0.0` 범위에 있으며 우선 순위가 가장 낮은 리소스는 먼저 제거 됩니다.

자세한 내용은 Apple의 [Nsbundle 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)를 참조 하세요.

### <a name="changes-to-nsprocessinfo"></a>NSProcessInfo에 대 한 변경 내용

IOS 장치에서 실행 되는 각 프로세스에는 단일 PIA ( _프로세스 정보 에이전트_ )가 있습니다. [Nsprocessinfo](xref:Foundation.NSProcessInfo) 클래스를 사용 하 여 현재 PIA에 대 한 정보를 제공 하 고 지정 된 프로세스에 대 한 전원 및 열 관리를 제어 합니다.

예를 들어 프로세스의 자동 종료를 제어 하려면 다음 코드를 사용할 수 있습니다.

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

자세한 내용은 Apple의 [Nsprocessinfo 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)를 참조 하세요.

### <a name="reacting-to-low-power-mode"></a>낮은 전원 모드로 응답

[Nsprocessinfo](xref:Foundation.NSProcessInfo) 클래스의 `LowPowerModeEnabled` 속성을 사용 하 여 앱이 실행 되 고 있는 iOS 장치에서 낮은 전원 모드를 사용 하도록 설정 했는지 확인 합니다. 예를 들면 다음과 같습니다.:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit 프레임 워크 변경

Apple에는 iOS 9의 [HealthKit](xref:HealthKit) 프레임 워크에 대 한 다음과 같은 변경 내용이 포함 되어 있습니다.

- HealthKit 데이터베이스의 항목에 대 한 대량 삭제 및 삭제 추적 지원. 자세한 내용은 Apple의 [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) 및 [HKHealthStore 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) 를 참조 하세요.
- `HKQuantityTypeIdentifier` 클래스 (예: `UVExposure`) 및 `HKCategoryTypeIdentifier` 클래스 (예: `OvulationTestResult`)에 새로운 추적 범주 및 특징이 추가 되었습니다. 자세한 내용은 Apple의 [HealthKit 상수 참조](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) 를 참조 하세요.

Xamarin.ios에서 HealthKit를 사용 하는 방법에 대 한 자세한 내용은 [HealthKit 소개 설명서를](~/ios/platform/healthkit.md) 참조 하세요.

## <a name="local-authentication-framework-changes"></a>로컬 인증 프레임 워크 변경

Apple에는 iOS 9의 [로컬 인증](xref:LocalAuthentication) 프레임 워크에 대 한 다음과 같은 변경 내용이 포함 되어 있습니다.

- 이제 [LAContext](xref:LocalAuthentication.LAContext) 클래스의 `EvaluateAccessControl` 및 `EvaluatePolicy` 메서드를 사용하여 이전에 성공한 잠금 해제 시도에서 Touch ID 일치를 재사용할 수 있습니다.
- 현재 등록 된 손가락의 목록을 가져올 수 있습니다.
- 손가락을 인증에서 추가 하거나 제거 하는 경우 추적을 지원 합니다.
- 키 집합 호출에서 _인증 컨텍스트_ 를 사용 하 고 키 집합 액세스 제어 목록을 평가 하는 기능이 제공 됩니다.
- 코드에서 사용자 프롬프트를 취소할 수 있는 권한입니다.

자세한 내용은 [xamarin.ios를 사용 하 여 TOUCH id 및 얼굴 id](~/ios/platform/touch-id-face-id.md)를 참조 하세요.

### <a name="lacontext-changes"></a>LAContext 변경

IOS 9의 [LAContext](xref:LocalAuthentication.LAContext) 클래스에 대해 다음과 같이 변경 되었습니다.

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -touch ID 인증을 다시 사용할 수 있는 최대 시간을 반환 합니다.
- **EvaluatedPolicyDomainState** -평가 된 정책의 상태를 가져오거나 설정 합니다.
- **MaxBiometryFailures** -iOS 9에서 사용 되었습니다.
- **TouchIdAuthenticationAllowableReuseDuration** Touch ID 인증을 다시 사용할 수 있는 시간을 가져오거나 설정 합니다.
- **EvaluateAccessControl** -비동기적으로 인증 정책을 평가 합니다.
- **무효화** -지정 된 touch ID 인증을 무효화 합니다.
- **Iscredentialset** -자격 증명이 현재 설정 된 경우 `true`를 반환 합니다.
- **Setcredentialtype** 지정 된 자격 증명 형식을 설정 합니다.

자세한 내용은 Apple의 [LAContext 참조](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) 를 참조 하세요.

## <a name="mapkit-framework-changes"></a>MapKit 프레임 워크 변경

Apple에는 iOS 9의 [Mapkit](xref:MapKit) 프레임 워크가 다음과 같이 변경 되었습니다.

- 이제 MapKit은 [MKLaunchOptions](xref:MapKit.MKLaunchOptions) 및 [MKDirections](xref:MapKit.MKLaunchOptions) 클래스를 사용 하 여 전송 방향으로 직접 맵 앱을 시작 하 고 전송 예상 도착 시간 (에타)을 쿼리 하는 기능을 제공 합니다.
- MapKit에서 반환 된 검색 결과 및 [CLGeocoder](xref:CoreLocation.CLGeocoder) 클래스는 결과의 표준 시간대를 제공할 수도 있습니다.
- 이제 [MKAnnotationView](xref:MapKit.MKAnnotationView) 클래스의 `DetailCalloutAccessoryView` 속성을 사용 하 여 iOS 앱에서 제공 하는 맵 주석을 완전히 사용자 지정할 수 있습니다.

자세한 내용은 [Ios 맵](~/ios/user-interface/controls/ios-maps/index.md) 및 [연습-mapkit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md)의 Xamarin.iOS 및 Apple의 [CLGeocoder 참조](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder)에서 주석과 오버레이 탐색 설명서에서 맵과 주석을 사용하는 방법에 대한 자세한 내용은 참조하세요.

## <a name="passkit-framework-additions"></a>PassKit 프레임 워크 추가

Apple에는 iOS 9의 [PassKit](xref:PassKit) 프레임 워크에 대 한 다음과 같은 변경 내용이 포함 되어 있습니다.

- 이제 Apple Pay는 검색 카드와 함께 저장 직불 및 신용 카드를 모두 지원 합니다. 자세한 내용은 Apple의 [PKPaymentRequest 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) 의 **결제 네트워크** 섹션을 참조 하세요.
- 이제 Xamarin.ios 앱 내에서 직접 지불 네트워크와 카드 발급자를 Apple Pay에 추가할 수 있습니다. 자세한 내용은 Apple의 [PKAddPaymentPassViewController 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) 를 참조 하세요.

Xamarin.ios에서 PassKit를 사용 하는 방법에 대 한 자세한 내용은 [PassKit 소개 설명서를](~/ios/platform/passkit.md) 참조 하세요.

## <a name="safari-services-framework-additions"></a>Safari 서비스 프레임 워크 추가

Apple에는 iOS 9의 [Safari 서비스](xref:SafariServices) 프레임 워크에 대 한 다음과 같은 변경 내용이 포함 되어 있습니다.

- 이제 새 [SFSafariViewController](xref:SafariServices.SFSafariViewController) 클래스를 사용 하 여 xamarin.ios 앱 내에 웹 콘텐츠를 표시할 수 있습니다. 웹 사이트 데이터 및 쿠키를 Safari 앱과 공유 하는 기능을 제공 하 고 몇 가지 Safari 기능 (예: 읽기 및 자동 채우기)을 포함 합니다. [SFSafariViewController](xref:SafariServices.SFSafariViewController) 기능 웹 콘텐츠 보기를 완료 한 후 사용자를 앱에 반환 하는 **완료** 단추를 제공 합니다.

[SFSafariViewController](xref:SafariServices.SFSafariViewController) 클래스는 웹 콘텐츠의 단일 페이지를 표시 하도록 맞춤 되어 있기 때문에 기존 xamarin.ios 앱 내에서 [WKWebKit](xref:WebKit.WKWebView) 또는 [uiwebview 보기](xref:UIKit.UIWebView) 컨트롤을 대체 하는 데이 클래스를 사용 하는 것이 좋습니다.

### <a name="displaying-a-website"></a>웹 사이트 표시

다음 코드는 다른 뷰 컨트롤러 내에서 [SFSafariViewController](xref:SafariServices.SFSafariViewController) 를 호출 하는 예입니다.

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit 프레임 워크 변경

Apple에는 iOS 9 용 [Uikit](xref:UIKit) 프레임 워크의 여러 요소에 대 한 많은 향상 된 기능이 포함 되어 있습니다. 다음 섹션에서는 이러한 변경 내용에 대해 자세히 설명 합니다.

### <a name="3d-touch-events"></a>3D 터치 이벤트

IOS 9 및 iPhone 6s와 iPhone 6s Plus를 처음 접하는 3D Touch는 iOS 앱에 압력 민감한 제스처를 추가 합니다. 따라서 앱이 iOS 9 이상에서 실행 중이 고 iOS 장치에서 3D 터치를 지원할 수 있는 경우 압력을 변경 하면 `TouchesMoved` 이벤트가 발생 합니다.

이러한 동작 변경으로 인해 X/Y 좌표가 변경 되지 않은 경우에도 `TouchesMoved` 이벤트를 더 자주 호출 하도록 iOS 앱을 준비 해야 합니다.

자세한 내용은 [3D 터치 가이드 소개](~/ios/platform/3d-touch.md) 를 참조 하세요.

### <a name="document-open-in-place-functionality"></a>문서 열기 기능

이제 [Uiapplicationdelegate](xref:UIKit.UIApplicationDelegate) 클래스의 `FinishedLaunching (application, launchOptions)` 또는 `WillFinishLaunching (Application, launchOptions)` 메서드를 사용 하 여 문서를 열고 현재 위치의 문서를 수정할 수 있습니다 (복사본에서 작업 하는 것과 반대).

새 열기 기능을 지원 하려면 `LSSupportsOpeningDocumentsInPlace` 키를 `YES`값이 포함 된 Xamarin.ios 앱의 **info.plist** 파일에 추가 합니다.

자세한 내용은 Apple의 [Uiapplicationdelegate 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) 를 참조 하세요.

### <a name="enhanced-touch-events"></a>향상 된 터치 이벤트

Apple은 iOS 9의 터치 이벤트에 대해 몇 가지 향상 된 기능을 제공 합니다. 여기에는 터치 예측을 사용 하 고 디스플레이 새로 고침 사이에 중간 터치에 액세스할 수 있는 기능이 포함 됩니다.

자세한 내용은 [iOS에 대 한 Apple의 이벤트 처리 가이드](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) 를 참조 하세요.

### <a name="fetching-tailored-content"></a>맞춤형 콘텐츠 페치

새 `NSDataAsset` 클래스를 사용 하면 Xamarin.ios 앱이 현재 실행 중인 iOS 장치의 메모리 및 그래픽 기능에 맞게 조정 된 콘텐츠를 가져올 수 있습니다.

### <a name="new-layout-anchors"></a>새 레이아웃 앵커

새 `NSLayoutAnchor` 및 `NSLayoutDimension` 레이아웃 앵커 클래스는 [Uiview](xref:UIKit.UIView) 클래스의 새 앵커 속성 (예: `LeadingAnchor` 및 `WidthAnchor`)을 사용 하 여 iOS 9에서 레이아웃을 보다 쉽게 만들 수 있습니다.

Xamarin.ios 앱 및 Apple의 [Nslayoutanchor 참조](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor)에서 자동 레이아웃 및 크기 클래스로 작업 하는 방법에 대 한 자세한 내용은 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서를 참조하고 [NSLayoutDimension Reference](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) 참조를 참조 하세요. 자세한 내용은 [UIView 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView)를 참조 하세요.

### <a name="new-readable-content-margins"></a>새 읽을 수 있는 콘텐츠 여백

새 `UILayoutGuide` 클래스를 사용 하 여 읽기 가능한 콘텐츠 여백을 제공 하 고 뷰 내에서 콘텐츠의 그리기 영역을 정의할 수 있습니다. 자세한 내용은 Apple의 [UILayoutGuide 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) 를 참조 하세요.

### <a name="text-input-in-notifications-modifications"></a>알림에서 텍스트 입력을 수정 합니다.

[Uiusernotificationaction](xref:UIKit.UIUserNotificationAction) 클래스에는 알림에서 텍스트 입력을 지 원하는 데 사용할 수 있는 새로운 `Behavior` 속성이 있습니다.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate 변경 내용

Apple에서는 공식적으로 사용 되지 않지만 [Uiapplicationdelegate](xref:UIKit.UIApplicationDelegate) 클래스의 `FinishedLaunching (UIApplication application)` 메서드에 대 한 모든 호출을 `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` 또는 `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` 메서드로 대체 하는 것이 좋습니다.

자세한 내용은 Apple의 [Uiapplicationdelegate 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) 를 참조 하세요.

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics 변경 내용

Apple에는 iOS 9의 UIKit Dynamics에 대 한 다음과 같은 변경 내용이 포함 되어 있습니다.

- 이제 Dynamics에서 사각형이 아닌 충돌 경계를 지원 합니다.
- 새로운 사용자 지정 가능 `UIFieldBehavior` 클래스를 사용 하 여 다양 한 필드 형식을 지원할 수 있습니다.
- 추가 첨부 파일 형식이 `UIAttachmentBehavior` 클래스에 추가 되었습니다.

자세한 내용은 Apple의 [Uiattachment 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) 를 참조 하세요.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView 및 UIDatePicker 변경 내용

IOS 9 이전에 [UIPickerView](xref:UIKit.UIPickerView) 및 [UIDatePicker](xref:UIKit.UIDatePicker) 컨트롤은 크기를 조정할 수 없으며, 컨테이너의 너비를 채우도록 자동으로 크기가 조정 됩니다 (일반적으로 앱이 실행 되는 iOS 장치의 너비).

IOS 9에서는 이러한 자동 크기 조정이 더 이상 발생 하지 않으며 화면 크기 및 방향에 관계 없이 모든 iOS 장치에서 320 포인트 너비로 컨트롤이 렌더링 됩니다.

이러한 상황을 해결 하려면 자동 레이아웃 및 크기 클래스를 사용 하 여 컨트롤의 너비를 부모 컨테이너 (뷰)의 가장자리에 고정 하 고 필요한 높이를 지정 합니다. Xamarin.ios 앱에서 자동 레이아웃 및 크기 클래스로 작업 하는 방법에 대 한 자세한 내용은 [통합 Storyboard 소개 설명서를](~/ios/user-interface/storyboards/unified-storyboards.md) 참조 하세요.

### <a name="new-uitextinputassistantitem-class"></a>새 UITextInputAssistantItem 클래스

새 `UITextInputAssistantItem` 클래스를 사용 하 여 _바로 가기 표시줄_의 레이아웃 막대 단추 그룹을 만듭니다. 바로 가기 모음은 소프트 키보드에서 입력 바로 가기 키를 제공 하는 데 사용할 수 있는 새로운 영역입니다.

## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0의 새로운 기능](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
