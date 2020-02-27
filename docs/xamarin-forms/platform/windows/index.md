---
title: Windows 플랫폼 기능
description: 이 문서에서는 Xamarin.ios에서 사용할 수 있는 Windows 플랫폼 지원을 설명 합니다.
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 694ec24697937b114036eceab1cafd5aae3617d8
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635551"
---
# <a name="windows-platform-features"></a>Windows 플랫폼 기능

Windows 플랫폼용 Xamarin Forms 응용 프로그램을 개발 하려면 Visual Studio가 필요 합니다. [지원 되는 플랫폼 페이지](~/get-started/supported-platforms.md) 에는 필수 구성 요소에 대 한 자세한 정보가 포함 되어 있습니다.

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>플랫폼 사양

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

UWP (유니버설 Windows 플랫폼)에서 Xamarin.ios 뷰, 페이지 및 레이아웃에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`VisualElement`](xref:Xamarin.Forms.VisualElement)에 대 한 액세스 키를 설정 합니다. 자세한 내용은 [Windows의 Visualelement 액세스 키](visualelement-access-keys.md)를 참조 하세요.
- 지원 되는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)에서 레거시 색 모드를 사용 하지 않도록 설정 합니다. 자세한 내용은 [Windows의 Visualelement 레거시 색 모드](legacy-color-mode.md)를 참조 하세요.

UWP의 Xamarin 폼 보기에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor)및 [`Label`](xref:Xamarin.Forms.Label) 인스턴스의 텍스트 콘텐츠에서 읽는 순서를 검색 합니다. 자세한 내용은 [InputView 읽기용 Order On Windows](inputview-reading-order.md)을 참조 하세요.
- [`ListView`](xref:Xamarin.Forms.ListView)에서 탭 제스처 지원 사용. 자세한 내용은 [ListView SelectionMode On Windows](listview-selectionmode.md)을 참조 하세요.
- 변경할 `RefreshView`의 끌어오기 방향을 사용 하도록 설정 합니다. 자세한 내용은 [Windows의 Refreshview 끌어오기 방향](refreshview-pulldirection.md)을 참조 하세요.
- 맞춤법 검사 엔진과 상호 작용 하는 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 사용 자세한 내용은 [Windows의 Searchbar Spell Check](searchbar-spell-check.md)를 참조 하세요.
- [`WebView`](xref:Xamarin.Forms.WebView) 를 사용 하 여 UWP 메시지 대화 상자에서 JavaScript 경고 표시 자세한 내용은 [Windows의 웹 보기 JavaScript 경고](webview-javascript-alert.md)를 참조 하세요.

UWP의 Xamarin Forms 페이지에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 탐색 모음 축소 자세한 내용은 [Windows의 MasterDetailPage 탐색 모음](masterdetailpage-navigation-bar.md)을 참조 하세요.
- 도구 모음 배치 옵션을 설정 합니다. 자세한 내용은 [Windows의 페이지 도구 모음 배치](page-toolbar-placement.md)를 참조 하세요.
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 도구 모음에 페이지 아이콘이 표시 되도록 설정 자세한 내용은 [Windows의 TabbedPage 아이콘](tabbedpage-icons.md)을 참조하세요.

UWP의 Xamarin.ios [`Application`](xref:Xamarin.Forms.Application) 클래스에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 이미지 자산이 로드 되는 프로젝트의 디렉터리를 지정 합니다. 자세한 내용은 [Windows의 기본 이미지 디렉터리](default-image-directory.md)를 참조 하십시오.

## <a name="platform-support"></a>플랫폼 지원

Visual Studio에서 사용할 수 있는 Xamarin Forms 템플릿에는 UWP (유니버설 Windows 플랫폼) 프로젝트가 포함 되어 있습니다.

> [!NOTE]
> Xamarin.ios 1.x와 2.x는 _Windows Phone 8 개의 Silverlight_, _Windows Phone 8.1_및 _Windows 8.1_ 응용 프로그램 개발을 지원 합니다. 그러나 이러한 프로젝트 형식은 더 이상 사용 되지 않습니다.

## <a name="getting-started"></a>시작

Visual Studio에서 **파일 > 새 > 프로젝트** 로 이동 하 고 **플랫폼 간 > 빈 앱 (xamarin.ios)** 템플릿 중 하나를 선택 하 여 시작 합니다.

이전 Xamarin.ios 솔루션 또는 macOS에서 만든 이러한 솔루션에는 위에 나열 된 모든 Windows 프로젝트가 없지만 수동으로 추가 해야 합니다. 대상으로 하려는 Windows 플랫폼이 솔루션에 아직 없는 경우 [설치 지침](installation/index.md) 을 방문 하 여 원하는 windows 프로젝트 형식/s를 추가 합니다.

## <a name="samples"></a>샘플

Charles Petzold의 책에 대 한 [모든 샘플에는](https://github.com/xamarin/xamarin-forms-book-preview-2) [*xamarin.ios를 사용 하 여 Mobile Apps 만들기*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) 유니버설 Windows 플랫폼 (Windows 10) 프로젝트가 포함 되어 있습니다.

["Scott Hanselman" demo 앱](https://github.com/jamesmontemagno/Hanselman.Forms) 은 별도로 제공 되며 Apple Watch 및 Android 출시 프로젝트도 포함 합니다 (각각 Xamarin.ios 및 xamarin.ios를 사용 하 여 Xamarin. Forms는 해당 플랫폼에서 실행 되지 않음).

## <a name="related-links"></a>관련 링크

- [Windows 프로젝트 설정](~/xamarin-forms/platform/windows/installation/index.md)
