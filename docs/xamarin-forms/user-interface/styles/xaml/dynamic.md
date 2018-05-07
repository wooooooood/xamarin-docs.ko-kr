---
title: 동적 스타일
description: 스타일 속성 변경에 응답 하지 않으며 응용 프로그램의 기간 동안 변경 되지 않습니다. 예를 들어 스타일에 시각적 요소, Setter 인스턴스 중 하나가 수정, 제거 또는 추가 된 새로운 Setter 인스턴스를 할당 한 후 시각적 요소에 변경 내용이 적용 되지 않습니다. 그러나 응용 프로그램 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경 내용에 응답할 수 있습니다.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c484bdc90ec039a8d70209deabbe283cf7100610
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="dynamic-styles"></a>동적 스타일

_스타일 속성 변경에 응답 하지 않으며 응용 프로그램의 기간 동안 변경 되지 않습니다. 예를 들어 스타일에 시각적 요소, Setter 인스턴스 중 하나가 수정, 제거 또는 추가 된 새로운 Setter 인스턴스를 할당 한 후 시각적 요소에 변경 내용이 적용 되지 않습니다. 그러나 응용 프로그램 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경 내용에 응답할 수 있습니다._

`DynamicResource` 태그 확장은 비슷합니다는 `StaticResource` 에서 값을 가져오는 모두 사전 키 사용에서 태그 확장은 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다. 그러나는 `StaticResource` 단일 사전 조회를 수행 된 `DynamicResource` 사전 키에 대 한 링크를 유지 관리 합니다. 따라서 사전 항목 키와 연결 된 대체 되는 경우 변경 시각적 요소에 적용 됩니다. 이 응용 프로그램에서 만들어지는 런타임 스타일 변경할 수 있습니다.

다음 코드 예제에서는 *동적* XAML 페이지의 스타일:

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

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 인스턴스를 사용 하 여는 `DynamicResource` 태그 확장 참조 하는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 라는 `searchBarStyle`, XAML에서 정의 되지 않은 합니다. 그러나 때문에 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 의 속성은 `SearchBar` 인스턴스를 사용 하 여 설정 됩니다는 `DynamicResource`, 예외가 throw 되 고 누락 된 사전 키 ± ¸ á ö.

대신, 코드 숨김 파일에서 생성자를 만듭니다는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 항목 키와 `searchBarStyle`다음 코드 예제에 나온 것 처럼:

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

경우는 `OnButtonClicked` 이벤트 처리기가 실행 `searchBarStyle` 는 간에 전환 `blueSearchBarStyle` 및 `greenSearchBarStyle`합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](dynamic-images/dynamic-style-blue.png "예제 동적 스타일 파란색")](dynamic-images/dynamic-style-blue-large.png#lightbox "예제 동적 스타일 파란색")
[![](dynamic-images/dynamic-style-green.png "예제 동적 스타일 녹색") ] (dynamic-images/dynamic-style-green-large.png#lightbox "예제 동적 스타일 녹색")

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
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button  }
        };
    }
    ...
}
```

C#에서 [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 인스턴스를 사용 하 여는 [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) 참조할 메서드를 `searchBarStyle`합니다. `OnButtonClicked` 이벤트 처리기 코드는 동일 하 고 XAML 예제 및를 실행 하면 `searchBarStyle` 는 간에 전환 `blueSearchBarStyle` 및 `greenSearchBarStyle`합니다.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>동적 스타일 상속

동적 스타일에서 파생 하는 스타일을 얻을 수 없는 사용 하 여 [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) 속성입니다. 대신,는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 클래스에 포함 되어는 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) 속성을 설정할 수 있는 사전 키에 값을 갖는 동적으로 변경 될 수 있습니다.

다음 코드 예제에서는 *동적* XAML 페이지에 스타일을 상속 합니다.

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

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 인스턴스를 사용 하 여는 `StaticResource` 태그 확장 참조 하는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 라는 `tealSearchBarStyle`합니다. 이 `Style` 몇 가지 추가 속성을 설정 하 고 사용 하 여는 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) 참조할 속성 `searchBarStyle`합니다. `DynamicResource` 태그 확장이 필요 하지 않은 `tealSearchBarStyle` 변경 되지 것입니다을 제외 하 고는 `Style` 에서 파생 됩니다. 따라서 `tealSearchBarStyle` 원하는 옵션을 `searchBarStyle` 기본 스타일이 변경 될 때를 대체 합니다.

생성자에서 코드 숨김 파일을 만듭니다는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 항목 키와 `searchBarStyle`, 동적 스타일을 설명 하는 이전 예제를 기준으로 합니다. 경우는 `OnButtonClicked` 이벤트 처리기가 실행 `searchBarStyle` 는 간에 전환 `blueSearchBarStyle` 및 `greenSearchBarStyle`합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](dynamic-images/dynamic-style-inheritance-blue.png "동적 스타일 상속 예제 파란색")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "상속 예제 동적 스타일 파란색")
[![](dynamic-images/dynamic-style-inheritance-green.png "동적 스타일 녹색 상속 예제")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "상속 예제 동적 스타일 녹색")

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

`tealSearchBarStyle` 에 직접 할당 되는 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성은 [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 인스턴스. 이 `Style` 몇 가지 추가 속성을 설정 하 고 사용 하 여는 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) 참조할 속성 `searchBarStyle`합니다. [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) 메서드 필요 하지 않습니다 여기 때문에 `tealSearchBarStyle` 변경 되지 것입니다을 제외 하 고는 `Style` 에서 파생 됩니다. 따라서 `tealSearchBarStyle` 원하는 옵션을 `searchBarStyle` 기본 스타일이 변경 될 때를 대체 합니다.

## <a name="summary"></a>요약

스타일 속성 변경에 응답 하지 않으며 응용 프로그램의 기간 동안 변경 되지 않습니다. 그러나 응용 프로그램 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경 내용에 응답할 수 있습니다. 또한 *동적* 와 스타일에서 파생 될 수는 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) 속성입니다.



## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [동적 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [스타일 (샘플) 작업을](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
