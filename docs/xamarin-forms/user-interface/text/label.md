---
title: 자마린.양식 레이블
description: 이 문서에서는 Xamarin.Forms 레이블 클래스를 사용하여 응용 프로그램에서 단일 및 다중 줄 텍스트를 표시하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: bed6b8a774ecb352427f16b78d10e50821f92701
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987597"
---
# <a name="xamarinforms-label"></a>자마린.양식 레이블

[![샘플](~/media/shared/download.png) 다운로드 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_자마린.양식에 텍스트 표시_

뷰는 [`Label`](xref:Xamarin.Forms.Label) 단일 및 다중 줄 모두 텍스트를 표시하는 데 사용됩니다. 레이블에는 텍스트 장식, 색상 텍스트가 있고 사용자 지정 글꼴(패밀리, 크기 및 옵션)을 사용할 수 있습니다.

## <a name="text-decorations"></a>텍스트 장식

밑줄 및 스트라이크스루 텍스트 장식은 속성을 하나 이상의 [`Label`](xref:Xamarin.Forms.Label) `TextDecorations` 열거 멤버로 설정하여 인스턴스에 적용할 수 있습니다. `Label.TextDecorations`

- `None`
- `Underline`
- `Strikethrough`

다음 XAML 예제에서는 `Label.TextDecorations` 속성 설정을 보여 줍니다.

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

다음 스크린샷은 `TextDecorations` 인스턴스에 적용된 열거형 멤버를 [`Label`](xref:Xamarin.Forms.Label) 보여 준다.

![텍스트 장식이 있는 레이블](label-images/label-textdecorations.png)

> [!NOTE]
> 텍스트 장식을 인스턴스에도 [`Span`](xref:Xamarin.Forms.Span) 적용할 수 있습니다. 클래스에 `Span` 대한 자세한 내용은 [서식이 지정된 텍스트](#Formatted_Text)를 참조하십시오.

## <a name="character-spacing"></a>문자 간격

속성값을 값으로 설정하여 [`Label`](xref:Xamarin.Forms.Label) `Label.CharacterSpacing` 인스턴스에 문자 간격을 `double` 적용할 수 있습니다.

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

그 결과 에 의해 표시되는 텍스트의 [`Label`](xref:Xamarin.Forms.Label) 문자는 장치 독립적 단위의 간격이 분리됩니다. `CharacterSpacing`

## <a name="new-lines"></a>새로운 줄

XAML에서 새 [`Label`](xref:Xamarin.Forms.Label) 줄에 텍스트를 강제로 적용하는 두 가지 주요 기술이 있습니다.

1. "&#10;"인 유니코드 줄 피드 문자를 사용합니다.
1. *속성 요소* 구문을 사용하여 텍스트를 지정합니다.

다음 코드는 두 기술의 예를 보여 주며 다음과 같은 두 가지 기술을 보여 주며 다음과 같은 예제를 보여 주며 다음과 같은 두 가지 기술을 보여 주며

```xaml
<!-- Unicode line feed character -->
<Label Text="First line &#10; Second line" />

<!-- Property element syntax -->
<Label>
    <Label.Text>
        First line
        Second line
    </Label.Text>
</Label>
```

C#에서 텍스트는 "\n" 문자가 있는 새 줄에 강제로 정렬될 수 있습니다.

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>색

레이블은 바인딩 가능한 속성을 통해 사용자 지정 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 텍스트 색상을 사용하도록 설정할 수 있습니다.

각 플랫폼에서 색상을 사용할 수 있도록 특별한주의가 필요합니다. 각 플랫폼마다 텍스트 및 배경 색에 대한 기본값이 다르므로 각 플랫폼에서 작동하는 기본값을 선택해야 합니다.

다음 XAML 예제는 `Label`다음의 텍스트 색상을 설정합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a green label." />
    </StackLayout>
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a green label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

다음 스크린샷은 속성을 설정한 `TextColor` 결과를 보여 준다.

![레이블 텍스트색상 예제](label-images/textcolor.png)

색상에 대한 자세한 내용은 [색상](~/xamarin-forms/user-interface/colors.md)을 참조하십시오.

## <a name="fonts"></a>글꼴

에서 글꼴 `Label`을 지정하는 방법에 대한 자세한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조하십시오.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>잘림 및 포장

속성에서 노출되는 여러 가지 방법 중 하나로 한 줄에 맞지 않는 `LineBreakMode` 텍스트를 처리하도록 레이블을 설정할 수 있습니다. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)은 다음 값을 가진 열거형입니다.

- **헤드잘렌션은** &ndash; 텍스트의 머리를 잘리고 끝을 표시합니다.
- **문자 줄 바꿈문자** &ndash; 경계에서 새 줄로 텍스트를 줄 바꿈합니다.
- **중간 Truncation은** &ndash; 텍스트의 시작과 끝을 표시하고 가운데는 타원으로 바꿉을 바꿉습니다.
- **NoWrap은** &ndash; 한 줄에 들어갈 수 있는 만큼의 텍스트만 표시하여 텍스트를 줄 바꿈하지 않습니다.
- **TailTruncation은** &ndash; 텍스트의 시작 부분을 표시하여 끝을 잘립니다.
- **WordWrap은** &ndash; 단어 경계에서 텍스트를 줄 바꿈합니다.

## <a name="display-a-specific-number-of-lines"></a>특정 줄 수 표시

에 의해 표시되는 줄 의 수는 `int` 속성을 값으로 설정하여 지정할 수 있습니다. [`Label`](xref:Xamarin.Forms.Label) `Label.MaxLines`

- 기본값인 `MaxLines` `Label` -1이면 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성 값이 한 줄만 표시되거나 잘리거나 모든 텍스트가 있는 모든 줄이 표시됩니다.
- 0이면 `MaxLines` `Label` 표시되지 않습니다.
- `MaxLines` 1이면 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 결과는 속성을 " [`NoWrap`](xref:Xamarin.Forms.LineBreakMode) [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode) [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)또는 [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)에 설정하는 것과 동일합니다. 그러나, `Label` 해당되는 경우, [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 타원의 배치에 관한 속성의 가치를 존중합니다.
- 1보다 `MaxLines` `Label` 크면 해당되는 경우 타원 배치와 관련하여 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성 값을 유지하면서 지정된 줄 수까지 표시됩니다. 그러나 `MaxLines` 속성을 1보다 큰 값으로 설정하면 속성이 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)로 설정되어 있으면 효과가 없습니다.

