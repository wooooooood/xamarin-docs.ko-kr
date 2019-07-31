---
title: ListView 셀 모양 사용자 지정
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 ListView 컨트롤의 간편함을 활용 하는 동안 데이터를 표시 하기 위한 옵션을 살펴봅니다.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: ecb9e84473778f5185276a854bfbf2fdfcbb6528
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654841"
---
# <a name="customizing-listview-cell-appearance"></a>ListView 셀 모양 사용자 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

[`ListView`](xref:Xamarin.Forms.ListView)를 사용 `ViewCell`하 여 사용자 지정할 수 있는 스크롤 가능한 목록을 제공 합니다. `ViewCells` 텍스트 및 이미지를 표시 하 고, true/false 상태를 나타내는, 사용자 입력을 받고 사용할 수 있습니다.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>기본 제공된 셀
Xamarin.Forms는 대부분의 간단한 응용 프로그램에 대해 작동 하는 기본 제공 셀으로 제공 됩니다.

- **TextCell** &ndash; 텍스트 표시
- **ImageCell** &ndash; 텍스트를 사용 하 여 이미지를 표시 합니다.

두 개의 추가 셀 [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) 하 고 [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) 일반적으로 사용 되지 않지만 사용할 수 있는 `ListView`합니다. 참조 [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) 이러한 셀에 대 한 자세한 내용은 합니다.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) 두 번째 줄을 세부 정보 텍스트를 사용 하 여 필요에 따라 텍스트를 표시 하는 것에 대 한 셀이 됩니다.

성능은 매우 좋은 사용자 지정 비교 하므로 TextCells 런타임에 네이티브 컨트롤로 렌더링 됩니다 `ViewCell`합니다. TextCells은 사용자 지정 가능한, 설정할 수 있습니다.

- `Text` &ndash; 큰 글꼴로 첫 번째 줄에 표시 되는 텍스트입니다.
- `Detail` &ndash; 첫 번째 줄을 더 작은 글꼴을 아래 표시 되는 텍스트입니다.
- `TextColor` &ndash; 텍스트의 색입니다.
- `DetailColor` &ndash; 세부 정보 텍스트의 색

![](customizing-cell-appearance-images/text-cell-default.png "기본 TextCell 예제")

![](customizing-cell-appearance-images/text-cell-custom.png "사용자 지정된 TextCell 예제")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell)같은 `TextCell`, 텍스트 및 보조 정보 텍스트를 표시 하기 위해 사용할 수 있으며 각 플랫폼의 네이티브 컨트롤을 사용 하 여 뛰어난 성능을 제공 합니다. `ImageCell` 다른 `TextCell` 텍스트의 왼쪽에 이미지를 표시 합니다.

`ImageCell` 연락처의 영화 목록 등의 시각적 측면이 사용 하 여 데이터의 목록을 표시 해야 할 때 유용 합니다. ImageCells은 사용자 지정 가능한, 설정할 수 있습니다.

- `Text` &ndash; 큰 글꼴로 첫 번째 줄에 표시 되는 텍스트
- `Detail` &ndash; 첫 번째 줄을 더 작은 글꼴을 아래 표시 되는 텍스트
- `TextColor` &ndash; 텍스트의 색
- `DetailColor` &ndash; 세부 정보 텍스트의 색
- `ImageSource` &ndash; 텍스트 옆에 표시할 이미지를

![](customizing-cell-appearance-images/image-cell-default.png "기본 ImageCell 예제")

![](customizing-cell-appearance-images/image-cell-custom.png "사용자 지정된 ImageCell 예제")

<a name="customcells" />

## <a name="custom-cells"></a>사용자 지정 셀
기본 제공 셀 필요한 레이아웃을 제공 하지 않습니다, 경우 사용자 지정 셀 필요한 레이아웃을 구현 합니다. 예를 들어, 다음 동일한 가중치는 두 개의 레이블을 사용 하 여 셀을 표시 하는 것이 좋습니다. `TextCell` 충분 한 것 때문에 `TextCell` 작은 하나 레이블이 있습니다. 대부분의 셀 사용자 지정 추가 읽기 전용 데이터 (예: 추가 레이블, 이미지 또는 기타 표시 정보)를 추가합니다.

모든 사용자 지정 셀에서 파생 되어야 합니다 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)를 사용 하 여 유형의 모든 기본 제공 셀 동일한 기본 클래스입니다.

Xamarin.Forms 2에 새로 도입 [캐싱 동작](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) 에 `ListView` 일부 유형의 사용자 지정 셀의 스크롤 성능 향상을 위해 설정할 수 있는 컨트롤입니다.

