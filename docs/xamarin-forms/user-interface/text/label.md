---
title: Xamarin.Forms 레이블
description: 이 문서에서는 Xamarin.Forms 레이블 클래스를 사용 하 여 응용 프로그램에서 단일 및 여러 줄 텍스트를 표시 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/28/2019
ms.openlocfilehash: 64fa15a15468a84ada3a377a9ac85bbf6310099c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305590"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms 레이블

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin.ios에 텍스트 표시_

[`Label`](xref:Xamarin.Forms.Label) 뷰는 단일 및 여러 줄 텍스트를 표시 하는 데 사용 됩니다. 레이블 (패밀리, 크기 및 옵션) 사용자 지정 글꼴을 사용 및 텍스트 장식을 색이 지정 된 텍스트를 구축할 수 있습니다.

## <a name="text-decorations"></a>텍스트 장식

`Label.TextDecorations` 속성을 하나 이상의 `TextDecorations` 열거형 멤버로 설정 하 여 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에 밑줄 및 취소선 텍스트 장식을 적용할 수 있습니다.

- `None`
- `Underline`
- `Strikethrough`

다음 XAML 예제에서는 `Label.TextDecorations` 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

해당 하는 C# 코드가입니다.

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

다음 스크린샷에는 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에 적용 된 `TextDecorations` 열거형 멤버가 나와 있습니다.

![텍스트 장식이 있는 레이블](label-images/label-textdecorations.png)

> [!NOTE]
> 텍스트 장식을 [`Span`](xref:Xamarin.Forms.Span) 인스턴스에 적용할 수도 있습니다. `Span` 클래스에 대 한 자세한 내용은 서식 있는 [텍스트](#Formatted_Text)를 참조 하세요.

## <a name="character-spacing"></a>문자 간격

문자 간격은 `Label.CharacterSpacing` 속성을 `double` 값으로 설정 하 여 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에 적용할 수 있습니다.

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

해당 하는 C# 코드가입니다.

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

그 결과 [`Label`](xref:Xamarin.Forms.Label) 표시 되는 텍스트의 문자는 장치 독립적 단위 `CharacterSpacing` 간격이 떨어져 있습니다.

## <a name="colors"></a>색

바인딩 가능한 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 속성을 통해 사용자 지정 텍스트 색을 사용 하도록 레이블을 설정할 수 있습니다.

특별 한 주의 색 각 플랫폼에서 사용할 수 있는지 확인 하는 데 필요한 경우 각 플랫폼에 텍스트 및 배경색에 대 한 다른 기본값 때문에 각에서 작동 하는 기본값을 선택 하도록 주의 해야 합니다.

다음 XAML 예제에서는 `Label`텍스트 색을 설정 합니다.

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

해당 하는 C# 코드가입니다.

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

다음 스크린샷에는 `TextColor` 속성을 설정 하는 결과가 나와 있습니다.

![레이블 TextColor 예](label-images/textcolor.png)

색에 대 한 자세한 내용은 [색](~/xamarin-forms/user-interface/colors.md)을 참조 하세요.

## <a name="fonts"></a>글꼴

`Label`에서 글꼴을 지정 하는 방법에 대 한 자세한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하십시오.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>잘라내기 및 줄 바꿈

`LineBreakMode` 속성에 의해 노출 되는 여러 가지 방법 중 하나로 한 줄에 맞지 않는 텍스트를 처리 하도록 레이블을 설정할 수 있습니다. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 는 다음 값을 가진 열거형입니다.

- 헤드 **잘림** &ndash;는 텍스트의 헤드를 자르고 끝을 표시 합니다.
- **CharacterWrap** &ndash; 문자 경계의 새 줄로 텍스트를 래핑합니다.
- **MiddleTruncation** &ndash; 텍스트의 시작과 끝을 표시 하 고 중간은 줄임표로 바꿉니다.
- **NoWrap** &ndash;는 텍스트를 줄 바꿈하지 않습니다. 텍스트를 한 줄에 맞출 수 만큼만 표시 됩니다.
- **TailTruncation** &ndash;는 텍스트의 시작 부분을 표시 하 고 끝을 자릅니다.
- **WordWrap** &ndash; 단어 경계에서 텍스트를 래핑합니다.

## <a name="display-a-specific-number-of-lines"></a>특정 줄 수를 표시 합니다.

`Label.MaxLines` 속성을 `int` 값으로 설정 하 여 [`Label`](xref:Xamarin.Forms.Label) 표시 되는 줄 수를 지정할 수 있습니다.

- `MaxLines`가 기본값 인-1 인 경우 `Label`는 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성의 값을 사용 하 여 한 줄만 표시 하거나 잘린 상태로 표시 하거나 모든 텍스트를 포함 하는 모든 줄을 표시 합니다.
- `MaxLines` 0 이면 `Label` 표시 되지 않습니다.
- `MaxLines` 1 이면 결과는 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성을 [`NoWrap`](xref:Xamarin.Forms.LineBreakMode), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)또는 [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)로 설정 하는 것과 같습니다. 그러나 `Label`는 줄임표의 배치와 관련 하 여 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성의 값을 적용 합니다 (해당 하는 경우).
- `MaxLines`가 `Label` 1 보다 크면 지정 된 개수의 줄까지 표시 되 고 줄임표 (해당 하는 경우)의 배치와 관련 하 여 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성의 값을 사용 합니다. 그러나 `MaxLines` 속성을 1 보다 큰 값으로 설정 하면 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성이 [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)으로 설정 된 경우에는 영향을 주지 않습니다.

