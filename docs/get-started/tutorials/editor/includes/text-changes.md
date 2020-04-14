---
ms.openlocfilehash: 653d3677f96d7da78af61531c535b1b7db684e7e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "77135097"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Editor`](xref:Xamarin.Forms.Editor) 선언을 수정하여 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Editor.Completed) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    이 코드는 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 이벤트를 `OnEditorTextChanged`라는 이벤트 처리기로 설정하고, [`Completed`](xref:Xamarin.Forms.Editor.Completed) 이벤트를 `OnEditorCompleted`라는 이벤트 처리기로 설정합니다. 두 가지 이벤트 처리기는 다음 단계에서 생성됩니다.

1. **솔루션 탐색기**의 **EditorTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnEditorTextChanged` 및 `OnEditorCompleted` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    [`Editor`](xref:Xamarin.Forms.Editor)에 있는 텍스트가 변경되면 `OnEditorTextChanged` 메서드가 실행됩니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Editor` 개체이며 `Editor` 개체에 액세스하는 데 사용될 수 있습니다. [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 인수는 앞뒤의 텍스트 변경 내용에서 이전 및 새 텍스트 값을 제공합니다.

    편집이 완료되면 `OnEditorCompleted` 메서드가 실행됩니다. [`Editor`](xref:Xamarin.Forms.Editor)의 포커스를 제거하거나 추가로 iOS에서 "완료" 단추를 눌러서 수행합니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Editor` 개체이며 `Editor` 개체에 액세스하는 데 사용될 수 있습니다.

    > [!IMPORTANT]
    > [`Editor`](xref:Xamarin.Forms.Editor)에 입력된 텍스트는 [`Text`](xref:Xamarin.Forms.InputView.Text) 속성에 저장됩니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 텍스트를 포함하는 편집기의 스크린샷](../images/text-changes.png "텍스트를 포함하는 편집기")](../images/text-changes-large.png#lightbox "텍스트를 포함하는 편집기")

    두 개의 이벤트 처리기에서 중단점을 설정하고, [`Editor`](xref:Xamarin.Forms.Editor)에 텍스트를 입력하고, [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 이벤트가 발생하는지 살펴봅니다. `Editor`의 포커스를 제거하여 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생하는지 살펴봅니다.

    [`Editor`](xref:Xamarin.Forms.Editor) 이벤트에 대한 자세한 내용은 [Xamarin.Forms 편집기](~/xamarin-forms/user-interface/text/editor.md) 가이드에서 [대화형 작업](~/xamarin-forms/user-interface/text/editor.md#interactivity)을 참조하세요.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`Editor`](xref:Xamarin.Forms.Editor) 선언을 수정하여 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Editor.Completed) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    이 코드는 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 이벤트를 `OnEditorTextChanged`라는 이벤트 처리기로 설정하고, [`Completed`](xref:Xamarin.Forms.Editor.Completed) 이벤트를 `OnEditorCompleted`라는 이벤트 처리기로 설정합니다. 두 가지 이벤트 처리기는 다음 단계에서 생성됩니다.

1. **Solution Pad**의 **EditorTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnEditorTextChanged` 및 `OnEditorCompleted` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    [`Editor`](xref:Xamarin.Forms.Editor)에 있는 텍스트가 변경되면 `OnEditorTextChanged` 메서드가 실행됩니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Editor` 개체이며 `Editor` 개체에 액세스하는 데 사용될 수 있습니다. [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 인수는 앞뒤의 텍스트 변경 내용에서 이전 및 새 텍스트 값을 제공합니다.

    편집이 완료되면 `OnEditorCompleted` 메서드가 실행됩니다. [`Editor`](xref:Xamarin.Forms.Editor)의 포커스를 제거하거나 추가로 iOS에서 "완료" 단추를 눌러서 수행합니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Editor` 개체이며 `Editor` 개체에 액세스하는 데 사용될 수 있습니다.

    > [!IMPORTANT]
    > [`Editor`](xref:Xamarin.Forms.Editor)에 입력된 텍스트는 [`Text`](xref:Xamarin.Forms.InputView.Text) 속성에 저장됩니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 텍스트를 포함하는 편집기의 스크린샷](../images/text-changes.png "텍스트를 포함하는 편집기")](../images/text-changes-large.png#lightbox "텍스트를 포함하는 편집기")

    두 개의 이벤트 처리기에서 중단점을 설정하고, [`Editor`](xref:Xamarin.Forms.Editor)에 텍스트를 입력하고, [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 이벤트가 발생하는지 살펴봅니다. `Editor`의 포커스를 제거하여 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생하는지 살펴봅니다.

    [`Editor`](xref:Xamarin.Forms.Editor) 이벤트에 대한 자세한 내용은 [Xamarin.Forms 편집기](~/xamarin-forms/user-interface/text/editor.md) 가이드에서 [대화형 작업](~/xamarin-forms/user-interface/text/editor.md#interactivity)을 참조하세요.
