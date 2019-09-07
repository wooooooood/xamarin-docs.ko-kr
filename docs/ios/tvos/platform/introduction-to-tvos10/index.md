---
title: tvOS 10 소개
description: 이 문서에서는 tvOS 개발자를 위해 tvOS 10에서 제공 되는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다.
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 8c338f8a5b2f1d41b1ea0f61778a1c14eb84ce08
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769150"
---
# <a name="introduction-to-tvos-10"></a>tvOS 10 소개

_이 문서에서는 tvOS 개발자를 위해 tvOS 10에서 제공 되는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다._

새 tvOS 10 SDK Apple에는 개발자가 새로운 범주의 앱 및 기능을 만들 수 있도록 하는 새로운 Api 및 서비스가 포함 되었습니다. 

TvOS 10에 대 한 자세한 내용은 Apple의 [tvOS + Apps](https://developer.apple.com/tvos/) 설명서를 참조 하세요.

## <a name="whats-new-in-tvos-10"></a>TvOS 10의 새로운 기능

Apple은 다음을 비롯 한 기존 기능에 대 한 여러 향상 된 기능을 통해 tvOS 10에 몇 가지 새로운 Api 및 서비스를 추가 했습니다.

## <a name="new-user-interface-styles"></a>새 사용자 인터페이스 스타일

이제 tvOS 10은 사용자의 기본 설정에 따라 모든 기본 UIKit 컨트롤이 자동으로 조정 되는 짙은 사용자 인터페이스 테마를 모두 지원 합니다.

새 사용자 지정 UI 컨트롤을 만들고 구현할 때 개발자는 [Uitraitcollection](https://developer.apple.com/reference/uikit/uitraitcollection) 클래스를 사용 하 여 사용자가 선택한 테마에 맞게 조정 해야 합니다.

자세한 내용은 [새로운 사용자 인터페이스 스타일](~/ios/tvos/platform/user-interface-styles.md) 설명서를 참조 하세요.

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 향상

Apple은 tvOS 10의 보안 및 개인 정보 모두에 대해 몇 가지 향상 된 기능을 통해 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보를 확인 하는 데 도움이 됩니다.

따라서 watchOS 3 이상에서 실행 되는 앱은 해당 `Info.plist` 파일에 하나 이상의 개인 정보 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스할 수 있도록 정적으로 선언 해야 합니다.

TvOS 10은 이러한 변경 내용을 iOS 10과 공유 하므로 iOS 10 [보안 및 개인 정보 향상](~/ios/app-fundamentals/security-privacy.md) 가이드를 참조 하 여 자세한 내용을 확인 하세요.

## <a name="video-subscriber-account"></a>비디오 구독자 계정

TvOS 10의 새로운 기능으로, 비디오 구독자 계정 프레임 워크는 인증 된 스트리밍 또는 주문형 비디오를 지 원하는 앱이 최종 사용자에 게 Single Sign-on 환경을 사용 하 여 케이블 또는 위성 TV 공급자를 인증 하도록 허용 합니다.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>와이드 컬러

tvOS 10은 핵심 그래픽, 핵심 이미지, 금속 및 AVFoundation과 같은 프레임 워크를 포함 하 여 시스템 전체의 확장 범위 픽셀 형식 및 넓은 색 영역 색 공간에 대 한 지원을 확장 합니다. 넓은 색 표시를 사용 하는 장치에 대 한 지원은 전체 그래픽 스택에이 동작을 제공 하 여 추가로 줄어들 됩니다.

또한는 새로운 확장 된 sRGB colorspace에서 작동 하도록 수정 되어 상당한 성능 손실 없이 광범위 한 색 gamuts 색을 더 쉽게 혼합할 수 있게 되었습니다. `UIKit`

Apple은 넓은 색으로 작업할 때 다음과 같은 모범 사례를 제공 합니다.

- `UIColor`이제는 sRGB 색 공간을 사용 하며이 값은 더 이상 `0.0` to `1.0` 범위로 값을 클램프 하지 않습니다. 앱이 이전 클램프 동작에 의존 하는 경우에는 tvOS 10에 대해 수정 해야 합니다.
- 앱이의 `UIImages`사용자 지정 렌더링을 수행 하는 경우 새 [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) 클래스를 사용 하 여 확장 범위 또는 표준 범위 형식의 사용을 지정 합니다.
- 핵심 그래픽 또는 금속과 같은 하위 수준 API를 사용 하 여 이미지 처리를 제공 하는 경우 앱은 16 비트 부동 소수점 값을 지 원하는 확장 된 범위 색 공간과 픽셀 형식을 사용 해야 합니다. 필요한 경우 앱은 색 구성 요소 값을 수동으로 클램프 해야 합니다.
- 핵심 그래픽, 핵심 이미지 및 금속 성능 셰이더는 모두 두 색상 공간 간을 변환 하기 위한 새로운 방법을 제공 합니다.

자세히 알아보려면 [Wide Color 소개](~/ios/platform/wide-color.md) 가이드를 참조 하세요.

## <a name="newly-available-existing-frameworks"></a>새로 사용 가능한 기존 프레임 워크

IOS에서 사용할 수 있는 몇 가지 프레임 워크 (tvOS 아님)는 tvOS 10 (예:)에서 사용할 수 있었습니다.

- ExternalAccessory
- HomeKit
- MultipeerConnectivity
- 사진
- ReplayKit
- UserNotification

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경

Apple은 위에 나열 된 주요 프레임 워크 변경 및 추가 사항 외에도 tvOS 10에서 추가 사소한 프레임 워크 변경을 많이 수행 했습니다.

자세히 알아보려면 [추가 프레임 워크 변경](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) 가이드를 참조 하세요.

## <a name="deprecated-apis"></a>사용되지 않는 API

Api 또는 프레임 워크는 tvOS 10에서 더 이상 사용 되지 않습니다. API 수정의 전체 목록은 Apple의 [tvOS 10 Api 차이점](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) 설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
