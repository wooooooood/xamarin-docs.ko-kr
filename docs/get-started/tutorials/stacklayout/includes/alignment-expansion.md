---
ms.openlocfilehash: 97e52d593e2b59d127da324d8ad74ad7b2096cdd
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83343421"
---
[`StackLayout`](xref:Xamarin.Forms.StackLayout) 내에 자식 뷰의 크기와 위치는 자식 뷰의 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest), [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성 값과 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성 값에 따라 달라집니다.

[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성은 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 구조체의 필드로 설정할 수 있습니다. 이 구조체는 두 가지 레이아웃 기본 설정을 캡슐화합니다.

- **맞춤** – 자식 뷰의 기본 맞춤은 해당 부모 레이아웃 내의 위치와 크기를 결정합니다.
- **확장** - 사용 가능한 경우([`StackLayout`](xref:Xamarin.Forms.StackLayout)에서만 사용) 자식 뷰가 추가 공간을 사용해야 하는지 여부를 나타냅니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 선언을 수정하여 각 [`Label`](xref:Xamarin.Forms.Label)에 대한 맞춤 및 확장 옵션을 설정합니다.

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    이 코드는 처음 네 개의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스의 맞춤 기본과 마지막 네 개의 `Label` 인스턴스의 확장 기본을 설정합니다. [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) 및 [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 필드는 부모 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 내에서 [`Label`](xref:Xamarin.Forms.Label) 인스턴스의 맞춤을 정의하는 데 사용됩니다. [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) 및 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) 필드는 맞춤 기본 설정과 부모 `StackLayout` 내에서 사용 가능한 경우 보기에서 더 많은 공간을 차지할 것인지 여부를 정의하는 데 사용됩니다.

    > [!NOTE]
    > 보기 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성의 기본값은 [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)입니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 맞춤 및 확장 옵션이 설정된 StackLayout 내 자식 뷰의 스크린샷](../images/alignment-expansion.png "맞춤 및 확장이 설정된 레이블 인스턴스를 포함하는 StackLayout")](../images/alignment-expansion-large.png#lightbox "맞춤 및 확장이 설정된 레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 `StackLayout` 방향과 반대 방향에 있는 자식 뷰의 맞춤 기본 설정을 따릅니다. 따라서 세로 방향 `StackLayout` 내의 [`Label`](xref:Xamarin.Forms.Label) 자식 뷰는 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성을 맞춤 필드 중 하나로 설정합니다.

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 왼쪽에 [`Label`](xref:Xamarin.Forms.Label)을 배치합니다.
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 [`Label`](xref:Xamarin.Forms.Label)을 집중시킵니다.
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End)는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 오른쪽에 [`Label`](xref:Xamarin.Forms.Label)을 배치합니다.
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)은 [`Label`](xref:Xamarin.Forms.Label)이 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 너비를 채웁니다.

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 해당 방향으로만 자식 뷰를 확장할 수 있습니다. 따라서 세로 방향 `StackLayout`은 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 확장 필드 중 하나로 설정하는 [`Label`](xref:Xamarin.Forms.Label) 자식 뷰를 확장할 수 있습니다. 즉, 세로 맞춤의 경우 각 `Label`은 `StackLayout` 내의 동일한 공간을 차지합니다. 그러나 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)로 설정하는 최종 `Label`만 크기가 다릅니다.

    > [!IMPORTANT]
    > [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 모든 공간을 사용할 때 확장 기본 설정은 아무런 영향을 미치지 않습니다.

    맞춤 및 확장에 대한 자세한 내용은 [xamarin.Forms의 레이아웃 옵션](~/xamarin-forms/user-interface/layouts/layout-options.md)을 참조하세요.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 선언을 수정하여 각 [`Label`](xref:Xamarin.Forms.Label)에 대한 맞춤 및 확장 옵션을 설정합니다.

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    이 코드는 처음 네 개의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스의 맞춤 기본과 마지막 네 개의 `Label` 인스턴스의 확장 기본을 설정합니다. [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) 및 [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 필드는 부모 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 내에서 [`Label`](xref:Xamarin.Forms.Label) 인스턴스의 맞춤을 정의하는 데 사용됩니다. [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) 및 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) 필드는 맞춤 기본 설정과 부모 `StackLayout` 내에서 사용 가능한 경우 보기에서 더 많은 공간을 차지할 것인지 여부를 정의하는 데 사용됩니다.

    > [!NOTE]
    > 보기 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성의 기본값은 [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)입니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 맞춤 및 확장 옵션이 설정된 StackLayout 내 자식 뷰의 스크린샷](../images/alignment-expansion.png "맞춤 및 확장이 설정된 레이블 인스턴스를 포함하는 StackLayout")](../images/alignment-expansion-large.png#lightbox "맞춤 및 확장이 설정된 레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 `StackLayout` 방향과 반대 방향에 있는 자식 뷰의 맞춤 기본 설정을 따릅니다. 따라서 세로 방향 `StackLayout` 내의 [`Label`](xref:Xamarin.Forms.Label) 자식 뷰는 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성을 맞춤 필드 중 하나로 설정합니다.

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 왼쪽에 [`Label`](xref:Xamarin.Forms.Label)을 배치합니다.
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 [`Label`](xref:Xamarin.Forms.Label)을 집중시킵니다.
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End)는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 오른쪽에 [`Label`](xref:Xamarin.Forms.Label)을 배치합니다.
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)은 [`Label`](xref:Xamarin.Forms.Label)이 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 너비를 채웁니다.

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 해당 방향으로만 자식 뷰를 확장할 수 있습니다. 따라서 세로 방향 `StackLayout`은 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 확장 필드 중 하나로 설정하는 [`Label`](xref:Xamarin.Forms.Label) 자식 뷰를 확장할 수 있습니다. 즉, 세로 맞춤의 경우 각 `Label`은 `StackLayout` 내의 동일한 공간을 차지합니다. 그러나 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)로 설정하는 최종 `Label`만 크기가 다릅니다.

    > [!IMPORTANT]
    > [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 모든 공간을 사용할 때 확장 기본 설정은 아무런 영향을 미치지 않습니다.

    맞춤 및 확장에 대한 자세한 내용은 [xamarin.Forms의 레이아웃 옵션](~/xamarin-forms/user-interface/layouts/layout-options.md)을 참조하세요.
