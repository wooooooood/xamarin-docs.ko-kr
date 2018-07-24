---
title: Xamarin.Forms 레이블
description: 이 문서에서는 Xamarin.Forms 레이블 클래스를 사용 하 여 응용 프로그램에서 단일 및 여러 줄 텍스트를 표시 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: b5069381126db0859508480df5596ed5ec85686f
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203038"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms 레이블

_Xamarin.Forms에서 텍스트를 표시 합니다._

합니다 [ `Label` ](xref:Xamarin.Forms.Label) 보기 단일 및 여러 줄 텍스트를 표시 하기 위해 사용 됩니다. 레이블 (패밀리, 크기 및 옵션) 사용자 지정 글꼴 및 색이 지정 된 텍스트를 가질 수 있습니다.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>잘라내기 및 줄 바꿈

레이블을 설정할 수 있는 의해 노출 되는 여러 가지 방법 중 하나에서 한 줄에 맞지 않는 텍스트를 처리 합니다 `LineBreakMode` 속성입니다. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 이 다음 값으로 열거형:

- **HeadTruncation** &ndash; 끝 표시 텍스트의 머리를 자릅니다.
- **CharacterWrap** &ndash; 문자 경계에서 새 줄에 텍스트를 배치 합니다.
- **MiddleTruncation** &ndash; 줄임표로 중간 replace 사용 하 여 텍스트의 시작과 끝을 표시 합니다.
- **NoWrap** &ndash; 만 표시할 텍스트를 래핑하지 않으며 수 만큼 텍스트 한 줄에 적합 합니다.
- **TailTruncation** &ndash; 끝 잘라내기 텍스트의 시작 부분을 보여줍니다.
- **WordWrap** &ndash; 단어 경계에서 텍스트를 배치 합니다.

## <a name="fonts"></a>글꼴

글꼴을 지정 하는 방법에 대 한 자세한 내용은 `Label`를 참조 하세요 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)합니다.

## <a name="colors"></a>색

`Label`s는 바인딩 가능한를 통해 사용자 지정 텍스트 색을 사용 하도록 설정할 수 있습니다 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) 속성입니다.

특별 한 주의 색 각 플랫폼에서 사용할 수 있는지 확인 하는 데 필요한 경우 각 플랫폼에 텍스트 및 배경색에 대 한 다른 기본값 때문에 각에서 작동 하는 기본값을 선택 하도록 주의 해야 합니다.

레이블의 텍스트 색을 설정 하려면 다음 XAML을 사용 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
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
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

설정의 결과 표시 하는 다음 스크린샷에서 `TextColor` 속성:

![](label-images/textcolor.png "레이블 TextColor 예")

색에 대 한 자세한 내용은 참조 하세요. [색](~/xamarin-forms/user-interface/colors.md)합니다.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>서식 있는 텍스트

레이블 표시를 [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) 같은 보기에서 색 및 여러 글꼴을 사용 하 여 텍스트의 표현을 수 있는 속성입니다.

`FormattedText` 형식의 속성은 [ `FormattedString` ](xref:Xamarin.Forms.FormattedString)로 구성 된 하나 이상의 [ `Span` ](xref:Xamarin.Forms.Span) 인스턴스를 통해 설정 된 [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) 속성 . 각 `Span` 다음 속성을 소유 합니다.

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) -s p a n 배경의 색입니다.
- [`Font`](xref:Xamarin.Forms.Span.Font) – 범위에 있는 텍스트의 글꼴입니다.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – 범위에 있는 텍스트에 대 한 글꼴 특성입니다.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – 범위에 있는 텍스트의 글꼴을 속해 있는 글꼴 패밀리입니다.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – 범위에 있는 텍스트의 글꼴 크기입니다.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – 범위에 있는 텍스트의 색입니다. 이 속성은 사용 되지 않습니다 및 바뀌었습니다는 `TextColor` 속성입니다.
- [`Style`](xref:Xamarin.Forms.Span.Style) – 범위에 적용할 스타일입니다.
- [`Text`](xref:Xamarin.Forms.Span.Text) – 텍스트 범위입니다.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – 범위에 있는 텍스트의 색입니다.

다음 XAML 예제는 `FormattedText` 속성의 세 가지 구성 된 `Span` 인스턴스:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
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
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> 합니다 [ `Text` ](xref:Xamarin.Forms.Span.Text) 의 속성을 `Span` 데이터 바인딩을 통해 설정할 수 있습니다. 자세한 내용은 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.

다음 스크린샷은 설정의 결과 표시 합니다 `FormattedString` 속성을 3 `Span` 인스턴스:

![](label-images/formattedtext.png "레이블 FormattedText 예")

## <a name="styling-a-label"></a>레이블 스타일 지정

이전 섹션에 설정 적용 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스별 기준 속성입니다. 그러나 속성 집합이 하나 이상의 보기에 일관 되 게 적용 되는 하나의 스타일 그룹화 할 수 있습니다. 코드의 가독성을 증가 하며 쉽게 디자인 변경 내용을 구현 합니다. 자세한 내용은 [스타일](~/xamarin-forms/user-interface/text/styles.md)합니다.

## <a name="related-links"></a>관련 링크

- [3 장 Xamarin.Forms를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [레이블 API](xref:Xamarin.Forms.Label)
