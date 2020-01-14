---
title: Xamarin.Forms Shell 페이지 구성
description: Shell 클래스는 Xamarin.Forms Shell 애플리케이션에서 페이지의 모양을 구성하는 데 사용할 수 있는 연결된 속성을 정의합니다. 여기에는 페이지 색 설정, 탐색 모음을 사용하지 않도록 설정, 탭 표시줄을 사용하지 않도록 설정 및 탐색 모음에 뷰 표시가 포함됩니다.
ms.prod: xamarin
ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2019
ms.openlocfilehash: e207949d607219393ffeb51fce818ddfb68ae344
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489910"
---
# <a name="xamarinforms-shell-page-configuration"></a>Xamarin.Forms Shell 페이지 구성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

`Shell` 클래스는 Xamarin.Forms Shell 애플리케이션에서 페이지의 모양을 구성하는 데 사용할 수 있는 연결된 속성을 정의합니다. 여기에는 페이지 색 설정, 탐색 모음을 사용하지 않도록 설정, 탭 표시줄을 사용하지 않도록 설정 및 탐색 모음에 뷰 표시가 포함됩니다.

## <a name="set-page-colors"></a>페이지 색 설정

`Shell` 클래스는 Shell 애플리케이션에서 페이지 색을 설정하는 데 사용할 수 있는 다음 연결된 속성을 정의합니다.

- Shell 크롬에서 배경색을 정의하는 `Color` 형식의 `BackgroundColor`. 셸 콘텐츠 뒤의 색은 채워지지 않습니다.
- 사용할 수 없는 텍스트 및 아이콘을 음영 처리할 색을 정의하는 `Color` 형식의 `DisabledColor`.
- 텍스트 및 아이콘을 음영 처리할 색을 정의하는 `Color` 형식의 `ForegroundColor`.
- 현재 페이지의 제목에 사용되는 색을 정의하는 `Color` 형식의 `TitleColor`.
- 셸 크롬에서 선택되지 않은 텍스트 및 아이콘에 사용되는 색을 정의하는 `Color` 형식의 `UnselectedColor`.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며 XAML 스타일을 사용하여 스타일 지정됩니다. 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다. CSS(CSS 스타일시트)를 사용하여 속성을 설정할 수도 있습니다. 자세한 내용은 [Xamarin.Forms 셸 특정 속성](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)을 참조하세요.

