---
ms.openlocfilehash: fa88f6e7844899926a194e9d0cdd455a497c2b31
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83435397"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Grid`](xref:Xamarin.Forms.Grid) 선언을 수정하여 열과 행을 정의하고 열과 행에 분산된 콘텐츠를 배치합니다.

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에 대한 열과 행을 정의하고, 특정 열과 행에 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 배치합니다. 첫 번째 `Label`은 텍스트가 여러 열에 분산되도록 [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 연결된 속성을 설정합니다. `ColumnSpan` 속성은 2로 설정되며 `Label`이 분산된 열 수를 나타냅니다. 두 번째 `Label`은 텍스트가 여러 행에 분산되도록 [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 연결된 속성을 설정합니다. `RowSpan` 속성은 2로 설정되며 `Label`이 분산된 행 수를 나타냅니다.

1. Visual Studio 도구 모음에서 선택한 iOS 원격 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 여러 열 및 행에 걸쳐 있는 콘텐츠를 포함하는 그리드의 스크린샷](../images/span-columns-rows.png "여러 열 및 행에 걸쳐 있는 콘텐츠를 포함하는 그리드")](../images/span-columns-rows-large.png#lightbox "여러 열 및 행에 걸쳐 있는 콘텐츠를 포함하는 그리드")

    열과 행을 분산하는 방법에 대한 자세한 내용은 [Xamarin.Forms 그리드](~/xamarin-forms/user-interface/layouts/grid.md) 가이드에서 [행 및 열](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns)을 참조하세요.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`Grid`](xref:Xamarin.Forms.Grid) 선언을 수정하여 열과 행을 정의하고 열과 행에 분산된 콘텐츠를 배치합니다.

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에 대한 열과 행을 정의하고, 특정 열과 행에 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 배치합니다. 첫 번째 `Label`은 텍스트가 여러 열에 분산되도록 [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 연결된 속성을 설정합니다. `ColumnSpan` 속성은 2로 설정되며 `Label`이 분산된 열 수를 나타냅니다. 두 번째 `Label`은 텍스트가 여러 행에 분산되도록 [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 연결된 속성을 설정합니다. `RowSpan` 속성은 2로 설정되며 `Label`이 분산된 행 수를 나타냅니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 여러 열 및 행에 걸쳐 있는 콘텐츠를 포함하는 그리드의 스크린샷](../images/span-columns-rows.png "여러 열 및 행에 걸쳐 있는 콘텐츠를 포함하는 그리드")](../images/span-columns-rows-large.png#lightbox "여러 열 및 행에 걸쳐 있는 콘텐츠를 포함하는 그리드")

    열과 행을 분산하는 방법에 대한 자세한 내용은 [Xamarin.Forms 그리드](~/xamarin-forms/user-interface/layouts/grid.md) 가이드에서 [행 및 열](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns)을 참조하세요.
