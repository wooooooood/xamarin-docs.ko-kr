---
title: Xamarin.Forms 레이블
description: 이 문서에서는 Xamarin.Forms 레이블 클래스를 사용 하 여 응용 프로그램에서 단일 및 여러 줄 텍스트를 표시 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: 19cc24c3937921f4f4d95584e1cd656c37b4155f
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105972"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms 레이블

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin.Forms에서 텍스트를 표시 합니다._

합니다 [ `Label` ](xref:Xamarin.Forms.Label) 보기 단일 및 여러 줄 텍스트를 표시 하기 위해 사용 됩니다. 레이블 (패밀리, 크기 및 옵션) 사용자 지정 글꼴을 사용 및 텍스트 장식을 색이 지정 된 텍스트를 구축할 수 있습니다.

## <a name="text-decorations"></a>텍스트 장식

밑줄 및 취소선 텍스트 장식을 적용할 수 있습니다 [ `Label` ](xref:Xamarin.Forms.Label) 설정 하 여 인스턴스를 `Label.TextDecorations` 속성을 하나 이상의 `TextDecorations` 열거형 멤버:

- `None`
- `Underline`
- `Strikethrough`

다음 XAML 예제에서는 설정 된 `Label.TextDecorations` 속성:

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

다음 스크린샷에서 표시 된 `TextDecorations` 열거형 멤버에 적용 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스:

![텍스트 장식이 있는 레이블](label-images/label-textdecorations.png)

> [!NOTE]
> 텍스트 장식을 적용할 수도 있습니다 [ `Span` ](xref:Xamarin.Forms.Span) 인스턴스. 에 대 한 자세한 내용은 합니다 `Span` 클래스를 참조 하십시오 [서식 있는 텍스트](#Formatted_Text)합니다.

## <a name="colors"></a>색

레이블은 바인딩 가능한를 통해 사용자 지정 텍스트 색을 사용 하도록 설정할 수 있습니다 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) 속성입니다.

특별 한 주의 색 각 플랫폼에서 사용할 수 있는지 확인 하는 데 필요한 경우 각 플랫폼에 텍스트 및 배경색에 대 한 다른 기본값 때문에 각에서 작동 하는 기본값을 선택 하도록 주의 해야 합니다.

텍스트 색을 설정 하는 다음 XAML 예제는 `Label`:

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

설정의 결과 표시 하는 다음 스크린샷에서 `TextColor` 속성:

![레이블 TextColor 예](label-images/textcolor.png)

색에 대 한 자세한 내용은 참조 하세요. [색](~/xamarin-forms/user-interface/colors.md)합니다.

## <a name="fonts"></a>글꼴

글꼴을 지정 하는 방법에 대 한 자세한 내용은 `Label`를 참조 하세요 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)합니다.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>잘라내기 및 줄 바꿈

레이블을 설정할 수 있는 의해 노출 되는 여러 가지 방법 중 하나에서 한 줄에 맞지 않는 텍스트를 처리 합니다 `LineBreakMode` 속성입니다. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 이 다음 값으로 열거형:

- **HeadTruncation** &ndash; 끝 표시 텍스트의 머리를 자릅니다.
- **CharacterWrap** &ndash; 문자 경계에서 새 줄에 텍스트를 배치 합니다.
- **MiddleTruncation** &ndash; 줄임표로 중간 replace 사용 하 여 텍스트의 시작과 끝을 표시 합니다.
- **NoWrap** &ndash; 만 표시할 텍스트를 래핑하지 않으며 수 만큼 텍스트 한 줄에 적합 합니다.
- **TailTruncation** &ndash; 끝 잘라내기 텍스트의 시작 부분을 보여줍니다.
- **WordWrap** &ndash; 단어 경계에서 텍스트를 배치 합니다.

## <a name="displaying-a-specific-number-of-lines"></a>특정 개수의 줄을 표시합니다.

표시 되는 줄 번호를 [ `Label` ](xref:Xamarin.Forms.Label) 설정 하 여 지정할 수 있습니다 합니다 `Label.MaxLines` 속성을는 `int` 값:

