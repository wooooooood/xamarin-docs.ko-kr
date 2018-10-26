---
title: TvOS 10 소개
description: 이 아티클에서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 10에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 260d01d6aa8344dd3cf107f1ffc34167c457a491
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120985"
---
# <a name="introduction-to-tvos-10"></a>TvOS 10 소개

_이 아티클에서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 10에서에서 사용할 수 있는 기능을 소개합니다._

새 tvOS를 사용 하 여 새 Api 및 서비스 개발자가 앱 및 기능의 새 범주를 만들 수 있도록 10 SDK Apple가 포함 됩니다. 

TvOS 10에 대 한 자세한 내용은 Apple의를 참조 하세요 [tvOS 앱 +](https://developer.apple.com/tvos/) 설명서.

## <a name="whats-new-in-tvos-10"></a>TvOS 10에서 새 란

Apple이 Api 및 서비스에 대 한 몇 가지 새 tvOS 10 함께 기존 기능을 비롯 한 향상 된 기능에에서 추가 합니다.

## <a name="new-user-interface-styles"></a>새로운 사용자 인터페이스 스타일

tvOS 10 지원은 자동으로 컨트롤에 UIKit의 모든 어둡게 및 밝게 사용자 인터페이스 테마에 맞게, 이제 사용자의 기본 설정을 기반으로 합니다.

개발자를 만들고 새 사용자 지정 UI 컨트롤을 구현 하는 경우 사용 해야 합니다 [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) 사용자의 선택한 테마에 맞게 클래스입니다.

자세한 내용은 참조 하십시오 우리의 [새 사용자 인터페이스 스타일](~/ios/tvos/platform/user-interface-styles.md) 설명서.

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상

Apple은 보안 및 개인 정보 보호 개발자가 앱의 보안을 강화 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 tvOS 10에에서 모두 몇 가지 향상 되었습니다.

WatchOS 3 (이상)에서 실행 중인 앱에서 하나 이상의 개인 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하려면 의도 해야 정적으로 선언 하는 결과적으로, 해당 `Info.plist` 앱에 액세스 하려는 이유는 사용자에 게 설명 하는 파일입니다.

TvOS 10 iOS 10 사용 하 여 이러한 변경 내용을 공유 하므로 iOS 10을 참조 하세요 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 자세한 가이드입니다.

## <a name="video-subscriber-account"></a>비디오 구독자 계정

새 tvOS 10에 대 한 비디오 구독자 계정 프레임 워크 앱 허용 스트리밍 인증 지원 또는 주문형 비디오는 단일 로그인 환경을 사용 하 여 최종 사용자에 게 해당 케이블 또는 위성 TV 공급자를 사용 하 여 인증 하는 합니다.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>와이드 컬러

tvOS 10 확장 범위 픽셀 형식 및 AVFoundation 핵심 그래픽, Core 이미지, 금속 등 프레임 워크를 비롯 한 시스템 전체에서 광범위 한 색상 공간에 대 한 지원도 확장 되었습니다. 와이드 컬러 디스플레이 사용 하 여 장치에 대 한 지원은 전체 그래픽 스택 전체에서이 동작을 제공 하 여 용이 추가 됩니다.

또한 `UIKit` 수정 된 새 작업을 확장 **sRGB** 색상 공간을 간편 하 게 중요 한 성능 손실 없이 광범위 한 색 색상의 색을 혼합 합니다.

Apple에서는 다양 한 색을 사용 하 여 작업 하는 경우 다음 모범 사례를 제공 합니다.

 - `UIColor` 사용 하는 sRGB 색 공간을 더 이상 고정 값을 이제는 `0.0` 에 `1.0` 범위입니다. 앱을 이전 하는 제한 동작에 의존 하는 경우 10 tvOS에 대 한 수정 해야 합니다.
 - 앱의 사용자 지정 렌더링을 수행 하는 경우 `UIImages`를 사용 하 여 새 [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) 확장 범위 또는 표준 범위 형식의 사용을 지정 하는 클래스입니다.
 - 핵심 그래픽 또는 체제 미 설치 컴퓨터와 같은 하위 수준 API를 사용 하 여 이미지 처리를 앱을 확장된 범위 색 공간과 픽셀 지 원하는 형식으로 16 비트 부동 소수점 값을 사용 해야 합니다. 필요한 경우 앱을 수동으로 색 구성 요소 값을 고정 해야 합니다.
 - 핵심 그래픽, Core 이미지 및 체제 미 설치 컴퓨터 성능 셰이더 모든 두 색 공간 변환 하기 위한 새 메서드를 제공 합니다.

자세한 내용을 참조 하세요 우리의 [와이드 컬러 소개](~/ios/platform/wide-color.md) 가이드입니다.

## <a name="newly-available-existing-frameworks"></a>새로 사용 가능한 기존 프레임 워크

IOS (및 tvOS 없습니다)에서 사용할 수 있었던 여러 프레임 워크 사용할 수 있게 된 tvOS 10에 대 한 예:

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - 사진
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>프레임 워크 추가 변경 내용

주요 프레임 워크 변경 내용 및 위에 나열 된 추가 외에 Apple tvOS 10에서에서 많은 사소한 추가 프레임 워크 변경을 했습니다.

자세한 내용을 참조 하세요 우리의 [추가 프레임 워크 변경 내용](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) 가이드입니다.

## <a name="deprecated-apis"></a>사용되지 않는 API

Api 또는 프레임 워크 없습니다 된 tvOS 10에서 사용 되지 않습니다. Apple의를 참조 하세요 [tvOS 10 API 차이점](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) API 수정 목록은 설명서.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
