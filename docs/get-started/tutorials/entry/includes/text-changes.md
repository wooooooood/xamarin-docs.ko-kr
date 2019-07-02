---
ms.openlocfilehash: 3130c20d39e0140695eed92ffa4941d6bafe796e
ms.sourcegitcommit: b4c9c574b771ae0265171ca5e938aed1c5e35028
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67394547"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Entry`](xref:Xamarin.Forms.Entry) 선언을 수정하여 [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    이 코드는 [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 이벤트를 `OnEntryTextChanged`라는 이벤트 처리기로 설정하고, [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트를 `OnEntryCompleted`라는 이벤트 처리기로 설정합니다. 두 가지 이벤트 처리기는 다음 단계에서 생성됩니다.

1. **솔루션 탐색기**의 **EntryTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnEntryTextChanged` 및 `OnEntryCompleted` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    [`Entry`](xref:Xamarin.Forms.Entry)에 있는 텍스트가 변경되면 `OnEntryTextChanged` 메서드가 실행됩니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Entry` 개체이며 `Entry` 개체에 액세스하는 데 사용될 수 있습니다. [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 인수는 앞뒤의 텍스트 변경 내용에서 이전 및 새 텍스트 값을 제공합니다.

    반환 키를 사용하여 [`Entry`](xref:Xamarin.Forms.Entry)에서 텍스트 입력을 완료하면 `OnEntryCompleted` 메서드가 실행됩니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Entry` 개체이며 `Entry` 개체에 액세스하는 데 사용될 수 있습니다.

    > [!IMPORTANT]
    > [`Entry`](xref:Xamarin.Forms.Entry)에 입력된 텍스트는 [`Text`](xref:Xamarin.Forms.Entry.Text) 속성에 저장됩니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 텍스트가 포함된 항목의 스크린샷](../images/text-changes.png "텍스트가 포함된 항목")](../images/text-changes-large.png#lightbox "텍스트가 포함된 항목")

    두 개의 이벤트 처리기에서 중단점을 설정하고, [`Entry`](xref:Xamarin.Forms.Entry)에 텍스트를 입력하고, [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생하는지 살펴봅니다.

    [`Entry`](xref:Xamarin.Forms.Entry) 이벤트에 대한 자세한 내용은 [Xamarin.Forms 항목](~/xamarin-forms/user-interface/text/entry.md) 가이드에서 [이벤트 및 대화형 작업](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity)을 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml**에서 [`Entry`](xref:Xamarin.Forms.Entry) 선언을 수정하여 [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    이 코드는 [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 이벤트를 `OnEntryTextChanged`라는 이벤트 처리기로 설정하고, [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트를 `OnEntryCompleted`라는 이벤트 처리기로 설정합니다. 두 가지 이벤트 처리기는 다음 단계에서 생성됩니다.

1. **Solution Pad**의 **EntryTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnEntryTextChanged` 및 `OnEntryCompleted` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    [`Entry`](xref:Xamarin.Forms.Entry)에 있는 텍스트가 변경되면 `OnEntryTextChanged` 메서드가 실행됩니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Entry` 개체이며 `Entry` 개체에 액세스하는 데 사용될 수 있습니다. [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 인수는 앞뒤의 텍스트 변경 내용에서 이전 및 새 텍스트 값을 제공합니다.

    반환 키를 사용하여 [`Entry`](xref:Xamarin.Forms.Entry)에서 텍스트 입력을 완료하면 `OnEntryCompleted` 메서드가 실행됩니다. `sender` 인수는 `TextChanged` 이벤트의 실행을 담당하는 `Entry` 개체이며 `Entry` 개체에 액세스하는 데 사용될 수 있습니다.

    > [!IMPORTANT]
    > [`Entry`](xref:Xamarin.Forms.Entry)에 입력된 텍스트는 [`Text`](xref:Xamarin.Forms.Entry.Text) 속성에 저장됩니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android에서 텍스트가 포함된 항목의 스크린샷](../images/text-changes.png "텍스트가 포함된 항목")](../images/text-changes-large.png#lightbox "텍스트가 포함된 항목")

    두 개의 이벤트 처리기에서 중단점을 설정하고, [`Entry`](xref:Xamarin.Forms.Entry)에 텍스트를 입력하고, [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생하는지 살펴봅니다.

    [`Entry`](xref:Xamarin.Forms.Entry) 이벤트에 대한 자세한 내용은 [Xamarin.Forms 항목](~/xamarin-forms/user-interface/text/entry.md) 가이드에서 [이벤트 및 대화형 작업](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity)을 참조하세요.
