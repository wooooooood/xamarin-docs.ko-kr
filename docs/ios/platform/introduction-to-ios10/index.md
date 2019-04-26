---
title: iOS 10 소개
description: 이 문서에서는 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 10에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: b018fe343a7d46f1323119b03a22cc3831a02d9f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61402405"
---
# <a name="introduction-to-ios-10"></a>iOS 10 소개

_이 문서에서는 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 10에서에서 사용할 수 있는 기능을 소개합니다._

## <a name="introducing-ios-10"></a>IOS 10 소개

새 iOS 10 SDK, Apple 새 Api 및 개발자가 앱 및 기능의 새 범주를 만들 수 있도록 하는 서비스 포함 했습니다. 이제 iOS 앱 이전에 사용할 수 없는 최종 사용자에 게 풍부 하 고 매력적인 기능을 제공 하도록 메시지, Siri, Phone 및 맵 앱을 확장할 수 있습니다.

IOS 10에 대 한 자세한 내용은 Apple의를 참조 하세요 [iOS 앱 +](https://developer.apple.com/ios/) 설명서.

## <a name="whats-new-in-ios-10"></a>IOS 10에서 새 란

Apple이 Api 및 서비스에 대 한 몇 가지 새 기존 기능을 비롯 한 향상 된 기능와 함께 iOS 10에에서 추가 합니다.


## <a name="adapting-to-the-true-tone-display"></a>True 성조 표시에 적응할 수

Apple의 True 성조 표시 기술은 색 및 현재 조명 조건과 일치 하는 디스플레이의 강도 동적으로 조정 하려면 iOS 장치에서 주변 광원 센서를 사용 합니다. 새 iOS 10 제공 [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) 앱을 추가할 수 있는 키 `Info.plist` 파일과 True 톤 표준 색 shift를 적용 하는 방법을 제어 합니다. 

다음 값을 사용할 수 있습니다.

- `UIWhitePointAdaptivityStyleStandard` **기본** -표준 흰색 지점 하면을 사용 합니다.
- `UIWhitePointAdaptivityStyleReading` -읽기 중심 앱에 대 한 사용 됩니다.
- `UIWhitePointAdaptivityStyleGame` -게임 중심 앱에 대 한 사용 됩니다.
- `UIWhitePointAdaptivityStyleVideo` -비디오 중심 앱에 대 한 사용 됩니다.
- `UIWhitePointAdaptivityStylePhoto` 사용 되는 사진 중심 앱에 대 한 색상 충실도 떨어집니다 환경 흰색 지점 조정 보다 더 중요 한 인 합니다.

<a name="app-extensions" />

## <a name="app-extensions"></a>앱 확장

Apple는 iOS 10에서에서 몇 가지 새 앱 확장 점을 제공 했습니다.

- 디렉터리를 호출 합니다.
- 인 텐트와 인 텐트 UI
- 메시지
- 알림 콘텐츠
- Notification Services
- 스티커 팩

또한 타사 키보드 앱 확장에는 다음과 같은 향상 된 기능:

- 새 `DocumentInputMode` 의 속성을 `UITextDocumentProxy` 클래스가 문서의 입력된 언어를 확인할 수 있으며 해당 언어에 맞게 키보드 확장을 허용 합니다.
- 새 `HandleInputModeList` 메서드를 사용 하면 키보드 확장 탭 되 고 전 세계 키에 대 한 응답으로 시스템의 키보드 선택 메뉴를 표시 합니다.

자세한 내용은 참조 하십시오 우리의 [확장 프로그램 소개](~/ios/platform/extensions.md), [메시지 앱 통합](~/ios/platform/message-app-integration/index.md)를 [소개 자동 제안](~/ios/platform/search/proactive-suggestions.md), [ SiriKit 소개](~/ios/platform/sirikit/index.md)하십시오 [사용자 알림 소개](~/ios/platform/user-notifications/index.md) 과 Apple [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)합니다.

## <a name="app-search-enhancements"></a>앱 검색 기능 향상

IOS 10에서에서 핵심 스포트라이트를 같은 앱 검색에 몇 가지 향상 된 기능 제공:

- **(사용 하 여 차등 개인) 라우드 딥 링크 인기도** -검색 결과에서 앱 딥 링크 된 콘텐츠를 홍보 하는 방법을 제공 합니다.
- **앱에서 검색** -새 `CSSearchQuery` 클래스를 설명 하는 메일, 메시지 및 메모 앱의 작동 방식을 유사한 앱 스포트라이트 검색 기능을 제공 합니다.
- **연속 검색** -추천 또는 Safari에서 검색을 시작 하 고 앱을 열 수 있도록 하 고 검색을 계속 합니다.
- **유효성 검사 결과의 시각화** -Apple [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) preforming 테스트 하는 경우 이제 웹 사이트의 태그 및 딥 링크에 대 한 시각적 표현을 표시 됩니다.
- **앱 이미지 공유 메시지** -스포트라이트 검색에서 표시할 메시지 (메시지 앱 확장을)를 통해 공유에 대해 제공 된 인기 있는 앱에서 이미지를 허용 합니다.

자세한 내용을 참조 하세요 우리의 [앱 검색 기능 향상](~/ios/platform/search/app-search-enhancements.md) 가이드입니다.

## <a name="apple-pay-enhancements"></a>Apple Pay 향상 된 기능

Apple 웹 사이트에서 Siri 및 맵 상호 작용을 통해 보안 지불 하는 사용자를 허용 하는 iOS 10에서에서 Apple Pay에 몇 가지 향상 된 기능을 했습니다.

IOS 10 사용 하 여 몇 가지 새로운 Api가 사용 하 여 iOS 및 watchOS 동적 결제 네트워크 및 새 샌드박스 테스트 환경을 지원 하기 위해 추가 되었습니다.

PassKit framework 외부에서 Apple Pay를 지원 하도록 확장 되었습니다 또한 `UIKit` 하 고 자신의 앱 내에서 해당 카드를 제공 하는 카드 발급자를 허용 합니다.

자세한 내용을 참조 하세요 우리의 [Apple 지불 향상](~/ios/platform/apple-pay.md) 가이드입니다.

## <a name="alternate-app-icons"></a>대체 앱 아이콘

Apple이 앱을 해당 아이콘을 관리 하는 iOS 10.3 몇 가지 향상 된 기능 추가.

 - `ApplicationIconBadgeNumber` -앱 아이콘 배지를 Springboard에서 설정 하거나 가져옵니다.
 - `SupportsAlternateIcons` - `true` 앱 아이콘 집합을 대체 했습니다.
 - `AlternateIconName` -현재 선택한 대체 아이콘의 이름을 반환 합니다. 또는 `null` 기본 아이콘을 사용 하는 경우.
 - `SetAlternameIconName` -지정 된 대체 아이콘 앱 아이콘을 전환 하려면이 메서드를 사용 합니다.

자세한 내용을 참조 하세요 우리의 [대체 앱 아이콘](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) 가이드입니다.


## <a name="introduction-to-callkit"></a>CallKit 소개

IOS 10에서에서 새 CallKit API와 통합 하는 iPhone UI 사용 하 여 친숙 한 인터페이스를 제공 합니다. 최종 사용자 환경을 앱이 VOIP 제공 합니다. 이 API를 사용 하 여 사용자 수 보기 iOS 장치의 잠금 화면에서 VOIP 호출을 사용 하 여 상호 작용 및 Phone 앱을 사용 하 여 연락처를 관리할 **즐겨찾기** 하 고 **최근** 뷰.

또한 CallKit API 이름 (호출자 ID)를 사용 하 여 전화 번호를 연결 하거나 여야 하는 경우 시스템 차단 (차단 호출)를 알릴 수 있는 앱 확장을 만들 수가 있습니다.

자세한 내용을 참조 하세요 우리의 [Callkit 소개](~/ios/platform/callkit.md) 가이드입니다.

## <a name="message-app-integration"></a>메시지 앱 통합

iOS 10과 통합 된 Xamarin.iOS 솔루션에서 메시지 앱 확장을 포함을 허용 합니다 **메시지** 사용자에 게 새로운 기능을 앱 및 표시 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다. 두 유형의 메시지 앱 확장을 사용할 수 있습니다.

- **스티커 팩** -스티커 사용자 메시지를 추가할 수 있는 컬렉션을 포함 합니다. 코드 작성 없이 스티커 팩을 만들 수 있습니다.
- **iMessage 앱** -스티커를 선택 하면, 텍스트를 입력, 선택적 형식 변환) (로 미디어 파일을 포함 하 여 및 만들기, 편집 및 상호 작용 메시지를 보내기에 대 한 메시지 앱 내에서 사용자 지정 사용자 인터페이스를 제공할 수 있습니다.

자세한 내용을 참조 하세요 우리의 [메시지 앱 통합](~/ios/platform/message-app-integration/index.md) 가이드입니다.

## <a name="news-publisher-enhancements"></a>뉴스 게시자 향상 된 기능

IOS 10 사용 하 여 Apple 주요 잡지 및 블로거, 독립 게시자 등록 및 제품에 새 조직에서 허용 되며 Apple 뉴스 앱에 콘텐츠를 제공 합니다. 자세한 내용은 Apple의를 참조 하세요 [뉴스 리소스](https://newsresources.apple.com/) 설명서.

## <a name="providing-haptic-feedback"></a>햅틱 피드백 제공

IPhone 7 및 iPhone의 7 및 Apple가 실제로 사용자를 참여 하는 다른 방법을 제공 하는 새 haptics 응답에 포함 합니다. 사용자의 주의 가져오고 해당 작업을 보강 새 tactile 피드백 옵션을 사용 합니다.

여러 기본 제공 UI 요소는 이미 선택, 스위치 및 슬라이더와 같은 햅 틱 피드백을 제공합니다. iOS 10 이제 haptics의 구체적 하위 클래스를 사용 하 여 프로그래밍 방식으로 실행 하는 기능을 추가 합니다 `UIFeedbackGenerator` 클래스입니다.

자세한 내용을 참조 하세요 우리의 [햅 틱 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md) 가이드입니다.

## <a name="proactive-suggestions"></a>자동 제안

iOS 10 시스템 사전에 존재 하는 유용한 정보를 자동으로 사용자에 게 적절 한 시간을 허용 하 여 주행 engagement 앱에는 새로운 방법을 제공 합니다. IOS와 마찬가지로 9 앱 내의 다음 위치에서 시스템에서 사용자에 게 표시할 수 있는 기능을 노출할 수 있습니다 하는 iOS 10 사용 하 여 추천, 전달 및 Siri 제안을 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 합니다.

- 응용 프로그램 전환기
- 잠금 화면
- CarPlay
- 맵
- Siri 상호 작용
- QuickType 제안 

앱과 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출 [NSUserActivity](xref:Foundation.NSUserActivity), 핵심 스포트라이트, MapKit, Media Player 및 UIKit 웹 태그입니다.

자세한 내용을 참조 하세요 우리의 [자동 제안 소개](~/ios/platform/search/proactive-suggestions.md) 가이드입니다.

## <a name="request-app-review"></a>앱 검토 요청

Ios 10.3, 새를 `RequestReview()` 메서드를 사용 하면 iOS 앱이 사용자 속도 또는 검토 하도록 요청 합니다. 언제 든 지는 것이 합리적 사용자 환경에이 메서드를 호출할 수 있습니다, 검토 프로세스를 제어 하 고 앱 스토어 정책에 의해 처리 되어 합니다. 결과적으로,이 될 수 있습니다 또는 경고가 표시 되지 메서드와 단추를 탭 하는 같은 사용자 작업에 대 한 응답으로 호출 되 면 안 합니다.

자세한 내용을 참조 하세요 우리의 [앱 검토 요청](~/ios/platform/request-app-review.md) 가이드입니다.

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상

Apple은 개발자가 앱의 보안을 강화 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 iOS 10에서에서 개인 정보 보호와 보안에 몇 가지 향상 되었습니다.

IOS 10 이상에서 실행 되는 앱에서 하나 이상의 개인 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하려면 의도 해야 정적으로 선언 하는 결과적으로, 해당 `Info.plist` 앱에 액세스 하려는 이유는 사용자에 게 설명 하는 파일입니다.

자세한 내용을 참조 하세요 우리의 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 가이드입니다.

## <a name="sirikit"></a>SiriKit

새 ios 10, SiriKit Xamarin.iOS 앱을 iOS 장치에서 Siri를 사용 하 여 사용자가 액세스할 수 있는 서비스를 제공할 수 있습니다. 이 기능은 새를 사용 하 여 하나 이상의 앱 확장에서 제공 됩니다 **의도** 하 고 **인 텐트 UI** 프레임 워크입니다.

SiriKit 다음 서비스 도메인을 지원합니다.

- 오디오 또는 비디오 호출 합니다.
- 승객을 예약 합니다.
- 달리기를 관리 합니다.
- 메시징입니다.
- 사진을 검색 합니다.
- 지불을 주고받지 설정 합니다.

SiriKit 보냅니다 확장 사용자 요청 하면 Siri의 관련 앱 확장의 서비스 중 하나에 **의도** 지 원하는 모든 데이터와 함께 사용자의 요청을 설명 하는 개체입니다. 앱 확장 한 다음 적절 한 생성 **응답** 개체를 지정 **의도**, 확장 요청을 처리할 수 있는 방법을 자세히 설명 합니다.

Siri에서 일반적으로 모든 사용자 상호 작용을 처리 하는 동안 앱 확장을 사용할 수는 **의도 UI** 를 풍부 하 고 사용자 지정 사용자 인터페이스 특징으로 앱의 브랜딩 표시 하기 위해 프레임 워크 및 추가 정보.

자세한 내용을 참조 하세요 우리의 [SiriKit 소개](~/ios/platform/sirikit/index.md) 가이드입니다.

## <a name="speech-recognition"></a>음성 인식

iOS 10 텍스트로 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)를에서 기록 하도록 앱을 허용 하는 새 Speech API를 포함 합니다.

음성 인식 전송 및 앱을 Apple 서버에서 데이터의 임시 저장소를 필요로 하기 때문에 _해야 합니다_ 포함 하 여 인식을 수행할 수 있는 사용자의 권한을 요청 합니다 `NSSpeechRecognitionUsageDescription` 키에 해당 `Info.plist` 파일 및 호출 된 `SFSpeechRecognizer.RequestAutorization` 메서드.

자세한 내용을 참조 하세요 우리의 [음성 인식 소개](~/ios/platform/speech.md) 가이드입니다.

## <a name="user-notifications"></a>사용자 알림

새 ios 10 프레임 워크를 제공 하 고 로컬 및 원격 알림이 처리에 대 한 허용 사용자 알림입니다. 앱 또는 앱 확장은이 프레임 워크를 사용 하 여, 위치와 같은 조건 집합 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

또한 앱 또는 확장 수 수신 (및 잠재적으로 수정할) 로컬 및 원격 알림이 사용자의 iOS 장치에 배달 하는 대로 합니다.

새 사용자 알림 UI 프레임 워크에는 앱 또는 앱 확장 사용자에 게 표시 될 때 로컬 및 원격 알림의 모양을 사용자 지정할 수 있습니다.

자세한 내용을 참조 하세요 우리의 [사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md) 가이드입니다.

## <a name="video-subscriber-account"></a>비디오 구독자 계정

새 iOS 10에 대 한 비디오 구독자 계정 프레임 워크 앱 허용 스트리밍 인증 지원 또는 주문형 비디오는 단일 로그인 환경을 사용 하 여 최종 사용자에 게 해당 케이블 또는 위성 TV 공급자를 사용 하 여 인증 하는 합니다.

## <a name="wide-color"></a>와이드 컬러

iOS 10 확장 범위 픽셀 형식 및 AVFoundation 핵심 그래픽, Core 이미지, 금속 등 프레임 워크를 비롯 한 시스템 전체에서 광범위 한 색상 공간에 대 한 지원도 확장 되었습니다. 와이드 컬러 디스플레이 사용 하 여 장치에 대 한 지원은 전체 그래픽 스택 전체에서이 동작을 제공 하 여 용이 추가 됩니다.

또한 [UIKit](xref:UIKit) 수정 된 새 작업을 확장 **sRGB** 색상 공간을 간편 하 게 중요 한 성능 손실 없이 광범위 한 색 색상의 색을 혼합 합니다.

Apple에서는 다양 한 색을 사용 하 여 작업 하는 경우 다음 모범 사례를 제공 합니다.

- [UIColor](xref:UIKit.UIColor) 사용 하는 sRGB 색 공간을 더 이상 고정 값을 이제는 `0.0` 에 `1.0` 범위입니다. 앱을 이전 하는 제한 동작에 의존 하는 경우 iOS 10에 대 한 수정 해야 합니다.
- 사용자 지정을 수행할 때 sRGB 색 공간에 대 한 그리기 환경 구성 됩니다 `UIView` iPad Pro 토대로 합니다.
- 앱의 사용자 지정 렌더링을 수행 하는 경우 `UIImages`를 사용 하 여 새 [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) 확장 범위 또는 표준 범위 형식의 사용을 지정 하는 클래스입니다.
- 핵심 그래픽 또는 체제 미 설치 컴퓨터와 같은 하위 수준 API를 사용 하 여 이미지 처리를 개발자는 확장된 범위 색 공간과 픽셀 지 원하는 형식으로 16 비트 부동 소수점 값을 사용 해야 합니다. 필요한 경우 개발자는 수동으로 색 구성 요소 값을 고정 해야 합니다.
- 핵심 그래픽, Core 이미지 및 체제 미 설치 컴퓨터 성능 셰이더 모든 두 색 공간 변환 하기 위한 새 메서드를 제공 합니다.

자세한 내용을 참조 하세요 우리의 [와이드 컬러 소개](~/ios/platform/wide-color.md) 가이드입니다.

## <a name="widget-enhancements"></a>위젯 기능

Apple 위젯 시스템을 확인 하는 위젯 10 잠금 화면에서 새 iOS 존재 하는 백그라운드에서 훌륭해 여러 개선 된 기능을 도입 되었습니다. 합니다 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) 속성이 사용 되지 않으며 새 바뀌었습니다 [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) 또는 [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) 속성입니다. 또한 위젯을 포함 이제는 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 속성을 사용 하면 얼마나 많은 콘텐츠를 사용할 수를 설명 하기 위해 개발자 및 확장 하 고 콘텐츠를 축소 하면입니다.

