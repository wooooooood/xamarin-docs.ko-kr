---
ms.openlocfilehash: a7c2aa15521b89e4930746dc5421ce67fa26b2b9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "67560010"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 선언을 수정하여 그 자식 요소를 세로가 아닌 가로로 정렬합니다.

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    이 코드는 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 속성을 [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal)로 설정합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 StackLayout 내 가로 방향 자식 뷰의 스크린샷](../images/orientation.png "가로 방향 레이블 인스턴스를 포함하는 StackLayout")](../images/orientation-large.png#lightbox "가로 방향 레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) 내의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스는 이제 세로가 아닌 가로로 정렬됩니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 선언을 수정하여 그 자식 요소를 세로가 아닌 가로로 정렬합니다.

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    이 코드는 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 속성을 [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal)로 설정합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 StackLayout 내 가로 방향 자식 뷰의 스크린샷](../images/orientation.png "가로 방향 레이블 인스턴스를 포함하는 StackLayout")](../images/orientation-large.png#lightbox "가로 방향 레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) 내의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스는 이제 세로가 아닌 가로로 정렬됩니다.
