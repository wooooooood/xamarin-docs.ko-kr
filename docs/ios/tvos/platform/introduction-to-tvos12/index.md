---
title: tvOS 12 소개
description: 이 문서에서는 현재 Xamarin preview 릴리스가 바인딩을 제공 C# 하는 tvOS 12의 새롭고 업데이트 된 기능에 대 한 개략적인 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: c8d1f3dda459bc679048291e5df94bd68fbb80b2
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200050"
---
# <a name="introduction-to-tvos-12"></a>tvOS 12 소개

이 문서는 새로 제공 되거나 업데이트 된 tvOS 12에 대 한 개략적인 개요를 제공 합니다.

Xamarin을 사용 하 여 tvOS 12 앱 빌드를 시작 하려면 [시작 가이드](~/ios/platform/introduction-to-ios12/get-started.md)를 살펴보세요.

## <a name="tvuikit"></a>TVUIKit

tvOS 12에는 tvOS 개발자가 포스터 보기, 캡션 단추, 카드 보기 및 monogram 뷰와 같은 일반적인 tvOS 컨트롤을 사용할 수 있도록 하는 Api 집합인 TVUIKit가 포함 되어 있습니다. tvOS 12에는 레이블이 너무 길어서 완전히 표시 되지 않는 텍스트를 스크롤할 수 있는 속성도 도입 되었습니다.

## <a name="password-autofill"></a>암호 자동 채우기

TvOS 12를 사용 하면 사용자는 자신의 iOS 장치를 사용 하 여 한 번의 탭으로 tvOS 앱에 로그인 할 수 있습니다. 이는 `UITextContentType` 사용을 조합 하 여 사용자 이름 및 암호 필드를 지정 하 고, 연결 된 도메인을 사용 하 여 iOS 앱과 tvOS 앱 간의 관계를 설정 하 고, 사용자가 포커스를 받을 항목을 선택할 수 있는 기본 포커스 환경을 사용 하도록 설정 합니다. 사용자 이름 및 암호를 제공 합니다.

## <a name="focus-engine-enhancements"></a>포커스 엔진 향상

tvOS 12는 모든 앱이 렌더링 되는 방식에 관계 없이 포커스 엔진과 상호 작용 하도록 허용 합니다. 사용자의 Siri 원격 상호 작용을 통해 포커스 엔진을 모든 앱과 함께 사용 하 여 항목을 선택 하 고, 가능한 포커스 변경에서 힌트를 선택 하 고, 자연스럽 게 업데이트할 수 있습니다. 이 기능은 uikit의 `IUIFocusItemContainer` 인터페이스 `UIFocusMovementHint` , 클래스, `IUIFocusItemScrollableContainer` 인터페이스 및 기타 관련 클래스와 메서드를 통해 사용자 지정 응용 프로그램에서 사용할 수 있습니다.

## <a name="vision-framework"></a>비전 프레임 워크

비전 프레임 워크에는 여러 방향으로 얼굴을 검색할 수 있는 향상 된 얼굴 감지기가 포함 되어 있습니다. 또한 이제 요청 수정 버전을 사용 하 여 특정 비전 프레임 워크 알고리즘 수정 버전을 선택할 수 있습니다.

## <a name="natural-language-framework"></a>자연어 프레임 워크

자연어 프레임 워크를 사용 하면 응용 프로그램에서 다양 한 유형의 언어 분석을 수행할 수 있습니다. 예를 들어, 음성의 일부를 식별 하 고 텍스트 블록이 나타내는 언어를 결정 하는 데 사용할 수 있습니다.

## <a name="deprecations"></a>결함

TvOS 12를 사용 하 여 Apple은 OpenGL ES를 사용 하지 않으므로 개발자가 금속을 채택 하도록 [장려](https://developer.apple.com/tvos/whats-new/) 합니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS – apple Developer (apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple)의 새로운 기능 (비디오)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