자세한 내용을 참조 하세요 우리의 [검색 및 홈 화면 위젯 기능](~/ios/platform/search/widgets.md) 가이드입니다.

## <a name="additional-framework-changes"></a>프레임 워크 추가 변경 내용

주요 프레임 워크 변경 내용 및 위에 나열 된 추가 외에 Apple iOS 10에서에서 많은 사소한 프레임 워크 추가 변경을 했습니다.

자세한 내용을 참조 하세요 우리의 [추가 프레임 워크 변경 내용](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) 가이드입니다.

## <a name="deprecated-apis"></a>사용되지 않는 API

IOS 10에서에서 다음 Api가 사용 되지 않습니다.

- `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` 고 `CKFetchRecordChangesOperation` 클래스에서에서 사용 되지 CloudKit iOS 10에 대 한 합니다. 사용 된 [CKDiscoverAllUserIdentitiesOperation](xref:CloudKit.CKDiscoverUserIdentitiesOperation), [CKUserIdentity](xref:CloudKit.CKUserIdentity) 및 [CKFetchRecordZoneChangesOperation](xref:CloudKit.CKFetchRecordZoneChangesOperation) 클래스 (레코드 공유를 지) 대신 합니다.
- 몇 가지 [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) Api (예: 영역 및 쿼리 기반 구독)가 사용 되지 않습니다. 사용 된 [CKRecordZoneSubscription](xref:CloudKit.CKRecordZoneSubscription) 하 고 [CKQuerySubscription](xref:CloudKit.CKQuerySubscription) Api 대신 합니다.
- [NSPersistentStoreCoordnator](xref:CoreData.NSPersistentStoreCoordinator) 유비쿼터스 콘텐츠와 관련 된 기호 사용 되지 않습니다.
- `ADBannerView`를 `ADInterstitialAd` 과의 기호를 [UIViewController](xref:UIKit.UIViewController) 클래스 사용 되지 않습니다.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) 부동 소수점 값에 관련 된 기호 사용 되지 않습니다.
- `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`를 `UIUserNotificationCategory` 및 `UIUserNotificationSettings` UIKit의 클래스 사용 되지 않습니다. 사용 된 [사용자 알림](#user-notifications) framework 대신 합니다.
- 합니다 `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`를 `DidReceiveLocalNotification` 및 `DidReceiveRemoteNotification` WatchKit 메서드 사용 되지 않습니다. 사용 된 `HandleActionForNotification` 고 `DidReceiveNotification` 메서드 대신 합니다.
- `DidReceiveLocalNotification` 하 고 `DidReceiveRemoteNotification` 의 메서드는 [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) 사용 되지 않습니다. 인스턴스를 만듭니다 [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) 적절 한 메서드를 구현 하 고 할당 합니다 `Delegate` 의 속성을 [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) 개체.
- 합니다 **게임 센터 앱** 사용 되지 않으며 iOS에서 제거 되었습니다. 앱 GameKit를 사용 하는 경우 해당 _해야_ 순위표 등 GameKit 기능을 표시 하는 자체 인터페이스를 제공 합니다.

Apple의를 참조 하세요 [iOS 10.0 API 차이점을 iOS 9.3](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) 결함의 전체 목록에 대 한 설명서입니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
