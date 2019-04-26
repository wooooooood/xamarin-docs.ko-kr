---
title: tvOS 12 소개
description: 이 문서는 Xamarin의 미리 보기 릴리스에 대 한 12 tvOS에서 새롭고 업데이트 된 기능의 개요는 고급 C# 바인딩을 현재 제공을 제공 합니다.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: f7fb8cc379a070b848c5154c9c1d4fbfc8186266
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60932545"
---
# <a name="introduction-to-tvos-12"></a>tvOS 12 소개

이 문서는 새롭고 업데이트 된 tvOS 12의 대략적인 개요를 제공합니다.

시작 하려면 Xamarin 사용 하 여 12 tvOS 앱 구축 살펴보겠습니다 합니다 [시작 가이드](~/ios/platform/introduction-to-ios12/get-started.md)합니다.

## <a name="tvuikit"></a>TVUIKit

tvOS 12 TVUIKit, tvOS 포스터 뷰, 캡션 단추, 카드 보기 및 monogram 뷰와 같은 일반적인 tvOS 컨트롤을 사용 하는 개발자를 위한 수 있게 해 주는 Api 집합을 포함 합니다. 또한 12 tvOS 레이블이 너무 깁니다. 완전히 표시 되도록 하는 텍스트를 스크롤할 수 있는 속성을 소개 합니다.

## <a name="password-autofill"></a>암호 자동 채우기

TvOS 12 사용 하 여 사용자가 한 번의 탭을 사용 하 여 tvOS 앱에 로그인 할 iOS 장치를 사용할 수 있습니다. 결합을 통해 활성화 됩니다 `UITextContentType` iOS 앱 및 tvOS 앱을 기본 포커스 포커스를 받을 사용자 후 항목을 선택 하는 환경 간의 관계를 설정 하려면 연결 된 도메인 사용자 이름 및 암호를 지정 하는 사용량 필드 사용자 이름 및 암호를 제공합니다.

## <a name="focus-engine-enhancements"></a>포커스 엔진 향상 된 기능

tvOS 12 어떻게 렌더링 되며, 포커스 엔진과 상호 작용에 관계 없이 모든 앱을 허용 합니다. Siri 원격을 사용 하 여 사용자의 상호 작용을 통해 항목을 선택 가능한 포커스 변경 힌트, 포커스를 자연스럽 게 업데이트를 포커스 엔진 모든 앱을 사용 하 여 사용할 수 있습니다. UIKit의를 통해 사용자 지정 응용 프로그램에서 사용 됩니다 `IUIFocusItemContainer` 인터페이스를 `UIFocusMovementHint` 클래스는 `IUIFocusItemScrollableContainer` 인터페이스, 기타 관련된 클래스 및 메서드.

## <a name="vision-framework"></a>비전 프레임 워크

비전 프레임 워크에 다양 한 방향에서 얼굴을 검색할 수 있는 향상 된 얼굴 탐지기를 포함 합니다. 또한 요청 수정 이제 용도 특정 비전 framework 알고리즘 수정 버전을 선택 합니다.

## <a name="natural-language-framework"></a>자연 언어 프레임 워크

자연 언어 프레임 워크에는 응용 프로그램을을 다양 한 유형의 언어 분석을 수행할 수 있습니다. 예를 들어, 요소를 확인 하 고 텍스트 블록을 나타내는 언어를 결정 하려면 사용할 수 있습니다.

## <a name="deprecations"></a>결함

TvOS 12 사용 하 여 Apple에 사용 되지 않는 OpenGL ES [개발자 들에 게](https://developer.apple.com/tvos/whats-new/) 체제 미 설치 컴퓨터를 채택 해야 합니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) (비디오)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)