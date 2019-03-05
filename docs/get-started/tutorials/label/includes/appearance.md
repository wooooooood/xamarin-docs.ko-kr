# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Label`](xref:Xamarin.Forms.Label) 선언을 수정하여 해당 시각적 개체 모양을 변경합니다.

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    이 코드는 [`Label`](xref:Xamarin.Forms.Label)의 시각적 개체 모양을 변경하는 속성을 설정합니다. [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 속성은 `Button` 텍스트의 색상을 설정합니다. [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) 속성은 레이블의 글꼴을 기울임꼴로 설정하고 [`FontSize`](xref:Xamarin.Forms.Label.FontSize) 속성은 글꼴 크기를 설정합니다. 또한 해당 [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) 속성을 설정하여 밑줄 텍스트 장식을 `Label`에 적용하고 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성을 [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)로 설정하여 가로 가운데에 배치합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Label`](xref:Xamarin.Forms.Label) 모양이 변경되었음을 확인합니다.

    [![iOS 및 Android에서 시각적 개체 모양이 변경된 레이블의 스크린샷](../images/change-label-appearance.png "변경된 모양으로 레이블 지정")](../images/change-label-appearance-large.png#lightbox "변경된 모양으로 레이블 지정")

    [`Label`](xref:Xamarin.Forms.Label) 모양 설정에 대한 자세한 내용은 [Xamarin.Forms 레이블](~/xamarin-forms/user-interface/text/label.md) 가이드를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml**에서 [`Label`](xref:Xamarin.Forms.Label) 선언을 수정하여 해당 시각적 개체 모양을 변경합니다.

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    이 코드는 [`Label`](xref:Xamarin.Forms.Label)의 시각적 개체 모양을 변경하는 속성을 설정합니다. [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 속성은 `Button` 텍스트의 색상을 설정합니다. [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) 속성은 레이블의 글꼴을 기울임꼴로 설정하고 [`FontSize`](xref:Xamarin.Forms.Label.FontSize) 속성은 글꼴 크기를 설정합니다. 또한 해당 [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) 속성을 설정하여 밑줄 텍스트 장식을 `Label`에 적용하고 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성을 [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)로 설정하여 가로 가운데에 배치합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Label`](xref:Xamarin.Forms.Label) 모양이 변경되었음을 확인합니다.

    [![iOS 및 Android에서 시각적 개체 모양이 변경된 레이블의 스크린샷](../images/change-label-appearance.png "변경된 모양으로 레이블 지정")](../images/change-label-appearance-large.png#lightbox "변경된 모양으로 레이블 지정")

    [`Label`](xref:Xamarin.Forms.Label) 모양 설정에 대한 자세한 내용은 [Xamarin.Forms 레이블](~/xamarin-forms/user-interface/text/label.md) 가이드를 참조하세요.
