---
ms.openlocfilehash: bd3f37082443e93f10f60d9466fe22aae8571b14
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "61373440"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`Editor`](xref:Xamarin.Forms.Editor) 선언을 수정하여 해당 동작을 사용자 지정합니다.

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    이 코드는 [`Editor`](xref:Xamarin.Forms.Editor)의 동작을 사용자 지정하는 속성을 설정합니다. [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) 속성은 [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)로 설정됩니다. 즉, `Editor`가 텍스트로 채워지면 높이가 증가하고 텍스트가 제거되면 높이가 감소한다는 것을 나타냅니다. [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성은 `Editor`에 허용되는 입력 길이를 제한합니다. 또한 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성이 `false`로 설정되어 맞춤법 검사를 사용하지 않도록 설정하는 동시에, `IsTextPredictionEnabled` 속성도 `false`로 설정되어 텍스트 예측 및 자동 텍스트 예측을 사용하지 않도록 설정합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Editor`](xref:Xamarin.Forms.Entry)에 텍스트를 입력하고, `Editor`가 텍스트로 채워지면 높이가 증가하고 텍스트가 제거되면 높이가 감소하며 입력할 수 있는 문자의 최대 수는 200개임을 살펴봅니다.

    [![iOS 및 Android에서 자동 크기 조정 편집기의 스크린샷](../images/customize-behavior.png "자동 크기 조정 편집기")](../images/customize-behavior-large.png#lightbox "자동 크기 조정 편집기")

    [`Editor`](xref:Xamarin.Forms.Editor) 동작을 사용자 지정하는 방법에 대한 자세한 내용은 [Xamarin.Forms 편집기](~/xamarin-forms/user-interface/text/editor.md) 가이드를 참조하세요.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`Editor`](xref:Xamarin.Forms.Editor) 선언을 수정하여 해당 동작을 사용자 지정합니다.

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    이 코드는 [`Editor`](xref:Xamarin.Forms.Editor)의 동작을 사용자 지정하는 속성을 설정합니다. [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) 속성은 [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)로 설정됩니다. 즉, `Editor`가 텍스트로 채워지면 높이가 증가하고 텍스트가 제거되면 높이가 감소한다는 것을 나타냅니다. [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성은 `Editor`에 허용되는 입력 길이를 제한합니다. 또한 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성이 `false`로 설정되어 맞춤법 검사를 사용하지 않도록 설정하는 동시에, `IsTextPredictionEnabled` 속성도 `false`로 설정되어 텍스트 예측 및 자동 텍스트 예측을 사용하지 않도록 설정합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Editor`](xref:Xamarin.Forms.Entry)에 텍스트를 입력하고, `Editor`가 텍스트로 채워지면 높이가 증가하고 텍스트가 제거되면 높이가 감소하며 입력할 수 있는 문자의 최대 수는 200개임을 살펴봅니다.

    [![iOS 및 Android에서 자동 크기 조정 편집기의 스크린샷](../images/customize-behavior.png "자동 크기 조정 편집기")](../images/customize-behavior-large.png#lightbox "자동 크기 조정 편집기")

    [`Editor`](xref:Xamarin.Forms.Editor) 동작을 사용자 지정하는 방법에 대한 자세한 내용은 [Xamarin.Forms 편집기](~/xamarin-forms/user-interface/text/editor.md) 가이드를 참조하세요.
