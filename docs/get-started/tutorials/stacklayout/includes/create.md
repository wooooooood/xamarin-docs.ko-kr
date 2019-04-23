---
ms.openlocfilehash: a7b23e47269ecca4d36a344ce3589443a4a85e31
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382557"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio를 실행하고, **StackLayoutTutorial**이라는 비어 있는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **StackLayoutTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **솔루션 탐색기**의 **StackLayoutTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Label`](xref:Xamarin.Forms.Label) 인스턴스 세 개로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 단일 줄에 해당 자식 보기(`Label` 인스턴스)를 배치하고, 기본적으로 세로 방향으로 표시합니다. 또한 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 내에서 `StackLayout`의 렌더링 위치를 나타냅니다.

    > [!NOTE]
    > [`Margin`](xref:Xamarin.Forms.View.Margin) 속성 외에도 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 및 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 속성은 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 설정될 수도 있습니다. [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성 값은 `StackLayout`에서 보기 사이의 거리를 지정하고, [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 속성 값은 `StackLayout`에서 각 자식 요소 사이의 공백을 지정합니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 StackLayout 자식 보기의 스크린샷](../images/create-stacklayout.png "레이블 인스턴스를 포함하는 StackLayout")](../images/create-stacklayout-large.png#lightbox "레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 대한 자세한 내용은 [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)을 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio를 실행하고, **StackLayoutTutorial**이라는 비어 있는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **StackLayoutTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **Solution Pad**의 **StackLayoutTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Label`](xref:Xamarin.Forms.Label) 인스턴스 세 개로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 단일 줄에 해당 자식 보기(`Label` 인스턴스)를 배치하고, 기본적으로 세로 방향으로 표시합니다. 또한 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 내에서 `StackLayout`의 렌더링 위치를 나타냅니다.

    > [!NOTE]
    > [`Margin`](xref:Xamarin.Forms.View.Margin) 속성 외에도 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 및 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 속성은 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 설정될 수도 있습니다. [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성 값은 `StackLayout`에서 보기 사이의 거리를 지정하고, [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 속성 값은 `StackLayout`에서 각 자식 요소 사이의 공백을 지정합니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 StackLayout 자식 보기의 스크린샷](../images/create-stacklayout.png "레이블 인스턴스를 포함하는 StackLayout")](../images/create-stacklayout-large.png#lightbox "레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 대한 자세한 내용은 [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)을 참조하세요.
