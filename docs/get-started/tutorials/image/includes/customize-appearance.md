---
ms.openlocfilehash: 8a4fbed10fa52c08a63f3aed306fe5cde21804b4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384577"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Image`](xref:Xamarin.Forms.Image) 선언을 수정하여 모양을 사용자 지정합니다.

    ```xaml
    <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    이 코드는 이미지의 크기 조정 모드를 정의하는 [`Aspect`](xref:Xamarin.Forms.Image.Aspect) 속성을 [`Fill`](xref:Xamarin.Forms.Aspect.Fill)로 설정합니다. `Fill` 멤버는 [`Aspect`](xref:Xamarin.Forms.Aspect) 열거형에서 정의되고, 이미지를 왜곡하는지에 관계 없이 보기를 완전히 채우도록 이미지를 확장합니다. 이미지 크기 조정에 대한 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [이미지 표시](~/xamarin-forms/user-interface/images.md#displaying-images)를 참조하세요.

    `OnPlatform` 태그 확장을 사용하면 플랫폼별 기준으로 UI 모양을 사용자 지정할 수 있습니다. 이 예제에서는 태그 확장을 사용하여 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 및 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성을 iOS에서 300개 디바이스 독립적 단위로 지정하고, Android에서 250개 디바이스 독립적 단위로 지정합니다. `OnPlatform` 태그 확장에 대한 자세한 내용은 [XAML 태그 확장 사용](~/xamarin-forms/xaml/markup-extensions/consuming.md) 가이드에서 [OnPlatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)을 참조하세요.

    또한 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성은 이미지를 가로 방향으로 가운데에 맞추도록 지정합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 다르게 크기가 지정된 이미지의 스크린샷](../images/customize-appearance.png "플랫폼별 기준으로 크기가 지정된 이미지")](../images/customize-appearance-large.png#lightbox "플랫폼별 기준으로 크기가 지정된 이미지")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml**에서 [`Image`](xref:Xamarin.Forms.Image) 선언을 수정하여 모양을 사용자 지정합니다.

    ```xaml
    <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    이 코드는 이미지의 크기 조정 모드를 정의하는 [`Aspect`](xref:Xamarin.Forms.Image.Aspect) 속성을 [`Fill`](xref:Xamarin.Forms.Aspect.Fill)로 설정합니다. `Fill` 멤버는 [`Aspect`](xref:Xamarin.Forms.Aspect) 열거형에서 정의되고, 이미지를 왜곡하는지에 관계 없이 보기를 완전히 채우도록 이미지를 확장합니다. 이미지 크기 조정에 대한 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [이미지 표시](~/xamarin-forms/user-interface/images.md#displaying-images)를 참조하세요.

    `OnPlatform` 태그 확장을 사용하면 플랫폼별 기준으로 UI 모양을 사용자 지정할 수 있습니다. 이 예제에서는 태그 확장을 사용하여 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 및 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성을 iOS에서 300으로 설정하고, Android에서 250으로 설정합니다. `OnPlatform` 태그 확장에 대한 자세한 내용은 [XAML 태그 확장 사용](~/xamarin-forms/xaml/markup-extensions/consuming.md) 가이드에서 [OnPlatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)을 참조하세요.

    또한 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성은 이미지를 가로 방향으로 가운데에 맞추도록 지정합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 다르게 크기가 지정된 이미지의 스크린샷](../images/customize-appearance.png "플랫폼별 기준으로 크기가 지정된 이미지")](../images/customize-appearance-large.png#lightbox "플랫폼별 기준으로 크기가 지정된 이미지")