- 때 `MaxLines` 0를 `Label` 의 값을 준수 합니다 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) 하거나 잘릴 수 있습니다, 하나의 줄을 표시 하는 속성 또는 모든 텍스트를 사용 하 여 모든 줄.
- 때 `MaxLines` 가 1 이면 결과 동일 설정에는 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) 속성을 [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode)를 [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode), 또는 [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode)합니다. 그러나 합니다 `Label` 의 값을 적용 합니다는 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) 줄임표에 해당 하는 경우의 배치와 관련 하 여 속성입니다.
- 때 `MaxLines` 1 보다 크면 합니다 `Label` 최대 선을 지정 된 수의 값을 유지 하는 동안 표시 됩니다는 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) 줄임표에 해당 하는 경우의 배치와 관련 하 여 속성. 그러나 설정를 `MaxLines` 1 영향을 주지 않습니다 경우는 보다 큰 값으로 속성을 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) 속성이로 설정 되어 [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode).

다음 XAML 예제에서는 설정 된 `MaxLines` 속성을 [ `Label` ](xref:Xamarin.Forms.Label):

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

설정의 결과 표시 하는 다음 스크린샷에서 `MaxLines` 를 2, 텍스트는 2 개 이상의 줄을 차지 하기에 충분히 길지 속성:

![레이블 MaxLines 예](label-images/label-maxlines.png)

<a name="Formatted_Text" />

## <a name="formatted-text"></a>서식 있는 텍스트

레이블은 동일한 보기 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 에서 여러 글꼴 및 색을 사용 하 여 텍스트를 표시할 수 있는 속성을 노출 합니다.

`FormattedText` 형식의 속성은 [ `FormattedString` ](xref:Xamarin.Forms.FormattedString)로 구성 된 하나 이상의 [ `Span` ](xref:Xamarin.Forms.Span) 인스턴스를 통해 설정 된 [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) 속성 . 다음 `Span` 모양을 설정 하려면 속성을 사용할 수 있습니다.

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) -s p a n 배경의 색입니다.
- [`Font`](xref:Xamarin.Forms.Span.Font) – 범위에 있는 텍스트의 글꼴입니다.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – 범위에 있는 텍스트에 대 한 글꼴 특성입니다.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – 범위에 있는 텍스트의 글꼴을 속해 있는 글꼴 패밀리입니다.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – 범위에 있는 텍스트의 글꼴 크기입니다.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – 범위에 있는 텍스트의 색입니다. 이 속성은 사용 되지 않습니다 및 바뀌었습니다는 `TextColor` 속성입니다.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -범위의 기본 줄 높이에 적용할 승수입니다. 자세한 내용은 [줄 높이](#line-height)합니다.
- [`Style`](xref:Xamarin.Forms.Span.Style)-범위에 적용 되는 스타일입니다.
- [`Text`](xref:Xamarin.Forms.Span.Text) – 텍스트 범위입니다.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – 범위에 있는 텍스트의 색입니다.
- `TextDecorations` -범위에 있는 텍스트에 적용할 장식 합니다. 자세한 내용은 [텍스트 장식을](#text-decorations)합니다.

> [!NOTE]
> , 및 바인딩 가능한 속성 [의`OneWay`](xref:Xamarin.Forms.BindingMode)기본 바인딩 모드는입니다. [`Text`](xref:Xamarin.Forms.Span.Text) [`Text`](xref:Xamarin.Forms.Span.Text) [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) 이 바인딩 모드에 대 한 자세한 내용은 [바인딩 모드](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) 가이드에서 [기본 바인딩 모드](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) 를 참조 하세요.

또한 합니다 [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) 응답할 제스처에서 제스처 인식기의 컬렉션을 정의 하려면 속성을 사용할 수 있습니다 합니다 [ `Span` ](xref:Xamarin.Forms.Span)합니다.

다음 XAML 예제는 `FormattedText` 세 가지 구성 된 속성 [ `Span` ](xref:Xamarin.Forms.Span) 인스턴스:

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
> 합니다 [ `Text` ](xref:Xamarin.Forms.Span.Text) 의 속성을 `Span` 데이터 바인딩을 통해 설정할 수 있습니다. 자세한 내용은 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.

한 [ `Span` ](xref:Xamarin.Forms.Span) 는 범위에 추가 되는 모든 제스처에 응답할 수도 [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) 컬렉션입니다. 예를 들어, 한 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 두 번째 추가 되었습니다 `Span` 위의 코드 예제에서 합니다. 따라서,이 `Span` 를 탭 할 합니다 `TapGestureRecognizer` 실행 하 여 응답 합니다를 `ICommand` 정의한를 [ `Command` ](xref:Xamarin.Forms.TapGestureRecognizer.Command) 속성. 제스처 인식기에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 제스처](~/xamarin-forms/app-fundamentals/gestures/index.md)합니다.

다음 스크린샷은 설정의 결과 표시 합니다 `FormattedString` 속성을 3 `Span` 인스턴스:

![레이블 FormattedText 예제](label-images/formattedtext.png)

## <a name="line-height"></a>줄 높이

세로 높이 [ `Label` ](xref:Xamarin.Forms.Label) 와 [ `Span` ](xref:Xamarin.Forms.Span) 설정 하 여 사용자 지정할 수 있습니다 합니다 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) 속성 또는 [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) 에 `double` 값입니다. IOS 및 Android에서 이러한 값은 원래 줄 높이 및 유니버설 Windows 플랫폼 (UWP)에서 승수는 `Label.LineHeight` 속성 값이 레이블 글꼴 크기의 승수입니다.

