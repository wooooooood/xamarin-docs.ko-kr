---
title: IOS 10 소개
description: 이 문서 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 10에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: c3bee0f15016394005a67e98cd8435e6d63b3ac6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30786477"
---
# <a name="introduction-to-ios-10"></a>IOS 10 소개

_이 문서 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 10에서에서 사용할 수 있는 기능을 소개합니다._

## <a name="introducing-ios-10"></a>IOS 10 소개

새 ios 10 SDK, 사과 새 Api와 개발자는 응용 프로그램 및 기능 새 범주를 만들 수 있도록 해 주는 서비스 포함 했습니다. 이제 iOS 앱 이전에 제공 된 최종 사용자에 게 풍부한 고 흥미로운 기능을 제공 하는 메시지, Siri, 전화 번호, 지도 응용 프로그램을 확장할 수 있습니다.

IOS 10에 대 한 자세한 내용은 Apple의를 참조 하십시오 [iOS 앱 +](https://developer.apple.com/ios/) 설명서입니다.

## <a name="whats-new-in-ios-10"></a>IOS 10에서 새로운 이란

Apple이 몇 가지 새로운 Api 및 서비스를 비롯 한 기존 기능에는 많은 향상 된 구성 요소가 iOS 10에에서 추가:


## <a name="adapting-to-the-true-tone-display"></a>True 톤 디스플레이에 맞게 조정

Apple의 톤 표시 기술은 색 및 현재 조명 조건에 맞게 디스플레이의 강도 동적으로 조정 하는 iOS 장치에는 앰비언트 센서를 사용 합니다. iOS 10 제공 새 [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) 응용 프로그램에 추가할 수 있는 키 `Info.plist` 파일 및 True 톤 표준 색 shift를 적용 하는 방법을 제어 합니다. 

다음 값을 사용할 수 있습니다.

- `UIWhitePointAdaptivityStyleStandard` **기본** -표준 흰색 지점 하면을 사용 합니다.
- `UIWhitePointAdaptivityStyleReading` 읽기 중심 응용 프로그램에 사용 되는 합니다.
- `UIWhitePointAdaptivityStyleGame` 게임에 중점을 둔 응용 프로그램에 사용 되는 합니다.
- `UIWhitePointAdaptivityStyleVideo` 비디오에 중점을 둔 응용 프로그램에 사용 되는 합니다.
- `UIWhitePointAdaptivityStylePhoto` 사용 되는 사진에 중점을 둔 응용 프로그램에 색상 충실도 환경 흰색 포인트 조정 보다 더 중요 합니다.

<a name="app-extensions" />

## <a name="app-extensions"></a>응용 프로그램 확장

Apple는 iOS 10의에서 몇 가지 새 응용 프로그램 확장 지점 제공 했습니다.

- 디렉터리를 호출 합니다.
- 의도 및 의도 UI
- 메시지
- 알림 콘텐츠
- Notification Services
- 스티커 팩

또한 타사 바로 응용 프로그램 확장은 다음과 같은 기능 향상:

- 새 `DocumentInputMode` 의 속성은 `UITextDocumentProxy` 클래스는 문서의 입력된 언어를 확인 하 고 해당 언어에 맞춰 키보드 확장명 허용 합니다.
- 새 `HandleInputModeList` 메서드를 사용 하면 키보드 확장 탭 되 고 구형 키에 대 한 응답으로 시스템의 키보드 선택 메뉴를 표시 합니다.

자세한 내용은 참조 하십시오 우리의 [확장명 소개](~/ios/platform/extensions.md), [메시지 응용 프로그램 통합](~/ios/platform/message-app-integration/index.md), [사전 제안 소개](~/ios/platform/search/proactive-suggestions.md), [ SiriKit 소개](~/ios/platform/sirikit/index.md), [사용자 알림 소개](~/ios/platform/user-notifications/index.md) 과 Apple [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)합니다.

## <a name="app-search-enhancements"></a>앱 검색의 향상 된 기능

IOS 10에서에서 핵심 스포트라이트 여러 개선 된 기능 앱 검색와 같은 제공:

- **(차등 개인 정보)를 Crowdsourced 딥 링크 인기도** -검색 결과의 앱 딥 링크 된 콘텐츠를 승격 하는 방법을 제공 합니다.
- **앱에서 검색** -새로운 `CSSearchQuery` 메일, 메시지 및 메모 앱 작동 방법을 비슷합니다 앱에서 스포트라이트 검색 기능을 제공 하는 클래스입니다.
- **연속 검색** -스포트라이트 또는 Safari, 검색을 시작한 다음 앱을 열 수 있도록 허용 하 고 해당 검색을 계속 합니다.
- **유효성 검사 결과의 시각화** -Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) preforming 테스트 하는 경우 이제 웹 사이트의 태그와 딥 링크 시각적 표시 됩니다.
- **응용 프로그램 이미지 공유 메시지** -스포트라이트 검색에서 표시할 메시지 (메시지 응용 프로그램 확장)를 통해 공유 하는 데 제공 하는 인기 있는 앱에서 이미지를 허용 합니다.

