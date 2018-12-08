---
title: Xamarin.Forms에서 동적 스타일
description: 이 문서는 동적 리소스를 사용 하 여 Xamarin.Forms 응용 프로그램을 런타임에 동적으로 스타일 변경에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f1c491bd2e19f44151e2efb317fe40d2d122ecae
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058528"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Xamarin.Forms에서 동적 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)

_스타일 속성 변경에 응답 하지 않으며 응용 프로그램의 기간 동안 변경 되지 않습니다. 예를 들어 스타일에 시각적 요소, Setter 인스턴스 중 하나가 수정 되 면 제거 또는 추가 된 새 Setter 인스턴스에 할당 한 후 변경 내용은 시각적 요소에 적용 되지 않습니다. 그러나 응용 프로그램 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경 내용에 응답할 수 있습니다._

합니다 `DynamicResource` 태그 확장은 비슷합니다는 `StaticResource` 에서 값을 가져와 사전 키를 사용 둘 다에서 태그 확장을 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 그러나 합니다 `StaticResource` 는 단일 사전을 조회를 수행 합니다 `DynamicResource` 사전 키에 대 한 링크를 유지 관리 합니다. 따라서 사전 항목 키에 연결 된 대체 되는 경우 변경 내용은 시각적 요소에 적용 됩니다. 이 가능 하도록 응용 프로그램에서 런타임 스타일 변경할 수 있습니다.

다음 코드 예제를 보여 줍니다 *동적* XAML 페이지의 스타일:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 인스턴스를 사용 하 여를 `DynamicResource` 태그 확장 참조 하는 [ `Style` ](xref:Xamarin.Forms.Style) 라는 `searchBarStyle`는 XAML에서 정의 되지 않은 합니다. 그러나 때문에 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 의 속성을 `SearchBar` 인스턴스를 사용 하 여 설정 됩니다는 `DynamicResource`, 누락 사전 키 throw 예외가 발생 하지 않습니다.

대신 코드 숨김 파일에서 생성자를 만듭니다는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 키를 사용 하 여 항목 `searchBarStyle`다음 코드 예제 에서처럼:

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

경우는 `OnButtonClicked` 이벤트 처리기가 실행 `searchBarStyle` 간을 전환 합니다 `blueSearchBarStyle` 고 `greenSearchBarStyle`입니다. 이 인해 다음 스크린샷에 표시 된 모양:

[![](dynamic-images/dynamic-style-blue.png "동적 스타일 예제 파란색")](dynamic-images/dynamic-style-blue-large.png#lightbox "예제 동적 스타일 파란색")
[![](dynamic-images/dynamic-style-green.png "예제 동적 스타일 녹색") ] (dynamic-images/dynamic-style-green-large.png#lightbox "녹색 동적 Style 예제")

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

C#에서는 합니다 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 인스턴스를 사용 하 여를 [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) 참조할 메서드를 `searchBarStyle`입니다. 합니다 `OnButtonClicked` 이벤트 처리기 코드를 실행 하는 경우 및 XAML 예제와 동일 `searchBarStyle` 간을 전환 합니다 `blueSearchBarStyle` 고 `greenSearchBarStyle`합니다.

## <a name="dynamic-style-inheritance"></a>동적 스타일 상속

동적 스타일에서 스타일을 파생을 조정할 수 없으므로 사용 하 여 [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) 속성입니다. 대신 합니다 [ `Style` ](xref:Xamarin.Forms.Style) 클래스를 포함 합니다 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) 속성을 설정할 수 있는 사전 키 값을 갖는 동적으로 변경 될 수 있습니다.

다음 코드 예제를 보여 줍니다 *동적* 상속 XAML 페이지의 스타일 지정 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 인스턴스를 사용 하 여를 `StaticResource` 태그 확장 참조 하는 [ `Style` ](xref:Xamarin.Forms.Style) 라는 `tealSearchBarStyle`합니다. 이 `Style` 몇 가지 추가 속성을 설정 하 고 사용 합니다 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) 참조할 속성 `searchBarStyle`합니다. 합니다 `DynamicResource` 하므로 태그 확장이 필요 하지 않습니다 `tealSearchBarStyle` 변경 되지 것입니다 제외 하 고는 `Style` 에서 파생 합니다. 따라서 `tealSearchBarStyle` 에 대 한 링크를 유지 관리 `searchBarStyle` 기본 스타일이 변경 될 때 변경 됩니다.

코드 숨김 파일에서 생성자를 만듭니다는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 키를 사용 하 여 항목 `searchBarStyle`, 동적 스타일을 설명 하는 앞의 예제를 기준으로 합니다. 경우는 `OnButtonClicked` 이벤트 처리기가 실행 `searchBarStyle` 간을 전환 합니다 `blueSearchBarStyle` 고 `greenSearchBarStyle`입니다. 이 인해 다음 스크린샷에 표시 된 모양:

[![](dynamic-images/dynamic-style-inheritance-blue.png "동적 스타일 상속 예제 파란색")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "동적 스타일 상속 예제 파란색")
[![](dynamic-images/dynamic-style-inheritance-green.png "동적 스타일 녹색 상속 예제")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "녹색 동적 스타일 상속 예제")

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

`tealSearchBarStyle` 에 직접 할당 된를 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성을 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 인스턴스. 이 `Style` 몇 가지 추가 속성을 설정 하 고 사용 합니다 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) 참조할 속성 `searchBarStyle`합니다. [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) 메서드는 필요치 여기 하므로 `tealSearchBarStyle` 변경 되지 것입니다 제외 하 고는 `Style` 에서 파생 합니다. 따라서 `tealSearchBarStyle` 에 대 한 링크를 유지 관리 `searchBarStyle` 기본 스타일이 변경 될 때 변경 됩니다.

## <a name="summary"></a>요약

스타일 속성 변경에 응답 하지 않으며 응용 프로그램의 기간 동안 변경 되지 않습니다. 그러나 응용 프로그램 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경 내용에 응답할 수 있습니다. 또한 *동적* 사용 하 여 스타일에서 파생 될 수는 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) 속성입니다.



## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [동적 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [스타일 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
