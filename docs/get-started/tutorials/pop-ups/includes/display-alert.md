---
ms.openlocfilehash: 1e7b4b2ce4b7592c2350af20cf516436e244d95f
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66193783"
---
Xamarin.Forms에는 사용자에게 경고를 표시하고 간단한 질문을 묻는 모달 팝업(경고라고도 함)이 있습니다. 이 연습에서는 [`Page`](xref:Xamarin.Forms.Page) 클래스에서 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드를 사용하여 사용자에게 경고를 표시하고 간단한 질문을 묻습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio를 실행하고 **PopupsTutorial**이라는 비어 있는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **PopupsTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **솔루션 탐색기**의 **PopupsTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Button`](xref:Xamarin.Forms.Button) 개체 두 개로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Button.Text`](xref:Xamarin.Forms.Button.Text) 속성은 `Button`의 각각에서 표시되는 텍스트를 지정하고, [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트는 다음 단계에서 생성될 이벤트 처리기로 설정됩니다.

1. **솔루션 탐색기**의 **PopupsTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnDisplayAlertButtonClicked` 및 `OnDisplayAlertQuestionButtonClicked` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 누르면 해당하는 이벤트 처리기 메서드가 실행됩니다. `OnDisplayAlertButtonClicked` 메서드는 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드를 호출하여 단일 취소 단추를 포함하는 모달 경고를 표시합니다. 경고가 해제되면 사용자는 애플리케이션과 계속 상호 작용할 수 있습니다.

    `OnDisplayAlertQuestionButtonClicked` 메서드는 [ `DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드의 오버로드를 호출하여 수락 단추와 취소 단추를 포함하는 모달 경고를 표시합니다. 사용자가 단추 중 하나를 선택하면 선택 영역이 `boolean`으로 반환됩니다.

    > [!IMPORTANT]
    > [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드는 비동기이며 `await` 키워드와 함께 항상 대기해야 합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. 그런 다음, 첫 번째 [`Button`](xref:Xamarin.Forms.Button)을 누릅니다.

    [![iOS 및 Android에서 경고 스크린샷](../images/alert.png "경고")](../images/alert-large.png#lightbox "경고")

    경고를 해제한 후에 두 번째 [`Button`](xref:Xamarin.Forms.Button)을 누릅니다.

    [![iOS 및 Android에서 질문을 묻는 경고 메시지의 스크린샷](../images/alert-question.png "질문을 묻는 경고")](../images/alert-question-large.png#lightbox "질문을 묻는 경고")

    질문에 대한 응답을 선택한 후에 응답이 Visual Studio **출력** 창에 출력되는지 살펴봅니다.

    경고를 표시하는 방법에 대한 자세한 내용은 [팝업 표시](~/xamarin-forms/user-interface/pop-ups.md) 가이드에서 [경고 표시](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio를 실행하고 **PopupsTutorial**이라는 새 Xamarin.Forms 앱을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지를 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각에서 솔루션의 이름이 **PopupsTutorial**이어야 합니다. 이 자습서의 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)에서 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **Solution Pad**의 **PopupsTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Button`](xref:Xamarin.Forms.Button) 개체 두 개로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Button.Text`](xref:Xamarin.Forms.Button.Text) 속성은 `Button`의 각각에서 표시되는 텍스트를 지정하고, [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트는 다음 단계에서 생성될 이벤트 처리기로 설정됩니다.

1. **Solution Pad**의 **PopupsTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnDisplayAlertButtonClicked` 및 `OnDisplayAlertQuestionButtonClicked` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 누르면 해당하는 이벤트 처리기 메서드가 실행됩니다. `OnDisplayAlertButtonClicked` 메서드는 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드를 호출하여 단일 취소 단추를 포함하는 모달 경고를 표시합니다. 경고가 해제되면 사용자는 애플리케이션과 계속 상호 작용할 수 있습니다.

    `OnDisplayAlertQuestionButtonClicked` 메서드는 [ `DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드의 오버로드를 호출하여 수락 단추와 취소 단추를 포함하는 모달 경고를 표시합니다. 사용자가 단추 중 하나를 선택하면 선택 영역이 `boolean`으로 반환됩니다.

    > [!IMPORTANT]
    > [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드는 비동기이며 `await` 키워드와 함께 항상 대기해야 합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. 그런 다음, 첫 번째 [`Button`](xref:Xamarin.Forms.Button)을 누릅니다.

    [![iOS 및 Android에서 경고 스크린샷](../images/alert.png "경고")](../images/alert-large.png#lightbox "경고")

    경고를 해제한 후에 두 번째 [`Button`](xref:Xamarin.Forms.Button)을 누릅니다.

    [![iOS 및 Android에서 질문을 묻는 경고 메시지의 스크린샷](../images/alert-question.png "질문을 묻는 경고")](../images/alert-question-large.png#lightbox "질문을 묻는 경고")

    질문에 대한 응답을 선택한 후에 응답이 Mac용 Visual Studio **애플리케이션 출력** 창에 출력되는지 살펴봅니다.

    경고를 표시하는 방법에 대한 자세한 내용은 [팝업 표시](~/xamarin-forms/user-interface/pop-ups.md) 가이드에서 [경고 표시](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert)를 참조하세요.