자세한 내용은 참조 하세요 우리의 [앱 검색 향상](~/ios/platform/search/app-search-enhancements.md) 가이드입니다.

## <a name="apple-pay-enhancements"></a>Apple Pay 향상 된 기능

Apple 웹 사이트에서 및 Siri 및 지도와 상호 작용을 통해 보안 상환를 사용할 수 있는 iOS 10에서에서 Apple 지불을 위해 여러 개선 된 기능을 했습니다.

10 ios 몇 가지 새로운 Api가 iOS 및 watchOS 동적 지불 네트워크와 새 샌드박스 테스트 환경을 지원 하기 위해 둘 다에서 작동 하는 추가 되었습니다.

외부의 Apple Pay 지원 하도록 확장 되었습니다 PassKit 프레임 워크 또한 `UIKit` 앱 내에서 카드를 표시 하도록 카드 발급자를 허용 하 고 있습니다.

자세한 내용은 참조 하세요 우리의 [Apple 지불 향상 된 기능](~/ios/platform/apple-pay.md) 가이드입니다.

## <a name="alternate-app-icons"></a>대체 앱 아이콘

Apple이 iOS 10.3 해당 아이콘을 관리 하는 응용 프로그램을 사용할 수 있는 몇 가지 향상 된 기능 추가.

 - `ApplicationIconBadgeNumber` -응용 프로그램 아이콘의 배지는 Springboard에서 설정 하거나 가져옵니다.
 - `SupportsAlternateIcons` -If `true` 응용 프로그램에 다른 아이콘 집합입니다.
 - `AlternateIconName` -현재 선택 된 대체 아이콘의 이름이 반환 또는 `null` 기본 아이콘을 사용 하는 경우.
 - `SetAlternameIconName` -지정 된 대체 아이콘을 응용 프로그램의 아이콘을 전환 하려면이 메서드를 사용 합니다.

자세한 내용은 참조 하세요 우리의 [대체 앱 아이콘](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) 가이드입니다.


## <a name="introduction-to-callkit"></a>CallKit 소개

IOS 10에서에서 새 CallKit API VOIP 앱 iPhone UI와 통합 하 고 친숙 한 인터페이스를 제공 하 고 최종 사용자에 게 발생 하는 방법을 제공 합니다. 이 API를 사용 하 여 사용자가 볼 수 및 iOS 장치 잠금 화면에서 VOIP 전화와 상호 작용 고 관리할 전화 앱을 사용 하 여 연락처 **즐겨찾기** 및 **제외할지** 뷰.

또한 CallKit API 이름 (호출자 ID)와 전화 번호를 연결 하거나 숫자 해야 하는 경우 시스템 차단 (차단 호출)을 알 수 있는 앱 확장을 만드는 기능을 제공 합니다.

자세한 내용은 참조 하세요 우리의 [Callkit 소개](~/ios/platform/callkit.md) 가이드입니다.

## <a name="message-app-integration"></a>메시지 앱 통합

iOS 10와 통합 된 Xamarin.iOS 솔루션에서 메시지 앱 확장의 포함 여부를 사용 하면는 **메시지** 사용자에 게 새 기능을 응용 프로그램 및 표시 합니다. 확장은 텍스트 스티커, 미디어 파일, 대화형 메시지를 보낼 수 있습니다. 메시지 앱 확장의 두 종류 사용할 수 있습니다.

- **스티커 팩** -사용자는 메시지에 추가할 수 있는 스티커의 컬렉션을 포함 합니다. 코드를 작성 하지 않고 스티커 팩을 만들 수 있습니다.
- **응용 프로그램 iMessage** -스티커 선택, 텍스트를 입력, 미디어 파일 (선택 사항 형식 변환)를 포함 하 여 및 만들기, 편집 및 상호 작용 메시지를 보내는 메시지 앱 내에서 사용자 지정 사용자 인터페이스를 표시할 수 있습니다.

