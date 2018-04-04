---
title: 셀 모양
description: ListView에서 편리 하 게 활용 하는 동안 데이터를 표시 하기 위한 옵션을 탐색 합니다.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: df0e113f0c76ea9bde58da7a7ceccd50edd5b227
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="cell-appearance"></a>셀 모양

ListView를 스크롤할 수 있는 그룹을 사용 하 여 사용자 지정할 수 있는 표시 `ViewCell`s입니다. `ViewCells` 텍스트 및 이미지를 표시, true/false 상태를 나타내는 사용자 입력을 받는 데 사용할 수 있습니다.

ListView 셀에서 원하는 모양으로 두 가지가 있습니다.

- **[기본 제공 셀 사용자 지정](#Built_in_Cells)**  &ndash; 보다 쉽게 구현 및 더 나은 성능을 저하 가능성입니다.
- **[사용자 지정 셀 만들기](#customcells)**  &ndash; 더 최종 결과 제어할 수 있지만 올바르게 구현 되지 않은 경우 성능 문제에 대 한 가능성이 있는 합니다.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>셀에 기본 제공
Xamarin.Forms는 대부분의 간단한 응용 프로그램에 대해 작동 하는 기본 제공 셀으로 제공 됩니다.

- **TextCell** &ndash; 텍스트를 표시 하기 위한
- **ImageCell** &ndash; 텍스트와 이미지를 표시 합니다.

두 개의 추가 셀 [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) 및 [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) 일반적으로 사용 되지 않지만 사용할 수 있는 `ListView`합니다. 참조 [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) 이러한 셀에 대 한 자세한 내용은 합니다.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) 자세한 내용으로 두 번째 줄과 필요에 따라 텍스트 표시에 대 한 셀이 됩니다.

사용자 지정에 비해 성능은 매우 양호 하므로 TextCells 실행 시간에 네이티브 컨트롤으로 렌더링 됩니다 `ViewCell`합니다. TextCells은 설정 하 여 사용자 지정이 가능.

- `Text` &ndash; 큰 글꼴로 첫 번째 줄에 표시 되는 텍스트입니다.
- `Detail` &ndash; 더 작은 글꼴에 첫 번째 줄 아래에 표시 되는 텍스트입니다.
- `TextColor` &ndash; 텍스트의 색입니다.
- `DetailColor` &ndash; 세부 정보 텍스트의 색

![](customizing-cell-appearance-images/text-cell-default.png "기본 TextCell 예")

![](customizing-cell-appearance-images/text-cell-custom.png "사용자 지정 된 TextCell 예제")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)같은 `TextCell`, 텍스트 및 보조 정보 텍스트를 표시 하기 위해 사용할 수 있으며 각 플랫폼에 네이티브 컨트롤을 사용 하 여 뛰어난 성능을 제공 합니다. `ImageCell` 와 다른 `TextCell` 텍스트의 왼쪽에 이미지를 표시 하 합니다.

`ImageCell` 연락처 또는 동영상 목록 등의 시각적 측면이 사용 하 여 데이터의 목록을 표시 해야 할 때 유용 합니다. ImageCells은 설정 하 여 사용자 지정이 가능.

- `Text` &ndash; 큰 글꼴로 첫 번째 줄에 표시 되는 텍스트
- `Detail` &ndash; 더 작은 글꼴에 첫 번째 줄 아래에 표시 되는 텍스트
- `TextColor` &ndash; 텍스트의 색
- `DetailColor` &ndash; 세부 정보 텍스트의 색
- `ImageSource` &ndash; 텍스트 옆에 표시할 이미지를

Windows Phone 8.1을 대상으로 할 때 사용자에 게 유의 `ImageCell` 기본적으로 이미지를 조정 하지 것입니다. 또한 Windows Phone 8.1 인지 되는 정보에 텍스트 표시 되는 유일한 플랫폼에 동일한 색 및 글꼴 기본 텍스트는 기본적으로 note 합니다. Windows Phone 8.0 렌더링 `ImageCell` 아래와 같이:

![](customizing-cell-appearance-images/image-cell-default.png "기본 ImageCell 예")

![](customizing-cell-appearance-images/image-cell-custom.png "사용자 지정 된 ImageCell 예제")

<a name="customcells" />

## <a name="custom-cells"></a>사용자 지정 셀
기본 제공 셀 필요한 레이아웃을 제공 하지 않습니다, 필요한 레이아웃 사용자 지정 셀 구현. 예를 들어 다음 동일한 가중치는 두 개의 레이블이 있는 셀을 표시 하는 것이 좋습니다. A `LabelCell` 충분 한 수 없기 때문에 `LabelCell` 작은 하나의 레이블이 있습니다. 대부분의 셀 사용자 지정 (예: 추가 레이블, 이미지 또는 다른 표시 정보) 읽기 전용 데이터를 추가 합니다.

