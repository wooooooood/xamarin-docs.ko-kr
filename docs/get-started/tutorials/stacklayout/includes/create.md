---
ms.openlocfilehash: b2e1f11579e8647593e20e7d56936e8e75661e78
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80634711"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

이 자습서를 완료하려면 **.NET을 사용한 모바일 개발** 워크로드가 설치된 Visual Studio 2019(최신 릴리스)가 있어야 합니다. 또한 iOS에서 자습서 애플리케이션을 빌드하려면 페어링된 Mac이 필요합니다. Xamarin 플랫폼 설치에 대한 자세한 내용은 [Xamarin 설치](~/get-started/installation/index.md)를 참조하세요. Visual Studio 2019를 Mac 빌드 호스트에 연결하는 방법에 대한 자세한 내용은 [Xamarin.iOS 개발을 위해 Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)을 참조하세요.

1. Visual Studio를 실행하고, **StackLayoutTutorial**이라는 비어 있는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **StackLayoutTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/quickstarts/deepdive.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/quickstarts/deepdive.md#anatomy-of-a-xamarinforms-application)을 참조하세요.

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

    [![iOS 및 Android에서 StackLayout의 자식 보기 스크린샷](../images/create-stacklayout.png "레이블 인스턴스를 포함하는 StackLayout")](../images/create-stacklayout-large.png#lightbox "레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 대한 자세한 내용은 [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)을 참조하세요.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/vsmac)

이 자습서를 완료하려면 iOS 및 Android 플랫폼 지원이 설치된 Mac용 Visual Studio(최신 릴리스)가 있어야 합니다. 또한 Xcode(최신 릴리스)도 필요합니다. Xamarin 플랫폼 설치에 대한 자세한 내용은 [Xamarin 설치](~/get-started/installation/index.md)를 참조하세요.

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

    [![iOS 및 Android에서 StackLayout의 자식 보기 스크린샷](../images/create-stacklayout.png "레이블 인스턴스를 포함하는 StackLayout")](../images/create-stacklayout-large.png#lightbox "레이블 인스턴스를 포함하는 StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 대한 자세한 내용은 [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)을 참조하세요.