> [!NOTE]
>
> - Ios의 경우는 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) 하 고 [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) 단일 줄에 적합 한 텍스트 및 여러 줄으로 줄 바꿈됩니다는 텍스트 줄 높이 변경 하는 속성입니다.
> - Android에서 합니다 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) 하 고 [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) 속성만 여러 줄으로 줄 바꿈됩니다 텍스트 줄 높이 변경 합니다.
> - UWP에는 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) 여러 줄으로 줄 바꿈됩니다 텍스트 줄 높이 변경 하는 속성 및 [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) 속성이 적용 되지 않습니다.

다음 XAML 예제에서는 설정 된 [ `LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) 속성을 [ `Label` ](xref:Xamarin.Forms.Label):

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

다음 스크린샷은 설정의 결과 표시 합니다 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) 1.8 속성:

![레이블 LineHeight 예](label-images/label-lineheight.png)

다음 XAML 예제에서는 설정 된 [ `LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) 속성을 [ `Span` ](xref:Xamarin.Forms.Span):

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

다음 스크린샷은 설정의 결과 표시 합니다 [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) 1.8 속성:

![Span LineHeight 예](label-images/span-lineheight.png)

## <a name="hyperlinks"></a>하이퍼링크

[`Label`](xref:Xamarin.Forms.Label) [및`Span`](xref:Xamarin.Forms.Span) 인스턴스에 의해 표시 되는 텍스트는 다음과 같은 방법으로 하이퍼링크로 전환 될 수 있습니다.

1. `TextColor` `TextDecoration` 또는 [의및`Span`속성](xref:Xamarin.Forms.Span)을 설정 합니다. [`Label`](xref:Xamarin.Forms.Label)
1. [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) 속성이에 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) [바인딩되고해당`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성에 열려는 URL이 포함 되어 있는 또는의 컬렉션에를 추가 합니다. `ICommand`
1. `ICommand` [에`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)의해 실행 될를 정의 합니다.
1. 에 `ICommand`의해 실행 되는 코드를 작성 합니다.

[Hyperlink 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) 샘플에서 가져온 다음 코드 예제는 여러 [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) 인스턴스에서 해당 콘텐츠가 설정 된를 보여 줍니다.

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

