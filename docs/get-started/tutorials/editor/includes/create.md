---
ms.openlocfilehash: ce3e0f18d299d2bdf8d9bd81c467d45924d0d2bc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61373428"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio를 실행하고 **EditorTutorial**라는 새 Xamarin.Forms 애플리케이션을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각은 솔루션의 이름이 **EditorTutorial**이어야 합니다. 이 자습서에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)의 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **솔루션 탐색기**의 **EditorTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Editor`](xref:Xamarin.Forms.Editor)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) 속성은 `Editor`가 처음 표시될 때 표시되는 자리 표시자 텍스트를 지정합니다. 또한 [`HeightRequest`](xref:Xamarin.Forms.VisualElement) 속성은 디바이스 독립적 단위로 `Editor`의 높이를 지정합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android의 편집기 스크린샷](../images/create-editor.png "자리 표시자 텍스트가 포함된 편집기")](../images/create-editor-large.png#lightbox "자리 표시자 텍스트가 포함된 편집기")

    > [!NOTE]
    > Android는 [`Editor`](xref:Xamarin.Forms.Editor)의 높이를 나타내지만 iOS는 그렇지 않습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio를 실행하고 **EditorTutorial**라는 새 Xamarin.Forms 애플리케이션을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각은 솔루션의 이름이 **EditorTutorial**이어야 합니다. 이 자습서에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)의 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **Solution Pad**의 **EditorTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Editor`](xref:Xamarin.Forms.Editor)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) 속성은 `Editor`가 처음 표시될 때 표시되는 자리 표시자 텍스트를 지정합니다. 또한 [`HeightRequest`](xref:Xamarin.Forms.VisualElement) 속성은 디바이스 독립적 단위로 `Editor`의 높이를 지정합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android의 편집기 스크린샷](../images/create-editor.png "자리 표시자 텍스트가 포함된 편집기")](../images/create-editor-large.png#lightbox "자리 표시자 텍스트가 포함된 편집기")

    > [!NOTE]
    > Android는 [`Editor`](xref:Xamarin.Forms.Editor)의 높이를 나타내지만 iOS는 그렇지 않습니다.
