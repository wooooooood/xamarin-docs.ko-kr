---
title: 10 tvOS 소개
description: 이 문서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 10에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 642a851cbfc0450ee8f5f5c4d798c40778e3d3dd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30779757"
---
# <a name="introduction-to-tvos-10"></a>10 tvOS 소개

_이 문서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 10에서에서 사용할 수 있는 기능을 소개합니다._

새 tvOS와 10 SDK Apple가 새 Api와 개발자는 응용 프로그램 및 기능 새 범주를 만들 수 있도록 해 주는 서비스를 포함 합니다. 

10 tvOS에 대 한 자세한 내용은 Apple의를 참조 하십시오 [tvOS + 앱](https://developer.apple.com/tvos/) 설명서입니다.

## <a name="whats-new-in-tvos-10"></a>10 tvOS에 새로운 이란

Apple이 몇 가지 새로운 Api 및 서비스를 비롯 한 기존 기능에는 많은 향상 된 구성 요소가 tvOS 10에에서 추가:

## <a name="new-user-interface-styles"></a>새로운 사용자 인터페이스 스타일

tvOS 10 지원이 자동으로 제어에 빌드 UIKit의 모든 Light 사용자 인터페이스와 어두운 테마 여건에 맞춰 적용, 이제 사용자의 기본 설정을 기반으로 합니다.

를 만들고 새 사용자 지정 UI 컨트롤을 구현할 때 개발자 사용할지는 [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) 클래스에는 사용자의 선택한 테마에 맞게 합니다.

자세한 내용은 참조 하십시오 우리의 [새로운 사용자 인터페이스 스타일](~/ios/tvos/platform/user-interface-styles.md) 설명서입니다.

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상 된 기능

Apple은 보안과 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 tvOS 10의에서 개인 정보에 몇 가지 향상 되었습니다.

결과적으로, watchOS 3 (또는 이후 버전)에서 실행 되는 앱 해야 정적으로 선언할에 하나 이상의 개인 정보 보호 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 `Info.plist` 사용자 응용 프로그램에 액세스 하려는 이유를 설명 하는 파일입니다.

IOS 또는 10과 이러한 변경 내용을 공유 하는 10 tvOS, 하므로 iOS 10을 참조 하십시오 [보안 및 개인 정보 보호 향상 된 기능](~/ios/app-fundamentals/security-privacy.md) 자세한 정보에 대 한 가이드입니다.

## <a name="video-subscriber-account"></a>비디오 구독자 계정

새로운 10 tvOS에 대 한 비디오 구독자 계정 프레임 워크에서는 앱 해당 스트리밍 인증 지원 또는 주문형 비디오 케이블 또는 위성 TV 공급자에 게 최종 사용자에 대 한 단일 로그인 환경을 사용 하 여 인증할 수 있습니다.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>광범위 한 색

10 tvOS 확장 범위 픽셀 형식 및 코어 그래픽, Core 이미지, 금속 및 AVFoundation 등의 프레임 워크를 비롯 하 여 시스템 전체에서 광범위 한 색상 공간에 대 한 지원을 확장 합니다. 전체 전체 그래픽 스택에서이 동작을 제공 하 여 광범위 한 색 표시를 사용 하 여 장치에 대 한 지원은 쉽게 할 추가 합니다.

또한 `UIKit` 수정 된 새에서 작동 하도록 확장 **sRGB** 상당한 성능 손실 없이 색상 광범위 한 색 영역에서 색을 혼합를 더욱 쉽게 색상 공간입니다.

Apple에서는 와이드 색으로 작업 하는 경우 다음 모범 사례를 제공 합니다.

 - `UIColor` 이제는 sRGB 색 공간을 더 이상 사용 하 여 값을 clamp는 `0.0` 를 `1.0` 범위입니다. 응용 프로그램 이전 제한 동작에 의존 하는 경우 10 tvOS에 맞게 수정 해야 합니다.
 - 응용 프로그램의 사용자 지정 렌더링을 수행 하는 경우 `UIImages`, 새로운 [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) 클래스의 확장 된 범위 또는 범위 표준 형식 사용을 지정 합니다.
 - 코어 그래픽, 금속 등의 하위 수준 API를 사용 하 여 제공 이미지 처리를 응용 프로그램에는 확장된 범위가 색 공간 및 픽셀 형식으로 16 비트 부동 소수점 값을 지 원하는 사용 해야 합니다. 필요한 경우 응용 프로그램 색상 구성 요소 값을 수동으로 clamp 해야 합니다.
 - 코어 그래픽, Core 이미지 및 금속 성능 셰이더 모두 두 개의 색 공간 형식 사이의 변환에 대 한 새로운 메서드를 제공 합니다.

자세한 내용은 참조 하세요 우리의 [광범위 한 색 소개](~/ios/platform/wide-color.md) 가이드입니다.

## <a name="newly-available-existing-frameworks"></a>새로 사용 가능한 기존 프레임 워크

IOS (및 하지 tvOS)에서 사용할 수 있는 여러 프레임 워크 적용 된 tvOS 10에 사용할 수와 같은:

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - 사진
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경 내용

주요 framework 변경 내용 및 위에 나열 된 추가 기능 뿐만 아니라 Apple tvOS 10에서에서 많은 추가 부 프레임 워크 변경을 했습니다.

자세한 내용은 참조 하세요 우리의 [추가 프레임 워크 변경](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) 가이드입니다.

## <a name="deprecated-apis"></a>사용되지 않는 API

Api 또는 프레임 워크 없습니다 된 tvOS 10에서 사용 되지 않습니다. Apple의 참조 [tvOS 10 API 차이점](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) API 수정 작업의 전체 목록은 대 한 설명서입니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
