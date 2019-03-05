# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Button`](xref:Xamarin.Forms.Button) 선언을 수정하여 해당 시각적 개체 모양을 변경합니다.

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    이 코드는 [`Button`](xref:Xamarin.Forms.Button)의 시각적 개체 모양을 변경하는 속성을 설정합니다. [`TextColor`](xref:Xamarin.Forms.Button.TextColor) 속성은 `Button` 텍스트 색을 설정하고, [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 속성은 텍스트의 배경색을 설정합니다. [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 속성은 `Button` 주위의 영역 색을 설정하고, [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 속성은 테두리의 너비를 설정합니다. 기본적으로 `Button`은 직사각형이지만 [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 속성을 적절한 값으로 설정하여 둥근 모서리를 만들 수 있습니다. 또한 해당하는 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 설정하여 `Button`의 크기를 변경합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Button`](xref:Xamarin.Forms.Button) 모양이 변경되었음을 확인합니다.

    [![iOS 및 Android에서 시각적 개체 모양이 변경된 단추의 스크린샷](../images/change-button-appearance.png "변경된 모양의 단추")](../images/change-button-appearance-large.png#lightbox "변경된 모양의 단추")

    [`Button`](xref:Xamarin.Forms.Button) 모양 설정에 대한 자세한 내용은 [Xamarin.Forms 단추](~/xamarin-forms/user-interface/button.md) 가이드에서 [단추 모양](~/xamarin-forms/user-interface/button.md#button-appearance)을 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml**에서 [`Button`](xref:Xamarin.Forms.Button) 선언을 수정하여 해당 시각적 개체 모양을 변경합니다.

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    이 코드는 [`Button`](xref:Xamarin.Forms.Button)의 시각적 개체 모양을 변경하는 속성을 설정합니다. [`TextColor`](xref:Xamarin.Forms.Button.TextColor) 속성은 `Button` 텍스트 색을 설정하고, [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 속성은 텍스트의 배경색을 설정합니다. [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 속성은 `Button` 주위의 영역 색을 설정하고, [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 속성은 테두리의 너비를 설정합니다. 기본적으로 `Button`은 직사각형이지만 [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 속성을 적절한 값으로 설정하여 둥근 모서리를 만들 수 있습니다. 또한 해당하는 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 설정하여 `Button`의 크기를 변경합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Button`](xref:Xamarin.Forms.Button) 모양이 변경되었음을 확인합니다.

    [![iOS 및 Android에서 시각적 개체 모양이 변경된 단추의 스크린샷](../images/change-button-appearance.png "변경된 모양의 단추")](../images/change-button-appearance-large.png#lightbox "변경된 모양의 단추")

    [`Button`](xref:Xamarin.Forms.Button) 모양 설정에 대한 자세한 내용은 [Xamarin.Forms 단추](~/xamarin-forms/user-interface/button.md) 가이드에서 [단추 모양](~/xamarin-forms/user-interface/button.md#button-appearance)을 참조하세요.
