---
title: Xamarin.Forms에서 동적 스타일
description: 이 문서는 동적 리소스를 사용 하 여 Xamarin.Forms 응용 프로그램을 런타임에 동적으로 스타일 변경에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
ms.custom: video
ms.openlocfilehash: 9a26532d13b843b812da94739be071c7accac212
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78918669"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Xamarin.Forms에서 동적 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_스타일은 속성 변경에 응답 하지 않으며 응용 프로그램 기간 동안 변경 되지 않은 상태로 유지 됩니다. 예를 들어 비주얼 요소에 스타일을 할당 한 후 Setter 인스턴스 중 하나를 수정, 제거 또는 새 Setter 인스턴스를 추가 하면 변경 내용이 시각적 요소에 적용 되지 않습니다. 그러나 응용 프로그램은 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경에 응답할 수 있습니다._

`DynamicResource` 태그 확장은 사전 키를 사용 하 여 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 값을 인출 한다는 점에서 `StaticResource` 태그 확장과 비슷합니다. 그러나 `StaticResource` 단일 사전 조회를 수행 하는 동안 `DynamicResource`는 사전 키에 대 한 링크를 유지 관리 합니다. 따라서 사전 항목 키에 연결 된 대체 되는 경우 변경 내용은 시각적 요소에 적용 됩니다. 이 가능 하도록 응용 프로그램에서 런타임 스타일 변경할 수 있습니다.

다음 코드 예제에서는 XAML 페이지의 *동적* 스타일을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`SearchBar`](xref:Xamarin.Forms.SearchBar) 인스턴스는 `DynamicResource` 태그 확장을 사용 하 여 XAML에 정의 되지 않은 `searchBarStyle`이라는 [`Style`](xref:Xamarin.Forms.Style) 를 참조 합니다. 그러나 `SearchBar` 인스턴스의 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성이 `DynamicResource`을 사용 하 여 설정 되기 때문에 누락 된 사전 키로 인해 예외가 throw 되지 않습니다.

대신, 코드 숨김이 파일에서 생성자는 다음 코드 예제와 같이 키 `searchBarStyle`를 사용 하 여 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 항목을 만듭니다.

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

`OnButtonClicked` 이벤트 처리기가 실행 되 면 `searchBarStyle` `blueSearchBarStyle`와 `greenSearchBarStyle`사이를 전환 합니다. 이 인해 다음 스크린샷에 표시 된 모양:

[![Blue 동적 스타일 예제](dynamic-images/dynamic-style-blue.png)](dynamic-images/dynamic-style-blue-large.png#lightbox)
[![녹색 동적 스타일 예제](dynamic-images/dynamic-style-green.png)](dynamic-images/dynamic-style-green-large.png#lightbox)

다음 코드 예제에서는 C#의 해당 페이지를 보여 줍니다.

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

에서 C# [`SearchBar`](xref:Xamarin.Forms.SearchBar) 인스턴스는 [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) 메서드를 사용 하 여 `searchBarStyle`를 참조 합니다. `OnButtonClicked` 이벤트 처리기 코드는 XAML 예제와 동일 하며 실행 되는 경우 `searchBarStyle` `blueSearchBarStyle`와 `greenSearchBarStyle`사이를 전환 합니다.

## <a name="dynamic-style-inheritance"></a>동적 스타일 상속

[`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn) 속성을 사용 하 여 동적 스타일에서 스타일을 파생 시킬 수 없습니다. 대신 [`Style`](xref:Xamarin.Forms.Style) 클래스에는 값이 동적으로 변경 될 수 있는 사전 키로 설정할 수 있는 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 속성이 포함 되어 있습니다.

다음 코드 예제에서는 XAML 페이지의 *동적* 스타일 상속을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`SearchBar`](xref:Xamarin.Forms.SearchBar) 인스턴스는 `StaticResource` 태그 확장을 사용 하 여 `tealSearchBarStyle`이라는 [`Style`](xref:Xamarin.Forms.Style) 를 참조 합니다. 이 `Style`에서는 몇 가지 추가 속성을 설정 하 고 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 속성을 사용 하 여 `searchBarStyle`를 참조 합니다. `DynamicResource` 태그 확장은 `tealSearchBarStyle`가 파생 된 `Style`를 제외 하 고는 변경 되지 않기 때문에 필요 하지 않습니다. 따라서 `tealSearchBarStyle`는 `searchBarStyle`에 대 한 링크를 유지 관리 하며 기본 스타일이 변경 될 때 변경 됩니다.

코드 기반 파일에서 생성자는 동적 스타일을 보여 준 이전 예제와 같이 키 `searchBarStyle`를 사용 하 여 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 항목을 만듭니다. `OnButtonClicked` 이벤트 처리기가 실행 되 면 `searchBarStyle` `blueSearchBarStyle`와 `greenSearchBarStyle`사이를 전환 합니다. 이 인해 다음 스크린샷에 표시 된 모양:

[![Blue Dynamic Style 상속 예제](dynamic-images/dynamic-style-inheritance-blue.png)](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox)
[![녹색 동적 스타일 상속 예제](dynamic-images/dynamic-style-inheritance-green.png)](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox)

다음 코드 예제에서는 C#의 해당 페이지를 보여 줍니다.

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

`tealSearchBarStyle`는 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 인스턴스의 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성에 직접 할당 됩니다. 이 `Style`에서는 몇 가지 추가 속성을 설정 하 고 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 속성을 사용 하 여 `searchBarStyle`를 참조 합니다. [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) 메서드는에서 파생 된 `Style`를 제외 하 고 `tealSearchBarStyle` 변경 되지 않기 때문에 필요 하지 않습니다. 따라서 `tealSearchBarStyle`는 `searchBarStyle`에 대 한 링크를 유지 관리 하며 기본 스타일이 변경 될 때 변경 됩니다.

## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [동적 스타일 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [스타일 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Dynamic-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