다음 XAML 예제에서는 `MaxLines` [`Label`](xref:Xamarin.Forms.Label)다음에 속성을 설정하는 것을 보여 줍니다.

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

다음 스크린샷은 텍스트가 `MaxLines` 2개 이상의 줄을 차지할 만큼 충분히 긴 경우 속성을 2로 설정한 결과를 보여 준다.

![최대선 예제에 레이블 지정](label-images/label-maxlines.png)

## <a name="display-html"></a>HTML 표시

[`Label`](xref:Xamarin.Forms.Label) 클래스에는 `TextType` `Label` 인스턴스에 일반 텍스트를 표시할지 또는 HTML 텍스트를 표시할지 여부를 결정하는 속성이 있습니다. 이 속성은 열거형의 `TextType` 멤버 중 하나로 설정해야 합니다.

- `Text`은 `Label` 일반 텍스트를 표시하고 `Label.TextType` 속성의 기본값임을 나타냅니다.
- `Html`은 `Label` HTML 텍스트를 표시합니다.

따라서 [`Label`](xref:Xamarin.Forms.Label) 인스턴스는 `Label.TextType` 속성을 로 설정하고 `Html` `Label.Text` 속성을 HTML 문자열로 설정하여 HTML을 표시할 수 있습니다.

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

위의 예에서 HTML의 큰따옴표 문자는 기호를 `\` 사용하여 이스케이프되어야 합니다.

XAML에서 HTML 문자열은 `<` 다음 및 `>` 기호를 추가로 이스케이프하기 때문에 읽을 수 없게 될 수 있습니다.

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

또는 가독성을 높이기 위해 `CDATA` 섹션에 HTML을 인줄 부착할 수 있습니다.

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

이 예제에서는 `Label.Text` 속성이 섹션에 인라인된 HTML 문자열로 `CDATA` 설정됩니다. 속성이 클래스에 `Text` `ContentProperty` 대 한 `Label` 때문에 작동 합니다.

다음 스크린샷에는 [`Label`](xref:Xamarin.Forms.Label) 표시된 HTML이 표시됩니다.

![iOS 및 Android에서 HTML을 표시하는 레이블의 스크린샷](label-images/label-html.png)

> [!IMPORTANT]
> 에 [`Label`](xref:Xamarin.Forms.Label) HTML을 표시하는 것은 기본 플랫폼에서 지원하는 HTML 태그로 제한됩니다.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>서식 있는 텍스트

레이블은 동일한 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 뷰에서 여러 글꼴과 색상으로 텍스트를 표시할 수 있는 속성을 노출합니다.

속성은 `FormattedText` 하나 [`FormattedString`](xref:Xamarin.Forms.FormattedString)이상의 [`Span`](xref:Xamarin.Forms.Span) 인스턴스로 구성된 형식이며 [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) 속성을 통해 설정됩니다. 다음 `Span` 속성을 사용하여 시각적 모양을 설정할 수 있습니다.

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)– 범위 배경의 색상.
- `double` 형식의 `CharacterSpacing`은 `Span` 텍스트를 구성하는 문자 사이의 간격입니다.
- [`Font`](xref:Xamarin.Forms.Span.Font)– 범위의 텍스트에 대한 글꼴입니다.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)– 범위의 텍스트에 대한 글꼴 속성입니다.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)– 범위의 텍스트에 대한 글꼴이 속하는 글꼴 패밀리입니다.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)– 범위의 텍스트에 대한 글꼴의 크기입니다.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)– 범위의 텍스트에 대한 색상입니다. 이 속성은 더 이상 사용되지 않으며 `TextColor` 속성으로 대체되었습니다.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)- 스팬의 기본 선 높이에 적용할 승수입니다. 자세한 내용은 [선 높이](#line-height)를 참조하십시오.
- [`Style`](xref:Xamarin.Forms.Span.Style)– 스팬에 적용할 스타일입니다.
- [`Text`](xref:Xamarin.Forms.Span.Text)– 범위의 텍스트입니다.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)– 범위의 텍스트에 대한 색상입니다.
- `TextDecorations`- 장식은 범위의 텍스트에 적용할 수 있습니다. 자세한 내용은 [텍스트 장식 을](#text-decorations)참조하십시오.

[`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)에 [`Text`](xref:Xamarin.Forms.Span.Text)대해 [`Text`](xref:Xamarin.Forms.Span.Text) 및 바인딩할 수 있는 [`OneWay`](xref:Xamarin.Forms.BindingMode)속성의 기본 바인딩 모드가 있습니다. 이 바인딩 모드에 대한 자세한 내용은 바인딩 [모드](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) 의 [기본 바인딩 모드를](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) 참조하십시오.

