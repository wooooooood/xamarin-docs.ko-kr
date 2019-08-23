---
title: iOS 10 소개
description: 이 문서에서는 Xamarin.ios 개발자를 위한 iOS 10에서 사용할 수 있는 새로운 Api 및 수정 된 Api 및 기능을 소개 합니다.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: edc585364df2e0b2129135e7bf5977c33a77a6e0
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647419"
---
# <a name="introduction-to-ios-10"></a>iOS 10 소개

새로운 iOS 10 SDK를 사용 하는 Apple에는 개발자가 새로운 범주의 앱 및 기능을 만들 수 있도록 하는 새로운 Api 및 서비스가 포함 되었습니다. 이제 iOS 앱에서 메시지, Siri, 휴대폰 및 Maps 앱을 확장 하 여 이전에 사용할 수 없었던 최종 사용자에 게 풍부한 기능을 제공 하는 기능을 제공할 수 있습니다.

IOS 10에 대 한 자세한 내용은 Apple의 [ios + 앱](https://developer.apple.com/ios/) 설명서를 참조 하세요.

## <a name="whats-new-in-ios-10"></a>IOS 10의 새로운 기능

Apple은 다음을 비롯 한 기존 기능에 대 한 여러 향상 된 기능을 비롯 하 여 iOS 10에서 몇 가지 새로운 Api 및 서비스를 추가 했습니다

## <a name="adapting-to-the-true-tone-display"></a>트루 톤 표시에 적응

Apple의 트루 톤 표시 기술은 iOS 장치에서 주변 광원 센서를 사용 하 여 현재 조명 조건과 일치 하도록 디스플레이의 색 및 강도를 동적으로 조정 합니다. iOS 10은 앱의 `Info.plist` 파일에 추가할 수 있는 새 [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) 키를 제공 하 고, 참 톤이 표준 컬러 시프트를 적용 하는 방식을 제어 합니다. 

사용할 수 있는 값은 다음과 같습니다.

- `UIWhitePointAdaptivityStyleStandard`**Default** -표준 adaptivity를 사용 합니다.
- `UIWhitePointAdaptivityStyleReading`-읽기 중심 앱에 사용 됩니다.
- `UIWhitePointAdaptivityStyleGame`-게임 중심 앱에 사용 됩니다.
- `UIWhitePointAdaptivityStyleVideo`-비디오 중심 앱에 사용 됩니다.
- `UIWhitePointAdaptivityStylePhoto`-색 충실도가 환경 흰색 포인트 조정 보다 더 중요 한 사진 중심 앱에 사용 됩니다.

## <a name="app-extensions"></a>앱 확장

Apple은 iOS 10에서 몇 가지 새로운 앱 확장 지점이 제공 됩니다.

- 호출 디렉터리
- 의도 및 의도 UI
- 메시지
- 알림 콘텐츠
- Notification Services
- 스티커 팩

또한 타사 키보드 앱 확장에는 다음과 같은 향상 된 기능이 있습니다.

- 클래스의 새 `DocumentInputMode` 속성은 문서의 입력 언어를 결정 하 고 해당 언어에 맞게 키보드 확장을 허용할 수 있습니다. `UITextDocumentProxy`
- 새 `HandleInputModeList` 메서드를 사용 하면 키보드 확장에서 탭 할 지구본 키에 대 한 응답으로 시스템의 키보드 선택기 메뉴를 표시할 수 있습니다.

자세한 내용은 [확장 소개](~/ios/platform/extensions.md), [메시지 앱 통합](~/ios/platform/message-app-integration/index.md), [사전 권장 사항](~/ios/platform/search/proactive-suggestions.md)소개, [sirikit](~/ios/platform/sirikit/index.md)소개, [사용자 알림 소개](~/ios/platform/user-notifications/index.md) 및 Apple의 [소개를 참조 하세요. 앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## <a name="app-search-enhancements"></a>앱 검색 기능 향상

IOS 10의 핵심 스포트라이트는 다음과 같은 앱 검색에 대 한 몇 가지 향상 된 기능을 제공 합니다.

- **라우드 소싱 딥 링크 인기도 (차등 개인 정보 포함)** -검색 결과에서 딥 링크 된 앱 콘텐츠를 승격 하는 방법을 제공 합니다.
- **앱 내 검색** -새 `CSSearchQuery` 클래스를 사용 하 여 메일, 메시지 및 메모 앱이 작동 하는 방식과 유사한 앱 내 스포트라이트 검색 기능을 제공 합니다.
- **연속 검색** -사용자가 스포트라이트 또는 Safari에서 검색을 시작한 다음 앱을 열고 검색을 계속할 수 있습니다.
- **유효성 검사 결과 시각화** -Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) 는 이제 테스트를 미리 구성할 때 웹 사이트의 태그 및 딥 링크를 시각적으로 표시 합니다.
- **메시지 앱 이미지 공유** -메시지에서 공유할 수 있도록 제공 되는 인기 있는 앱 이미지 (메시지 앱 확장을 통해)는 스포트라이트 검색에 표시 됩니다.

자세히 알아보려면 [앱 검색 기능 향상](~/ios/platform/search/app-search-enhancements.md) 가이드를 참조 하세요.

## <a name="apple-pay-enhancements"></a>향상 된 Apple Pay

Apple은 사용자가 웹 사이트와 Siri 및 Maps의 상호 작용을 통해 보안 지불을 수행할 수 있도록 하는 iOS 10의 Apple Pay에 대 한 몇 가지 향상 된 기능을 만들었습니다.

IOS 10을 사용 하 여 동적 지불 네트워크 및 새 샌드박스 테스트 환경을 지원 하기 위해 iOS와 watchOS 모두에서 작동 하는 몇 가지 새로운 Api가 추가 되었습니다.

또한 PassKit 프레임 워크는 외부 `UIKit` Apple Pay를 지원 하도록 확장 되었으며 카드 발급자가 자신의 앱 내에서 카드를 제공할 수 있도록 합니다.

자세한 내용은 [Apple Pay 향상 된 기능](~/ios/platform/apple-pay.md) 가이드를 참조 하세요.

## <a name="alternate-app-icons"></a>대체 앱 아이콘

Apple은 앱의 아이콘 관리를 허용 하는 iOS 10.3에 대 한 몇 가지 향상 된 기능을 추가 했습니다.

- `ApplicationIconBadgeNumber`-Springboard에서 앱 아이콘의 배지를 가져오거나 설정 합니다.
- `SupportsAlternateIcons`-앱 `true` 에 대체 아이콘 집합이 있는 경우입니다.
- `AlternateIconName`-현재 선택한 대체 아이콘의 이름을 반환 하거나 `null` 기본 아이콘을 사용 하는 경우을 반환 합니다.
- `SetAlternameIconName`-이 메서드를 사용 하 여 앱의 아이콘을 지정 된 대체 아이콘으로 전환할 수 있습니다.

자세히 알아보려면 [대체 앱 아이콘](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) 가이드를 참조 하세요.

## <a name="introduction-to-callkit"></a>CallKit 소개

IOS 10의 새로운 CallKit API는 VOIP 앱이 iPhone UI와 통합 되 게 하 고 최종 사용자에 게 친숙 한 인터페이스와 경험을 제공할 수 있는 방법을 제공 합니다. 이 API를 사용 하면 사용자가 iOS 장치의 잠금 화면에서 VOIP 호출을 확인 하 고 상호 작용 하 고 Phone 앱의 **즐겨찾기** 및 **최근** 보기를 사용 하 여 연락처를 관리할 수 있습니다.

또한 CallKit API는 전화 번호를 이름 (호출자 ID)과 연결 하는 앱 확장을 만들 수 있는 기능을 제공 하거나, 번호를 차단 해야 할 경우 시스템에 알릴 수 있습니다 (호출 차단).

자세히 알아보려면 [Callkit 소개](~/ios/platform/callkit.md) 가이드를 참조 하세요.

## <a name="message-app-integration"></a>메시지 앱 통합

iOS 10은 **메시지** 앱과 통합 되 고 사용자에 게 새로운 기능을 제공 하는 xamarin.ios 솔루션에 메시지 앱 확장을 포함할 수 있도록 허용 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다. 다음과 같은 두 가지 유형의 메시지 앱 확장을 사용할 수 있습니다.

- **스티커 팩** -사용자가 메시지에 추가할 수 있는 스티커의 컬렉션을 포함 합니다. 스티커 팩은 코드를 작성 하지 않고도 만들 수 있습니다.
- **IMessage apps** -스티커를 선택 하 고, 미디어 파일 (선택적 형식 변환 포함)을 포함 하 여 텍스트를 입력 하 고, 상호 작용 메시지를 만들고 편집 하 고 보내는 메시지 앱 내에 사용자 지정 사용자 인터페이스를 제공할 수 있습니다.

자세히 알아보려면 [메시지 앱 통합](~/ios/platform/message-app-integration/index.md) 가이드를 참조 하세요.

## <a name="news-publisher-enhancements"></a>뉴스 게시자 향상

IOS 10을 사용 하 여 Apple은 주요 잡지 및 신규 조직의 모든 사용자가 블로거와 독립 된 게시자에 게 등록 및 제품을 제공 하 고 콘텐츠를 Apple News 앱에 제공할 수 있도록 허용 합니다. 자세한 내용은 Apple의 [News Resources](https://newsresources.apple.com/) 설명서를 참조 하세요.

## <a name="providing-haptic-feedback"></a>햅틱 피드백 제공

IPhone 7 및 iPhone 7 Plus에서 Apple에는 사용자에 게 물리적으로 참여 하는 추가 방법을 제공 하는 새로운 haptics 응답이 포함 되었습니다. 새 tactile 사용자 의견 옵션을 사용 하 여 사용자의 관심을 얻고 작업을 보강 합니다.

여러 기본 제공 UI 요소는 이미 선택기, 스위치 및 슬라이더와 같은 햅 피드백을 제공 합니다. 이제 iOS 10은 `UIFeedbackGenerator` 클래스의 구체적 하위 클래스를 사용 하 여 프로그래밍 방식으로 haptics를 트리거하는 기능을 추가 합니다.

자세히 알아보려면 [햅 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md) 가이드를 참조 하세요.

## <a name="proactive-suggestions"></a>자동 제안

iOS 10은 시스템이 적절 한 시간에 자동으로 유용한 정보를 사용자에 게 자동으로 제공할 수 있도록 하 여 앱에 대 한 참여를 유도 하는 새로운 방법을 제공 합니다. IOS 9에서 스포트라이트, 전달 및 Siri 제안을 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 하는 것 처럼 iOS 10에서 앱은 다음 위치에서 시스템을 통해 사용자에 게 제공할 수 있는 기능을 노출할 수 있습니다.

- 앱 전환기
- 잠금 화면
- CarPlay
- 지도
- Siri 상호 작용
- QuickType 제안

앱은 [NSUserActivity](xref:Foundation.NSUserActivity), 웹 마크업, Core 스포트라이트, mapkit, Media Player 및 uikit와 같은 기술 컬렉션을 사용 하 여 시스템에이 기능을 노출 합니다.

자세히 알아보려면 [사전 권장 사항 소개 가이드를](~/ios/platform/search/proactive-suggestions.md) 참조 하세요.

## <a name="request-app-review"></a>앱 검토 요청

Ios 10.3의 새로운 기능으로 `RequestReview()` ,이 방법을 사용 하면 ios 앱에서 사용자에 게 평가 또는 검토를 요청할 수 있습니다. 사용자 환경에 적합 한 지점에서이 메서드를 호출할 수 있지만, 검토 프로세스는 앱 스토어 정책에 의해 제어 되 고 처리 됩니다. 결과적으로이 메서드는 경고를 표시 하거나 표시 하지 않을 수 있으며 단추 누르기와 같은 사용자 동작에 대 한 응답으로 호출 되 면 안 됩니다.

자세히 알아보려면 [요청 앱 검토](~/ios/platform/request-app-review.md) 가이드를 참조 하세요.

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 향상

Apple에서는 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보를 확인 하는 데 도움이 되는 iOS 10의 보안 및 개인 정보 모두에 대해 몇 가지 기능이 향상 되었습니다.

따라서 iOS 10 (또는 이후 버전)에서 실행 되는 앱은 사용자에 게 액세스 권한을 얻으려고 하는 이유를 설명 하는 개인 정보 보호 키 `Info.plist` 를 하나 이상 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 의도를 정적으로 선언 해야 합니다.

자세히 알아보려면 [보안 및 개인 정보 보호 기능](~/ios/app-fundamentals/security-privacy.md) 가이드를 참조 하세요.

## <a name="sirikit"></a>SiriKit

IOS 10을 처음 사용 하는 SiriKit를 사용 하면 Xamarin.ios 앱이 iOS 장치에서 Siri를 사용 하 여 사용자에 게 액세스할 수 있는 서비스를 제공할 수 있습니다. 이 기능은 새 **의도** 및 **의도 UI** 프레임 워크를 사용 하 여 하나 이상의 앱 확장에서 제공 됩니다.

SiriKit는 다음 서비스 도메인을 지원 합니다.

- 오디오나 비디오를 호출 합니다.
- 을 예약 합니다.
- 워크를 관리 합니다.
- 전송.
- 사진을 검색 하 고 있습니다.
- 지불 보내기 또는 받기.

사용자가 앱 확장의 서비스 중 하나를 포함 하는 Siri를 요청 하면 SiriKit에서 해당 확장을 지원 데이터와 함께 사용자의 요청을 설명 하는 **의도** 개체에 보냅니다. 그러면 앱 확장이 지정 된 **의도**에 대 한 적절 한 **응답** 개체를 생성 하 여 확장에서 요청을 처리할 수 있는 방법을 자세히 설명 합니다.

Siri는 일반적으로 모든 사용자 상호 작용을 처리 하지만 앱 확장은 **의도 UI** 프레임 워크를 사용 하 여 앱의 브랜딩 및 추가 정보를 제공 하는 풍부한 사용자 지정 사용자 인터페이스를 제공할 수 있습니다.

자세히 알아보려면 [SiriKit 소개](~/ios/platform/sirikit/index.md) 가이드를 참조 하세요.

## <a name="speech-recognition"></a>음성 인식

iOS 10에는 앱에서 연속 음성 인식 및 높여줄 음성 (라이브 또는 녹화 된 오디오 스트림에서)을 텍스트로 사용할 수 있도록 하는 새로운 Speech API 포함 되어 있습니다.

음성 인식에는 Apple 서버에서 데이터의 전송 및 임시 저장소가 필요 하므로 앱은 `Info.plist` 파일에 키를 `NSSpeechRecognitionUsageDescription` 포함 하 고 다음을 호출하여사용자가인식할수있는권한을요청해야합니다.`SFSpeechRecognizer.RequestAutorization` 메서드.

자세히 알아보려면 [음성 인식 소개](~/ios/platform/speech.md) 가이드를 참조 하세요.

## <a name="user-notifications"></a>사용자 알림

IOS 10의 새로운 기능으로, 사용자 알림 프레임 워크를 사용 하면 로컬 및 원격 알림을 배달 하 고 처리할 수 있습니다. 이 프레임 워크를 사용 하 여 앱 또는 앱 확장은 위치 또는 시간 등의 조건 집합을 지정 하 여 로컬 알림의 배달을 예약할 수 있습니다.

또한 앱 또는 확장은 사용자의 iOS 장치에 전달 될 때 로컬 및 원격 알림을 수신 하 고 잠재적으로 수정할 수 있습니다.

새 사용자 알림 UI 프레임 워크를 사용 하면 앱 또는 앱 확장이 사용자에 게 표시 될 때 로컬 및 원격 알림의 모양을 사용자 지정할 수 있습니다.

자세히 알아보려면 [사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md) 가이드를 참조 하세요.

## <a name="video-subscriber-account"></a>비디오 구독자 계정

IOS 10의 새로운 기능으로, 비디오 구독자 계정 프레임 워크는 인증 된 스트리밍 또는 주문형 비디오를 지 원하는 앱이 최종 사용자에 게 Single Sign-on 환경을 사용 하 여 케이블 또는 위성 TV 공급자를 인증 하도록 허용 합니다.

## <a name="wide-color"></a>와이드 컬러

iOS 10은 핵심 그래픽, 핵심 이미지, 금속 및 AVFoundation과 같은 프레임 워크를 포함 하 여 시스템 전체에서 확장 범위 픽셀 형식 및 넓은 색 영역 색 공간에 대 한 지원을 확장 합니다. 넓은 색 표시를 사용 하는 장치에 대 한 지원은 전체 그래픽 스택에이 동작을 제공 하 여 추가로 줄어들 됩니다.

또한 [Uikit](xref:UIKit) 는 새로운 확장 된 **sRGB** colorspace에서 작동 하도록 수정 되어 상당한 성능 손실 없이 광범위 한 색 gamuts 색을 쉽게 혼합할 수 있게 해줍니다.

Apple은 넓은 색으로 작업할 때 다음과 같은 모범 사례를 제공 합니다.

- 이제 [UIColor](xref:UIKit.UIColor) 는 sRGB 색 공간을 사용 하 고 값을 `0.0` to `1.0` 범위에 더 이상 클램프 하지 않습니다. 앱이 이전 클램프 동작에 의존 하는 경우에는 iOS 10에 대해 수정 해야 합니다.
- IPad Pro에서 사용자 지정 `UIView` 그리기를 수행 하는 경우에는 sRGB 색 공간에 대해 그리기 환경이 구성 됩니다.
- 앱이의 `UIImages`사용자 지정 렌더링을 수행 하는 경우 새 [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) 클래스를 사용 하 여 확장 범위 또는 표준 범위 형식의 사용을 지정 합니다.
- 핵심 그래픽 또는 금속과 같은 하위 수준 API를 사용 하 여 이미지 처리를 제공 하는 경우 개발자는 16 비트 부동 소수점 값을 지 원하는 확장 된 범위 색 공간과 픽셀 형식을 사용 해야 합니다. 필요한 경우 개발자는 색 구성 요소 값을 수동으로 클램프 해야 합니다.
- 핵심 그래픽, 핵심 이미지 및 금속 성능 셰이더는 모두 두 색상 공간 간을 변환 하기 위한 새로운 방법을 제공 합니다.

자세히 알아보려면 [Wide Color 소개](~/ios/platform/wide-color.md) 가이드를 참조 하세요.

## <a name="widget-enhancements"></a>위젯 향상

Apple은 위젯 시스템에 몇 가지 향상 된 기능을 도입 하 여 새 iOS 10 잠금 화면에 있는 모든 배경에서 위젯을 잘 보이도록 합니다. [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) 속성은 더 이상 사용 되지 않으며 new [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) 또는 [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) 속성으로 대체 되었습니다. 또한 위젯을 사용 하 여 사용 가능한 콘텐츠의 양을 설명 하 고 사용자가 콘텐츠를 확장 및 축소할 수 있도록 하는 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 속성이 포함 됩니다.

자세히 알아보려면 [검색 및 홈 화면 위젯 개선 사항](~/ios/platform/search/widgets.md) 가이드를 참조 하세요.

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경

Apple은 위에 나열 된 주요 프레임 워크 변경 및 추가 사항 외에도 iOS 10에서 추가 사소한 프레임 워크 변경을 많이 수행 했습니다.

자세히 알아보려면 [추가 프레임 워크 변경](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) 가이드를 참조 하세요.

## <a name="deprecated-apis"></a>사용되지 않는 API

다음 Api는 iOS 10에서 더 이상 사용 되지 않습니다.

- `CKDiscoverAllContactsOperation` ,`CKDiscoveredUserInfo`및 클래스는iOS10용cloudkit에서더이상사용되지않습니다.`CKFetchRecordChangesOperation` `CKDiscoverUserInfosOperation` 대신 [CKDiscoverAllUserIdentitiesOperation](xref:CloudKit.CKDiscoverUserIdentitiesOperation), [CKUserIdentity](xref:CloudKit.CKUserIdentity) 및 [CKFetchRecordZoneChangesOperation](xref:CloudKit.CKFetchRecordZoneChangesOperation) 클래스 (레코드 공유 지원)를 사용 합니다.
- 일부 [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) api (예: 영역 기반 및 쿼리 기반 구독)는 더 이상 사용 되지 않습니다. [CKRecordZoneSubscription](xref:CloudKit.CKRecordZoneSubscription) 및 [CKQuerySubscription](xref:CloudKit.CKQuerySubscription) api를 대신 사용 합니다.
- 사용할 수 있는 콘텐츠와 관련 된 [NSPersistentStoreCoordinator](xref:CoreData.NSPersistentStoreCoordinator) 기호는 더 이상 사용 되지 않습니다.
- `ADBannerView`, `ADInterstitialAd` 및 [uiviewcontroller](xref:UIKit.UIViewController) 클래스의 관련 기호는 더 이상 사용 되지 않습니다.
- 부동 소수점 값과 관련 된 지 수의 [균일](https://developer.apple.com/reference/spritekit/skuniform) 기호는 사용 되지 않습니다.
- Uikit `UIMutableUserNotificationAction`의 `UIMutableUserNotificationCategory`,, ,`UIUserNotificationCategory` 및클래스는`UILocalNotification`더이상 사용되지않습니다.`UIUserNotificationAction` `UIUserNotificationSettings` 대신 [사용자 알림](#user-notifications) 프레임 워크를 사용 합니다.
- `HandleActionForLocalNotification` ,`HandleActionForRemoteNotification`및 WatchKit`DidReceiveRemoteNotification` 메서드 는더이상사용되지않습니다.`DidReceiveLocalNotification` 대신 및 `HandleActionForNotification` `DidReceiveNotification` 메서드를 사용 합니다.
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate)의 `DidReceiveLocalNotification` 및 `DidReceiveRemoteNotification` 메서드는 더 이상 사용 되지 않습니다. 적절 한 메서드를 구현 하 고 [unusernotificationcenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) 개체의 `Delegate` 속성에 할당 하는 [unusernotificationcenter 대리자](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) 의 인스턴스를 만듭니다.
- **Game Center 앱** 은 더 이상 사용 되지 않으며 iOS에서 제거 되었습니다. 앱에서 GameKit를 사용 하는 경우 순위표 등과 같은 GameKit 기능을 표시 하는 고유한 인터페이스를 제공 _해야 합니다_ .

결함의 전체 목록은 Apple의 [ios 9.3에서 ios 10.0 API 차이점 설명서를](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) 참조 하세요.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
