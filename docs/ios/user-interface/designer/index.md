---
title: IOS 디자이너를 사용 하 여 사용자 인터페이스 빌드
description: 이 문서에 디자이너를 사용 하 여 Xamarin iOS에 대 한 스토리 보드 및.xib 파일을 사용 하 여 앱의 사용자 인터페이스를 작성 하는 방법을 설명 합니다. 도구의 가용성, 기본 기능을 디자인할 수 있는 컨트롤에 설명 하 고 용도 대 한 연습을 제공 하는 문서로 연결 됩니다.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/31/2018
ms.openlocfilehash: 7c6529c539ab502fb6c13226acd18a57f8f6d58e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109746"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>IOS 디자이너를 사용 하 여 사용자 인터페이스 빌드

_IOS 용 Xamarin 디자이너에는 Mac 및 Visual Studio 용 Visual Studio를 사용 하 여 완전히 통합 되는 iOS 스토리 보드 및 Interface Builder 형식에 대 한 시각적 디자이너입니다. IOS 디자이너는 Mac 용 Visual Studio 또는 Xcode의 Interface Builder 외에도 Visual Studio에서 파일을 편집할 수 있도록 스토리 보드 및.xib 형식과 완벽 한 호환성을 유지 합니다. 또한 iOS 용 Xamarin 디자이너는 디자인 타임 편집기에서 렌더링 하는 사용자 지정 컨트롤과 같은 고급 기능을 지원 합니다._

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Mac 용 Visual Studio 디자이너에에서 iOS](images/designer-vsmac-sml.png "iOS 디자이너")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio 디자이너에에서 iOS](images/designer-vs.png "iOS 디자이너")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>가용성

IOS 용 Xamarin 디자이너는 Windows의 Visual Studio 2017 및 Mac 용 Visual Studio에서 사용할 수 있습니다.

이 가이드에서 다루는 내용에 익숙하다고 가정 합니다 [Xamarin.iOS 시작 가이드](~/ios/get-started/index.md)합니다.

## <a name="ios-designer-basicsintroductionmd"></a>[iOS Designer 기본 사항](introduction.md)

이 가이드에서는 Xamarin iOS 디자이너의 기능을 설명합니다. 디자이너를 사용 하 여 시각적으로 컨트롤을 배치 하는 방법 및 속성을 편집 하는 방법을 보여 주는 디자이너 기본 사항을 설명 합니다.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[디자인할 수 있는 컨트롤 개요](ios-designable-controls-overview.md)

이 가이드에서는 사용자 지정 컨트롤을 깊이에서 작성 된 방식 및 요구 사항을 디자인 화면에 렌더링할 충족 해야 합니다. 또한 디자인할 수 있는 컨트롤을 사용 하는 경우 발생할 수 있는 일반적인 문제를 디버그 하는 방법을 보여 줍니다.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[연습-iOS 디자이너를 사용 하 여 사용자 지정 컨트롤을 사용 하 여](ios-designable-controls-walkthrough.md)

이 문서에서는 사용자 지정 컨트롤을 만들고 iOS 디자이너에서 사용 하는 방법을 보여 주는 단계별 연습을 제공 합니다. 끌기/삭제 보기로 하므로 컨트롤 디자이너의 도구 상자에서 사용할 수 있도록 하는 방법을 보여 줍니다. 또한 디자인 타임 및 런타임 제대로 렌더링 컨트롤을 구현 하는 방법 뿐만 아니라 디자인 타임에 설정할 수 있는 속성을 만드는 방법을 보여 줍니다.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Xamarin iOS 디자이너를 사용 하 여 자동 레이아웃](designer-auto-layout.md)

이 가이드에서는 iOS 자동 레이아웃 및 iOS 디자이너에서 사용할 수 있는 새 제약 조건 워크플로 소개 합니다.
