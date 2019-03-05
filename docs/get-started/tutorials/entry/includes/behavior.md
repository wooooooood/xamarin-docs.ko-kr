# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Entry`](xref:Xamarin.Forms.Entry) 선언을 수정하여 해당 동작을 사용자 지정합니다.

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    이 코드는 [`Entry`](xref:Xamarin.Forms.Entry)의 동작을 사용자 지정하는 속성을 설정합니다. [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성은 `Entry`에 허용되는 입력 길이를 제한하고, [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성은 `false`로 설정되어 맞춤법 검사를 사용하지 않도록 설정합니다. 마찬가지로, [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성은 `false`로 설정되어 텍스트 예측 및 자동 텍스트 예측을 사용하지 않도록 설정합니다. 또한 [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) 속성은 입력된 문자가 암호 문자(검은색 원)로 표시되어 있는지 확인합니다.

    > [!NOTE]
    > 일부 텍스트 항목 시나리오(예: 암호 입력)의 경우 맞춤법 검사 및 텍스트 예측은 부정적인 환경을 제공하므로 사용되지 않도록 설정되어야 합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Entry`](xref:Xamarin.Forms.Entry)에 텍스트를 입력한 다음, 각 문자를 암호 마스크 문자로 바꾸고 입력할 수 있는 최대 문자 수가 15개인지를 확인합니다.

    [![iOS 및 Android에서 암호 문자로 표시된 텍스트를 포함하는 항목의 스크린샷](../images/customize-behavior.png "표시된 암호 문자를 포함하는 항목")](../images/customize-behavior-large.png#lightbox "표시된 암호 문자를 포함하는 항목")

    [`Entry`](xref:Xamarin.Forms.Entry) 동작을 사용자 지정하는 방법에 대한 자세한 내용은 [Xamarin.Forms 항목](~/xamarin-forms/user-interface/text/entry.md) 가이드를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml**에서 [`Entry`](xref:Xamarin.Forms.Entry) 선언을 수정하여 해당 동작을 사용자 지정합니다.

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    이 코드는 [`Entry`](xref:Xamarin.Forms.Entry)의 동작을 사용자 지정하는 속성을 설정합니다. [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성은 `Entry`에 허용되는 입력 길이를 제한하고, [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성은 `false`로 설정되어 맞춤법 검사를 사용하지 않도록 설정합니다. 마찬가지로, [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성은 `false`로 설정되어 텍스트 예측 및 자동 텍스트 예측을 사용하지 않도록 설정합니다. 또한 [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) 속성은 입력된 문자가 암호 문자(검은색 원)로 표시되어 있는지 확인합니다.

    > [!NOTE]
    > 일부 텍스트 항목 시나리오(예: 암호 입력)의 경우 맞춤법 검사 및 텍스트 예측은 부정적인 환경을 제공하므로 사용되지 않도록 설정되어야 합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Entry`](xref:Xamarin.Forms.Entry)에 텍스트를 입력한 다음, 각 문자를 암호 마스크 문자로 바꾸고 입력할 수 있는 최대 문자 수가 15개인지를 확인합니다.

    [![iOS 및 Android에서 암호 문자로 표시된 텍스트를 포함하는 항목의 스크린샷](../images/customize-behavior.png "표시된 암호 문자를 포함하는 항목")](../images/customize-behavior-large.png#lightbox "표시된 암호 문자를 포함하는 항목")

    [`Entry`](xref:Xamarin.Forms.Entry) 동작을 사용자 지정하는 방법에 대한 자세한 내용은 [Xamarin.Forms 항목](~/xamarin-forms/user-interface/text/entry.md) 가이드를 참조하세요.