모든 사용자 지정 셀에서 파생 되어야 [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), 기본 제공 된 셀의 모든 사용을 입력 하는 동일한 기본 클래스입니다.

Xamarin.Forms 2에는 새로 도입 되었습니다. [캐싱 동작](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) 에 `ListView` 일부 형식의 사용자 지정 셀에 대 한 스크롤 성능 향상을 위해 설정할 수 있는 제어 합니다.

다음은 사용자 지정 셀의 예입니다.

![](customizing-cell-appearance-images/custom-cell.png "사용자 지정 셀 예제")

### <a name="xaml"></a>XAML
XAML 위의 레이아웃을 만들 수는 다음과 같습니다.

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

위의 XAML를 많이 수행 합니다. 분리 해 보겠습니다.

- 사용자 지정 셀 내에 중첩 된는 `DataTemplate`,이 내부 `ListView.ItemTemplate`합니다. 다른 셀을 사용 하 여 동일한 프로세스입니다.
- `ViewCell` 사용자 지정 셀의 유형이입니다. 자식은 `DataTemplate` 요소 형식에서 파생 하거나 이어야 함 `ViewCell`합니다.
- 해당 내부 표시는 `ViewCell`, 레이아웃에서 관리 되는 `StackLayout`합니다. 이 레이아웃 배경 색을 사용자 지정할 수 있습니다. 모든 속성 `StackLayout` 즉 바인딩할 수 있습니다 하는 표시 되지 않지만 여기에 사용자 지정 셀 내부 바인딩할 수 있습니다.

### <a name="cnum"></a>C&num;

C#에서 사용자 지정 셀을 지정 하는 것은 XAML에 해당 하는 보다 좀 더 자세한 정보입니다. 이 형식에 대해 살펴보겠습니다.

먼저 포함 한 사용자 지정 셀 클래스를 정의 `ViewCell` 기본 클래스로:

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

사용 하 여 페이지에 대 한 생성자는 `ListView`, ListView의 설정 `ItemTemplate` 속성을 새 `DataTemplate`:

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

에 대 한 생성자 `DataTemplate` 형식을 사용 합니다. Typeof 연산자에 대 한 CLR 형식을 가져옵니다 `CustomCell`합니다.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>바인딩 컨텍스트 변경

사용자 지정 셀 형식의에 바인딩할 때 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 인스턴스를 표시 하는 UI 컨트롤의 `BindableProperty` 값을 사용할지는 [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) 에 표시할 데이터를 설정 하려면 재정의 각 셀에 다음 코드 예제에서와 같이 셀 생성자 보다는:

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

[ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) 재정의 될 때 호출할는 [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/) 이벤트 발생의 값에 대 한 응답에는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성을 변경 합니다. 따라서 때는 `BindingContext` 변경 표시 하는 UI 컨트롤의 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 값 데이터를 설정 해야 합니다. `BindingContext` 확인할는 `null` 값이 있는 차례로 됩니다 가비지 수집에 대 한 Xamarin.Forms 하 여 설정할 수 있습니다는 `OnBindingContextChanged` 호출 되 고 재정의 합니다.

또한 UI 컨트롤에 바인딩할 수는 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 재정의할 필요가 없으므로 해당 값을 표시 하는 인스턴스는 `OnBindingContextChanged` 메서드.

> [!NOTE]
> 재정의 하는 경우 `OnBindingContextChanged`, 기본 클래스의 확인 `OnBindingContextChanged` 받도록 등록 된 대리자 메서드는 `BindingContextChanged` 이벤트입니다.

XAML에서 바인딩 사용자 지정 셀 유형 데이터를 얻을 수 있습니다 다음 코드 예제에 표시 된 대로:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

그러면 바인딩됩니다는 `Name`, `Age`, 및 `Location` 에 바인딩 가능한 속성의 `CustomCell` 인스턴스를 `Name`, `Age`, 및 `Location` 기본 컬렉션의 각 개체의 속성입니다.

C#의 동등한 바인딩 다음 코드 예제에 표시 됩니다.

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

IOS 및 Android에서 하는 경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 요소 재활용 되 고 사용자 지정 셀 사용자 지정 렌더러를 사용 하 여, 사용자 지정 렌더러 속성 변경 알림을 올바르게 구현 해야 합니다. 바인딩 컨텍스트를 사용할 수 있는 셀의 업데이트 된 경우 해당 속성 값이 변경 됩니다 셀이 다시 사용 하는 경우 `PropertyChanged` 발생 하는 이벤트입니다. 자세한 내용은 참조 [는 ViewCell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)합니다. 셀 재활용 하는 방법에 대 한 자세한 내용은 참조 [캐싱 전략](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)합니다.

## <a name="related-links"></a>관련 링크

- [셀 (샘플)에 기본 제공](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [사용자 지정 셀 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [바인딩 컨텍스트를 변경할 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