다음 XAML 예제에서는 [`Label`](xref:Xamarin.Forms.Label)의 `MaxLines` 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

해당 하는 C# 코드가입니다.

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

다음 스크린샷은 `MaxLines` 속성을 2로 설정 하는 경우의 결과를 보여 줍니다.

![레이블 MaxLines 예](label-images/label-maxlines.png)

## <a name="display-html"></a>HTML 표시

[`Label`](xref:Xamarin.Forms.Label) 클래스에는 `Label` 인스턴스가 일반 텍스트 또는 HTML 텍스트를 표시 해야 하는지 여부를 결정 하는 `TextType` 속성이 있습니다. 이 속성은 `TextType` 열거형의 멤버 중 하나로 설정 해야 합니다.

- `Text`은 `Label` 일반 텍스트를 표시 하 고 `Label.TextType` 속성의 기본값을 나타냅니다.
- `Html`은 `Label` HTML 텍스트를 표시 함을 나타냅니다.

따라서 [`Label`](xref:Xamarin.Forms.Label) 인스턴스는 `Label.TextType` 속성을 `Html`로 설정 하 고 `Label.Text` 속성을 html 문자열로 설정 하 여 html을 표시할 수 있습니다.

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

위의 예제에서 HTML의 큰따옴표 문자는 `\` 기호를 사용 하 여 이스케이프 해야 합니다.

XAML에서 `<` 및 `>` 기호를 추가로 이스케이프 하면 HTML 문자열을 읽을 수 없게 됩니다.

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

또는 가독성을 높이기 위해 `CDATA` 섹션에서 HTML을 인라인 처리할 수 있습니다.

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

이 예제에서 `Label.Text` 속성은 `CDATA` 섹션에서 인라인 된 HTML 문자열로 설정 됩니다. 이는 `Text` 속성이 `Label` 클래스의 `ContentProperty` 이기 때문에 작동 합니다.

다음 스크린샷에는 HTML을 표시 하는 [`Label`](xref:Xamarin.Forms.Label) 표시 됩니다.

![IOS 및 Android에서 HTML을 표시 하는 레이블의 스크린샷](label-images/label-html.png)

> [!IMPORTANT]
> [`Label`](xref:Xamarin.Forms.Label) 에 html을 표시 하는 것은 기본 플랫폼에서 지원 되는 html 태그로 제한 됩니다.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>서식 있는 텍스트

레이블은 동일한 보기에서 여러 글꼴 및 색을 사용 하 여 텍스트를 표시할 수 있는 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 속성을 노출 합니다.

`FormattedText` 속성은 [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) 속성을 통해 설정 된 하나 이상의 [`Span`](xref:Xamarin.Forms.Span) 인스턴스로 구성 되는 [`FormattedString`](xref:Xamarin.Forms.FormattedString)유형입니다. 시각적 효과를 설정 하는 데 사용할 수 있는 `Span` 속성은 다음과 같습니다.

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – span 배경의 색입니다.
- `CharacterSpacing` 형식의 `double`은 `Span` 텍스트를 구성하는 문자 사이의 간격입니다.
- [`Font`](xref:Xamarin.Forms.Span.Font) – 범위의 텍스트 글꼴입니다.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – 범위의 텍스트에 대 한 글꼴 특성입니다.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – 범위의 텍스트 글꼴이 속한 글꼴 패밀리입니다.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – 범위의 텍스트에 대 한 글꼴 크기입니다.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – 범위의 텍스트 색입니다. 이 속성은 사용 되지 않으며 `TextColor` 속성으로 대체 되었습니다.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -범위의 기본 선 높이에 적용 되는 승수입니다. 자세한 내용은 [줄 높이](#line-height)를 참조 하세요.
- [`Style`](xref:Xamarin.Forms.Span.Style) – 범위에 적용 되는 스타일입니다.
- [`Text`](xref:Xamarin.Forms.Span.Text) – 범위의 텍스트입니다.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – 범위의 텍스트 색입니다.
- `TextDecorations`-범위의 텍스트에 적용할 장식입니다. 자세한 내용은 [텍스트 장식](#text-decorations)을 참조 하세요.

[`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor), [`Text`](xref:Xamarin.Forms.Span.Text)및 [`Text`](xref:Xamarin.Forms.Span.Text) 바인딩 가능한 속성은 [`OneWay`](xref:Xamarin.Forms.BindingMode)의 기본 바인딩 모드입니다. 이 바인딩 모드에 대 한 자세한 내용은 [바인딩 모드](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) 가이드에서 [기본 바인딩 모드](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) 를 참조 하세요.

