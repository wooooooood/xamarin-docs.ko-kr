---
title: IOS Designer를 사용 하 여 사용자 인터페이스 빌드
description: 이 문서에서는 Xamarin Designer for iOS를 사용 하 여 스토리 보드 및 xib 파일을 사용 하 여 앱의 사용자 인터페이스를 빌드하는 방법을 설명 합니다. 도구의 가용성, 기본 기능, 디자인할 수 있는 컨트롤을 설명 하 고 사용 연습을 제공 하는 문서에 연결 됩니다.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/31/2018
ms.openlocfilehash: 157e16da2c524029c29e767cd6b3e5eb550a2389
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021757"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>IOS Designer를 사용 하 여 사용자 인터페이스 빌드

_Xamarin Designer for iOS는 iOS Storyboard의 비주얼 디자이너 이며 Mac용 Visual Studio와 Visual Studio와 완전히 통합 된 Interface Builder 형식입니다. IOS Designer는 스토리 보드 및 xib 형식과 완전히 호환 되므로 Xcode의 Interface Builder 외에도 Mac용 Visual Studio 또는 Visual Studio에서 파일을 편집할 수 있습니다. 또한 Xamarin Designer for iOS은 편집기에서 디자인 타임에 렌더링 하는 사용자 지정 컨트롤과 같은 고급 기능을 지원 합니다._

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![iOS Designer Mac용 Visual Studio](images/designer-vsmac-sml.png "IOS 디자이너")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio의 iOS 디자이너](images/designer-vs.png "IOS 디자이너")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>가용성

Xamarin Designer for iOS는 Windows의 Visual Studio 2017 및 Mac용 Visual Studio에서 사용할 수 있습니다.

이러한 가이드에서는 [Xamarin.ios 시작 가이드](~/ios/get-started/index.md)에 설명 된 내용에 대해 잘 알고 있는 것으로 가정 합니다.

## <a name="ios-designer-basicsintroductionmd"></a>[iOS Designer 기본 사항](introduction.md)

이 가이드에서는 Xamarin iOS 디자이너의 기능에 대해 설명 합니다. 디자이너를 사용 하 여 컨트롤을 시각적으로 레이아웃 하는 방법 및 속성을 편집 하는 방법을 보여 주는 디자이너 기본 사항에 대해 설명 합니다.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[디자인 가능한 컨트롤 개요](ios-designable-controls-overview.md)

이 가이드는 사용자 지정 컨트롤, 생성 방법 및 디자인 화면에서 렌더링 하기 위해 충족 해야 하는 요구 사항에 대해 자세히 살펴봅니다. 또한 디자인 가능한 컨트롤을 사용 하는 경우 발생할 수 있는 일반적인 문제를 디버그 하는 방법을 보여 줍니다.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[연습-iOS 디자이너에서 사용자 지정 컨트롤 사용](ios-designable-controls-walkthrough.md)

이 문서에서는 사용자 지정 컨트롤을 만들고 iOS 디자이너에서 사용 하는 방법을 보여 주는 단계별 연습을 제공 합니다. 디자이너의 도구 상자에서 컨트롤을 사용할 수 있도록 하는 방법을 보여 줍니다. 그러면 뷰로 끌어서 놓을 수 있습니다. 또한 디자인 타임 및 런타임에 적절히 렌더링 되는 컨트롤을 구현 하 고 디자인 타임에 설정할 수 있는 속성을 만드는 방법을 보여 줍니다.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Xamarin iOS Designer를 사용 하 여 자동 레이아웃](designer-auto-layout.md)

이 가이드에서는 ios 자동 레이아웃 및 iOS 디자이너에서 사용할 수 있는 새 제약 조건 워크플로를 소개 합니다.
