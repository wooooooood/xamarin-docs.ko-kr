---
title: Windows 플랫폼 기능
description: 이 문서에서는 Xamarin.Forms에서 사용할 수 있는 Windows 플랫폼 지원에 설명 합니다.
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
ms.openlocfilehash: b1ba6a9f2ee15cf078658b49124c1d9203a3f3d9
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207949"
---
# <a name="windows-platform-features"></a>Windows 플랫폼 기능

Windows 플랫폼용 Xamarin.Forms 응용 프로그램 개발에 Visual Studio가 필요 합니다. 합니다 [요구 사항 페이지](~/xamarin-forms/get-started/installation.md) 필수 조건에 대 한 자세한 정보를 포함 합니다.

![](images/allhanselman.png "Windows를 실행 하는 Xamarin.Forms 응용 프로그램")

## <a name="platform-specifics"></a>플랫폼별

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

Xamarin.Forms 뷰, 페이지 및 유니버설 Windows 플랫폼 (UWP)의 레이아웃에 대해 다음 플랫폼 특정 기능을 제공 됩니다.

- 설정에 대 한 액세스 키를 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [VisualElement 액세스 키 Windows](#visualelement-accesskeys)합니다.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [VisualElement 레거시 색 모드의 경우 Windows](#legacy-color-mode)합니다.

Xamarin.Forms 보기 UWP에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 검색에서 텍스트 콘텐츠를 읽는 순서 [ `Entry` ](xref:Xamarin.Forms.Entry)를 [ `Editor` ](xref:Xamarin.Forms.Editor), 및 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스. 자세한 내용은 [Windows에 있던 InputView 읽는 순서](#inputview-readingorder)합니다.
- 탭 제스처 지원을 사용 하도록 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 자세한 내용은 [Windows에서 ListView SelectionMode](#listview-selectionmode)합니다.
- 사용 하도록 설정 된 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 맞춤법 검사 엔진과 상호 작용을 합니다. 자세한 내용은 [Windows에서 SearchBar Spell Check](#searchbar-spellcheck)합니다.
- 사용을 [ `WebView` ](xref:Xamarin.Forms.WebView) UWP 메시지 대화 상자에서 JavaScript 경고를 표시 하도록 합니다. 자세한 내용은 [Windows에서 WebView JavaScript 경고](#webview-javascript-alert)합니다.

다음 플랫폼 특정 기능을 UWP에 Xamarin.Forms 페이지에 대 한 제공 됩니다.

- 축소 된 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 탐색 모음입니다. 자세한 내용은 [MasterDetailPage 탐색 모음의 Windows](#collapsable_navigation_bar)합니다.
- 도구 모음 배치 옵션을 설정 합니다. 자세한 내용은 [Windows에서 페이지 도구 모음 배치](#toolbar_placement)합니다.
- 페이지에 표시 되는 아이콘을 사용 하도록 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 도구 모음입니다. 자세한 내용은 [TabbedPage Windows 아이콘](#tabbedpage-icons)합니다.

## <a name="platform-support"></a>플랫폼 지원

Visual Studio에서 Xamarin.Forms 템플릿을 유니버설 Windows 플랫폼 (UWP) 프로젝트를 포함 합니다.

> [!NOTE]
> Xamarin.Forms 1.x 및 2.x 지원을 _Windows Phone 8 Silverlight_하십시오 _Windows Phone 8.1_, 및 _Windows 8.1_ 응용 프로그램 개발. 그러나 이러한 프로젝트 형식은 되지 않습니다.

## <a name="getting-started"></a>시작

로 이동 **파일 > 새로 만들기 > 프로젝트** Visual Studio에서 중 하나를 선택 합니다 **플랫폼 간 > 비어 있는 앱 (Xamarin.Forms)** 시작 하려면 템플릿.

이전 Xamarin.Forms 솔루션 또는 macOS에서 만든는 위에 나열 된 모든 Windows 프로젝트 (없지만 수동으로 추가 해야) 합니다. 대상으로 하려는 Windows 플랫폼 부하량 솔루션에 있지 않은 경우는 [설치 지침](installation/index.md) 원하는 Windows 프로젝트 형식/s를 추가 합니다.

## <a name="samples"></a>샘플

[모든 샘플](https://github.com/xamarin/xamarin-forms-book-preview-2) 에 대 한 Charles Petzold의 저서 [ *Creating Mobile Apps with Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) 유니버설 Windows 플랫폼 (Windows 10) 프로젝트를 포함 합니다.

합니다 ["Scott Hanselman" 데모 앱](https://github.com/jamesmontemagno/Hanselman.Forms) 는 개별적으로 사용할 수 있으며 또한 Apple Watch 및 Android Wear 프로젝트 (Xamarin.iOS 및 Xamarin.Android를 사용 하는 각각, Xamarin.Forms 실행 되지 않습니다 서비스도).

## <a name="related-links"></a>관련 링크

- [Windows 프로젝트 설정](~/xamarin-forms/platform/windows/installation/index.md)