또한 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) 속성을 사용 하 여 [`Span`](xref:Xamarin.Forms.Span)제스처에 응답 하는 제스처 인식기의 컬렉션을 정의할 수 있습니다.

> [!NOTE]
> [`Span`](xref:Xamarin.Forms.Span)에 HTML을 표시할 수 없습니다.

다음 XAML 예제에서는 3 개의 [`Span`](xref:Xamarin.Forms.Span) 인스턴스로 구성 된 `FormattedText` 속성을 보여 줍니다.

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

해당 하는 C# 코드가입니다.

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
> `Span`의 [`Text`](xref:Xamarin.Forms.Span.Text) 속성은 데이터 바인딩을 통해 설정할 수 있습니다. 자세한 내용은 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

[`Span`](xref:Xamarin.Forms.Span) 는 범위의 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) 컬렉션에 추가 되는 제스처에도 응답할 수 있습니다. 예를 들어 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 는 위의 코드 예제에서 두 번째 `Span`에 추가 되었습니다. 따라서이 `Span` 탭 하면 `TapGestureRecognizer` [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) 속성으로 정의 된 `ICommand`을 실행 하 여 응답 합니다. 제스처 인식기에 대 한 자세한 내용은 [Xamarin.ios 제스처](~/xamarin-forms/app-fundamentals/gestures/index.md)를 참조 하세요.

다음 스크린샷은 `FormattedString` 속성을 3 개의 `Span` 인스턴스로 설정 하는 결과를 보여 줍니다.

![레이블 FormattedText 예제](label-images/formattedtext.png)

