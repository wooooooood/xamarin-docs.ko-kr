---
ms.openlocfilehash: 841dac9486097e27923ccfe582803b4ec50371cf
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "60896724"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서, 단일 `Label`에서 여러 형식을 사용하는 텍스트를 제공하도록 [`Label`](xref:Xamarin.Forms.Label) 선언을 수정합니다.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    이 코드는 단일 [`Label`](xref:Xamarin.Forms.Label)에서 여러 형식을 사용하는 텍스트를 표시합니다. 첫 번째 [`Span`](xref:Xamarin.Forms.Span)의 텍스트는 `Label`의 서식 지정 세트를 사용하여 표시되는 반면, 두 번째 및 세 번째 `Span` 인스턴스의 텍스트는 `Label`의 서식 지정 세트, 각 `Span`의 추가 서식 지정 세트를 사용하여 표시됩니다.

    > [!NOTE]
    > [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 속성은 [`FormattedString`](xref:Xamarin.Forms.FormattedString) 유형이며 하나 이상의 [`Span`](xref:Xamarin.Forms.Span) 인스턴스로 구성됩니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Label`](xref:Xamarin.Forms.Label) 모양이 변경되었음을 확인합니다.

    [![iOS 및 Android에서 서식 있는 텍스트를 표시하는 레이블의 스크린샷](../images/label-formatted-text.png "서식 있는 텍스트를 포함하는 레이블")](../images/label-formatted-text-large.png#lightbox "서식 있는 텍스트를 포함하는 레이블")

    [`Span`](xref:Xamarin.Forms.Span) 모양 설정에 대한 자세한 내용은 [Xamarin.Forms 레이블](~/xamarin-forms/user-interface/text/label.md) 가이드의 [서식 있는 텍스트](~/xamarin-forms/user-interface/text/label.md#formatted-text)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서, 단일 `Label`에서 여러 형식을 사용하는 텍스트를 제공하도록 [`Label`](xref:Xamarin.Forms.Label) 선언을 수정합니다.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    이 코드는 단일 [`Label`](xref:Xamarin.Forms.Label)에서 여러 형식을 사용하는 텍스트를 표시합니다. 첫 번째 [`Span`](xref:Xamarin.Forms.Span)의 텍스트는 `Label`의 서식 지정 세트를 사용하여 표시되는 반면, 두 번째 및 세 번째 `Span` 인스턴스의 텍스트는 `Label`의 서식 지정 세트, 각 `Span`의 추가 서식 지정 세트를 사용하여 표시됩니다.

    > [!NOTE]
    > [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 속성은 [`FormattedString`](xref:Xamarin.Forms.FormattedString) 유형이며 하나 이상의 [`Span`](xref:Xamarin.Forms.Span) 인스턴스로 구성됩니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Label`](xref:Xamarin.Forms.Label) 모양이 변경되었음을 확인합니다.

    [![iOS 및 Android에서 서식 있는 텍스트를 표시하는 레이블의 스크린샷](../images/label-formatted-text.png "서식 있는 텍스트를 포함하는 레이블")](../images/label-formatted-text-large.png#lightbox "서식 있는 텍스트를 포함하는 레이블")

    [`Span`](xref:Xamarin.Forms.Span) 모양 설정에 대한 자세한 내용은 [Xamarin.Forms 레이블](~/xamarin-forms/user-interface/text/label.md) 가이드의 [서식 있는 텍스트](~/xamarin-forms/user-interface/text/label.md#formatted-text)를 참조하세요.