이 예에서 첫 번째와 세 [`Span`](xref:Xamarin.Forms.Span) 번째 인스턴스는 텍스트를 구성 하는 반면 두 번째 `Span` 인스턴스는 tappable 하이퍼링크를 나타냅니다. 텍스트 색이 blue로 설정 되어 있으며 밑줄 텍스트 장식이 있습니다. 그러면 다음 스크린샷에 표시 된 것 처럼 하이퍼링크 모양이 생성 됩니다.

[![하이퍼링크](label-images/hyperlinks-small.png "하이퍼링크")](label-images/hyperlinks-large.png#lightbox)

하이퍼링크를 탭 하면는 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) 속성으로 정의 된을 `ICommand` 실행 하 여 응답 합니다. 또한 [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성에 지정 된 URL은 매개 변수로에 전달 `ICommand` 됩니다.

XAML 페이지의 코드 숨김으로는 `TapCommand` 구현이 포함 되어 있습니다.

```csharp
public partial class MainPage : ContentPage
{
    public ICommand TapCommand => new Command<string>(OpenBrowser);

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }

    void OpenBrowser(string url)
    {
        Device.OpenUri(new Uri(url));
    }
}
```

는 `TapCommand` `OpenBrowser` 메서드를 실행 하 여 [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성 값을 매개 변수로 전달 합니다. 그러면이 메서드는 [`Device.OpenUri`](xref:Xamarin.Forms.Device.OpenUri*) 메서드를 호출 하 여 웹 브라우저에서 URL을 엽니다. 따라서 전체적인 효과는 하이퍼링크가 페이지에서 탭 되 면 웹 브라우저가 나타나고 하이퍼링크와 연결 된 URL을 탐색 하는 것입니다.

### <a name="creating-a-reusable-hyperlink-class"></a>재사용 가능한 hyperlink 클래스 만들기

하이퍼링크를 만드는 이전 방법에는 응용 프로그램에서 하이퍼링크를 요구할 때마다 반복적인 코드를 작성 해야 합니다. 그러나 및 클래스를 [`Label`](xref:Xamarin.Forms.Label) 모두 [`Span`](xref:Xamarin.Forms.Span) 서브클래싱 하 여 및 `HyperlinkSpan` 클래스를 `HyperlinkLabel` 만들 수 있습니다. 여기에는 제스처 인식기와 텍스트 서식 지정 코드가 추가 됩니다.

[Hyperlink 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) 샘플에서 가져온 다음 코드 예제는 클래스를 `HyperlinkSpan` 보여 줍니다.

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
            Command = new Command(() => Device.OpenUri(new Uri(Url)))
        });
    }
}
```

클래스 `HyperlinkSpan` `Url` 는 속성 및 연결 된 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)를 정의 하 고, 생성자는 하이퍼링크 모양과 하이퍼링크를 누를 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 때 응답 하는를 설정 합니다. 를 `HyperlinkSpan` 탭 하면에서 메서드를 `TapGestureRecognizer` [`Device.OpenUri`](xref:Xamarin.Forms.Device.OpenUri*) 실행 `Url` 하 여 속성으로 지정 된 URL을 웹 브라우저에서 여는 방식으로 응답 합니다.

클래스의 인스턴스를 XAML에 추가 하 여 클래스를사용할수있습니다.`HyperlinkSpan`

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

이전 섹션에 설정 적용 [ `Label` ](xref:Xamarin.Forms.Label) 하 고 [ `Span` ](xref:Xamarin.Forms.Span) 인스턴스별 기준 속성입니다. 그러나 속성 집합이 하나 이상의 보기에 일관 되 게 적용 되는 하나의 스타일 그룹화 할 수 있습니다. 코드의 가독성을 증가 하며 쉽게 디자인 변경 내용을 구현 합니다. 자세한 내용은 [스타일](~/xamarin-forms/user-interface/text/styles.md)합니다.

## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [하이퍼링크 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [3 장 Xamarin.Forms를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [레이블 API](xref:Xamarin.Forms.Label)
- [API 범위](xref:Xamarin.Forms.Span)
