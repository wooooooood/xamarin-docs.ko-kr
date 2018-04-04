---
title: 목록 모양
description: 머리글, 바닥글, 그룹 및 가변 높이 셀을 사용 하 여 Listview에 사용자 지정 합니다.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: ff36deff996a92fca512158252c64e5c29046be9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="list-appearance"></a>목록 모양

`ListView` 에 기본 외에도 전체 목록 표시를 제어 하기 위한 옵션이 `ViewCell`s입니다. 다음 옵션을 사용할 수 있습니다.

- [**그룹화** ](#Grouping) &ndash; 쉽게 탐색할 수 있도록 및 향상 된 조직에 대 한 ListView에서 항목을 그룹화 합니다.
- [**머리글 및 바닥글** ](#Headers_and_Footers) &ndash; 다른 항목으로 스크롤 하는 보기의 시작과 끝에 정보를 표시 합니다.
- [**행 구분 기호** ](#Row_Separators) &ndash; 표시 하거나 숨길 항목 사이 구분 기호 선입니다.
- [**가변 높이 행** ](#Row_Heights) &ndash; 기본적으로 모든 행이 높이 같게 만들기 하지만 서로 다른 높이를 표시할 수 있는 행 수 있도록이 변경할 수 있습니다.

<a name="Grouping" />

## <a name="grouping"></a>그룹화
대개 큰 데이터 집합을 지속적으로 스크롤 목록에 나타날 때 반환 될 수 있습니다. 사용할 수 있도록 그룹화 더 잘 콘텐츠를 구성 하 고 데이터를 탐색 하는 플랫폼 특정 컨트롤을 활성화 하 여 이러한 경우 사용자 경험을 향상 시킬 수 있습니다.

에 대 한 그룹화가 활성화 된 경우는 `ListView`, 각 그룹에 대 한 머리글 행이 추가 됩니다.

다음 그룹화를 설정 하려면

- (그룹 목록, 요소 목록이 되 고 각 그룹) 목록의 목록을 만듭니다.
- 설정의 `ListView`의 `ItemsSource` 해당 목록에 있습니다.
- 설정 `IsGroupingEnabled` true로 합니다.
- 설정 [ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) 그룹의 제목으로 사용 되는 그룹의 속성에 바인딩됩니다.
- [선택 사항] 설정 [ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) 그룹에 대 한 짧은 이름으로 사용 되는 그룹의 속성에 바인딩됩니다. 약식 이름은 점프 목록 (ios, Windows Phone 타일 눈금 rigt 측 열)에 사용 됩니다.

그룹에 대 한 클래스를 만들어 시작 합니다.

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

위의 코드에서 `All` 바인딩 소스로 우리의 ListView에 제공 될 목록입니다. `Title` 및 `ShortName` 는 속성 그룹 머리글에 사용할입니다.

이 단계에서 `All` 은 빈 목록입니다. 프로그램 시작 시 목록 채워집니다 있도록 정적 생성자를 추가 합니다.

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        }
        All = Groups; //set the publicly accessible list
}
```

위의 코드에서 호출 또한 우리 `Add` 의 요소에 대해 `groups`, 형식의 인스턴스는 `PageTypeGroup`합니다. 이것이 가능 하기 때문에 `PageTypeGroup` 에서 상속 `List<PageModel>`합니다. 이 위에서 언급 한 목록 패턴 목록의 예입니다.

그룹화 된 목록을 표시 하기 위한 XAML 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
        GroupDisplayBinding="{Binding Title}"
        GroupShortNameBinding="{Binding ShortName}"
        IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                     Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

그 결과는 다음과 같습니다.

![](customizing-list-appearance-images/grouping-depth.png "ListView 그룹화 예제")

하겠습니다 참고:

- 설정 `GroupShortNameBinding` 에 `ShortName` 그룹 클래스에 정의 된 속성
- 설정 `GroupDisplayBinding` 에 `Title` 그룹 클래스에 정의 된 속성
- 설정 `IsGroupingEnabled` true로
- 변경 된 `ListView`의 `ItemsSource` 그룹화 된 목록에

### <a name="customizing-grouping"></a>그룹화를 사용자 지정
ListView에서 그룹화 기본 구현 하는 방법을 살펴본 했으므로 그룹 머리글 표시를 사용자 지정 하는 방법을 살펴보겠습니다.

방식과 유사한 `ListView` 에 `ItemTemplate` 행 표시 되는 방식을 정의 하기 위한 `ListView` 에 `GroupHeaderTemplate`합니다. 다음은 사용자 지정 된 그룹 머리글 템플릿 사용 하 여 위쪽에서 ListView의 예입니다.

![](customizing-list-appearance-images/grouping-depth.png "사용자 지정 된 GroupHeaderTemplate 있는 ListView")


XAML에서 해당 디자인을 수행 하는 방법을 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
         GroupDisplayBinding="{Binding Title}"
         GroupShortNameBinding="{Binding ShortName}"
         IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding Subtitle}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding ShortName}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>머리글 및 바닥글
목록 요소의 스크롤 하는 머리글 및 바닥글을 표시 하는 ListView에 대 한 것 같습니다. 머리글 및 바닥글에는 더 복잡 한 레이아웃 또는 텍스트 문자열을 수 있습니다. 이 별도의 [그룹 섹션](#Grouping)합니다.

설정할 수 있습니다는 `Header` 및/또는 `Footer` 을 간단한 문자열로 값을 설정할 수 있습니다 이러한 더 복잡 한 레이아웃에 있습니다.
또한 `HeaderTemplate` 및 `FooterTemplate` 수 있는 속성이 데이터 바인딩을 지 원하는 머리글 및 바닥글에 대 한 보다 복잡 한 레이아웃을 만들 합니다.

간단한 머리글/바닥글을 만들려면만 표시할 텍스트를 머리글 또는 바닥글 속성을 설정 합니다. 코드

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

Xaml의 경우:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "머리글 및 바닥글이 포함 된 ListView")

사용자 지정 된 머리글 및 바닥글을 만들려면 머리글 및 바닥글 뷰를 정의 합니다.

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
        TextColor="Olive"
        BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
        TextColor="Gray"
        BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "사용자 지정 된 머리글 및 바닥글이 포함 된 ListView")

<a name="Row_Separators" />

## <a name="row-separators"></a>행 구분 기호
사이 구분 기호 선을 표시할지 `ListView` iOS 및 Android에서 기본적으로는 요소입니다. Windows Phone 해당 플랫폼 UX 관련 지침 당 구분선을 지원 하지 않습니다. IOS 및 Android에서 구분 기호 선 숨기기를 사용 하려는 경우에 설정 된 `SeparatorVisibility` 프로그램 ListView에서 속성입니다. 에 대 한 옵션 `SeparatorVisibility` 됩니다.

* **기본** -iOS 및 Android에서 이루어진 구분선을 표시 합니다.
* **None** -모든 플랫폼에서 구분 기호를 숨깁니다.

기본 표시:

C#: 

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "기본 행 구분 기호와 함께 ListView")

사용 권한 없음:

C#: 

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "행 구분 기호가 없는 ListView")

통해 구분 기호 선 색을 설정할 수도 있습니다는 `SeparatorColor` 속성:

C#: 

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "녹색 행 구분 기호와 함께 ListView")

> [!NOTE]
> 이러한 속성 중 하나를 Android에서 로드 한 후 설정 된 `ListView` 큰 성능 저하가 발생 합니다.

<a name="Row_Heights" />

## <a name="row-heights"></a>행 높이
ListView의 모든 행 기본적으로 높이 같게 만들기를 갖습니다. ListView에는 해당 동작을 변경 하는 데 사용할 수 있는 두 가지 속성이 있습니다.

- `HasUnevenRows` &ndash; `true`/`false` 값을 행 다양 한 높이 있을 경우로 설정 `true`합니다. 기본값은 `false`입니다.
- `RowHeight` &ndash; 경우 집합의 각 높이 행 `HasUnevenRows` 은 `false`합니다.

모든 행의 높이 설정 하 여 설정할 수 있습니다는 `RowHeight` 속성에는 `ListView`합니다.

### <a name="custom-fixed-row-height"></a>사용자 정의 고정된 행 높이

C#: 

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "고정된 행 높이 인 ListView")


### <a name="uneven-rows"></a>균등 하지 않은 행

높이가 서로 다른 개별 행을 선택 하는 경우 설정할 수 있습니다는 `HasUnevenRows` 속성을 `true`합니다.
행 높이 수동으로 한 번 설정할 필요가 없습니다 참고 `HasUnevenRows` 가로 설정 된 `true`Xamarin.Forms로 높이 자동으로 계산 것 이므로, 합니다.


C#: 

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "균등 하지 않은 행이 있는 ListView")

### <a name="runtime-resizing-of-rows"></a>행의 크기 조정이 런타임

개별 `ListView` 런타임 시 제공 하는 행을 프로그래밍 방식으로 조정할 수 있는 `HasUnevenRows` 속성이 `true`합니다. [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) 메서드는 다음 코드 예제에서와 같이, 그럴 경우에 셀의 크기를 업데이트 합니다.

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped` 대 한 응답으로 이벤트 처리기가 실행 되는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 셀에 중인 탭 및 크기를 증가 `Image` 는 쉽게 볼 수 있도록 셀에 표시 합니다.

![](customizing-list-appearance-images/dynamic-row-resizing.png "런타임 행 크기 조정 된 ListView")

이 기능은 지나치게 사용 하는 경우 성능 저하의 강력한 가능성 임을 note 합니다.



## <a name="related-links"></a>관련 링크

- [그룹화 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [사용자 지정 렌더러 보기 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [동적 크기 조정의 행 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [1.4의 릴리스 정보](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 릴리스 정보](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