또한 속성은 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) [`Span`](xref:Xamarin.Forms.Span)에 제스처에 응답하는 제스처 인식기의 컬렉션을 정의하는 데 사용할 수 있습니다.

> [!NOTE]
> [`Span`](xref:Xamarin.Forms.Span)에 HTML을 표시할 수 없습니다.

다음 XAML 예제에서는 `FormattedText` 세 [`Span`](xref:Xamarin.Forms.Span) 가지 인스턴스로 구성된 속성을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label LineBreakMode="WordWrap">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}">
                        <Span.GestureRecognizers>
                            <TapGestureRecognizer Command="{Binding TapCommand}" />
                        </Span.GestureRecognizers>
                    </Span>
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });

        var span = new Span { Text = "default, " };
        span.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(async () => await DisplayAlert("Tapped", "This is a tapped Span.", "OK")) });
        formattedString.Spans.Add(span);
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!IMPORTANT]
> a의 [`Text`](xref:Xamarin.Forms.Span.Text) `Span` 속성은 데이터 바인딩을 통해 설정할 수 있습니다. 자세한 내용은 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

또한 는 [`Span`](xref:Xamarin.Forms.Span) 범위의 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) 컬렉션에 추가된 모든 제스처에 응답할 수도 있습니다. 예를 들어 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 위의 코드 예제에서 `Span` 두 번째에 a가 추가되었습니다. 따라서 이 `Span` 탭시 `TapGestureRecognizer` `ICommand` [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) 속성에 의해 정의된 을 실행하여 응답합니다. 제스처 인식기에 대한 자세한 내용은 [Xamarin.Forms 제스처 를](~/xamarin-forms/app-fundamentals/gestures/index.md)참조하십시오.

