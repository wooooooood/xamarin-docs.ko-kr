---
title: Xamarin.Forms 컨트롤 템플릿 소개
description: Xamarin.Forms 컨트롤 템플릿은 런타임 시 애플리케이션 페이지에 쉽게 테마를 적용하고 다시 테마를 적용하는 기능을 제공합니다. 이 문서에는 컨트롤 템플릿에 대한 소개를 제공합니다.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994427"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Xamarin.Forms 컨트롤 템플릿 소개

_Xamarin.Forms 컨트롤 템플릿은 런타임 시 애플리케이션 페이지에 쉽게 테마를 적용하고 다시 테마를 적용하는 기능을 제공합니다. 이 문서에는 컨트롤 템플릿에 대한 소개를 제공합니다._

컨트롤에는 `BackgroundColor` 및 `TextColor`와 같이 다양하게 컨트롤의 모양을 정의할 수 있는 다양한 속성이 있습니다. 이러한 속성은 [스타일](~/xamarin-forms/user-interface/styles/index.md)을 사용하여 설정할 수 있으며 기본 테마를 구현하기 위해 런타임 시 변경할 수 있습니다. 그러나 스타일은 페이지의 모양과 콘텐츠를 지속적으로 명확하게 분리하지 않으며 이러한 속성을 설정하여 변경할 수 있는 부분은 제한적입니다.

컨트롤 템플릿은 페이지의 모양과 콘텐츠를 명확하게 분리하기 때문에 테마를 쉽게 지정할 수 있는 페이지를 만들 수 있습니다. 예를 들어, 애플리케이션에는 밝은 테마 및 어두운 테마를 제공하는 애플리케이션 수준 컨트롤 템플릿이 있을 수 있습니다. 각 페이지에 표시되는 콘텐츠를 변경하지 않고 컨트롤 템플릿 중 하나를 적용하여 애플리케이션에 있는 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 테마를 적용할 수 있습니다. 또한 컨트롤 템플릿에서 제공된 테마는 컨트롤 속성을 변경하는 것으로 제한되지 않습니다. 테마를 구현하는 데 사용된 컨트롤도 변경할 수 있습니다.

## <a name="creating-a-controltemplate"></a>ControlTemplate 만들기

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)은 페이지 또는 보기의 모양을 지정하고, 루트 레이아웃을 포함하며 레이아웃 내에서 템플릿을 구현하는 컨트롤을 포함합니다. 일반적으로 `ControlTemplate`은 [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)를 활용하여 페이지 또는 보기로 표시할 콘텐츠가 나타나는 위치를 표시합니다. `ControlTemplate`을 사용하는 페이지 또는 보기는 `ContentPresenter`에 의해 표시될 콘텐츠를 정의합니다. 다음 다이어그램은 파란색 직사각형으로 표시된 `ContentPresenter`를 비롯하여 여러 컨트롤이 포함된 페이지의 `ControlTemplate`을 보여 줍니다.

![](introduction-images/control-template.png "페이지에 대한 컨트롤 템플릿")

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)은 해당 `ControlTemplate` 속성을 설정하여 다음 형식에 적용할 수 있습니다.

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)이 생성되어 이러한 형식에 할당되면 기존 모양이 `ControlTemplate`에 정의된 모양으로 바뀝니다. 또한 `ControlTemplate` 속성을 사용하여 모양을 설정하는 것은 물론 테마 기능을 더욱 확장하는 스타일을 사용해서도 컨트롤 템플릿을 적용할 수 있습니다.

> [!NOTE]
>  *이란 합니다 `TemplatedPage` 고 `TemplatedView` 형식?* `TemplatedPage`는 `ContentPage`의 기본 클래스로, Xamarin.Forms에서 제공한 가장 기본적인 페이지 형식입니다. `ContentPage`와 달리, `TemplatedPage`에는 `Content` 속성이 없습니다. 따라서 콘텐츠를 `TemplatedPage` 인스턴스에 직접 추가할 수 없습니다. 대신, `TemplatedPage` 인스턴스에 대한 컨트롤 템플릿을 설정하여 콘텐츠를 추가합니다. 마찬가지로 `TemplatedView`는 `ContentView`에 대한 기본 클래스입니다. `ContentView`와 달리, `TemplatedView`에는 `Content` 속성이 없습니다. 따라서 콘텐츠를 `TemplatedView` 인스턴스에 직접 추가할 수 없습니다. 대신, `TemplatedView` 인스턴스에 대한 컨트롤 템플릿을 설정하여 콘텐츠를 추가합니다.

컨트롤 템플릿은 XAML 및 C#으로 생성할 수 있습니다.

- XAML로 생성된 컨트롤 템플릿은 페이지의 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 컬렉션(더 일반적으로는 애플리케이션의 [`Resources`](xref:Xamarin.Forms.Application.Resources) 컬렉션)에 할당된 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 정의됩니다.
- C#으로 생성된 컨트롤 템플릿은 일반적으로 페이지의 클래스 또는 글로벌로 액세스할 수 있는 클래스에 정의됩니다.

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 인스턴스를 정의할 위치를 선택하면 사용할 수 있는 위치가 결정됩니다.

- 페이지 수준에서 정의된 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 인스턴스는 페이지에만 적용할 수 있습니다.
- 애플리케이션 수준에서 정의한 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 인스턴스는 애플리케이션 전체 페이지에 적용할 수 있습니다.

보기 계층 구조에서 하위 컨트롤 템플릿은 보다 상위에 정의된 것보다 우선합니다. 예를 들어 페이지 수준에서 정의된 `DarkTheme`라는 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)은 애플리케이션 수준에서 정의된 동일한 이름의 템플릿보다 우선합니다. 따라서 애플리케이션의 각 페이지에 적용할 테마를 정의하는 컨트롤 템플릿은 애플리케이션 수준에서 정의되어야 합니다.


## <a name="related-links"></a>관련 링크

- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
