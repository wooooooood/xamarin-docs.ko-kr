---
ms.openlocfilehash: 93ee0681adcc63fe05b4be88ff67f0aeee3e03ca
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384589"
---
이미지 파일은 플랫폼 프로젝트에 추가되고 Xamarin.Forms 공유 코드에서 참조될 수 있습니다. 이미지를 배포하는 이 메서드는 다양한 플랫폼 또는 약간 다른 디자인의 다양한 해결 방법을 사용하는 경우와 같이 이미지가 특정 플랫폼을 지정하는 경우에 필요합니다.

이 연습에서는 **ImageTutorial** 솔루션을 수정하여 URI에서 다운로드된 이미지가 아닌 로컬 이미지를 표시합니다. 로컬 이미지는 Xamarin 로고이며 아래 단추를 클릭하여 다운로드됩니다.

> [!div class="nextstepaction"]
> [XamarinLogo.png 다운로드](https://raw.githubusercontent.com/xamarin/xamarin-forms-samples/master/UserInterface/PlatformSpecifics/Droid/Resources/drawable/XamarinLogo.png)

> [!IMPORTANT]
> 모든 플랫폼에서 단일 이미지를 사용하려면 *모든 플랫폼에서 동일한 파일 이름을 사용해야* 하고, 해당 이름은 유효한 Android 리소스 이름(예: 소문자, 숫자, 밑줄 및 마침표만 허용됨)이어야 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **솔루션 탐색기**의 **ImageTutorial.iOS** 프로젝트에서 **자산 카탈로그**를 확장하고, **자산**을 두 번 클릭하여 엽니다. 그런 다음, **Assets.xcassets** 탭에서 **더하기** 단추를 클릭하고 **이미지 세트 추가**를 선택합니다.

    ![Visual Studio의 자산 카탈로그에서 새 이미지 세트를 만드는 스크린샷](../images/vs/new-image-set.png "새 자산 카탈로그 이미지 세트")

1. **Assets.xcassets** 탭에서 새 이미지 세트를 선택하면 편집기가 표시됩니다.

    ![Visual Studio의 자산 카탈로그에서 새 이미지 세트의 스크린샷](../images/vs/new-image-set-editor.png "자산 카탈로그 세트 편집기")

1. **유니버설** 범주의 경우 파일 시스템에서 **1x** 상자로 **XamarinLogo.png**를 끌어옵니다.

    ![Visual Studio에서 이미지가 포함된 이미지 세트의 스크린샷](../images/vs/image-set-with-image.png "이미지가 포함된 이미지 세트")

1. **Assets.xcassets** 탭에서 새 이미지 세트의 이름을 마우스 오른쪽 단추로 클릭하고 이름을 **XamarinLogo**로 바꿉니다.

    ![Visual Studio에서 이름이 바뀐 이미지 세트의 스크린샷](../images/vs/rename-image-set.png "이미지 세트 이름 변경")

    **Assets.xcassets** 탭을 저장 후 닫습니다.

1. **솔루션 탐색기**의 **ImageTutorial.Android** 프로젝트에서 **Resources** 폴더를 확장합니다. 그런 다음, 시스템에서 **drawable** 폴더로 **XamarinLogo.png** 파일을 끌어옵니다.

    ![Visual Studio에 있는 Android 리소스인 이미지 파일의 스크린샷](../images/vs/android-resource.png "Android 리소스 폴더에 있는 로컬 이미지 파일")

    > [!NOTE]
    > Visual Studio에서는 이미지에 대한 빌드 작업을 **AndroidResource**로 자동으로 설정합니다.

1. **ImageTutorial** 프로젝트의 **MainPage.xaml**에서 [`Image`](xref:Xamarin.Forms.Editor) 선언을 수정하여 로컬 **XamarinLogo**파일을 표시합니다.

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    이 코드는 [`Source`](xref:Xamarin.Forms.Image.Source) 속성을 표시할 로컬 파일로 설정합니다. [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성은 iOS에서 300개 디바이스 독립적 단위로 설정되고, Android에서 250개 디바이스 독립적 단위로 설정됩니다. 또한 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성은 이미지를 가로 방향으로 가운데에 맞추도록 지정합니다.

    > [!NOTE]
    > iOS에서 PNG 이미지의 경우 [`Source`](xref:Xamarin.Forms.Image.Source) 속성에 지정된 파일 이름에서 **.png** 확장명을 생략할 수 있습니다. 다른 이미지 형식의 경우 확장명이 필요합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 로컬 이미지를 표시하는 이미지 보기의 스크린샷](../images/local-file.png "로컬 이미지를 표시하는 이미지 보기")](../images/local-file-large.png#lightbox "로컬 이미지를 표시하는 이미지 보기")

    로컬 이미지에 대한 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [로컬 이미지](~/xamarin-forms/user-interface/images.md#local-images)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad**의 **ImageTutorial.iOS** 프로젝트에서 **Assets.xcassets**를 두 번 클릭하여 엽니다. 그런 다음, **자산 목록**에서 **새 이미지 세트**를 마우스 오른쪽 단추로 클릭합니다.

    ![Mac용 Visual Studio의 자산 카탈로그에서 새 이미지 세트를 만드는 스크린샷](../images/vsmac/new-image-set.png "새 자산 카탈로그 이미지 세트")

1. **자산 목록**에서 새 이미지 세트를 선택하면 편집기가 표시됩니다.

    ![Mac용 Visual Studio의 자산 카탈로그에서 새 이미지 세트의 스크린샷](../images/vsmac/new-image-set-editor.png "자산 카탈로그 세트 편집기")

1. **유니버설** 범주의 경우 파일 시스템에서 **1x** 상자로 **XamarinLogo.png**를 끌어옵니다.

    ![Mac용 Visual Studio에서 이미지가 포함된 이미지 세트의 스크린샷](../images/vsmac/image-set-with-image.png "이미지가 포함된 이미지 세트")

1. **자산 목록**에서 새 이미지 세트의 이름을 두 번 클릭하고 이름을 **XamarinLogo**로 바꿉니다.

    ![Mac용 Visual Studio에서 이름이 바뀐 이미지 세트의 스크린샷](../images/vsmac/rename-image-set.png "이미지 세트 이름 변경")

1. **Solution Pad**의 **ImageTutorial.Android** 프로젝트에서 **Resources** 폴더를 확장합니다. 그런 다음, 시스템에서 **drawable** 폴더로 **XamarinLogo.png** 파일을 끌어옵니다.

1. **폴더에 파일 추가** 대화 상자에서 **확인**을 선택합니다.

    ![Mac용 Visual Studio에 있는 Android 리소스인 이미지 파일의 스크린샷](../images/vsmac/android-resource.png "Android 리소스 폴더에 있는 로컬 이미지 파일")

    > [!NOTE]
    > Mac용 Visual Studio에서는 이미지에 대한 빌드 작업을 **AndroidResource**로 자동으로 설정합니다.

1. **ImageTutorial** 프로젝트의 **MainPage.xaml**에서 [`Image`](xref:Xamarin.Forms.Editor) 선언을 수정하여 로컬 **XamarinLogo**파일을 표시합니다.

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    이 코드는 [`Source`](xref:Xamarin.Forms.Image.Source) 속성을 표시할 로컬 파일로 설정합니다. [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성은 iOS에서 300개 디바이스 독립적 단위로 설정되고, Android에서 250개 디바이스 독립적 단위로 설정됩니다. 또한 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성은 이미지를 가로 방향으로 가운데에 맞추도록 지정합니다.

    > [!NOTE]
    > iOS에서 PNG 이미지의 경우 [`Source`](xref:Xamarin.Forms.Image.Source) 속성에 지정된 파일 이름에서 **.png** 확장명을 생략할 수 있습니다. 다른 이미지 형식의 경우 확장명이 필요합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 로컬 이미지를 표시하는 이미지 보기의 스크린샷](../images/local-file.png "로컬 이미지를 표시하는 이미지 보기")](../images/local-file-large.png#lightbox "로컬 이미지를 표시하는 이미지 보기")

    로컬 이미지에 대한 자세한 내용은 [Xamarin.Forms의 이미지](~/xamarin-forms/user-interface/images.md) 가이드에서 [로컬 이미지](~/xamarin-forms/user-interface/images.md#local-images)를 참조하세요.