다음 스크린샷은 속성을 세 `FormattedString` `Span` 가지 인스턴스로 설정한 결과를 보여 준다.

![레이블 서식텍스트 예제](label-images/formattedtext.png)

## <a name="line-height"></a>선 높이

[`Label`](xref:Xamarin.Forms.Label) a와 [`Span`](xref:Xamarin.Forms.Span) a의 세로 높이는 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 속성을 설정하거나 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) `double` 값을 지정하여 사용자 지정할 수 있습니다. iOS 및 Android에서 이러한 값은 원래 줄 높이의 승수이며 유니버설 Windows 플랫폼(UWP)에서는 `Label.LineHeight` 속성 값이 레이블 글꼴 크기의 승수입니다.

> [!NOTE]
>
> - iOS에서 및 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 속성은 한 줄에 맞는 텍스트의 줄 높이와 여러 줄로 줄 바꿈되는 텍스트를 변경합니다.
> - [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) Android및 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 속성은 여러 줄로 줄 바꿈되는 텍스트의 줄 높이만 변경합니다.
> - UWP에서 속성은 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 여러 줄로 줄 바꿈되는 텍스트의 줄 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 높이를 변경하고 속성은 영향을 주지 않습니다.

다음 XAML 예제에서는 [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) [`Label`](xref:Xamarin.Forms.Label)다음에 속성을 설정하는 것을 보여 줍니다.

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

다음 스크린샷은 속성을 1.8로 설정한 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 결과를 보여 준다.

![레이블 선높이 예제](label-images/label-lineheight.png)

다음 XAML 예제에서는 [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) [`Span`](xref:Xamarin.Forms.Span)다음에 속성을 설정하는 것을 보여 줍니다.

```xaml
<Label LineBreakMode="WordWrap">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. "
                  LineHeight="1.8"/>
            <Span Text="Nullam feugiat sodales elit, et maximus nibh vulputate id."
                  LineHeight="1.8" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. ",
  LineHeight = 1.8
});
formattedString.Spans.Add(new Span
{
  Text = "Nullam feugiat sodales elit, et maximus nibh vulputate id.",
  LineHeight = 1.8
});
var label = new Label
{
  FormattedText = formattedString,
  LineBreakMode = LineBreakMode.WordWrap
};
```

다음 스크린샷은 속성을 1.8로 설정한 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 결과를 보여 준다.

![범위 선높이 예제](label-images/span-lineheight.png)

## <a name="padding"></a>안쪽 여백

패딩은 요소와 자식 요소 사이의 공간을 나타내며 요소를 자체 콘텐츠와 구분하는 데 사용됩니다. 속성을 값으로 설정하여 [`Label`](xref:Xamarin.Forms.Label) `Label.Padding` 인스턴스에 패딩을 [`Thickness`](xref:Xamarin.Forms.Thickness) 적용할 수 있습니다.

