---
ms.openlocfilehash: 550828327eb6b72c1c9712e54e6e36c9e30bd1f0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384578"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio를 실행하고 **ImageTutorial**이라는 비어 있는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **ImageTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **솔루션 탐색기**의 **ImageTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Image`](xref:Xamarin.Forms.Image)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Image.Source`](xref:Xamarin.Forms.Image.Source) 속성은 URI를 통해 표시할 이미지를 지정합니다. [`Image.Source`](xref:Xamarin.Forms.Image.Source) 속성은 [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식입니다. 그러면 이미지를 파일, URI 또는 리소스에서 가져올 수 있습니다. 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [이미지 표시](~/xamarin-forms/user-interface/images.md#displaying-images)를 참조하세요.

    [`HeightRequest`](xref:Xamarin.Forms.VisualElement) 속성은 디바이스 독립적 단위로 `Image`의 높이를 지정합니다.

    > [!NOTE]
    > 하지만 이 예제에서 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성을 설정하는 데 필요하지 않습니다. 기본적으로 [`Image`](xref:Xamarin.Forms.Image)가 이미지의 가로 세로 비율을 유지 관리하기 때문입니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에 있는 이미지의 스크린샷](../images/create-image.png "이미지를 표시하는 이미지 보기")](../images/create-image-large.png#lightbox "이미지를 표시하는 이미지 보기")

    > [!NOTE]
    > [`Image`](xref:Xamarin.Forms.Image) 보기는 24시간 동안 다운로드된 이미지를 자동으로 캐시합니다. 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [다운로드된 이미지 캐싱](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)을 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio를 실행하고 **ImageTutorial**이라는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **ImageTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **Solution Pad**의 **ImageTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Image`](xref:Xamarin.Forms.Image)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Image.Source`](xref:Xamarin.Forms.Image.Source) 속성은 URI를 통해 표시할 이미지를 지정합니다. [`Image.Source`](xref:Xamarin.Forms.Image.Source) 속성은 [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식입니다. 그러면 이미지를 파일, URI 또는 리소스에서 가져올 수 있습니다. 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [이미지 표시](~/xamarin-forms/user-interface/images.md#displaying-images)를 참조하세요.

    [`HeightRequest`](xref:Xamarin.Forms.VisualElement) 속성은 디바이스 독립적 단위로 `Image`의 높이를 지정합니다.

    > [!NOTE]
    > 하지만 이 예제에서 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성을 설정하는 데 필요하지 않습니다. 기본적으로 [`Image`](xref:Xamarin.Forms.Image)가 이미지의 가로 세로 비율을 유지 관리하기 때문입니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에 있는 이미지의 스크린샷](../images/create-image.png "이미지를 표시하는 이미지 보기")](../images/create-image-large.png#lightbox "이미지를 표시하는 이미지 보기")

    > [!NOTE]
    > [`Image`](xref:Xamarin.Forms.Image) 보기는 24시간 동안 다운로드된 이미지를 자동으로 캐시합니다. 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [다운로드된 이미지 캐싱](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)을 참조하세요.