사용자 지정 셀의 예로 다음과 같습니다.

![](customizing-cell-appearance-images/custom-cell.png "사용자 지정 셀 예제")

### <a name="xaml"></a>XAML
위의 레이아웃을 만들려면 XAML은 다음과 같습니다.

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

위의 XAML 많이 수행 하는 합니다. 알아봅시다.

- 사용자 지정 셀 내에 중첩 되어는 `DataTemplate`, 내부는 `ListView.ItemTemplate`합니다. 이 다른 셀을 사용 하 여 프로세스와 동일 합니다.
- `ViewCell` 사용자 지정 셀의 유형이입니다. 자식 합니다 `DataTemplate` 요소 이어야 하거나 형식에서 파생 해야 `ViewCell`합니다.
- 내부를 확인 합니다 `ViewCell`, 레이아웃에서 관리 하는 `StackLayout`합니다. 이 레이아웃을 사용 하면 배경 색을 사용자 지정할 수 있습니다. 모든 속성 `StackLayout` 바인딩할 수 있습니다 즉 여기 표시 되지 않습니다 하지만 사용자 지정 셀 내에서 바인딩할 수 있습니다.
- 내에서 레이아웃은 모든 xamarin.ios 레이아웃 을통해관리할수있습니다.`ViewCell` 

### <a name="cnum"></a>C&num;

C#에서 사용자 지정 셀을 지정 하는 것은 XAML에 해당 하는 보다 조금 더 자세한 정보입니다. 이 형식에 대해 살펴보겠습니다.

먼저 사용자 지정 셀 클래스를 사용 하 여 정의 `ViewCell` 기본 클래스:

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

페이지에 대 한 생성자에는 `ListView`, ListView의 설정 `ItemTemplate` 속성을 새 `DataTemplate`:

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

생성자 `DataTemplate` 형식을 사용 합니다. Typeof 연산자에 대 한 CLR 형식을 가져옵니다 `CustomCell`합니다.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>바인딩 컨텍스트 변경

사용자 지정 셀 형식의 바인딩할 때 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 인스턴스를 표시 하는 UI 컨트롤을 `BindableProperty` 값을 사용할지는 [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) 에 표시할 데이터를 설정 하려면 재정의 각 셀에 다음 코드 예제에 설명 된 대로 셀 생성자 보다는:

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

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

[ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) 재정의 될 때 호출할를 [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged) 의 값에 대 한 응답에서 이벤트가 발생 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 속성이 변경 합니다. 따라서 경우는 `BindingContext` 표시 하는 UI 컨트롤을 변경 합니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 값 데이터를 설정 해야 합니다. 합니다 `BindingContext` 확인할를 `null` 값을 다시 하면 가비지 컬렉션에 대 한 Xamarin.Forms에서 설정할 수 있습니다는 `OnBindingContextChanged` 호출할 재정의 합니다.

또는 UI 컨트롤에 바인딩할 수는 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 인스턴스를 재정의할 필요를 제거 하는 해당 값을 표시 하는 `OnBindingContextChanged` 메서드.

> [!NOTE]
> 재정의 하는 경우 `OnBindingContextChanged`, 기본 클래스의 했는지 `OnBindingContextChanged` 받도록 등록 된 대리자 메서드는 `BindingContextChanged` 이벤트입니다.

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

바인딩합니다이 `Name`, `Age`, 및 `Location` 에 바인딩 가능한 속성을 `CustomCell` 인스턴스를 `Name`, `Age`, 및 `Location` 기본 컬렉션의 각 개체의 속성.

C#에서 해당 바인딩에 다음 코드 예제에 표시 됩니다.

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

IOS 및 Android의 경우는 [ `ListView` ](xref:Xamarin.Forms.ListView) 요소 재활용 되 및 사용자 지정 셀 사용자 지정 렌더러를 사용 하 여, 사용자 지정 렌더러 속성 변경 알림을 올바르게 구현 해야 합니다. 셀 재사용 되는 바인딩 컨텍스트를 사용할 수 있는 셀의 업데이트 된 경우 해당 속성 값 변경 됩니다 `PropertyChanged` 이벤트 발생 합니다. 자세한 내용은 [Viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)합니다. 참조 셀을 재활용 하는 방법에 대 한 자세한 내용은 [캐싱 전략](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)합니다.

## <a name="related-links"></a>관련 링크

- [기본 제공 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [사용자 지정 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [바인딩이 컨텍스트 변경 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