```xaml
<Label Padding="10">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum" />
            <Span Text="dolor sit amet." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
FormattedString formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum"
});
formattedString.Spans.Add(new Span
{
  Text = "dolor sit amet."
});
Label label = new Label
{
    FormattedText = formattedString,
    Padding = new Thickness(20)
};
```

> [!IMPORTANT]
> iOS에서 `Padding` 속성을 [`Label`](xref:Xamarin.Forms.Label) 설정하는 a를 만들면 패딩이 적용되고 나중에 패딩 값을 업데이트할 수 있습니다. 그러나 `Padding` 속성을 `Label` 설정 하지 않는 a를 만들 때 나중에 설정 하려고 하면 영향을 주지 않습니다.
>
> Android 및 유니버설 Windows 플랫폼에서 `Padding` 속성 값을 만들 `Label` 거나 나중에 지정할 수 있습니다.

패딩에 대한 자세한 내용은 [여백 및 패딩을](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)참조하십시오.

## <a name="hyperlinks"></a>하이퍼링크

표시 [`Label`](xref:Xamarin.Forms.Label) 된 텍스트와 [`Span`](xref:Xamarin.Forms.Span) 인스턴스는 다음 방법을 사용하여 하이퍼링크로 변환 할 수 있습니다.

1. `TextColor` [`Span`](xref:Xamarin.Forms.Span)또는 의 및 속성을 설정합니다. `TextDecoration` [`Label`](xref:Xamarin.Forms.Label)
1. [`Label`](xref:Xamarin.Forms.Label) 의 [`Span`](xref:Xamarin.Forms.Span) [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 컬렉션에 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) a를 추가합니다. [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) `ICommand` [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)
1. [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)에서 실행할 `ICommand` 을 정의합니다.
1. `ICommand`에서 실행할 코드를 작성합니다.

하이퍼링크 데모 샘플에서 가져온 다음 코드 예제는 [`Label`](xref:Xamarin.Forms.Label) 여러 [`Span`](xref:Xamarin.Forms.Span) 인스턴스에서 콘텐츠가 설정된 를 보여 [주며,](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Alternatively, click " />
            <Span Text="here"
                  TextColor="Blue"
                  TextDecorations="Underline">
                <Span.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding TapCommand}"
                                          CommandParameter="https://docs.microsoft.com/xamarin/" />
                </Span.GestureRecognizers>
            </Span>
            <Span Text=" to view Xamarin documentation." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

이 예제에서 첫 번째 [`Span`](xref:Xamarin.Forms.Span) 및 세 번째 `Span` 인스턴스는 텍스트를 구성하고 두 번째 인스턴스는 탭 가능한 하이퍼링크를 나타냅니다. 텍스트 색상이 파란색으로 설정되어 있으며 밑줄 텍스트 장식이 있습니다. 이렇게 하면 다음 스크린샷과 같이 하이퍼링크모양이 만들어집니다.

