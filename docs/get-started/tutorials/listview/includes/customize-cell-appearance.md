---
ms.openlocfilehash: 3c88b71cea834f5e6ef20d43332904c052c6e3a6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61037627"
---
이전에 [`ListView`](xref:Xamarin.Forms.ListView)는 데이터 바인딩을 사용하는 데이터로 채워졌습니다. 하지만 컬렉션에 데이터를 바인딩함에도 불구하고 컬렉션의 각 개체가 데이터의 여러 항목을 정의하는 경우 데이터의 단일 항목만 개체별로 표시되었습니다(`Monkey` 개체의 `Name` 속성).

이 연습에서는 [`ListView`](xref:Xamarin.Forms.ListView)가 각 행에 있는 데이터의 여러 항목을 표시하도록 **ListViewTutorial** 프로젝트를 수정합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`ListView`](xref:Xamarin.Forms.Image) 선언을 수정하여 각 행의 모양을 사용자 지정합니다.

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    이 코드는 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정합니다. 여기서는 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 각 행의 모양을 정의합니다. `DataTemplate`의 자식은 [`Cell`](xref:Xamarin.Forms.Cell) 형식이거나 이 형식에서 파생되어야 합니다. 이 코드는 `Cell`에서 파생되는 [`ViewCell`](xref:Xamarin.Forms.ViewCell)을 사용하여 각 `ListView` 행에 대한 사용자 지정 레이아웃을 만듭니다. `ViewCell` 내의 레이아웃은 여기에서 [`Grid`](xref:Xamarin.Forms.Grid)에 의해 관리됩니다. 여기에는 [`Image`](xref:Xamarin.Forms.Image) 개체 한 개와 [`Label`](xref:Xamarin.Forms.Label) 개체 두 개가 포함됩니다. `Image` 개체 데이터는 해당하는 [`Source`](xref:Xamarin.Forms.Image.Source) 속성을 각 `Monkey` 개체의 `ImageUrl` 속성에 바인딩하는 반면, `Label` 개체는 해당하는 [`Text`](xref:Xamarin.Forms.Label.Text) 속성을 `Monkey` 개체의 `Name` 및 `Location` 속성에 바인딩합니다.

    기본적으로 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 모든 행의 높이는 동일합니다. 그러나 이 코드는 [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) 속성을 `true`로 설정하여 내용에 맞게 행 높이를 조정할 수 있습니다. 여기서는 변수 길이 문자열을 포함하는 `Monkey` 개체의 `Name` 및 `Location` 속성을 적용합니다.

    [`ListView`](xref:Xamarin.Forms.ListView) 셀 모양에 대한 자세한 내용은 [ListView 셀 모양 사용자 지정](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)을 참조하세요. 데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![데이터 템플릿으로 항목의 템플릿이 지정된 ListView의 스크린샷](../images/customize-cell-appearance.png "템플릿 지정된 데이터를 표시하는 ListView")](../images/customize-cell-appearance-large.png#lightbox "템플릿 지정된 데이터를 표시하는 ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml**에서 [`ListView`](xref:Xamarin.Forms.Image) 선언을 수정하여 각 행의 모양을 사용자 지정합니다.

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    이 코드는 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정합니다. 여기서는 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 각 행의 모양을 정의합니다. `DataTemplate`의 자식은 [`Cell`](xref:Xamarin.Forms.Cell) 형식이거나 이 형식에서 파생되어야 합니다. 이 코드는 `Cell`에서 파생되는 [`ViewCell`](xref:Xamarin.Forms.ViewCell)을 사용하여 각 `ListView` 행에 대한 사용자 지정 레이아웃을 만듭니다. `ViewCell` 내의 레이아웃은 여기에서 [`Grid`](xref:Xamarin.Forms.Grid)에 의해 관리됩니다. 여기에는 [`Image`](xref:Xamarin.Forms.Image) 개체 한 개와 [`Label`](xref:Xamarin.Forms.Label) 개체 두 개가 포함됩니다. `Image` 개체 데이터는 해당하는 [`Source`](xref:Xamarin.Forms.Image.Source) 속성을 각 `Monkey` 개체의 `ImageUrl` 속성에 바인딩하는 반면, `Label` 개체는 해당하는 [`Text`](xref:Xamarin.Forms.Label.Text) 속성을 `Monkey` 개체의 `Name` 및 `Location` 속성에 바인딩합니다.

    기본적으로 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 모든 행의 높이는 동일합니다. 그러나 이 코드는 [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) 속성을 `true`로 설정하여 내용에 맞게 행 높이를 조정할 수 있습니다. 여기서는 변수 길이 문자열을 포함하는 `Monkey` 개체의 `Name` 및 `Location` 속성을 적용합니다.

    [`ListView`](xref:Xamarin.Forms.ListView) 셀 모양에 대한 자세한 내용은 [ListView 셀 모양 사용자 지정](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)을 참조하세요. 데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![데이터 템플릿으로 항목의 템플릿이 지정된 ListView의 스크린샷](../images/customize-cell-appearance.png "템플릿 지정된 데이터를 표시하는 ListView")](../images/customize-cell-appearance-large.png#lightbox "템플릿 지정된 데이터를 표시하는 ListView")
