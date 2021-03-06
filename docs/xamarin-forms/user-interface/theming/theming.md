---
title: Xamarin.Forms응용 프로그램 테마
description: Xamarin.Forms각 테마에 대해 ResourceDictionary를 만든 다음 DynamicResource 태그 확장을 사용 하 여 리소스를 로드 하 여 응용 프로그램에서 테마를 구현할 수 있습니다.
ms.prod: xamarin
ms.assetId: B7B17F66-4E37-4B50-9A57-351B62BE4FED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3341ada6c5605917eeec79aac96e38cb99b40fc4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138205"
---
# <a name="theme-a-xamarinforms-application"></a>Xamarin.Forms응용 프로그램 테마

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Xamarin.Forms응용 프로그램은 태그 확장을 사용 하 여 런타임에 동적으로 스타일 변경에 응답할 수 있습니다 `DynamicResource` . 이 태그 확장은 `StaticResource` 둘 다 사전 키를 사용 하 여에서 값을 인출 한다는 점에서 태그 확장과 비슷합니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . 그러나 `StaticResource` 태그 확장은 단일 사전 조회를 수행 하지만 `DynamicResource` 태그 확장은 사전 키에 대 한 링크를 유지 관리 합니다. 따라서 키와 연결 된 값이 대체 되 면 변경 내용이에 적용 됩니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) . 그러면 응용 프로그램에서 런타임 테마를 구현할 수 있습니다 Xamarin.Forms .

응용 프로그램에서 런타임 테마를 구현 하는 프로세스는 다음과 같습니다 Xamarin.Forms .

1. 에서 각 테마에 대 한 리소스를 정의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 합니다.
1. 태그 확장을 사용 하 여 응용 프로그램에서 테마 리소스를 사용 `DynamicResource` 합니다.
1. 응용 프로그램의 **app.xaml** 파일에서 기본 테마를 설정 합니다.
1. 런타임에 테마를 로드 하는 코드를 추가 합니다.

> [!IMPORTANT]
> `StaticResource`런타임에 응용 프로그램 테마를 변경할 필요가 없는 경우 태그 확장을 사용 합니다.

다음 스크린샷은 테마 페이지를 표시 하며, 진한 테마를 사용 하 여 밝은 테마와 Android 응용 프로그램을 사용 하는 iOS 응용 프로그램입니다.