[![하이퍼링크](label-images/hyperlinks-small.png "하이퍼링크")](label-images/hyperlinks-large.png#lightbox)

하이퍼링크를 탭하면 해당 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) `ICommand` [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) 속성에 정의된 을 실행하여 응답합니다. 또한 [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성에서 지정한 URL은 매개 `ICommand` 변수로 전달됩니다.

XAML 페이지의 코드 숨결 에는 `TapCommand` 구현이 포함되어 있습니다.

```csharp
public partial class MainPage : ContentPage
{
    // Launcher.OpenAsync is provided by Xamarin.Essentials.
    public ICommand TapCommand => new Command<string>(async (url) => await Launcher.OpenAsync(url));

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }
}
```

는 `TapCommand` [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 메서드를 `Launcher.OpenAsync` 실행하여 속성 값을 매개 변수로 전달합니다. 이 `Launcher.OpenAsync` 메서드는 Xamarin.Essentials에서 제공되며 웹 브라우저에서 URL을 엽니다. 따라서 전체적인 효과는 하이퍼링크를 페이지에 탭하면 웹 브라우저가 나타나고 하이퍼링크와 연결된 URL이 탐색된다는 것입니다.

### <a name="creating-a-reusable-hyperlink-class"></a>재사용 가능한 하이퍼링크 클래스 만들기

하이퍼링크를 만드는 이전 접근 방식은 응용 프로그램에서 하이퍼링크가 필요할 때마다 반복적인 코드를 작성해야 합니다. 그러나 제스처 [`Label`](xref:Xamarin.Forms.Label) 인식기와 `HyperlinkSpan` 텍스트 서식 `HyperlinkLabel` 지정 코드가 추가된 클래스와 [`Span`](xref:Xamarin.Forms.Span) 클래스를 모두 하위 클래스로 만들고 클래스를 만들 수 있습니다.

[하이퍼링크 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) 샘플에서 가져온 다음 코드 예제는 `HyperlinkSpan` 클래스를 보여 주며 다음과 같은 클래스를 보여 주며 다음과 같은 클래스를 보여 주며 다음과 같은 클래스를 보여 주며 다음과 같은 클래스를 보여 주며 다음과 같은 클래스를 보여 주며 다음과 같은 클래스를 보여 주

```csharp
public class HyperlinkSpan : Span
{
    public static readonly BindableProperty UrlProperty =
        BindableProperty.Create(nameof(Url), typeof(string), typeof(HyperlinkSpan), null);

    public string Url
    {
        get { return (string)GetValue(UrlProperty); }
        set { SetValue(UrlProperty, value); }
    }

    public HyperlinkSpan()
    {
        TextDecorations = TextDecorations.Underline;
        TextColor = Color.Blue;
        GestureRecognizers.Add(new TapGestureRecognizer
        {
            // Launcher.OpenAsync is provided by Xamarin.Essentials.
            Command = new Command(async () => await Launcher.OpenAsync(Url))
        });
    }
}
```

클래스는 `HyperlinkSpan` `Url` 속성 및 연결된 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)을 정의하고 생성자는 하이퍼링크 모양을 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 설정하고 하이퍼링크를 탭할 때 응답할 것을 설정합니다. 를 `HyperlinkSpan` 탭하면 웹 `TapGestureRecognizer` 브라우저에서 `Launcher.OpenAsync` `Url` 속성에서 지정한 URL을 여는 메서드를 실행하여 응답합니다.

클래스의 `HyperlinkSpan` 인스턴스를 XAML에 추가하여 클래스를 사용할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:HyperlinkDemo"
             x:Class="HyperlinkDemo.MainPage">
    <StackLayout>
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Alternatively, click " />
                    <local:HyperlinkSpan Text="here"
                                         Url="https://docs.microsoft.com/appcenter/" />
                    <Span Text=" to view AppCenter documentation." />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

## <a name="styling-labels"></a>레이블 스타일 지정

이전 섹션에서는 [`Label`](xref:Xamarin.Forms.Label) 인스턴스별로 설정 및 [`Span`](xref:Xamarin.Forms.Span) 속성을 다루었습니다. 그러나 속성 집합은 하나 또는 여러 뷰에 일관되게 적용되는 하나의 스타일로 그룹화할 수 있습니다. 이렇게 하면 코드의 가독성이 높아지고 디자인 변경을 보다 쉽게 구현할 수 있습니다. 자세한 내용은 [스타일](~/xamarin-forms/user-interface/text/styles.md)을 참조하십시오.

## <a name="related-links"></a>관련 링크

- [텍스트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [하이퍼링크(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Xamarin.Forms를 사용한 모바일 앱 만들기, 3장](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [레이블 API](xref:Xamarin.Forms.Label)
- [범위 API](xref:Xamarin.Forms.Span)
