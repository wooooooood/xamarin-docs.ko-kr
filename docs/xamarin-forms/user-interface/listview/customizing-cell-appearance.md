---
title: ListView 셀 모양 사용자 지정
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 ListView 컨트롤의 간편함을 활용 하는 동안 데이터를 표시 하기 위한 옵션을 살펴봅니다.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2019
ms.openlocfilehash: ab54b54c9f2f7d6d7748137ea079439b7c3ddfca
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916926"
---
# <a name="customizing-listview-cell-appearance"></a>ListView 셀 모양 사용자 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Xamarin.ios [`ListView`](xref:Xamarin.Forms.ListView) 클래스는 `ViewCell` 요소를 사용 하 여 사용자 지정할 수 있는 스크롤 가능한 목록을 표시 하는 데 사용 됩니다. `ViewCell` 요소는 텍스트와 이미지를 표시 하 고, true/false 상태를 표시 하 고, 사용자 입력을 받을 수 있습니다.

## <a name="built-in-cells"></a>기본 제공된 셀
Xamarin.ios는 여러 응용 프로그램에 대해 작동 하는 기본 제공 셀과 함께 제공 됩니다.

- [`TextCell`](#textcell) 컨트롤은 텍스트를 표시 하는 데 사용 됩니다 (선택 사항 텍스트의 경우 두 번째 줄).
- [`ImageCell`](#imagecell) 컨트롤은 `TextCell`와 비슷하지만 텍스트 왼쪽에 이미지를 포함 합니다.
- `SwitchCell` 컨트롤은 on/off 또는 true/false 상태를 표시 하 고 캡처하는 데 사용 됩니다.
- `EntryCell` 컨트롤은 사용자가 편집할 수 있는 텍스트 데이터를 표시 하는 데 사용 됩니다.

[`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell) 및 [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) 컨트롤은 [`TableView`](~/xamarin-forms/user-interface/tableview.md)컨텍스트에서 보다 일반적으로 사용 됩니다.

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) 는 텍스트를 표시 하는 셀 이며, 필요에 따라 두 번째 줄을 정보 텍스트로 표시 합니다. 다음 스크린샷에서는 iOS 및 Android의 `TextCell` 항목을 보여 줍니다.

![](customizing-cell-appearance-images/text-cell-default.png "Default TextCell Example")

TextCells은 런타임에 네이티브 컨트롤로 렌더링 되므로 사용자 지정 `ViewCell`에 비해 성능이 매우 좋습니다. TextCells은 사용자 지정이 가능 하므로 다음 속성을 설정할 수 있습니다.

- `Text` 첫째 줄에 표시 되는 텍스트를 크게 &ndash; 합니다.
- `Detail` 첫째 줄 아래에 표시 되는 텍스트를 더 작은 글꼴로 &ndash; 합니다.
- 텍스트 색을 &ndash; `TextColor`입니다.
- `DetailColor` &ndash; 정보 텍스트의 색입니다.

다음 스크린샷은 사용자 지정 된 색 속성을 가진 `TextCell` 항목을 보여 줍니다.

![](customizing-cell-appearance-images/text-cell-custom.png "Custom TextCell Example")

### <a name="imagecell"></a>ImageCell

`TextCell`와 같은 [`ImageCell`](xref:Xamarin.Forms.ImageCell)는 텍스트 및 보조 정보 텍스트를 표시 하는 데 사용 될 수 있으며, 각 플랫폼의 네이티브 컨트롤을 사용 하 여 뛰어난 성능을 제공 합니다. `ImageCell`는 텍스트 왼쪽에 이미지를 표시 한다는 점에서 `TextCell`와 다릅니다.

다음 스크린샷에서는 iOS 및 Android의 `ImageCell` 항목을 보여 줍니다. !["Default ImageCell Example"](customizing-cell-appearance-images/image-cell-default.png "기본 ImageCell 예제")

`ImageCell`는 연락처 또는 영화 목록과 같이 시각적 측면이 포함 된 데이터 목록을 표시 해야 하는 경우에 유용 합니다. `ImageCell`를 사용자 지정할 수 있으므로 다음과 같이 설정할 수 있습니다.

- `Text` 첫째 줄에 표시 되는 텍스트를 &ndash; 합니다.
- `Detail` 첫째 줄 아래에 표시 되는 텍스트를 더 작은 글꼴로 &ndash; 합니다.
- 텍스트 색을 &ndash; `TextColor`
- `DetailColor` &ndash; 정보 텍스트의 색입니다.
- 텍스트 옆에 표시할 이미지 &ndash; `ImageSource`

다음 스크린샷은 사용자 지정 된 색 속성의 `ImageCell` 항목을 보여 줍니다. !["사용자 지정 된 ImageCell 예"](customizing-cell-appearance-images/image-cell-custom.png "사용자 지정 된 ImageCell 예제")

## <a name="custom-cells"></a>사용자 지정 셀
사용자 지정 셀을 사용 하면 기본 제공 셀에서 지원 하지 않는 셀 레이아웃을 만들 수 있습니다. 예를 들어, 다음 동일한 가중치는 두 개의 레이블을 사용 하 여 셀을 표시 하는 것이 좋습니다. `TextCell`에 작은 레이블이 하나 있으므로 `TextCell` 충분 하지 않습니다. 대부분의 셀 사용자 지정 추가 읽기 전용 데이터 (예: 추가 레이블, 이미지 또는 기타 표시 정보)를 추가합니다.

모든 사용자 지정 셀은 기본 제공 셀 형식이 모두 사용 하는 것과 동일한 기본 클래스인 [`ViewCell`](xref:Xamarin.Forms.ViewCell)에서 파생 되어야 합니다.

Xamarin.ios는 `ListView` 컨트롤에 대 한 [캐싱 동작](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy) 을 제공 하 여 일부 형식의 사용자 지정 셀에 대 한 스크롤 성능을 향상 시킬 수 있습니다.

다음 스크린샷은 사용자 지정 셀의 예를 보여 줍니다.

!["사용자 지정 셀 예제"](customizing-cell-appearance-images/custom-cell.png "사용자 지정 셀 예제")

### <a name="xaml"></a>XAML
다음 XAML을 사용 하 여 이전 스크린샷에 표시 된 사용자 지정 셀을 만들 수 있습니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

XAML은 다음과 같이 작동 합니다.

- 사용자 지정 셀은 `ListView.ItemTemplate`안에 있는 `DataTemplate`내에 중첩 됩니다. 이는 기본 제공 셀을 사용 하는 프로세스와 동일 합니다.
- `ViewCell`은 사용자 지정 셀의 형식입니다. `DataTemplate` 요소의 자식은 `ViewCell` 클래스 이거나이 클래스에서 파생 되어야 합니다.
- `ViewCell`내에서 레이아웃은 Xamarin.ios 레이아웃을 통해 관리할 수 있습니다. 이 예제에서 레이아웃은 색을 사용자 지정할 수 있도록 하는 `StackLayout`에 의해 관리 됩니다.

> [!NOTE]
> 바인딩 가능한 `StackLayout`의 모든 속성은 사용자 지정 셀 내에 바인딩할 수 있습니다. 그러나이 기능은 XAML 예제에는 표시 되지 않습니다.

### <a name="code"></a>코드

코드에서 사용자 지정 셀을 만들 수도 있습니다. 먼저 `ViewCell`에서 파생 되는 사용자 지정 클래스를 만들어야 합니다.

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

페이지 생성자에서 ListView의 `ItemTemplate` 속성은 `CustomCell` 형식이 지정 된 `DataTemplate`로 설정 됩니다.

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

### <a name="binding-context-changes"></a>바인딩 컨텍스트 변경

사용자 지정 셀 형식의 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 인스턴스에 바인딩할 때 `BindableProperty` 값을 표시 하는 UI 컨트롤은 다음 코드 예제와 같이 [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) 재정의를 사용 하 여 각 셀에 표시할 데이터를 셀 생성자 대신 설정 합니다.

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name
    {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age
    {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location
    {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null)
        {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

[`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) 재정의는 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성 변경 값에 대 한 응답으로 [`BindingContextChanged`](xref:Xamarin.Forms.BindableObject.BindingContextChanged) 이벤트가 발생 하는 경우 호출 됩니다. 따라서 `BindingContext` 변경 되 면 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 값을 표시 하는 UI 컨트롤이 해당 데이터를 설정 해야 합니다. `BindingContext`은 `null` 값을 확인 해야 합니다 .이 값은 가비지 수집을 위해 Xamarin.ios에서 설정할 수 있으며,이 경우 `OnBindingContextChanged` 재정의가 호출 됩니다.

또는 UI 컨트롤의 값을 표시 하기 위해 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 인스턴스에 바인딩할 수 있습니다. 그러면 `OnBindingContextChanged` 메서드를 재정의 하지 않아도 됩니다.

> [!NOTE]
> `OnBindingContextChanged`를 재정의 하는 경우 등록 된 대리자가 `BindingContextChanged` 이벤트를 받도록 기본 클래스의 `OnBindingContextChanged` 메서드가 호출 되는지 확인 합니다.

XAML에 데이터 바인딩 사용자 지정 셀 유형을 수행할 수 있습니다 다음 코드 예제 에서처럼:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

이는 `CustomCell` 인스턴스의 바인딩 가능한 속성 `Name`, `Age`및 `Location`를 기본 컬렉션에 있는 각 개체의 `Name`, `Age`및 `Location` 속성에 바인딩합니다.

C#에서 해당 바인딩에 다음 코드 예제에 표시 됩니다.

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView
{
    ItemsSource = people,
    ItemTemplate = customCell
};
```

IOS 및 Android에서 [`ListView`](xref:Xamarin.Forms.ListView) 가 요소를 재생 하 고 사용자 지정 셀이 사용자 지정 렌더러를 사용 하는 경우 사용자 지정 렌더러에서 속성 변경 알림을 올바르게 구현 해야 합니다. 셀을 다시 사용 하는 경우 바인딩 컨텍스트가 사용 가능한 셀의 값으로 업데이트 되 면 `PropertyChanged` 이벤트가 발생 하 여 해당 속성 값이 변경 됩니다. 자세한 내용은 [ViewCell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)을 참조 하세요. 셀 재활용에 대 한 자세한 내용은 [캐싱 전략](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [기본 제공 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [사용자 지정 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [바인딩 컨텍스트가 변경 되었습니다 (샘플).](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
