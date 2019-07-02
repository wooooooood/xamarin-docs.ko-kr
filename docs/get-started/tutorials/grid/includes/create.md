---
ms.openlocfilehash: 807f0b7b2969d9f1039beb1a7ec7d535be5c84dc
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277349"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 자습서를 완료하려면 **.NET을 사용한 모바일 개발** 워크로드가 설치된 Visual Studio 2019(최신 릴리스)가 있어야 합니다. 또한 iOS에서 자습서 애플리케이션을 빌드하려면 페어링된 Mac이 필요합니다. Xamarin 플랫폼 설치에 대한 자세한 내용은 [Xamarin 설치](~/get-started/installation/index.md)를 참조하세요. Visual Studio 2019를 Mac 빌드 호스트에 연결하는 방법에 대한 자세한 내용은 [Xamarin.iOS 개발을 위해 Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)을 참조하세요.

1. Visual Studio를 실행하고 **GridTutorial**이라는 비어 있는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **GridTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **솔루션 탐색기**의 **GridTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="GridTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Label Text="The Grid has its Margin property set, to control the rendering position of the Grid." />
        </Grid>
    </ContentPage>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에서 [`Label`](xref:Xamarin.Forms.Label)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. 기본적으로 `Grid`는 단일 위치에 해당 자식 보기를 배치합니다. 따라서 여러 자식이 포함된 `Grid`는 열 및 행을 지정해야 합니다. 이에 대해 다음 연습에서 다룹니다. 또한 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 내에서 `Grid`의 렌더링 위치를 나타냅니다.

    > [!NOTE]
    > [`Margin`](xref:Xamarin.Forms.View.Margin) 속성 외에도 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성은 [`Grid`](xref:Xamarin.Forms.Grid)에서 설정될 수도 있습니다. [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성 값은 `Grid`에서 보기 간 거리를 지정합니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

1. Visual Studio 도구 모음에서 선택한 iOS 원격 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에 있는 그리드 레이블의 스크린샷](../images/create-grid.png "레이블이 포함된 그리드")](../images/create-grid-large.png#lightbox "레이블이 포함된 그리드s")

    [`Grid`](xref:Xamarin.Forms.Grid)에 대한 자세한 내용은 [Xamarin.Forms 그리드](~/xamarin-forms/user-interface/layouts/grid.md)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 자습서를 완료하려면 iOS 및 Android 플랫폼 지원이 설치된 Mac용 Visual Studio(최신 릴리스)가 있어야 합니다. 또한 Xcode(최신 릴리스)도 필요합니다. Xamarin 플랫폼 설치에 대한 자세한 내용은 [Xamarin 설치](~/get-started/installation/index.md)를 참조하세요.

1. Mac용 Visual Studio를 실행하고 **GridTutorial**이라는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **GridTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **Solution Pad**의 **GridTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="GridTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Label Text="The Grid has its Margin property set, to control the rendering position of the Grid." />
        </Grid>
    </ContentPage>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에서 [`Label`](xref:Xamarin.Forms.Label)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. 기본적으로 `Grid`는 단일 위치에 해당 자식 보기를 배치합니다. 따라서 여러 자식이 포함된 `Grid`는 열 및 행을 지정해야 합니다. 이에 대해 다음 연습에서 다룹니다. 또한 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 내에서 `Grid`의 렌더링 위치를 나타냅니다.

    > [!NOTE]
    > [`Margin`](xref:Xamarin.Forms.View.Margin) 속성 외에도 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성은 [`Grid`](xref:Xamarin.Forms.Grid)에서 설정될 수도 있습니다. [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성 값은 `Grid`에서 보기 간 거리를 지정합니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에 있는 그리드 레이블의 스크린샷](../images/create-grid.png "레이블이 포함된 그리드")](../images/create-grid-large.png#lightbox "레이블이 포함된 그리드s")

    [`Grid`](xref:Xamarin.Forms.Grid)에 대한 자세한 내용은 [Xamarin.Forms 그리드](~/xamarin-forms/user-interface/layouts/grid.md)를 참조하세요.
