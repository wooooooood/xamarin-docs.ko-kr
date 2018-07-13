---
title: Xamarin.Forms 컨트롤 템플릿 소개
description: Xamarin.Forms 컨트롤 템플릿은 런타임에 응용 프로그램 페이지 쉽게 테마와 다시 테마 수를 제공합니다. 이 문서에서는 컨트롤 템플릿 소개 합니다.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994427"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Xamarin.Forms 컨트롤 템플릿 소개

_Xamarin.Forms 컨트롤 템플릿은 런타임에 응용 프로그램 페이지 쉽게 테마와 다시 테마 수를 제공합니다. 이 문서에서는 컨트롤 템플릿 소개 합니다._

와 같은 다른 속성을 컨트롤에는 `BackgroundColor` 및 `TextColor`, 컨트롤의 모양의 측면을 정의 하는 수 있습니다. 사용 하 여 이러한 속성을 설정할 수 있습니다 [스타일](~/xamarin-forms/user-interface/styles/index.md), 기본 테마를 구현 하는 런타임 시 변경할 수 있습니다. 그러나 스타일에는 페이지의 모양과 내용을 명확한 구분을 유지 하지 하 고 이러한 속성을 설정 하 여 변경한 내용을 제한 됩니다.

컨트롤 템플릿 페이지의 모양과 내용을 쉽게 테마가 지정 된 수는 페이지를 사용 하도록 설정 하므로 명확한 구분을 제공 합니다. 예를 들어, 응용 프로그램에는 밝은 테마 및 어두운 테마를 제공 하는 응용 프로그램 수준 컨트롤 템플릿을 포함할 수 있습니다. 각 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 응용 프로그램에서 테마를 적용할 수 페이지 별로 표시 되는 콘텐츠를 변경 하지 않고 컨트롤 템플릿 중 하나를 적용 하 여 합니다. 또한 컨트롤 템플릿에서 제공 하는 테마는 컨트롤의 속성을 변경 제한 되지 않습니다. 또한 테마를 구현 하는 데 사용 하는 컨트롤을 변경할 수 있습니다.

## <a name="creating-a-controltemplate"></a>ControlTemplate 만들기

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 페이지 또는 보기의 모양을 지정 하 고 루트 레이아웃을 포함 하 고 템플릿을 구현 하는 컨트롤은 레이아웃 내에서. 일반적으로 `ControlTemplate` 를 이용 하는 [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) 페이지 또는 보기에서 표시할 콘텐츠가 표시 되는 위치를 표시 합니다. 페이지 또는 보기를 사용 하는 `ControlTemplate` 의해 표시 되는 콘텐츠를 정의 합니다는 `ContentPresenter`합니다. 다음 다이어그램은는 `ControlTemplate` 비롯 하 여 컨트롤의 수를 포함 하는 페이지는 `ContentPresenter` 파란색 사각형으로 표시:

![](introduction-images/control-template.png "페이지 컨트롤 템플릿")

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 설정 하 여 형식에 적용할 수는 `ControlTemplate` 속성:

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

경우는 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 생성 되어 할당 된 이러한 형식에 모든 기존 모양에 정의 된 모양으로 바뀝니다는 `ControlTemplate`합니다. 사용 하 여 모양을 설정 하는 또한 뿐만 아니라는 `ControlTemplate` 테마 기능을 확장 하는 속성, 컨트롤 템플릿 스타일을 추가 사용 하 여 적용할 수도 있습니다.

> [!NOTE]
>  *이란 합니다 `TemplatedPage` 고 `TemplatedView` 형식?* `TemplatedPage` 에 대 한 기본 클래스인 `ContentPage`, 고 Xamarin.Forms 제공한 가장 기본적인 페이지 형식입니다. 와 달리 `ContentPage`, `TemplatedPage` 없는 `Content` 속성입니다. 따라서 콘텐츠 직접에 추가할 수 없습니다는 `TemplatedPage` 인스턴스. 컨트롤 템플릿을 설정 하 여 콘텐츠가 추가 되었는지 알려면 대신는 `TemplatedPage` 인스턴스. 마찬가지로 `TemplatedView` 에 대 한 기본 클래스인 `ContentView`합니다. 와 달리 `ContentView`, `TemplatedView` 없는 `Content` 속성입니다. 따라서 콘텐츠 직접에 추가할 수 없습니다는 `TemplatedView` 인스턴스. 컨트롤 템플릿을 설정 하 여 콘텐츠가 추가 되었는지 알려면 대신는 `TemplatedView` 인스턴스.

XAML 및 C# 컨트롤 서식 파일을 만들 수 있습니다.

- 에 정의 된 XAML에서 만든 컨트롤 템플릿을 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 에 할당 된 합니다 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 컬렉션 페이지의 또는 보다 일반적으로 [ `Resources` ](xref:Xamarin.Forms.Application.Resources) 응용 프로그램의 컬렉션입니다.
- 페이지의 클래스에서 또는 전역적으로 액세스할 수 있는 클래스에 일반적으로 C#에서 만든 컨트롤 템플릿 정의 됩니다.

정의할 위치를 선택는 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 영향을 사용할 수 있는 인스턴스:

- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 페이지 수준에서 정의 된 인스턴스 페이지에만 적용할 수 있습니다.
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 응용 프로그램 수준에서 정의 된 인스턴스는 응용 프로그램에 걸쳐 있는 페이지에 적용할 수 있습니다.

뷰 계층 구조의 하위 컨트롤 템플릿을를 더 높은 정의 보다 우선 합니다. 예를 들어, 한 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 라는 `DarkTheme` 에서 정의 되는 페이지 수준 응용 프로그램 수준에서 정의 하는 동일 하 게 명명 된 템플릿 우선적으로 적용 됩니다. 따라서 응용 프로그램에서 각 페이지에 적용할 테마를 정의 하는 컨트롤 템플릿의 응용 프로그램 수준에서 정의 되어야 합니다.


## <a name="related-links"></a>관련 링크

- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
