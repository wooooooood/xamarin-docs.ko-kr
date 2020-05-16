---
ms.openlocfilehash: e1705e9311d4875a5b866ac5acb7d20aa107a2bb
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83435416"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Grid`](xref:Xamarin.Forms.Grid) 선언을 수정하여 열과 행을 정의하고 콘텐츠를 열과 행에 배치합니다.

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에 대한 열과 행 및 특정 열과 행에 있는 위치 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 정의합니다. 열 및 행 데이터는 각각 [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) 및 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) 개체의 컬렉션인 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 및 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 속성에 저장됩니다. 각 `ColumnDefinition`의 너비는 [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) 속성에 의해 설정되고 각 `RowDefinition`의 높이는 [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) 속성에 의해 설정됩니다. 유효한 너비와 높이 값은 다음과 같습니다.

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto)는 콘텐츠에 맞게 열이나 행의 크기를 조정합니다.
    - 비례 값은 열과 행의 크기를 나머지 공간의 비율로 나타냅니다. 비례 값은 `*`로 끝나는 것으로 표시됩니다.
    - 절대 값은 열 또는 행의 크기를 고정 값으로 지정합니다.

    따라서 위의 코드에서 각 열의 너비는 [`Grid`](xref:Xamarin.Forms.Grid)의 절반인 반면, 각 행은 디바이스 독립 단위 50개의 높이를 가지고 있습니다.

    [`Label`](xref:Xamarin.Forms.Label)에서 각 [`Grid`](xref:Xamarin.Forms.Grid)의 위치는 0 기반 인덱스를 사용하여 [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) 및 [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) 부속 속성으로 지정됩니다. 따라서 첫 번째 열은 0 열이고 첫 번째 행은 0 행입니다. 첫 번째 `Label`에는 이러한 연결된 속성이 없습니다. 설정하지 않은 모든 자식 뷰는 0 열, 0 행에 자동으로 렌더링되기 때문입니다.

    > [!NOTE]
    > [`Grid`](xref:Xamarin.Forms.Grid)의 열과 행 사이의 간격은 [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) 및 [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) 속성으로 설정할 수 있습니다. 자세한 내용은 [Xamarin.Forms 그리드](~/xamarin-forms/user-interface/layouts/grid.md#space-between-rows-and-columns) 가이드에서 [간격](~/xamarin-forms/user-interface/layouts/grid.md)을 참조하세요.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 열 및 행에 배치된 콘텐츠를 포함하는 그리드의 스크린샷](../images/columns-rows.png "열 및 행에 콘텐츠를 포함하는 그리드")](../images/columns-rows-large.png#lightbox "열 및 행에 콘텐츠를 포함하는 그리드")

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`Grid`](xref:Xamarin.Forms.Grid) 선언을 수정하여 열과 행을 정의하고 콘텐츠를 열과 행에 배치합니다.

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에 대한 열과 행 및 특정 열과 행에 있는 위치 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 정의합니다. 열 및 행 데이터는 각각 [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) 및 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) 개체의 컬렉션인 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 및 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 속성에 저장됩니다. 각 `ColumnDefinition`의 너비는 [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) 속성에 의해 설정되고 각 `RowDefinition`의 높이는 [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) 속성에 의해 설정됩니다. 유효한 너비와 높이 값은 다음과 같습니다.

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto)는 콘텐츠에 맞게 열이나 행의 크기를 조정합니다.
    - 비례 값은 열과 행의 크기를 나머지 공간의 비율로 나타냅니다. 비례 값은 `*`로 끝나는 것으로 표시됩니다.
    - 절대 값은 열 또는 행의 크기를 고정 값으로 지정합니다.

    따라서 위의 코드에서 각 열의 너비는 [`Grid`](xref:Xamarin.Forms.Grid)의 절반인 반면, 각 행은 디바이스 독립 단위 50개의 높이를 가지고 있습니다.

    [`Label`](xref:Xamarin.Forms.Label)에서 각 [`Grid`](xref:Xamarin.Forms.Grid)의 위치는 0 기반 인덱스를 사용하여 [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) 및 [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) 부속 속성으로 지정됩니다. 따라서 첫 번째 열은 0 열이고 첫 번째 행은 0 행입니다. 첫 번째 `Label`에는 이러한 연결된 속성이 없습니다. 설정하지 않은 모든 자식 뷰는 0 열, 0 행에 자동으로 렌더링되기 때문입니다.

    > [!NOTE]
    > [`Grid`](xref:Xamarin.Forms.Grid)의 열과 행 사이의 간격은 [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) 및 [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) 속성으로 설정할 수 있습니다. 자세한 내용은 [Xamarin.Forms 그리드](~/xamarin-forms/user-interface/layouts/grid.md#space-between-rows-and-columns) 가이드에서 [간격](~/xamarin-forms/user-interface/layouts/grid.md)을 참조하세요.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 열 및 행에 배치된 콘텐츠를 포함하는 그리드의 스크린샷](../images/columns-rows.png "열 및 행에 콘텐츠를 포함하는 그리드")](../images/columns-rows-large.png#lightbox "열 및 행에 콘텐츠를 포함하는 그리드")
