---
title: 추가 iOS 9 프레임 워크 변경
description: 이 문서에는 iOS 9에에서 도입 된 추가 프레임 워크 변경 내용을 설명 합니다. AVFoundation, AVKit, 및 CloudKit에 설명 합니다.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 5156259f8178da69595464f75a10cd8f41965519
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870328"
---
# <a name="additional-ios-9-frameworks-changes"></a>추가 iOS 9 프레임 워크 변경

_이 문서에서는 추가, 사소한 변경 하거나 iOS 9에 대 한 기존 프레임 워크의 향상 된 기능을 설명 합니다._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 Logo")](additional-framework-changes-images/ios9.png#lightbox)

IOS에 주요 변경 내용 외에도 Apple가 수정 및 여러 기존 프레임 워크의 향상 된 기능에 iOS 9 있게 되었습니다.

## <a name="avfoundation-framework-additions"></a>AVFoundation Framework Additions

AVFoundation 프레임 워크에는 [AVSpeechSynthesisVoice](xref:AVFoundation.AVSpeechSynthesisVoice) 클래스 이제 지정할 수 있습니다 음성 언어 외에도 식별자로.

예를 들어, 다음 코드는 모든 가능한 음성의 목록을 가져옵니다.

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

로 설정 하 여 목록에서 음성 중 다음 사용할 수는 `Voice` 인스턴스의 속성을 [AVSpeachUtterance](xref:AVFoundation.AVSpeechUtterance) 클래스입니다.

합니다 [AVQueuePlayer](xref:AVFoundation.AVQueuePlayer) 이제 클래스 큐에서 인터넷 및 파일 기반 스트리밍 미디어의 혼합을 지원 합니다. 이전 버전에는 같은 형식의 큐 미디어만 수 없습니다.

자세한 내용은 Apple의를 참조 하세요 [AVSpeechSynthesisVoice 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)합니다.

## <a name="avkit-framework-additions"></a>AVKit 프레임 워크 추가

새 그림-에-그림 (PIP) 기능을 사용 하려면 AVKit 프레임 워크는 새 `AVPictureInPictureController` 하 고 [AVPlayerViewController](xref:AVKit.AVPlayerViewController) 클래스:

- **AVPictureInPictureController** -이 클래스에는 iOS 9 앱 iPad에서 부동, 크기 조정 가능한 PIP 창에서 비디오 재생을 시작 사용자에 게 응답을 허용 합니다.
- **AVPlayerViewController** -관리는 `AVPlayer` iPad에서 부동, 크기 조정 가능한 PIP 창에서 비디오를 제공 하는 데 사용 하는 컨트롤러입니다.

