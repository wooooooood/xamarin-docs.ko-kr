---
ms.openlocfilehash: effebf02a7fb84ef955f4dcfda75d5273ef96b29
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384725"
---

Xamarin.Forms에는 작업 시트로 알려진 모달 팝업이 있으며, 이 팝업은 사용자에게 작업을 안내하는 데 사용할 수 있습니다. 이 연습에서는 [`Page`](xref:Xamarin.Forms.Page) 클래스의 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드를 사용하여 사용자에게 작업을 안내하는 작업 시트를 표시합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 작업 시트를 표시하는 새로운 [`Button`](xref:Xamarin.Forms.Button) 선언을 추가합니다.

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

     [`Button.Text`](xref:Xamarin.Forms.Button.Text) 속성은 `Button`에 나타나는 텍스트를 지정합니다. 또한 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트는 다음 단계에서 만들 `OnDisplayActionSheetButtonClicked`라는 이벤트 처리기로 설정됩니다.

1. **솔루션 탐색기**의 **PopupsTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnDisplayActionSheetButtonClicked` 처리기를 클래스에 추가합니다.

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 탭하면 `OnDisplayActionSheetButtonClicked` 메서드가 실행됩니다. 이 메서드는 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드를 호출하여 사용자에게 작업 진행 방법에 대한 일련의 대안을 제공합니다. 사용자가 대안 중 하나를 선택하면 선택 영역이 `string`으로 반환됩니다.

    > [!IMPORTANT]
    > [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드는 비동기이며 `await` 키워드와 함께 항상 대기해야 합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. 그런 다음, [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 추가된 [`Button`](xref:Xamarin.Forms.Button)을 탭합니다.

    [![iOS 및 Android의 작업 시트 스크린샷](../images/actionsheet.png "사용자에게 작업을 안내하는 작업 시트")](../images/actionsheet-large.png#lightbox "사용자에게 작업을 안내하는 작업 시트")

    작업 시트 대화 상자에서 대안을 선택한 후 선택 영역이 Visual Studio **출력** 창에 출력되는지 확인합니다.

    작업 시트를 표시하는 방법에 대한 자세한 내용은 [팝업 표시](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md) 안내서의 [사용자에게 작업 안내](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md#guiding-users-through-tasks)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml**에서 작업 시트를 표시하는 새로운 [`Button`](xref:Xamarin.Forms.Button) 선언을 추가합니다.

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

    [`Button.Text`](xref:Xamarin.Forms.Button.Text) 속성은 `Button`에 나타나는 텍스트를 지정합니다. 또한 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트는 다음 단계에서 만들 `OnDisplayActionSheetButtonClicked`라는 이벤트 처리기로 설정됩니다.

1. **Solution Pad**의 **PopupsTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnDisplayActionSheetButtonClicked` 처리기를 클래스에 추가합니다.

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 탭하면 `OnDisplayActionSheetButtonClicked` 메서드가 실행됩니다. 이 메서드는 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드를 호출하여 사용자에게 작업 진행 방법에 대한 일련의 대안을 제공합니다. 사용자가 대안 중 하나를 선택하면 선택 영역이 `string`으로 반환됩니다.

    > [!IMPORTANT]
    > [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드는 비동기이며 `await` 키워드와 함께 항상 대기해야 합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. 그런 다음, [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 추가된 [`Button`](xref:Xamarin.Forms.Button)을 탭합니다.

    [![iOS 및 Android의 작업 시트 스크린샷](../images/actionsheet.png "사용자에게 작업을 안내하는 작업 시트")](../images/actionsheet-large.png#lightbox "사용자에게 작업을 안내하는 작업 시트")

    작업 시트 대화 상자에서 대안을 선택한 후 선택 영역이 Mac용 Visual Studio **출력** 창에 출력되는지 확인합니다.

    작업 시트를 표시하는 방법에 대한 자세한 내용은 [팝업 표시](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md) 안내서의 [사용자에게 작업 안내](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md#guiding-users-through-tasks)를 참조하세요.