> [!NOTE]
> 탭 색을 정의할 수 있도록 설정하는 속성도 있습니다. 자세한 내용은 [탭 모양](tabs.md#tab-appearance)을 참조하세요.

다음 XAML에서는 서브클래싱된 `Shell` 클래스에서의 색 속성 설정을 보여 줍니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell"
       BackgroundColor="#455A64"
       ForegroundColor="White"
       TitleColor="White"
       DisabledColor="#B4FFFFFF"
       UnselectedColor="#95FFFFFF">

</Shell>
```

이 예제에서는 페이지 수준에서 재정의되지 않은 경우 색 값이 Shell 애플리케이션의 모든 페이지에 적용됩니다.

색 속성은 연결된 속성이므로 개별 페이지에서 설정하여 해당 페이지의 색을 설정할 수도 있습니다.

```xaml
<ContentPage ...
             Shell.BackgroundColor="Gray"
             Shell.ForegroundColor="White"
             Shell.TitleColor="Blue"
             Shell.DisabledColor="#95FFFFFF"
             Shell.UnselectedColor="#B4FFFFFF">

</ContentPage>
```

또는 XAML 스타일을 사용하여 색 속성을 설정할 수 있습니다.

```xaml
<Style x:Key="DomesticShell"
       TargetType="Element" >
    <Setter Property="Shell.BackgroundColor"
            Value="#039BE6" />
    <Setter Property="Shell.ForegroundColor"
            Value="White" />
    <Setter Property="Shell.TitleColor"
            Value="White" />
    <Setter Property="Shell.DisabledColor"
            Value="#B4FFFFFF" />
    <Setter Property="Shell.UnselectedColor"
            Value="#95FFFFFF" />
</Style>
```

XAML 스타일에 대한 자세한 내용은 [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)을 참조하세요.

## <a name="enable-navigation-bar-shadow"></a>탐색 모음 섀도 사용

`Shell` 클래스는 탐색 모음에 섀도가 포함되는 여부를 제어하는 `bool` 형식의 `NavBarHasShadow` 연결된 속성을 정의합니다. 기본적으로 속성의 값은 `false`입니다.

이 속성은 서브클래싱된 `Shell` 개체에서 설정할 수 있지만, 탐색 모음 섀도를 사용하려는 모든 페이지에서도 설정할 수 있습니다. 예를 들어 다음 XAML은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에서 탐색 모음 섀도를 사용할 수 있음을 보여줍니다.

```xaml
<ContentPage ...
             Shell.NavBarHasShadow="true">
    ...
</ContentPage>
```

그러면 탐색 모음 섀도가 사용 설정됩니다.

## <a name="disable-the-navigation-bar"></a>탐색 모음을 사용하지 않도록 설정

`Shell` 클래스는 페이지가 표시될 때 탐색 모음이 보이도록 할지 여부를 제어하는 `bool` 형식의 `NavBarIsVisible` 연결된 속성을 정의합니다. 기본적으로 속성의 값은 `true`입니다.

이 속성은 서브클래싱된 `Shell` 개체에서 설정할 수 있지만, 일반적으로 탐색 모음이 보이지 않도록 하려는 모든 페이지에 설정됩니다. 예를 들어 다음 XAML은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에서 탐색 모음을 사용할 수 없음을 보여 줍니다.

```xaml
<ContentPage ...
             Shell.NavBarIsVisible="false">
    ...
</ContentPage>
```

따라서 페이지가 표시되면 탐색 모음은 보이지 않게 됩니다.

![iOS 및 Android의 탐색 모음이 보이지 않는 셸 페이지의 스크린샷](configuration-images/navigationbar-invisible.png "탐색 모음이 보이지 않는 셸 페이지")

## <a name="disable-the-tab-bar"></a>탭 표시줄을 사용하지 않음

`Shell` 클래스는 페이지가 표시될 때 탭 표시줄이 보이도록 할지 여부를 제어하는 `bool` 형식의 `TabBarIsVisible` 연결된 속성을 정의합니다. 기본적으로 속성의 값은 `true`입니다.

이 속성은 서브클래싱된 `Shell` 개체에서 설정할 수 있지만, 일반적으로 탭 표시줄이 보이지 않도록 하려는 모든 페이지에 설정됩니다. 예를 들어 다음 XAML은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에서 탭 표시줄을 사용할 수 없음을 보여 줍니다.

```xaml
<ContentPage ...
             Shell.TabBarIsVisible="false">
    ...
</ContentPage>
```

따라서 페이지가 표시되면 탭 표시줄은 보이지 않게 됩니다.

![iOS 및 Android의 탭 모음이 보이지 않는 셸 페이지의 스크린샷](configuration-images/tabbar-invisible.png "탭 모음이 보이지 않는 셸 페이지")

## <a name="display-views-in-the-navigation-bar"></a>탐색 모음에서 보기 표시

`Shell` 클래스는 Xamarin.Forms [`View`](xref:Xamarin.Forms.View)를 탐색 모음에 표시할 수 있도록 하는 `View` 형식의 `TitleView` 연결된 속성을 정의합니다.

이 속성은 서브클래싱된 `Shell` 개체에서 설정할 수 있지만, 탐색 모음에 보기를 표시하려는 모든 페이지에서도 설정할 수 있습니다. 예를 들어 다음 XAML은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)의 탐색 모음에 [`Image`](xref:Xamarin.Forms.Image)가 표시된 것을 보여 줍니다.

```xaml
<ContentPage ...>
    <Shell.TitleView>
        <Image Source="xamarin_logo.png"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Shell.TitleView>
    ...
</ContentPage>
```

그 결과, 이미지가 페이지의 탐색 모음에 표시됩니다.

![iOS 및 Android의 제목이 있는 셸 페이지의 스크린샷](configuration-images/titleview.png "제목이 있는 셸 페이지")

> [!IMPORTANT]
> `NavBarIsVisible` 연결된 속성을 가진 탐색 모음이 보이지 않게 되면 제목 보기가 표시되지 않습니다.

보기의 크기가 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성으로 지정되지 않거나 보기의 위치가 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성으로 지정되지 않으면 탐색 모음에 많은 보기가 나타나지 않습니다.

[`Layout`](xref:Xamarin.Forms.Layout) 클래스는 [`View`](xref:Xamarin.Forms.View) 클래스에서 파생되므로 여러 보기를 포함하는 레이아웃 클래스를 표시하도록 `TitleView` 연결된 속성을 설정할 수 있습니다. 마찬가지로 [`ContentView`](xref:Xamarin.Forms.ContentView) 클래스는 결국 [`View`](xref:Xamarin.Forms.View) 클래스에서 파생되므로 `TitleView` 연결된 속성은 단일 보기를 포함하는 `ContentView`를 표시하도록 설정할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Xamarin.Forms CSS Shell 특정 속성](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