자세한 내용은 참조 하십시오 우리의 [iPad 용 멀티태스킹](~/ios/platform/introduction-to-ios9/index.md#multitasking) 설명서와 Apple [AVPictureInPictureController 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) 고 [AVPlayerViewController 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>CloudKit 웹 서비스를 소개합니다.

CloudKit 프레임 워크는 해당 액세스 iCloud 응용 프로그램 개발을 간소화합니다. 여기에 응용 프로그램 정보를 안전 하 게 저장할 수 뿐만 아니라 응용 프로그램 데이터 및 자산 권한을 검색 합니다. 이 키트는 개인 정보를 공유 하지 않고 Id가 iCloud 사용 하 여 응용 프로그램에 대 한 액세스를 허용 하 여 사용자가 계층 익명성을 제공 합니다.

새 _CloudKit 웹 서비스_ framework CloudKit 기반 데이터와 콘텐츠 Xamarin.iOS 앱에 대 한 액세스를 제공 하 여 웹 사이트에 통합할 수 있는 JavaScript 라이브러리 (CloudKit JS)를 제공 합니다.

> [!IMPORTANT]
> 액세스를 제공 하거나 CloudKit JS를 사용 하 여 CloudKit 데이터베이스에서 콘텐츠를 업데이트할 수 있습니다, 전에 이전에 정의 해야 해당 데이터베이스의 스키마입니다.




자세한 내용은 다음 문서를 참조 하세요.

- [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md) -CloudKit을 사용 하 여 Xamarin.iOS 앱에 도입 합니다.
- [빠른 시작 CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -Apple의 CloudKit 소개 합니다.
- [CloudKit JS 참조](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -Apple의 CloudKit JS 설명서.
- [CloudKit 웹 서비스 참조](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -CloudKit HTTP 인터페이스를 설명 하는 Apple의 참조 합니다.
- [CloudKit 카탈로그: (Cocoa 및 JavaScript) CloudKit 소개](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -CloudKit 및 CloudKit JS를 사용 하 여 Apple의 샘플 앱입니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="foundation-framework-additions"></a>Foundation 프레임 워크 추가

Apple는 iOS 9에에서 다음과 같이 변경 Foundation 프레임 워크를 포함:

### <a name="changes-to-nsbundle"></a>NSBundle 변경

에 다음 변경 사항이 생겼는지 합니다 [NSBundle](xref:Foundation.NSBundle) iOS 9에 대 한 클래스:

* `GetPreservationPriorityForTag (NSString tag)` -지정된 된 태그를 사용 하 여 리소스에 대 한 현재 보존 우선 순위를 가져옵니다. 유효한 값은 범위의 `0.0` 에 `1.0`, 낮은 우선 순위를 사용 하 여 리소스를 먼저 제거 됩니다.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -지정 된 태그를 사용 하 여 리소스에 대 한 현재 보존 우선 순위를 설정합니다. 유효한 값은 범위의 `0.0` 에 `1.0`, 낮은 우선 순위를 사용 하 여 리소스를 먼저 제거 됩니다.

자세한 내용은 Apple의를 참조 하세요 [NSBundle 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)합니다.

### <a name="changes-to-nsprocessinfo"></a>NSProcessInfo 변경

IOS 장치에서 실행 중인 각 프로세스에는 단일 _프로세스 정보 에이전트_ (PIA). 사용 된 [NSProcessInfo](xref:Foundation.NSProcessInfo) 현재 PIA 및 제어 기능을 지정된 된 프로세스에 대 한 열 관리 하는 방법에 대 한 정보를 제공 합니다.

예를 들어 프로세스의 자동 종료를 제어 하려면 다음 코드를 사용할 수 있습니다.

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

자세한 내용은 Apple의를 참조 하세요 [NSProcessInfo 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)합니다.

### <a name="reacting-to-low-power-mode"></a>절전 모드에 대응

사용 하 여를 `LowPowerModeEnabled` 의 속성을 [NSProcessInfo](xref:Foundation.NSProcessInfo) 낮은 전원 모드가 앱에서 실행 되는 iOS 장치에서 활성화 된 경우를 결정 하는 클래스입니다. 예를 들어:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit Framework 변경 내용

Apple 포함 다음과 같이 변경 합니다 [HealthKit](xref:HealthKit) iOS 9 프레임 워크:

- 대량 삭제 및 HealthKit 데이터베이스에 있는 항목의 삭제 추적을 지원 합니다. Apple의를 참조 하세요 [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) 하 고 [HKHealthStore 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) 자세한 내용은 합니다.
- 새 추적 범주 및 특성에 추가 된를 `HKQuantityTypeIdentifier` 클래스 (같은 `UVExposure`) 및 합니다 `HKCategoryTypeIdentifier` 클래스 (같은 `OvulationTestResult`). Apple의를 참조 하세요 [HealthKit 상수 참조](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) 자세한 내용은 합니다.

하십시오 우리의 [HealthKit 소개](~/ios/platform/healthkit.md) Xamarin.iOS에서 HealthKit 사용에 대 한 자세한 내용은 설명서.

## <a name="local-authentication-framework-changes"></a>로컬 인증 프레임 워크 변경 내용

Apple 포함 다음과 같이 변경 합니다 [Local Authentication](xref:LocalAuthentication) iOS 9 프레임 워크:

- 사용 하 여 합니다 `EvaluateAccessControl` 및 `EvaluatePolicy` 의 메서드는 [LAContext](xref:LocalAuthentication.LAContext) 클래스 이제 수 있습니다. 이전 성공적으로 잠금 해제에서 Touch ID와 일치 하는 다시 사용할 수 있도록 시도 합니다.
- 현재 등록 된 손가락 목록을 가져올 수 있습니다.
- 손가락을 추가 하거나 인증에서 제거할 때 추적을 지원 합니다.
- 사용할 수 있다는 _인증 컨텍스트_ Keychain 호출에 키 집합 액세스 제어를 평가 하는 것에 대 한 지원을 나열 합니다.
- 코드에서 사용자 프롬프트를 취소할 수 있습니다.

참조 하십시오 우리의 [Touch ID 소개](~/ios/platform/touchid.md) Xamarin.iOS에서 Touch ID를 사용 하는 방법은 설명서.

### <a name="lacontext-changes"></a>LAContext 변경

에 다음 변경 사항이 생겼는지 합니다 [LAContext](xref:LocalAuthentication.LAContext) iOS 9에 대 한 클래스:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -터치 ID 인증을 다시 사용할 수 있는 최대 기간을 반환 합니다.
- **EvaluatedPolicyDomainState** -가져오거나 계산 정책의 상태를 설정 합니다.
- **MaxBiometryFailures** -iOS 9에서에서 상각액이 되었습니다 했습니다.
- **TouchIdAuthenticationAllowableReuseDuration** touch ID 인증을 다시 사용할 수 있는 시간을 가져오거나 설정 합니다.
- **EvaluateAccessControl** -비동기적으로 인증 정책을 평가 합니다.
- **무효화** -지정 된 터치 ID 인증을 무효화 합니다.
- **IsCredentialSet** -반환 `true` 자격 증명이 현재 설정 하는 경우.
- **SetCredentialType** 지정 된 자격 증명 형식을 가져오거나 설정 합니다.

Apple의를 참조 하세요 [LAContext 참조](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) 대 한 자세한 내용은 합니다.

## <a name="mapkit-framework-changes"></a>MapKit Framework 변경 내용

Apple 포함 다음과 같이 변경 합니다 [MapKit](xref:MapKit) iOS 9 프레임 워크:

- 전송 지침에 직접 맵 앱 시작 및 전송 사용 (ETA) 도착 예상 시간을 쿼리 하기 위해 이제 MapKit 지원을 제공 합니다 [MKLaunchOptions](xref:MapKit.MKLaunchOptions) 하 고 [MKDirections](xref:MapKit.MKLaunchOptions) 클래스입니다.
- MapKit 반환한 검색 결과 및 [CLGeocoder](xref:CoreLocation.CLGeocoder) 클래스는 결과 표준 시간대를 제공할 수도 있습니다.
- 이제 완전히 사용자 지정할 수 있습니다 사용 하 여 iOS 앱에서 제공 하는 맵 주석 합니다 `DetailCalloutAccessoryView` 의 속성을 [MKAnnotationView](xref:MapKit.MKAnnotationView) 클래스.

참조 하십시오 우리의 [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) 하 고 [연습-주석 및 오버레이 MapKit의 탐색](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) 맵과 Xamarin.iOS에서 Apple 주석을사용한작업에대한자세한내용은설명서[CLGeocoder 참조](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) 자세한 내용은 합니다.

## <a name="passkit-framework-additions"></a>PassKit Framework 추가

Apple 포함 다음과 같이 변경 합니다 [PassKit](xref:PassKit) iOS 9 프레임 워크:

- 이제, Apple Pay 저장소 직불와 검색 카드와 함께 신용 카드를 모두 지원합니다. 참조를 **지불 네트워크** Apple의 섹션 [PKPaymentRequest 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) 자세한 내용은 합니다.
- Xamarin.iOS 앱 내에서 직접 추가할 수 있습니다 이제 결제 네트워크와 카드 발급자 Apple Pay까지. Apple의를 참조 하세요 [PKAddPaymentPassViewController 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) 대 한 자세한 내용은 합니다.

참조 하십시오 우리의 [PassKit 소개](~/ios/platform/passkit.md) PassKit Xamarin.iOS에서 작업 하는 방법은 설명서.

## <a name="safari-services-framework-additions"></a>Safari 서비스 프레임 워크 추가

Apple 포함 다음과 같이 변경 합니다 [Safari Services](xref:SafariServices) iOS 9 프레임 워크:

- 이제 사용할 수 있습니다 새 [필요한 SFSafariViewController](xref:SafariServices.SFSafariViewController) 클래스 Xamarin.iOS 앱 내에서 웹 콘텐츠를 표시 합니다. Safari 앱을 사용 하 여 웹 사이트 데이터 및 쿠키를 공유 하는 기능을 제공 하 고 다양 한 Safari의 기능 (예: 판독기 및 자동 채우기)을 포함 합니다. [필요한 SFSafariViewController](xref:SafariServices.SFSafariViewController) 기능을 **수행** 웹 콘텐츠를 볼 때 사용자가 앱에 반환 하는 단추입니다.

때문에 합니다 [필요한 SFSafariViewController](xref:SafariServices.SFSafariViewController) 클래스는 페이지만 웹 콘텐츠 표시에 맞게 조정 됩니다을 바꾸는 데 사용 하는 것이 좋습니다 [WKWebKit](xref:WebKit.WKWebView) 또는 [UIWebView](xref:UIKit.UIWebView)기존 Xamarin.iOS 앱 내에서 제어 합니다.

### <a name="displaying-a-website"></a>웹 사이트를 표시합니다.

아래 코드는 호출의 예는 [필요한 SFSafariViewController](xref:SafariServices.SFSafariViewController) 에서 다른 뷰 컨트롤러 내에서:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit 프레임 워크 변경 내용

Apple의 여러 요소에 다양 한 향상 기능이 포함 된 [UIKit](xref:UIKit) iOS 9 프레임 워크입니다. 다음 섹션에서는에서는 해당 변경 내용을 자세히 설명 합니다.

### <a name="3d-touch-events"></a>3D 터치 이벤트

IOS 9 iPhone 6s와 iPhone 6s 새 또한 3D 터치 하 여 iOS 앱에가 중 중요 한 제스처를 추가 합니다. 결과적으로, 앱이 iOS 9 이상에서 실행 하 고 3D 터치를 지 원하는 iOS 장치 수, 압력의 변경 하면는 `TouchesMoved` 이벤트가 발생 합니다.

동작에서 이러한 변경으로 인해 iOS 앱 준비 되어야 합니다는 `TouchesMoved` 더 자주 호출할 이벤트 경우에 X / Y 좌표는 변경 되지 않았습니다.

자세한 내용은 참조 하십시오 우리의 [3D 터치 소개](~/ios/platform/3d-touch.md) 가이드입니다.

### <a name="document-open-in-place-functionality"></a>문서 열기-전체 기능

사용 하 여 합니다 `FinishedLaunching (application, launchOptions)` 또는 `WillFinishLaunching (Application, launchOptions)` 의 메서드는 [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate) 클래스 이제 문서를 열 및 위치 (복사 작업) 대신에 수정할 수 있습니다.

새 위치에서 열기 기능을 지원 하려면 추가 합니다 `LSSupportsOpeningDocumentsInPlace` Xamarin.iOS 앱에 키 **Info.plist** 값을 사용 하 여 파일 `YES`합니다.

Apple의를 참조 하세요 [UIApplicationDelegate 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) 대 한 자세한 내용은 합니다.

### <a name="enhanced-touch-events"></a>향상 된 터치 이벤트

Apple는 iOS 9에서에서 터치 이벤트에 여러 향상 된 기능을 제공 했습니다. 여기에 터치 예측을 사용 하 고 중간 터치 디스플레이 새로 고침 간격에 액세스할 수 포함 됩니다.

Apple의를 참조 하세요 [iOS에 대 한 이벤트 처리 지침](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) 대 한 자세한 내용은 합니다.

### <a name="fetching-tailored-content"></a>맞춤형된 콘텐츠를 가져오는 중

새 `NSDataAsset` 클래스에는 메모리 및 iOS 장치에서 현재 실행 중인 그래픽 기능에 맞도록 꾸며진 콘텐츠를 가져올 Xamarin.iOS 앱을 허용 합니다.

### <a name="new-layout-anchors"></a>새 레이아웃 앵커

새 `NSLayoutAnchor` 및 `NSLayoutDimension` 레이아웃 앵커 클래스의 새 앵커 속성을 사용 하 여 작동 합니다 [UIView](xref:UIKit.UIView) 클래스 (같은 `LeadingAnchor` 및 `WidthAnchor`) 레이아웃을 iOS 9에서에서 쉽게 수행할 수 있도록 합니다.

참조 하세요 우리의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) Xamarin.iOS 앱에 Apple의 자동 레이아웃 및 Size 클래스를 사용 하는 방법은 설명서 [NSLayoutAnchor 참조](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor)합니다 [ NSLayoutDimension 참조](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) 하 고 [UIView 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) 자세한 내용은 합니다.

### <a name="new-readable-content-margins"></a>읽을 수 있는 새 콘텐츠 여백

새 `UILayoutGuide` 읽을 수 있는 내용 여백 제공 뷰 내에서 콘텐츠에 대 한 그리기 영역을 정의 하는 클래스를 사용할 수 있습니다. Apple의를 참조 하세요 [UILayoutGuide 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) 자세한 내용은 합니다.

### <a name="text-input-in-notifications-modifications"></a>알림 수정에 대 한 텍스트 입력

합니다 [UIUserNotificationAction](xref:UIKit.UIUserNotificationAction) 클래스에 있는 새 `Behavior` 알림에서 텍스트 입력을 지원 하기 위해 사용할 수 있는 속성입니다.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate 변경

에 대 한 모든 호출을 대체를 제안 하지 공식적으로 Apple에서 사용 되지 않음 동안는 `FinishedLaunching (UIApplication application)` 메서드를 [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate) 중 하나를 사용 하 여 클래스를 `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` 또는 `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` 메서드.

Apple의를 참조 하세요 [UIApplicationDelegate 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) 대 한 자세한 내용은 합니다.

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics 변경

Apple는 iOS 9에서에서 UIKit Dynamics에 다음 변경 내용을 포함:

- Dynamics은 이제 경계 사각형이 아닌 충돌에 대 한 지원을 제공합니다.
- 신규 사용자 지정 가능한 `UIFieldBehavior` 클래스는 다양 한 필드 유형을 지원 하는 데 사용 됩니다.
- 첨부 파일 추가 형식에 추가 된를 `UIAttachmentBehavior` 클래스입니다.

Apple의를 참조 하세요 [UIAttachment 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) 대 한 자세한 내용은 합니다.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView 및 UIDatePicker 변경 내용

IOS 9 이전 합니다 [UIPickerView](xref:UIKit.UIPickerView) 하며 [UIDatePicker](xref:UIKit.UIDatePicker) 컨트롤 크기 조정이 불가능 했으며 (일반적으로 너비 앱이 iOS 장치의 컨테이너의 너비를 채우도록 자동으로 조정 하는 실행).

Ios 9에서 더 이상 발생이 자동 크기 조정 하 고 컨트롤 화면 크기 및 방향에 관계 없이 모든 iOS 장치에서 320 지점 너비에 렌더링 됩니다.

이 문제를 해결 하려면 컨트롤을 부모 컨테이너 (뷰)의 가장자리의 너비를 고정 하 고 필요한 높이 지정 하려면 자동 레이아웃 및 Size 클래스를 사용 합니다. 하십시오 우리의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 자동 레이아웃 및 Size 클래스를 사용 하 여 작업 하 여 Xamarin.iOS 앱에 대 한 자세한 내용은 설명서.

### <a name="new-uitextinputassistantitem-class"></a>새 UITextInputAssistantItem 클래스

새 `UITextInputAssistantItem` 레이아웃 막대 단추 그룹에 클래스를 _도구 모음_합니다. 도구 모음에는 소프트 키보드 입력 바로 가기를 제공에서 사용할 수 있는 새 영역입니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [새로운 iOS 9.0에서](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
