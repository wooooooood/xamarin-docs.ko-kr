---
title: 추가 iOS 9 프레임 워크 변경 내용
description: 이 문서에서는 추가, 부 버전 변경 또는 iOS 9에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 0ae286ddbc61f48cbdd257dc453a2d9680bba703
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="additional-ios-9-frameworks-changes"></a>추가 iOS 9 프레임 워크 변경 내용

_이 문서에서는 추가, 부 버전 변경 또는 iOS 9에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 로고")](additional-framework-changes-images/ios9.png#lightbox)

IOS에 주요 변경 내용 외에도 Apple가 한 수정 및 여러 기존 프레임 워크의 향상 된 기능 iOS 9입니다.

## <a name="av-foundation-framework-additions"></a>AV Foundation 프레임 워크 추가

AV Foundation 프레임 워크에는 [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) 클래스 이제 지정할 수 있습니다는 음성 언어 외에도 식별자가 있습니다.

예를 들어 다음 코드는 모든 가능한 음성 목록을 가져옵니다.

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

로 설정 하 여 목록에서 음성 중 다음 사용할 수는 `Voice` 속성의 인스턴스는 [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) 클래스입니다.

[AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) 이제 클래스 큐에 있는 인터넷 스트리밍 및 파일 기반 미디어의 혼합을 지원 합니다. 이전 버전에는 같은 형식의 큐 미디어만 수 없습니다.

자세한 내용은 Apple의를 참조 하십시오 [AVSpeechSynthesisVoice 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)합니다.

## <a name="avkit-framework-additions"></a>AVKit 프레임 워크 추가

AVKit framework 그림-에-그림 PIP ()에서 새로운 기능을 사용 하려면 새 포함 `AVPictureInPictureController` 및 [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) 클래스:

- **AVPictureInPictureController** -iOS 9 앱 iPad에서 부동, resizable PIP 창에서 비디오 재생을 시작 하는 사용자에 게 응답 하도록이 클래스를 사용 합니다.
- **AVPlayerViewController** -관리 하는 `AVPlayer` iPad에서 부동, resizable PIP 창에서 비디오를 표시 하는 데 컨트롤러입니다.