자세한 내용은 참조 하세요 우리의 [메시지 응용 프로그램 통합](~/ios/platform/message-app-integration/index.md) 가이드입니다.

## <a name="news-publisher-enhancements"></a>뉴스 게시자의 향상 된 기능

10 iOS와 Apple 주요 잡지 및 블로거 및 등록 하려면 독립 게시자 및 제품에 새 조직에서 모든 사용자 허용 되며 Apple 뉴스 응용 프로그램에 콘텐츠를 제공 합니다. 자세한 내용은 Apple의를 참조 하십시오 [뉴스 리소스](https://newsresources.apple.com/) 설명서입니다.

## <a name="providing-haptic-feedback"></a>Haptic 사용자 의견 제공

IPhone의 7 iPhone에 7 및, 사과 실제로 사용자 의견을 교환 하는 다른 방법을 제공 하는 새 haptics 응답 포함 합니다. 새 tactile 의견 옵션을 사용 하 여 사용자의 주의 가져오고 그 작업을 강화 하 합니다.

여러 기본 제공 UI 요소는 이미 선택, 스위치 및 슬라이더와 같은 haptic 피드백을 제공 합니다. iOS 10 haptics의 구체적인 서브 클래스를 사용 하 여 프로그래밍 방식으로 실행 하는 기능을 이제 추가 `UIFeedbackGenerator` 클래스입니다.

자세한 내용은 참조 하세요 우리의 [Haptic 사용자 의견 제공](~/ios/user-interface/ios-ui/haptic-feedback.md) 가이드입니다.

## <a name="proactive-suggestions"></a>자동 관리 제안

iOS 10 시스템에 사전 유용한 정보를 자동으로 사용자에 게 표시 적절 한 시간에 허용 하 여 새를 응용 프로그램 구동 engagement 방법에 설명 합니다. IOS와 마찬가지로 9 스포트라이트, 전달 및 Siri 제안 iOS 10 응용 프로그램은 다음 위치 내에서 시스템 사용자에 게 표시 될 수 있는 기능을 노출할 수를 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 합니다.

- 응용 프로그램 전환기
- 잠금 화면
- CarPlay
- 맵
- Siri 상호 작용
- QuickType 제안 

와 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출 하는 응용 프로그램 [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), 코어 스포트라이트, MapKit, 미디어 플레이어 및 UIKit 웹 태그입니다.

자세한 내용은 참조 하세요 우리의 [사전 제안 소개](~/ios/platform/search/proactive-suggestions.md) 가이드입니다.

## <a name="request-app-review"></a>응용 프로그램 검토를 요청

새 iOS 10.3에 `RequestReview()` 메서드를 사용 하면 iOS 앱을 평가 하거나 검토 하려면 사용자에 게 요청 합니다. 이 메서드는 것이 적합 사용자 환경에 언제 든 지 호출 수, 하는 동안에 검토 프로세스 제한 되며 앱 스토어 정책에 의해 처리 합니다. 결과적으로,이 메서드 수 있습니다 또는 경고 메시지가 표시 되지 않을 수 있습니다 및 단추를 누르는 것과 같이 사용자 작업에 대 한 응답으로 호출 되 면 안 합니다.

자세한 내용은 참조 하세요 우리의 [응용 프로그램 요청 검토](~/ios/platform/request-app-review.md) 가이드입니다.

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상 된 기능

Apple은 보안 및 개인 정보는 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 iOS 10에서에서 둘 다에 몇 가지 향상 되었습니다.

결과적으로, 10 또는 그 이상의 iOS에서 실행 되는 앱 해야 정적으로 선언할에 하나 이상의 개인 정보 보호 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 `Info.plist` 사용자 응용 프로그램에 액세스 하려는 이유를 설명 하는 파일입니다.

자세한 내용은 참조 하세요 우리의 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 가이드입니다.

## <a name="sirikit"></a>SiriKit

새로운 iOS 10 SiriKit iOS 장치에서 Siri를 사용 하는 사용자에 액세스할 수 있는 서비스를 제공 하도록 Xamarin.iOS 앱을 허용 합니다. 새를 사용 하 여 하나 이상의 응용 프로그램 확장에서이 기능을 제공 **의도** 및 **의도 UI** 프레임 워크입니다.

SiriKit 다음 서비스 도메인을 지원합니다.

- 오디오 또는 비디오 호출 합니다.
- 안에서 예약 합니다.
- 체력 단련을 관리 합니다.
- 메시징입니다.
- 사진을 검색 합니다.
- 송신 하거나 수신 지불 합니다.

사용자가 앱 확장의 서비스 중 하나는 관련 된 Siri의 요청, SiriKit 확장 보냅니다는 **의도** 지원 데이터와 함께 사용자의 요청을 설명 하는 개체입니다. 앱 확장 한 다음 적절 한 생성 **응답** 개체에 대 한는 주어진 **의도**, 확장에서 요청을 처리할 수는 방법에 대해 자세히 설명 합니다.

Siri에서 일반적으로 모든 사용자 상호 작용을 처리 하는 동안 앱 확장 사용할 수는 **의도 UI** 는 다양 하 고 사용자 지정 사용자 인터페이스 특징으로 응용 프로그램의 브랜딩 제공 하기 위해 프레임 워크 및 추가 정보입니다.

자세한 내용은 참조 하세요 우리의 [SiriKit 소개](~/ios/platform/sirikit/index.md) 가이드입니다.

## <a name="speech-recognition"></a>음성 인식

iOS 10 텍스트로 응용 프로그램에서 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 변환 하도록 하는 새 음성 API를 포함 합니다.

음성 인식 전송 및 Apple 서버, 응용 프로그램에 데이터의 임시 저장소를 필요로 하기 때문에 _해야_ 인식 포함 하 여 수행할 사용자의 액세스 권한을 요청할는 `NSSpeechRecognitionUsageDescription` 키에 해당 `Info.plist` 파일 및 호출 된 `SFSpeechRecognizer.RequestAutorization` 메서드.

자세한 내용은 참조 하세요 우리의 [음성 인식 소개](~/ios/platform/speech.md) 가이드입니다.

## <a name="user-notifications"></a>사용자 알림

10, 배달 및 로컬 및 원격 알림 처리를 위한 프레임 워크에서는 사용자 알림 iOS를 처음 사용 합니다. 앱 또는 앱 확장은이 프레임 워크를 사용 하 여 위치와 같은 조건 집합이 또는 시간을 지정 하 여 로컬 알림의 배달은 예약할 수 있습니다.

또한 응용 프로그램 또는 확장 받을 (하 고 잠재적으로 수정할 수) 알림을 로컬 및 원격 사용자의 iOS 장치에 배달 될 때는 합니다.

새 사용자 알림 UI 프레임 워크에는 응용 프로그램 또는 사용자에 게 제공 하는 경우에 로컬 및 원격 알림 표시를 사용자 지정에 대 한 응용 프로그램 확장 수 있습니다.

자세한 내용은 참조 하세요 우리의 [사용자 알림을 프레임 워크](~/ios/platform/user-notifications/index.md) 가이드입니다.

## <a name="video-subscriber-account"></a>비디오 구독자 계정

새로운 iOS 10에 대 한 비디오 구독자 계정 프레임 워크에서는 앱 해당 스트리밍 인증 지원 또는 주문형 비디오 케이블 또는 위성 TV 공급자에 게 최종 사용자에 대 한 단일 로그인 환경을 사용 하 여 인증할 수 있습니다.

## <a name="wide-color"></a>광범위 한 색

iOS 10 확장 범위 픽셀 형식 및 코어 그래픽, Core 이미지, 금속 및 AVFoundation 등의 프레임 워크를 비롯 하 여 시스템 전체에서 광범위 한 색상 공간에 대 한 지원을 확장 합니다. 전체 전체 그래픽 스택에서이 동작을 제공 하 여 광범위 한 색 표시를 사용 하 여 장치에 대 한 지원은 쉽게 할 추가 합니다.

또한 [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) 수정 된 새에서 작동 하도록 확장 **sRGB** 상당한 성능 손실 없이 색상 광범위 한 색 영역에서 색을 혼합를 더욱 쉽게 색상 공간입니다.

Apple에서는 와이드 색으로 작업 하는 경우 다음 모범 사례를 제공 합니다.

- [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/) 는 sRGB 색 공간을 더 이상 사용 하 여 값을 clamp 이제는 `0.0` 를 `1.0` 범위입니다. 응용 프로그램 이전 제한 동작에 의존 하는 경우 iOS 10에 맞게 수정 해야 합니다.
- 그리기 환경 사용자 지정을 수행할 때 sRGB 색 공간에 대 한 구성 될 `UIView` iPad Pro에서 그리기 합니다.
- 응용 프로그램의 사용자 지정 렌더링을 수행 하는 경우 `UIImages`, 새로운 [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) 클래스의 확장 된 범위 또는 범위 표준 형식 사용을 지정 합니다.
- 코어 그래픽, 금속 등의 하위 수준 API를 사용 하 여 제공 이미지 처리를 개발자는 확장된 범위가 색 공간 및 픽셀을 지 원하는 형식 16 비트 부동 소수점 값을 사용 해야 합니다. 필요한 경우 개발자는 색상 구성 요소 값을 수동으로 clamp 해야 합니다.
- 코어 그래픽, Core 이미지 및 금속 성능 셰이더 모두 두 개의 색 공간 형식 사이의 변환에 대 한 새로운 메서드를 제공 합니다.

자세한 내용은 참조 하세요 우리의 [광범위 한 색 소개](~/ios/platform/wide-color.md) 가이드입니다.

## <a name="widget-enhancements"></a>위젯 향상 된 기능

Apple에 위젯 10 잠금 화면 새 iOS에 있는 모든 백그라운드에서 잘 한지 확인 하기 위하여 위젯 시스템에 여러 개선 된 기능이 도입 되었습니다. [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) 속성이 사용 되지 않으며 새으로 대체 되었습니다 [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) 또는 [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) 속성입니다. 또한 위젯을 포함 이제는 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 얼마나 많은 콘텐츠를 사용할 수를 설명 하는 개발자 및 확장 하 고 콘텐츠를 축소할 수 있도록 하는 속성입니다.

자세한 내용은 참조 하세요 우리의 [검색 및 홈 화면 위젯 향상](~/ios/platform/search/widgets.md) 가이드입니다.

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경 내용

주요 framework 변경 내용 및 위에 나열 된 추가 기능 뿐만 아니라 Apple iOS 10에서에서 많은 추가 부 프레임 워크 변경을 했습니다.

자세한 내용은 참조 하세요 우리의 [추가 프레임 워크 변경](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) 가이드입니다.

## <a name="deprecated-apis"></a>사용되지 않는 API

다음 Api에서에서 사용 되지 iOS 10:

- `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` 및 `CKFetchRecordChangesOperation` 클래스에서에서 사용 되지 CloudKit iOS 10에 대 한 합니다. 사용 하 여는 [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/), [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/) 및 [CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/) 클래스 (레코드 공유 지원) 대신.
- 여러 [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) Api (예: 영역을 기반으로 하 고 쿼리 기반 구독) 사용 되지 않습니다. 사용 하 여 [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/) 및 [CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) Api 대신 합니다.
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) 유비쿼터스 콘텐츠와 관련 된 기호 사용 되지 않습니다.
- `ADBannerView``ADInterstitialAd` 및 관련의 기호는 [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) 클래스 사용 되지 않습니다.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) 부동 소수점 값에 관련 된 기호 사용 되지 않습니다.
- `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` 및 `UIUserNotificationSettings` UIKit의 클래스 사용 되지 않습니다. 사용 하 여 [사용자 알림을](#User-Notifications) 프레임 워크 대신 합니다.
- `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` 및 `DidReceiveRemoteNotification` WatchKit 메서드 사용 되지 않습니다. 사용 하 여 `HandleActionForNotification` 및 `DidReceiveNotification` 메서드 대신 합니다.
- `DidReceiveLocalNotification` 및 `DidReceiveRemoteNotification` 의 메서드는 [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) 사용 되지 않습니다. 인스턴스를 만들고 [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) 에 할당 하는 적절 한 메서드를 구현 하는 `Delegate` 속성의는 [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) 개체입니다.
- **게임 센터 앱** 사용 되지 않으며 iOS에서 제거 되었습니다. 응용 프로그램 GameKit를 사용 하는 경우 그 _해야_ 순위표 등과 같은 GameKit 기능을 표시 하는 자체 인터페이스를 제공 합니다.

Apple의 참조 [10.0 iOS API 차이점을 iOS 9.3](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) 결함의 전체 목록은 대 한 설명서입니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
