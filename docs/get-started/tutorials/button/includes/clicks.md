# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Button`](xref:Xamarin.Forms.Button) 선언을 수정하여 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    이 코드는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 다음 단계에서 만들 `OnButtonClicked`라는 이벤트 처리기로 설정합니다.

1. **솔루션 탐색기**의 **ButtonTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnButtonClicked` 처리기를 클래스에 추가합니다.

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 탭하면 `OnButtonClicked` 메서드가 실행됩니다. `sender` 인수는 `Clicked` 이벤트의 실행을 담당하는 `Button` 개체이며 `Button` 개체에 액세스하는 데 사용될 수 있습니다. 이 이벤트 처리기는 `Button`에 표시된 텍스트를 업데이트합니다.

    > [!NOTE]
    > `Clicked` 이벤트 외에도 `Button`은 [`Pressed`](xref:Xamarin.Forms.Button.Pressed) 및 [`Released`](xref:Xamarin.Forms.Button.Released) 이벤트도 정의합니다. 자세한 내용은 [Xamarin.Forms 버튼](~/xamarin-forms/user-interface/button.md) 가이드의 [버튼 눌렀다 놓기](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button)를 참조하세요.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Button`](xref:Xamarin.Forms.Button)을 클릭하고 표시되는 텍스트가 변경되었음을 확인합니다.

    [![iOS 및 Android에서 클릭이 수신된 후 변경되는 버튼 텍스트 스크린샷](../images/handle-button-click.png "단추 클릭 처리")](../images/handle-button-click-large.png#lightbox "단추 클릭 처리")

    단추 클릭 처리에 대한 자세한 내용은 [Xamarin.Forms 버튼](~/xamarin-forms/user-interface/button.md) 가이드의 [단추 클릭 처리](~/xamarin-forms/user-interface/button.md#handling-button-clicks)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`Button`](xref:Xamarin.Forms.Button) 선언을 수정하여 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    이 코드는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 다음 단계에서 만들 `OnButtonClicked`라는 이벤트 처리기로 설정합니다.

1. **Solution Pad**의 **ButtonTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnButtonClicked` 처리기를 클래스에 추가합니다.

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 탭하면 `OnButtonClicked` 메서드가 실행됩니다. `sender` 인수는 `Clicked` 이벤트의 실행을 담당하는 `Button` 개체이며 `Button` 개체에 액세스하는 데 사용될 수 있습니다. 이 이벤트 처리기는 `Button`에 표시된 텍스트를 업데이트합니다.

    > [!NOTE]
    > `Clicked` 이벤트 외에도 `Button`은 [`Pressed`](xref:Xamarin.Forms.Button.Pressed) 및 [`Released`](xref:Xamarin.Forms.Button.Released) 이벤트도 정의합니다. 자세한 내용은 [Xamarin.Forms 버튼](~/xamarin-forms/user-interface/button.md) 가이드의 [버튼 눌렀다 놓기](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button)를 참조하세요.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Button`](xref:Xamarin.Forms.Button)을 클릭하고 표시되는 텍스트가 변경되었음을 확인합니다.

    [![iOS 및 Android에서 클릭이 수신된 후 변경되는 버튼 텍스트 스크린샷](../images/handle-button-click.png "단추 클릭 처리")](../images/handle-button-click-large.png#lightbox "단추 클릭 처리")

    단추 클릭 처리에 대한 자세한 내용은 [Xamarin.Forms 버튼](~/xamarin-forms/user-interface/button.md) 가이드의 [단추 클릭 처리](~/xamarin-forms/user-interface/button.md#handling-button-clicks)를 참조하세요.