## <a name="line-height"></a>줄 높이

[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 속성 또는 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 를 `double` 값으로 설정 하 여 [`Label`](xref:Xamarin.Forms.Label) 및 [`Span`](xref:Xamarin.Forms.Span) 의 세로 높이를 사용자 지정할 수 있습니다. IOS 및 Android에서 이러한 값은 원래 선 높이의 승수이 고 유니버설 Windows 플랫폼 (UWP)에서 `Label.LineHeight` 속성 값은 레이블 글꼴 크기의 승수입니다.

> [!NOTE]
>
> - IOS에서 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 및 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 속성은 한 줄에 맞는 텍스트의 줄 높이와 여러 줄로 줄 바꿈되는 텍스트를 변경 합니다.
> - Android에서 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 및 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 속성은 여러 줄로 래핑하는 텍스트의 줄 높이만 변경 합니다.
> - UWP에서 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 속성은 여러 줄로 줄 바꿈되는 텍스트의 줄 높이를 변경 하 고 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 속성은 영향을 주지 않습니다.

다음 XAML 예제에서는 [`Label`](xref:Xamarin.Forms.Label)의 [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

해당 하는 C# 코드가입니다.

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

다음 스크린샷에는 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) 속성을 1.8로 설정 하는 결과가 나와 있습니다.

![레이블 LineHeight 예](label-images/label-lineheight.png)

다음 XAML 예제에서는 [`Span`](xref:Xamarin.Forms.Span)의 [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 속성을 설정 하는 방법을 보여 줍니다.

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

해당 하는 C# 코드가입니다.

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

다음 스크린샷에는 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 속성을 1.8로 설정 하는 결과가 나와 있습니다.

![Span LineHeight 예](label-images/span-lineheight.png)

## <a name="padding"></a>안쪽 여백

안쪽 여백은 요소와 자식 요소 사이의 공간을 나타내며 요소를 자체 콘텐츠에서 구분 하는 데 사용 됩니다. `Label.Padding` 속성을 [`Thickness`](xref:Xamarin.Forms.Thickness) 값으로 설정 하 여 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에 패딩을 적용할 수 있습니다.

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

해당 하는 C# 코드가입니다.

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
> IOS에서 `Padding` 속성을 설정 하는 [`Label`](xref:Xamarin.Forms.Label) 만들어질 때 패딩이 적용 되 고 패딩 값은 나중에 업데이트할 수 있습니다. 그러나 `Padding` 속성을 설정 하지 않는 `Label`를 만든 경우 나중에 설정 하려는 시도는 영향을 주지 않습니다.
>
> Android 및 유니버설 Windows 플랫폼에서 `Padding` 속성 값은 `Label`를 만들 때 또는 나중에 지정할 수 있습니다.

안쪽 여백에 대 한 자세한 내용은 [여백 및 안쪽](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)여백을 참조 하세요.

## <a name="hyperlinks"></a>하이퍼링크

[`Label`](xref:Xamarin.Forms.Label) 및 [`Span`](xref:Xamarin.Forms.Span) 인스턴스로 표시 되는 텍스트는 다음과 같은 방법으로 하이퍼링크로 전환 될 수 있습니다.

1. [`Label`](xref:Xamarin.Forms.Label) 또는 [`Span`](xref:Xamarin.Forms.Span)의 `TextColor` 및 `TextDecoration` 속성을 설정 합니다.
1. [`Command`속성이 `ICommand`](xref:Xamarin.Forms.TapGestureRecognizer.Command) 에 바인딩되고 [`CommandParameter`속성이 열](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) URL을 포함 하는 [`Label`](xref:Xamarin.Forms.Label) 또는 [`Span`](xref:Xamarin.Forms.Span)의 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) 컬렉션에 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 를 추가 합니다.
1. [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)에 의해 실행 되는 `ICommand`을 정의 합니다.
1. `ICommand`에서 실행 되는 코드를 작성 합니다.

[Hyperlink 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) 샘플에서 가져온 다음 코드 예제는 여러 [`Span`](xref:Xamarin.Forms.Span) 인스턴스에서 내용이 설정 된 [`Label`](xref:Xamarin.Forms.Label) 를 보여 줍니다.

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

이 예에서 첫 번째 및 세 번째 [`Span`](xref:Xamarin.Forms.Span) 인스턴스는 텍스트를 구성 하는 반면 두 번째 `Span`는 tappable 하이퍼링크를 나타냅니다. 텍스트 색이 blue로 설정 되어 있으며 밑줄 텍스트 장식이 있습니다. 그러면 다음 스크린샷에 표시 된 것 처럼 하이퍼링크 모양이 생성 됩니다.

[![링크](label-images/hyperlinks-small.png "하이퍼링크")](label-images/hyperlinks-large.png#lightbox)

하이퍼링크를 누르면 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) 속성으로 정의 된 `ICommand`를 실행 하 여 응답 합니다. 또한 [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성으로 지정 된 URL은 매개 변수로 `ICommand` 전달 됩니다.

XAML 페이지의 코드 숨김으로는 `TapCommand` 구현이 포함 되어 있습니다.

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

`TapCommand` `Launcher.OpenAsync` 메서드를 실행 하 여 [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성 값을 매개 변수로 전달 합니다. `Launcher.OpenAsync` 메서드는 Xamarin.ios에서 제공 되며 웹 브라우저에서 URL을 엽니다. 따라서 전체적인 효과는 하이퍼링크가 페이지에서 탭 되 면 웹 브라우저가 나타나고 하이퍼링크와 연결 된 URL을 탐색 하는 것입니다.

### <a name="creating-a-reusable-hyperlink-class"></a>재사용 가능한 hyperlink 클래스 만들기

하이퍼링크를 만드는 이전 방법에는 응용 프로그램에서 하이퍼링크를 요구할 때마다 반복적인 코드를 작성 해야 합니다. 그러나 [`Label`](xref:Xamarin.Forms.Label) 및 [`Span`](xref:Xamarin.Forms.Span) 클래스를 서브클래싱 하 여 `HyperlinkLabel` 및 `HyperlinkSpan` 클래스를 만들 수 있습니다. 여기에는 제스처 인식기와 텍스트 서식 지정 코드가 추가 됩니다.

[Hyperlink 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) 샘플에서 가져온 다음 코드 예제는 `HyperlinkSpan` 클래스를 보여 줍니다.

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

`HyperlinkSpan` 클래스는 `Url` 속성 및 연결 된 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)을 정의 하 고, 생성자는 하이퍼링크 모양이 나 하이퍼링크를 누를 때 응답 하는 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 를 설정 합니다. `HyperlinkSpan` 탭 하면 `TapGestureRecognizer` `Launcher.OpenAsync` 메서드를 실행 하 여 웹 브라우저에서 `Url` 속성으로 지정 된 URL을 엽니다.

클래스의 인스턴스를 XAML에 추가 하 여 `HyperlinkSpan` 클래스를 사용할 수 있습니다.

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

## <a name="styling-labels"></a>스타일 레이블

이전 섹션에서는 인스턴스별 [`Label`](xref:Xamarin.Forms.Label) 설정 및 [`Span`](xref:Xamarin.Forms.Span) 속성에 대해 설명 했습니다. 그러나 속성 집합이 하나 이상의 보기에 일관 되 게 적용 되는 하나의 스타일 그룹화 할 수 있습니다. 코드의 가독성을 증가 하며 쉽게 디자인 변경 내용을 구현 합니다. 자세한 내용은 [스타일](~/xamarin-forms/user-interface/text/styles.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [하이퍼링크 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Xamarin.ios를 사용 하 여 Mobile Apps 만들기, 3 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [레이블 API](xref:Xamarin.Forms.Label)
- [범위 API](xref:Xamarin.Forms.Span)
