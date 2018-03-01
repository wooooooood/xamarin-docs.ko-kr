---
title: "소개"
description: "Xamarin.Forms 컨트롤 템플릿은 쉽게 테마 및 re 테마 하는 기능에서 런타임에 응용 프로그램 페이지를 제공합니다. 이 문서에 컨트롤 템플릿에 대 한 소개를 제공합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: c3973b94168706e047ee5c312a8823503c9b0fd8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction"></a>소개

_Xamarin.Forms 컨트롤 템플릿은 쉽게 테마 및 re 테마 하는 기능에서 런타임에 응용 프로그램 페이지를 제공합니다. 이 문서에 컨트롤 템플릿에 대 한 소개를 제공합니다._

와 같은 서로 다른 속성은 컨트롤에는 `BackgroundColor` 및 `TextColor`, 컨트롤의 모양을 정의할 수입니다. 사용 하 여 이러한 속성을 설정할 수 있습니다 [스타일](~/xamarin-forms/user-interface/styles/index.md)를 기본 테마를 구현 하는 런타임 시 변경할 수 있습니다. 그러나 스타일을 명확히 구분 페이지의 모양 및 해당 콘텐츠를 유지 하지 않으며 이러한 속성을 설정 하 여 변경한 내용을 제한 됩니다.

컨트롤 템플릿 페이지의 모양 및 해당 콘텐츠를 쉽게 테마를 적용할 수 있는 페이지를 만들 사용 명확한 구분을 제공 합니다. 예를 들어 응용 프로그램에 어두운 테마와 밝은 테마를 제공 하는 응용 프로그램 수준 컨트롤 템플릿을 포함할 수 있습니다. 각 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 응용 프로그램에 테마를 적용할 수는 각 페이지에 표시 되는 콘텐츠를 변경 하지 않고 컨트롤 템플릿 중 하나를 적용 하 여 합니다. 또한 제어 템플릿에서 제공 하는 테마는 컨트롤의 속성 변경로 제한 되지 않습니다. 테마를 구현 하는 컨트롤을 변경할 수 있습니다.

## <a name="creating-a-controltemplate"></a>ControlTemplate 만들기

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 페이지 또는 뷰 모양을 지정 하 고 루트 레이아웃을 포함 하 고 서식 파일을 구현 하는 컨트롤 레이아웃 내에서. 일반적으로 `ControlTemplate` 를 이용 하는 [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) 페이지 또는 보기에서 표시할 콘텐츠 표시 되는 위치를 표시 합니다. 페이지 또는 뷰를 사용 하 여 `ControlTemplate` 에서 표시할 콘텐츠를 정의 합니다는 `ContentPresenter`합니다. 다음 다이어그램에서는 한 `ControlTemplate` 포함 하 여 컨트롤의 수를 포함 하는 페이지는 `ContentPresenter` 파란색 사각형에 의해 표시:

![](introduction-images/control-template.png "페이지에 대 한 컨트롤 템플릿")

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 설정 하 여 형식에 적용할 수 자신의 `ControlTemplate` 속성:

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

경우는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 생성 되어 할당 된 이러한 형식에 모든 기존 모양에 정의 된 모양으로 바뀝니다는 `ControlTemplate`합니다. 모양을 사용 하 여 설정 뿐만 아니라 또한는 `ControlTemplate` 테마는 기능을 확장 하는 속성, 컨트롤 템플릿 스타일을 더 이상 사용 하 여 적용할 수도 있습니다.

> [!NOTE]
>  *이란는 `TemplatedPage` 및 `TemplatedView` 형식?* `TemplatedPage` 기본 클래스에 대 한 `ContentPage`, Xamarin.Forms에서 제공 하는 가장 기본적인 페이지 형식입니다. 와 달리 `ContentPage`, `TemplatedPage` 없는 `Content` 속성입니다. 따라서 콘텐츠 직접에 추가할 수 없습니다는 `TemplatedPage` 인스턴스. 에 대 한 컨트롤 템플릿을 설정 하 여 콘텐츠 대신 추가 되는 `TemplatedPage` 인스턴스. 마찬가지로, `TemplatedView` 의 기본 클래스에 대 한 `ContentView`합니다. 와 달리 `ContentView`, `TemplatedView` 없는 `Content` 속성입니다. 따라서 콘텐츠 직접에 추가할 수 없습니다는 `TemplatedView` 인스턴스. 에 대 한 컨트롤 템플릿을 설정 하 여 콘텐츠 대신 추가 되는 `TemplatedView` 인스턴스.

Xaml에서 및 C#에서 컨트롤 서식 파일을 만들 수 있습니다.

- 에 정의 된 XAML에서 만든 컨트롤 템플릿은 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 에 할당 된는 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 컬렉션 페이지의 또는 보다 일반적으로 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) 응용 프로그램의 컬렉션입니다.
- C#에서 만들어진 컨트롤 템플릿은 일반적으로 페이지의 클래스 또는 전체적으로 액세스할 수 있는 클래스에서 정의 됩니다.

정의할 위치를 선택는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 인스턴스에 사용할 수 있는 위치에 영향을 미칩니다.

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 페이지 수준에서 정의 된 인스턴스 페이지에만 적용할 수 있습니다.
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 응용 프로그램 수준에서 정의 된 인스턴스는 응용 프로그램에 걸쳐 있는 페이지에 적용할 수 있습니다.

컨트롤 템플릿 보기 계층 구조의 하위 높을수록를 정의 하는 것 보다 우선 합니다. 예를 들어 한 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 라는 `DarkTheme` 에서 정의 된 페이지 수준 응용 프로그램 수준에서 정의 하는 동일 하 게 명명 된 템플릿 보다 우선 합니다. 따라서 응용 프로그램에서 각 페이지에 적용할 테마를 정의 하는 컨트롤 템플릿은 응용 프로그램 수준에서 정의 되어야 합니다.


## <a name="related-links"></a>관련 링크

- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