자세한 내용은 참조 하십시오 우리의 [iPad 용 멀티태스킹](~/ios/platform/introduction-to-ios9/index.md#multitasking) 설명서 및 Apple의 [AVPictureInPictureController 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) 및 [AVPlayerViewController 참조](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>CloudKit 웹 서비스를 소개합니다.

CloudKit 프레임 워크는 해당 액세스 iCloud 응용 프로그램 개발을 간소화합니다. 여기에 응용 프로그램 데이터 및 자산 권한으로 응용 프로그램 정보를 안전 하 게 저장할 수를 검색 합니다. 이 키트 개인 정보를 공유 하지 않고 자신의 iCloud Id와 응용 프로그램에 대 한 액세스를 허용 하 여 사용자에 게 익명의 계층을 제공 합니다.

새 _CloudKit 웹 서비스_ 프레임 워크는 동일한 CloudKit 기반 데이터 및 콘텐츠에 Xamarin.iOS 앱에 대 한 액세스를 제공 하도록 웹 사이트에 포함 될 수 있는 JavaScript 라이브러리가 (CloudKit JS)를 제공 합니다.

> [!IMPORTANT]
> 액세스 제공 하거나 CloudKit JS를 사용 하 여 CloudKit 데이터베이스의 콘텐츠를 업데이트할 수 있습니다, 전에 이전에 정의 해야 해당 데이터베이스의 스키마입니다.




자세한 내용은 다음 문서를 참조 하십시오.

- [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md) -CloudKit Xamarin.iOS 앱에서 사용 하는 당사의 소개 합니다.
- [빠른 시작 CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -Apple의 CloudKit 소개 합니다.
- [CloudKit JS 참조](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -Apple의 CloudKit JS 설명서입니다.
- [웹 서비스 참조 CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -CloudKit HTTP 인터페이스를 설명 하는 Apple의 참조 합니다.
- [CloudKit 카탈로그의 경우: 소개 (Cocoa 및 JavaScript) CloudKit](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -CloudKit 및 CloudKit JS를 사용 하 여 Apple의 샘플 응용 프로그램입니다.

> [!IMPORTANT]
> Apple [도구 제공](https://developer.apple.com/support/allowing-users-to-manage-data/) 제대로 유럽 연합 일반 데이터 보호 규정 (GDPR)를 처리 하는 개발자가 수 있도록 합니다.

## <a name="foundation-framework-additions"></a>Foundation 프레임 워크 추가

Apple는 iOS 9에에서 다음과 같이 변경 Foundation 프레임 워크를 포함:

### <a name="changes-to-nsbundle"></a>NSBundle에 대 한 변경

다음과 같이 변경에 적용 된는 [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) iOS 9에 대 한 클래스:

* `GetPreservationPriorityForTag (NSString tag)` -지정 된 태그를 사용 하 여 리소스에 대 한 현재 보존 우선 순위를 가져옵니다. 유효한 값은 범위에 `0.0` 를 `1.0`, 가장 낮은 우선 순위를 사용 하 여 리소스를 먼저 제거 됩니다.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -지정 된 태그를 사용 하 여 리소스에 대 한 현재 보존 우선 순위를 설정합니다. 유효한 값은 범위에 `0.0` 를 `1.0`, 가장 낮은 우선 순위를 사용 하 여 리소스를 먼저 제거 됩니다.

자세한 내용은 Apple의를 참조 하십시오 [NSBundle 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)합니다.

### <a name="changes-to-nsprocessinfo"></a>NSProcessInfo에 대 한 변경

IOS 장치에서 실행 중인 각 프로세스에는 단일 _프로세스 정보 에이전트_ (PIA). 사용 하 여는 [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) PIA 및 제어 전력 및 지정된 된 프로세스에 대 한 열 관리 하는 방법에 대 한 정보를 제공 하는 클래스입니다.

예를 들어 프로세스의 자동 종료를 제어 하려면 다음 코드를 사용할 수 있습니다.

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

자세한 내용은 Apple의를 참조 하십시오 [NSProcessInfo 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)합니다.

### <a name="reacting-to-low-power-mode"></a>절전 모드에 응답

사용 하 여는 `LowPowerModeEnabled` 의 속성은 [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) 낮은 전원 모드 응용 프로그램에서 실행 되는 iOS 장치에서 설정 되어 있는지 확인 하려면 클래스입니다. 예를 들어:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit Framework 변경 내용

Apple의 다음 내용이 포함 된 [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) iOS 9에에서 프레임 워크:

- 대량 삭제 및 HealthKit 데이터베이스에 있는 항목의 삭제 추적 지원 합니다. Apple의 참조 [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) 및 [HKHealthStore 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) 자세한 정보에 대 한 합니다.
- 새 추적 범주 및 특성에 추가 된는 `HKQuantityTypeIdentifier` 클래스 (같은 `UVExposure`) 및는 `HKCategoryTypeIdentifier` 클래스 (같은 `OvulationTestResult`). Apple의 참조 [HealthKit 상수 참조](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) 자세한 정보에 대 한 합니다.

참조 하십시오 우리의 [HealthKit 소개](~/ios/platform/healthkit.md) HealthKit Xamarin.iOS에 사용한 작업에 대 한 자세한 내용은 설명서입니다.

## <a name="local-authentication-framework-changes"></a>로컬 인증 프레임 워크 변경

Apple의 다음 내용이 포함 된 [로컬 인증](https://developer.xamarin.com/api/namespace/LocalAuthentication/) iOS 9에에서 프레임 워크:

- 사용 하는 `EvaluateAccessControl` 및 `EvaluatePolicy` 의 메서드는 [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) 이전 성공 잠금 해제에서 Touch ID와 일치 하는 다시 사용할 수 있도록 시도 이제 수 클래스입니다.
- 현재 등록 된 손가락의 목록을 가져올 수 있습니다.
- 손가락 추가 또는 인증에서 제거 되 면 추적을 위한 지원 합니다.
- 사용 하는 기능은 _인증 컨텍스트_ 호출 키 집합 및 키 집합 액세스 제어를 평가 하는 것에 대 한 지원을 나열 합니다.
- 코드에서 사용자 프롬프트를 취소 하는 기능입니다.

참조 하십시오 우리의 [Touch ID 소개](~/ios/platform/touchid.md) Xamarin.iOS에 Touch ID 사용에 대 한 자세한 내용은 설명서입니다.

### <a name="lacontext-changes"></a>LAContext 변경 내용

다음과 같이 변경에 적용 된는 [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) iOS 9에 대 한 클래스:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -터치 ID 인증을 다시 사용할 수 있는 시간을 최대 크기를 반환 합니다.
- **EvaluatedPolicyDomainState** -가져옵니다 하거나 평가 정책의 상태를 설정 합니다.
- **MaxBiometryFailures** -iOS 9에에서 위해서 되었습니다.
- **TouchIdAuthenticationAllowableReuseDuration** 터치 ID 인증을 다시 사용할 수 있는 시간을 설정 하거나 가져옵니다.
- **EvaluateAccessControl** -비동기적으로 인증 정책을 평가 합니다.
- **무효화** -주어진된 터치 ID 인증을 무효화 합니다.
- **IsCredentialSet** -반환 `true` 자격 증명이 현재 설정 된 경우.
- **SetCredentialType** 지정 된 자격 증명 형식을 설정 합니다.

Apple의를 참조 하십시오 [LAContext 참조](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) 내용을 확인 합니다.

## <a name="mapkit-framework-changes"></a>MapKit Framework 변경 내용

Apple의 다음 내용이 포함 된 [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) iOS 9에에서 프레임 워크:

- 전송 directions에 직접 지도 응용 프로그램을 실행 하는 데 및 전송의 도착 (에타)를 사용 하 여 예상 시간을 쿼리 하기 위해 이제 MapKit 지원을 제공는 [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) 및 [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) 클래스입니다.
- MapKit에서 반환 된 검색 결과 및 [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) 클래스 결과 표준 시간대를 제공할 수도 있습니다.
- 이제 완전히 사용자 지정할 수 있습니다 지도 주석을 사용 하 여 iOS 앱에서 제공 되는 `DetailCalloutAccessoryView` 의 속성은 [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) 클래스입니다.

참조 하십시오 우리의 [iOS 지도](~/ios/user-interface/controls/ios-maps/index.md) 및 [연습-주석 및 MapKit 오버레이 탐색](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) 맵과 Xamarin.iOS 및 Apple의 에주석으로이루어진사용에대한자세한내용은설명서[CLGeocoder 참조](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) 자세한 정보에 대 한 합니다.

## <a name="passkit-framework-additions"></a>PassKit 프레임 워크 추가

Apple의 다음 내용이 포함 된 [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) iOS 9에에서 프레임 워크:

- 이제, Apple Pay 저장소 debit 및 Discover 카드 함께 신용 카드를 모두 지원합니다. 참조는 **지불 네트워크** Apple의 섹션 [PKPaymentRequest 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) 자세한 정보에 대 한 합니다.
- Xamarin.iOS 앱 내에서 직접 추가할 수 있습니다 이제 지불 네트워크와 카드 발급자에 Apple 비용을 지불 합니다. Apple의 참조 [PKAddPaymentPassViewController 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) 내용을 확인 합니다.

참조 하십시오 우리의 [PassKit 소개](~/ios/platform/passkit.md) PassKit Xamarin.iOS에 사용한 작업에 대 한 자세한 내용은 설명서입니다.

## <a name="safari-services-framework-additions"></a>Safari 서비스 프레임 워크 추가

Apple의 다음 내용이 포함 된 [Safari 서비스](https://developer.xamarin.com/api/namespace/SafariServices/) iOS 9에에서 프레임 워크:

- 이제 사용할 수 있습니다 새 [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) 클래스 Xamarin.iOS 앱 내에서 웹 콘텐츠를 표시 합니다. Safari 앱과 웹 사이트 데이터 및 쿠키를 공유 하는 기능을 제공 하 고 Safari의 기능 (예: 판독기 및 자동 채우기) 몇 개가 포함 되어 있습니다. [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) 기능은 **수행** 가 웹 콘텐츠를 보면 완료 되 면 사용자가 앱에 반환 하는 단추입니다.

때문에 [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) 웹 콘텐츠의 단일 페이지를 표시 하기 위한 맞게 조정 된 클래스을 바꾸는 데 사용 하는 것이 좋습니다 [WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/) 또는 [UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)기존 Xamarin.iOS 앱 내에서 컨트롤입니다.

### <a name="displaying-a-website"></a>웹 사이트를 표시합니다.

아래의 코드는 호출의 예는 [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) 에서 다른 뷰-컨트롤러 내:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit Framework 변경 내용

Apple의 여러 요소에는 많은 향상 된 기능에 포함 된 [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) iOS 9에 대 한 프레임 워크입니다. 다음 섹션에서는 변경 된 내용을 자세히 설명 합니다.

### <a name="3d-touch-events"></a>3D 터치 이벤트

IOS 9 iPhone 6s와 iPhone 6s 새 또한 3D 터치 iOS 앱에가 중 중요 한 제스처를 추가 합니다. 결과적으로, 앱이 iOS 9 (또는 그 이상)에서 실행 하는 경우 iOS 장치는 지원 3D 터치 수 압력의을 변경 하면는 `TouchesMoved` 이벤트를 발생 합니다. 

동작에이 변경으로 인해 iOS 앱 준비 되어야 합니다는 `TouchesMoved` 이벤트를 더 자주 호출 될 경우에 X / Y 좌표는 변경 되지 않았습니다. 

자세한 내용은 참조 하십시오 우리의 [3D 터치 소개](~/ios/platform/3d-touch.md) 가이드입니다.

### <a name="document-open-in-place-functionality"></a>문서 열기-내부 기능

사용 하 여는 `FinishedLaunching (application, launchOptions)` 또는 `WillFinishLaunching (Application, launchOptions)` 의 메서드는 [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) 클래스 이제 문서를 열 하 고 복사본에 대해 작업) (대비 위치에서 수정할 수 있습니다.

새 오픈 내부 기능을 지원 하려면 추가 `LSSupportsOpeningDocumentsInPlace` Xamarin.iOS 앱에 키 **Info.plist** 의 값을 갖는 파일 `YES`합니다.

Apple의를 참조 하십시오 [UIApplicationDelegate 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) 내용을 확인 합니다.

### <a name="enhanced-touch-events"></a>향상 된 터치 이벤트

Apple는 iOS 9에서에서 터치 이벤트에 여러 개선 된 기능을 제공 했습니다. 여기에 터치 예측을 사용 하 고 중간 터치 디스플레이 새로 고치는 간격에 대 한 액세스 권한을 포함 합니다.

Apple의를 참조 하십시오 [iOS에 대 한 이벤트를 처리 가이드](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) 내용을 확인 합니다.

### <a name="fetching-tailored-content"></a>사용자 지정 된 콘텐츠를 인출합니다.

새 `NSDataAsset` 클래스를 사용 하면 메모리 및 iOS 장치에서 현재 실행의 그래픽 기능에 맞게 조정 된 내용을 가져올 Xamarin.iOS 앱입니다.

### <a name="new-layout-anchors"></a>새로운 레이아웃 앵커

새 `NSLayoutAnchor` 및 `NSLayoutDimension` 레이아웃 앵커 클래스의 새 앵커 속성으로 작업은 [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) 클래스 (같은 `LeadingAnchor` 및 `WidthAnchor`) 레이아웃을 iOS 9에에서 쉽게 수행할 수 있도록 합니다.

참조 하십시오 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) Xamarin.iOS 앱과 Apple에 자동 레이아웃 및 크기 클래스 사용에 대 한 자세한 내용은 설명서 [NSLayoutAnchor 참조](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ NSLayoutDimension 참조](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) 및 [UIView 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) 자세한 정보에 대 한 합니다.

### <a name="new-readable-content-margins"></a>읽을 수 있는 새 콘텐츠 여백

새 `UILayoutGuide` 읽을 수 있는 내용 여백을 제공 하는 뷰 내에서 콘텐츠에 대 한 그리기 영역을 정의 클래스를 사용할 수 있습니다. Apple의 참조 [UILayoutGuide 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) 자세한 정보에 대 한 합니다.

### <a name="text-input-in-notifications-modifications"></a>알림 수정에 텍스트 입력

[UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/) 클래스에 있는 새 `Behavior` 알림에서 텍스트 입력을 지 원하는 데 사용할 수 있는 속성입니다.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate 변경 내용

에 대 한 모든 호출을 대체를 제안 하지 공식적으로 Apple에 의해 사용 되지 않는 동안는 `FinishedLaunching (UIApplication application)` 의 메서드는 [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) 중 하나를 사용 하 여 클래스는 `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` 또는 `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` 메서드.

Apple의를 참조 하십시오 [UIApplicationDelegate 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) 내용을 확인 합니다.

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics 변경 내용

Apple는 iOS 9에에서 UIKit 동적 특성을 다음과 같이 변경을 포함 합니다.

- 이제 dynamics 경계 사각형이 아닌 충돌 지원을 제공합니다.
- 새 사용자 지정 가능한 `UIFieldBehavior` 클래스는 다양 한 필드 유형을 지 원하는 데 사용 합니다.
- 추가 첨부 파일 형식에 추가 된는 `UIAttachmentBehavior` 클래스입니다.

Apple의를 참조 하십시오 [UIAttachment 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) 내용을 확인 합니다.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView 및 UIDatePicker 변경 내용

IOS 9 하기 전에 [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) 및 [UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/) 컨트롤 크기 조정이 불가능 했으며 자동으로 크기를 조정할 수 (일반적으로 너비 iOS 장치에서 앱을 해당 컨테이너의 너비를 채우도록 실행 중).

IOS 9에에서 더 이상 발생이 자동 크기 조정 하 고 컨트롤 화면 크기와 방향에 관계 없이 모든 iOS 장치에서 320 요소 너비에 렌더링 됩니다.

이 문제를 해결 하려면 컨트롤을 부모 컨테이너 (뷰)의 가장자리의 너비의 고정을 필요한 높이 지정할 자동 레이아웃 및 크기 클래스를 사용 합니다. 참조 하십시오 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) Xamarin.iOS 앱에서 자동 레이아웃 및 크기 클래스 사용에 대 한 자세한 내용은 설명서입니다.

### <a name="new-uitextinputassistantitem-class"></a>새 UITextInputAssistantItem 클래스

새로운 `UITextInputAssistantItem` 레이아웃 표시줄 단추 그룹에 클래스는 _바로 가기 모음_합니다. 도구 모음에는 입력 바로 가기를 제공 하는 소프트 키보드에서 사용할 수 있는 새 영역입니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0에 새로 이란](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
