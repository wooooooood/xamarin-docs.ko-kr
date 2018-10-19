---
title: IOS 12 소개
description: 이 문서는 Xamarin의 미리 보기 릴리스는 C# 바인딩을 제공 하는 일부 12 iOS api 자세히 설명 합니다.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/08/2018
ms.openlocfilehash: 81375e8c66e5504604d0d4cb3be34afd58f4269d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615150"
---
# <a name="introduction-to-ios-12"></a>IOS 12 소개

이 문서는 Xamarin의 미리 보기 릴리스는 C# 바인딩을 제공 하는 일부 12 iOS api 자세히 설명 합니다.

Xamarin 사용한 iOS 12 앱 빌드 시작, 참조는 [시작 가이드](get-started.md)

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

ARKit는 iOS를 사용 하 여 포함 된 보강 된 현실 프레임 워크입니다. ARKit 2는 확대 된 현실 장면에서 서로 상호 작용 하는 여러 사용자가 공간에서 개체를 유지 하 고 나중에로 돌아올 수 있도록 제공 2D 이미지 인식 및 추적 및 3D 개체 인식 합니다. 또한 iOS 12는 AR 빠른 보기를 usdz AR 앱 모델을 렌더링 하는 방법을 제공 합니다.

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Siri 바로 가기](siri-shortcuts.md)

Siri 바로 가기 Siri를 사용 하 여 해당 응용 프로그램을 보다 강력 하 게 통합 하는 개발자를 수 있습니다. Siri 바로 가기를 사용자 음성 명령을 사용 하 여 콘텐츠를 열거나 백그라운드 작업을 시작할 수 있습니다 또는 잠금 화면에서 Siri 제안 하는 바로 가기를 통해 이와 동일한 작업을 시작할 수 있습니다.

## <a name="core-ml-2coremlmd"></a>[코어 ML 2](coreml.md)

2 코어 ML 모델 양자화 및 유연한 모델을 통해 응용 프로그램 크기를 줄입니다, 그리고 새 일괄 처리 예측 API 사용 하 여 응용 프로그램 성능이 향상 됩니다 및 machine learning의 고급 기능을 지원 하기 위해 사용자 지정 모델을 사용 합니다.

## <a name="notification-improvementsnotificationsindexmd"></a>[알림 기능 향상](notifications/index.md)

Ios 12에서 그룹화 된 알림 수 있도록 앱 또는 스레드 관련 그룹에 사용자 알림을 표시 합니다. 요약 텍스트는 추가 알림 그룹에 대 한 정보를 제공 합니다.

알림 콘텐츠 확장 12 ios에서 사용자 지정 사용자 인터페이스 및 동적 실행 단추에 대 한 허용합니다.

## <a name="natural-language-frameworknatural-languagemd"></a>[자연 언어 프레임 워크](natural-language.md)

자연 언어 프레임 워크에는 응용 프로그램을을 다양 한 유형의 언어 분석을 수행할 수 있습니다. 예를 들어 수 요소를 식별 하 고 텍스트 블록을 나타내는 언어를 결정 합니다.

## <a name="vision-framework"></a>비전 프레임 워크

비전 프레임 워크에 다양 한 방향에서 얼굴을 검색할 수 있는 향상 된 얼굴 탐지기를 포함 합니다. 또한 요청 수정 특정 비전 framework 알고리즘 수정 버전을 선택할 수 있습니다.

## <a name="photo-and-video-apis"></a>사진 및 비디오 Api

Ios 12에서 세로 구분 API 세로 효과 매트 – 세로 이미지의 백그라운드에서 포그라운드를 설명 하 고 다양 한 이미지 효과 만드는 데 유용 하는 선형 마스크를 반환 합니다. iOS 12 하기가 실시간 비디오 효과 대 한 깊이 데이터로 TrueDepth 카메라를 사용할 수 있습니다.

## <a name="passwords"></a>암호

iOS 12 쉽게 사용자 및 암호를 사용 하는 개발자:

- 암호 자동 채우기 및 자동 강력한 암호를 사용 하면 자동으로 생성, 저장 및 iOS 응용 프로그램에 등록 하 고 응용 프로그램에 로그인 하는 경우에 강력한 암호를 사용 하는 작업을 수 있습니다.
- 보안 코드 자동 채우기 쉽게 기억할 수 있는 수동 잘라내기 및 붙여넣기 없이 SMS 기반 인증 코드를 사용할 수 있습니다.
- `ASWebAuthenticationSession` 페더레이션된 인증 서비스를 사용 하는 프로세스를 간소화 하는 클래스입니다.
- 자동 채우기 자격 증명 공급자 확장을 사용 하면 타사 암호 응용 프로그램에서 사용자 이름 및 암호 로그인 필드를 제공 합니다.

## <a name="healthkit-updates"></a>HealthKit 업데이트

iOS 11.3 도입 [건강 기록](https://www.apple.com/healthcare/health-records/), 다양 한 의료 기관에서 정보를 기록 하 고 iOS 장치에서 대시보드를 볼 사용자가 해당 상태를 다운로드할 수 있습니다. 타사 응용 프로그램에서이 데이터를 안전 하 게 액세스할 수 있도록 Api를 추가 하는 iOS 12입니다.

## <a name="imessage-app-presentation-contexts"></a>iMessage 앱 프레젠테이션 컨텍스트

IOS 12, iMessage 앱 일반 iMessage 앱으로 또는 사진 또는 비디오 효과의 컨텍스트에서 실행 되도록 앱을 허용 하는 프레젠테이션 컨텍스트를 지원 합니다.

## <a name="network-framework"></a>네트워크 프레임 워크

기본 네트워크 프레임 워크, 네트워크 스택는 `URLSession` iOS 응용 프로그램에서 일반적으로 사용 되는 Api은 이제 독립 실행형 프레임 워크를 쉽게 TCP, UDP, TLS, IPv4/i p v 6 등을 사용 하 여 작동 합니다.

## <a name="carplay"></a>CarPlay

12 iOS에서 타사 앱에에서 제공할 수 맵 및 설정에 설정 하 여 탐색 지침이 CarPlay 새로운 CarPlay 프레임 워크를 사용 하 여.

## <a name="deprecations"></a>결함

IOS 12 사용 하 여 Apple에 사용 되지 않습니다.

- OpenGL ES [개발자 들에 게](https://developer.apple.com/ios/whats-new/) 체제 미 설치 컴퓨터를 채택 해야 합니다.
- [`UIWebView`](https://developer.xamarin.com/api/type/UIKit.UIWebView/)하십시오 [기준 `WKWebView` ](https://developer.apple.com/documentation/webkit/wkwebview?language=objc)합니다.

## <a name="related-links"></a>관련 링크

- [IOS (Apple) 12에 대 한 준비](https://developer.apple.com/ios/)