[![IOS 및 Android](theming-images/main-page-both-themes.png "테마가 적용 된 앱의 기본 페이지")](theming-images/main-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 기본 페이지") 
 에서 테마가 적용 된 앱의 기본 페이지 스크린샷 [ ![IOS 및 Android에서 테마가 적용 된 앱의 세부 정보 페이지 스크린샷](theming-images/detail-page-both-themes.png "테마가 적용 된 앱의 세부 정보 페이지")](theming-images/detail-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 세부 정보 페이지")

> [!NOTE]
> 런타임에 테마를 변경 하려면 XAML 스타일을 사용 해야 하며 CSS를 사용 하 여 현재 사용할 수 없습니다.

## <a name="define-themes"></a>테마 정의

테마는에 저장 된 리소스 개체의 컬렉션으로 정의 됩니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .

다음 예제에서는 샘플 응용 프로그램의를 보여 줍니다 `LightTheme` .

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.LightTheme">
    <Color x:Key="PageBackgroundColor">White</Color>
    <Color x:Key="NavigationBarColor">WhiteSmoke</Color>
    <Color x:Key="PrimaryColor">WhiteSmoke</Color>
    <Color x:Key="SecondaryColor">Black</Color>
    <Color x:Key="PrimaryTextColor">Black</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">Gray</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

다음 예제에서는 샘플 응용 프로그램의를 보여 줍니다 `DarkTheme` .

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.DarkTheme">
    <Color x:Key="PageBackgroundColor">Black</Color>
    <Color x:Key="NavigationBarColor">Teal</Color>
    <Color x:Key="PrimaryColor">Teal</Color>
    <Color x:Key="SecondaryColor">White</Color>
    <Color x:Key="PrimaryTextColor">White</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">WhiteSmoke</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

각에는 각각 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Color`](xref:Xamarin.Forms.Color) `ResourceDictionary` 동일한 키 값을 사용 하 여 각각의 테마를 정의 하는 리소스가 포함 되어 있습니다. 리소스 사전에 대 한 자세한 내용은 [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)을 참조 하세요.

> [!IMPORTANT]
> 메서드를 호출 하는 각에는 코드 숨김이 필요 `ResourceDictionary` `InitializeComponent` 합니다. 이는 런타임에 선택 된 테마를 나타내는 CLR 개체를 만들 수 있도록 하는 데 필요 합니다.

## <a name="set-a-default-theme"></a>기본 테마 설정

응용 프로그램에는 기본 테마가 필요 하므로 컨트롤에 사용 되는 리소스에 대 한 값이 있습니다. 테마를 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `ResourceDictionary` **app.xaml**에 정의 된 응용 프로그램 수준에 병합 하 여 기본 테마를 설정할 수 있습니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>
        <ResourceDictionary Source="Themes/LightTheme.xaml" />
    </Application.Resources>
</Application>
```

리소스 사전을 병합 하는 방법에 대 한 자세한 내용은 [병합 된 리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md#merged-resource-dictionaries)을 참조 하세요.

## <a name="consume-theme-resources"></a>테마 리소스 사용

응용 프로그램에서 테마를 나타내는에 저장 된 리소스를 사용 하려는 경우 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 태그 확장을 사용 하 여이 작업을 수행 해야 합니다 `DynamicResource` . 이렇게 하면 런타임에 다른 테마를 선택 하면 새 테마의 값이 적용 됩니다.

다음 예제에서는 개체에 적용할 수 있는 샘플 응용 프로그램의 세 가지 스타일을 보여 줍니다 [`Label`](xref:Xamarin.Forms.Label) .

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>

        <Style x:Key="LargeLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource SecondaryTextColor}" />
            <Setter Property="FontSize"
                    Value="30" />
        </Style>

        <Style x:Key="MediumLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource PrimaryTextColor}" />
            <Setter Property="FontSize"
                    Value="25" />
        </Style>

        <Style x:Key="SmallLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource TertiaryTextColor}" />
            <Setter Property="FontSize"
                    Value="15" />
        </Style>

    </Application.Resources>
</Application>
```

이러한 스타일은 여러 페이지에서 사용할 수 있도록 응용 프로그램 수준 리소스 사전에 정의 됩니다. 각 스타일은 태그 확장이 있는 테마 리소스를 사용 `DynamicResource` 합니다.

이러한 스타일은 페이지에서 사용 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ThemingDemo"
             x:Class="ThemingDemo.UserSummaryPage"
             Title="User Summary"
             BackgroundColor="{DynamicResource PageBackgroundColor}">
    ...
    <ScrollView>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="200" />
                <RowDefinition Height="120" />
                <RowDefinition Height="70" />
            </Grid.RowDefinitions>
            <Grid BackgroundColor="{DynamicResource PrimaryColor}">
                <Label Text="Face-Palm Monkey"
                       VerticalOptions="Center"
                       Margin="15"
                       Style="{StaticResource MediumLabelStyle}" />
                ...
            </Grid>
            <StackLayout Grid.Row="1"
                         Margin="10">
                <Label Text="This monkey reacts appropriately to ridiculous assertions and actions."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Cynical but not unfriendly."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Seven varieties of grimaces."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Doesn't laugh at your jokes."
                       Style="{StaticResource SmallLabelStyle}" />
            </StackLayout>
            ...
        </Grid>
    </ScrollView>
</ContentPage>
```

테마 리소스를 직접 사용 하는 경우 태그 확장과 함께 사용 해야 합니다 `DynamicResource` . 그러나 태그 확장을 사용 하는 스타일을 사용 하는 경우 `DynamicResource` 태그 확장과 함께 사용 해야 합니다 `StaticResource` .

스타일 지정에 대 한 자세한 내용은 [ Xamarin.Forms XAML 스타일을 사용 하 여 앱 스타일](~/xamarin-forms/user-interface/styles/xaml/index.md)지정을 참조 하세요. 태그 확장에 대 한 자세한 내용은 `DynamicResource` [의 Xamarin.Forms 동적 스타일 ](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)을 참조 하십시오.

## <a name="load-a-theme-at-runtime"></a>런타임에 테마 로드

런타임에 테마를 선택 하면 응용 프로그램은 다음을 수행 해야 합니다.

1. 응용 프로그램에서 현재 테마를 제거 합니다. 이렇게 하려면 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 응용 프로그램 수준의 속성을 선택 취소 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 합니다.
2. 선택한 테마를 로드 합니다. 이렇게 하려면 선택한 테마의 인스턴스를 `MergedDictionaries` 응용 프로그램 수준의 속성에 추가 `ResourceDictionary` 합니다.

[`VisualElement`](xref:Xamarin.Forms.VisualElement)태그 확장을 사용 하 여 속성을 설정 하는 개체는 `DynamicResource` 새 테마 값을 적용 합니다. 이는 `DynamicResource` 태그 확장이 사전 키에 대 한 링크를 유지 하기 때문에 발생 합니다. 따라서 키와 연결 된 값이 대체 되 면 변경 내용이 개체에 적용 됩니다 `VisualElement` .

샘플 응용 프로그램에서는이 포함 된 모달 페이지를 통해 테마가 선택 됩니다 [`Picker`](xref:Xamarin.Forms.Picker) . 다음 코드에서는 `OnPickerSelectionChanged` 선택한 테마가 변경 될 때 실행 되는 메서드를 보여 줍니다.

```csharp
void OnPickerSelectionChanged(object sender, EventArgs e)
{
    Picker picker = sender as Picker;
    Theme theme = (Theme)picker.SelectedItem;

    ICollection<ResourceDictionary> mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    if (mergedDictionaries != null)
    {
        mergedDictionaries.Clear();

        switch (theme)
        {
            case Theme.Dark:
                mergedDictionaries.Add(new DarkTheme());
                break;
            case Theme.Light:
            default:
                mergedDictionaries.Add(new LightTheme());
                break;
        }
    }
}
```

## <a name="related-links"></a>관련 링크

- [테마 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [시스템 테마 변경에 응답](system-theme-changes.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [의 동적 스타일Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
