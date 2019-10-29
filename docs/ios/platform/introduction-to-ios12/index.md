---
title: iOS 12 소개
description: 이 문서에서는 Xamarin의 미리 보기 릴리스가 바인딩을 제공 C# 하는 일부 IOS 12 api에 대 한 개략적인 설명을 제공 합니다.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/08/2018
ms.openlocfilehash: 39d954626bc9e789446e7f1deac67e2e0fca51c8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031998"
---
# <a name="introduction-to-ios-12"></a>iOS 12 소개

이 문서에서는 Xamarin의 미리 보기 릴리스가 바인딩을 제공 C# 하는 일부 IOS 12 api에 대 한 개략적인 설명을 제공 합니다.

Xamarin을 사용 하 여 iOS 12 앱 빌드를 시작 하려면 [시작 가이드](get-started.md) 를 참조 하세요.

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

ARKit는 iOS에 포함 된 확대 된 현실 프레임 워크입니다. ARKit 2를 사용 하면 여러 사용자가 확대 된 현실 장면에서 서로 상호 작용할 수 있으므로 공간에 개체를 유지 하 고 나중에이를 반환할 수 있으며 2D 이미지 인식 및 추적 및 3D 개체 인식을 제공 합니다. iOS 12는 앱에서 usdz AR 모델을 렌더링 하는 방법인 AR 빠른 보기도 제공 합니다.

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Siri 바로 가기](siri-shortcuts.md)

Siri 바로 가기를 사용 하 여 개발자는 응용 프로그램을 Siri와 더욱 깊이 있게 통합할 수 있습니다. Siri 바로 가기를 사용 하면 사용자가 음성 명령을 사용 하 여 콘텐츠를 열거나 백그라운드 작업을 시작할 수 있으며, Siri가 잠금 화면에서 제안 하는 바로 가기를 통해 동일한 작업을 시작할 수 있습니다.

## <a name="core-ml-2coremlmd"></a>[Core ML 2](coreml.md)

핵심 ML 2는 모델 양자화 및 유연한 모델을 통해 응용 프로그램 크기를 줄이고, 새로운 일괄 처리 예측 API를 사용 하 여 응용 프로그램 성능을 향상 시키고, 사용자 지정 모델을 사용 하 여 기계 학습의 발전을 지원 합니다

## <a name="notification-improvementsnotificationsindexmd"></a>[알림 기능 향상](notifications/index.md)

IOS 12에서 그룹화 된 알림은 앱 또는 스레드 관련 그룹에 사용자 알림을 제공할 수 있도록 합니다. 요약 텍스트는 알림 그룹에 대 한 추가 정보를 제공 합니다.

IOS 12의 알림 콘텐츠 확장은 사용자 지정 사용자 인터페이스 및 동적 작업 단추를 허용 합니다.

## <a name="natural-language-frameworknatural-languagemd"></a>[자연어 프레임 워크](natural-language.md)

자연어 프레임 워크를 사용 하면 응용 프로그램에서 다양 한 유형의 언어 분석을 수행할 수 있습니다. 예를 들어 음성의 일부를 식별 하 고 텍스트 블록으로 표시 되는 언어를 결정할 수 있습니다.

## <a name="vision-frameworkiosplatformintroduction-to-ios11visionmd"></a>[비전 프레임 워크](~/ios/platform/introduction-to-ios11/vision.md)

비전 프레임 워크에는 여러 방향으로 얼굴을 검색할 수 있는 향상 된 얼굴 감지기가 포함 되어 있습니다. 또한 수정 요청은 특정 비전 프레임 워크 알고리즘 수정 버전을 선택할 수 있습니다.

## <a name="photo-and-video-apis"></a>사진 및 비디오 Api

IOS 12에서 세로 분할 API는 세로 효과 매트 (서술 세로 이미지의 배경 으로부터 전경을 구분 하는 선형 마스크)를 반환 하며 다양 한 이미지 효과를 만드는 데 유용 합니다. iOS 12를 사용 하면 실시간 비디오 효과를 위해 TrueDepth 카메라의 깊이 데이터를 사용할 수 있습니다.

## <a name="passwords"></a>암호

iOS 12를 사용 하면 사용자와 개발자가 암호를 사용 하 여 쉽게 작업할 수 있습니다.

- 암호 자동 채우기 및 자동 강력한 암호를 통해 응용 프로그램에 등록 하 고 로그인 할 때 iOS 응용 프로그램에서 강력한 암호를 자동으로 생성, 저장 및 사용할 수 있습니다.
- 보안 코드 자동 채우기를 사용 하면 수동 잘라내기와 붙여넣기 또는 기억할 하지 않고도 SMS 기반 인증 코드를 사용할 수 있습니다.
- `ASWebAuthenticationSession` 클래스는 페더레이션된 인증 서비스로 작업 하는 프로세스를 간소화 합니다.
- 자동 채우기 자격 증명 공급자 확장을 사용 하 여 타사 암호 응용 프로그램에서 로그인 필드에 사용자 이름과 암호를 제공할 수 있습니다.

## <a name="healthkit-updates"></a>HealthKit 업데이트

iOS 11.3은 사용자가 다양 한 의료 기관에서 자신의 상태 레코드 정보를 다운로드 하 여 iOS 장치에서 볼 수 있도록 하는 [상태 레코드](https://www.apple.com/healthcare/health-records/)를 도입 했습니다. iOS 12는 타사 응용 프로그램에서이 데이터에 안전 하 게 액세스할 수 있도록 하는 Api를 추가 합니다.

## <a name="imessage-app-presentation-contexts"></a>iMessage 앱 프레젠테이션 컨텍스트

IOS 12에서 iMessage apps는 앱이 일반 iMessage 앱 또는 사진 또는 비디오 효과의 컨텍스트에서 실행 될 수 있도록 하는 프레젠테이션 컨텍스트를 지원 합니다.

## <a name="network-framework"></a>네트워크 프레임 워크

IOS 응용 프로그램에서 일반적으로 사용 되는 `URLSession` Api를 기반으로 하는 네트워크 stack 네트워크 프레임 워크는 이제 독립 실행형 프레임 워크로 제공 되므로 TCP, UDP, TLS, IPv4/IPv6 등의 작업을 더 쉽게 수행할 수 있습니다.

## <a name="carplay"></a>CarPlay

IOS 12에서 타사 앱은 새 CarPlay 프레임 워크를 사용 하 여 CarPlay에서 지도를 제공 하 고 단계별 탐색 지침을 활성화할 수 있습니다.

## <a name="deprecations"></a>결함

IOS 12를 사용 하는 경우 Apple은 더 이상 사용 되지 않습니다.

- [개발자가](https://developer.apple.com/ios/whats-new/) 금속을 채택할 수 있도록 하는 OpenGL ES
- [`WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview?language=objc)를 [`UIWebView`](xref:UIKit.UIWebView)합니다.
