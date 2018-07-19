---
title: Xamarin.Forms 레이블
description: 이 문서에서는 Xamarin.Forms 레이블 클래스를 사용 하 여 응용 프로그램에서 단일 및 여러 줄 텍스트를 표시 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: ce602a84ea1024dc22298a3ec1567a9a34ad4a82
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995968"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms 레이블

_Xamarin.Forms에서 텍스트를 표시 합니다._

`Label` 보기 단일 및 여러 줄 텍스트를 표시 하기 위해 사용 됩니다. 레이블 (패밀리, 크기 및 옵션) 사용자 지정 글꼴 및 색이 지정 된 텍스트를 가질 수 있습니다. 이 문서에서는 다음 항목을 다룹니다.

- **[잘라내기 및 줄 바꿈](#Truncation_and_Wrapping)**  &ndash; 잘림 및 줄 바꿈 텍스트 한 줄에 맞지 않는 상황을 처리 하기 위한 옵션입니다.
- **[글꼴](#Font)**  &ndash; 글꼴 옵션입니다.
- **[색](#Color)**  &ndash; 레이블 텍스트 색 옵션입니다.
- **[서식 있는 텍스트](#Formatted_Text)**  &ndash; 여러 형식/스타일 인라인을 사용 하 여 텍스트를 표시 하기 위한 옵션입니다.

## <a name="styling-label"></a>스타일 레이블

다음 섹션에서 설명 속성의 설정 `Label` 인스턴스 단위로에서 수동으로. 속성 집합에 하나 이상의 보기에 일관 되 게 적용 되는 하나의 스타일 그룹화 할 수 있습니다. 코드의 가독성을 증가 하며 쉽게 디자인 변경 내용을 구현 합니다. 참조 [스타일](~/xamarin-forms/user-interface/text/styles.md) 자세한 내용은 합니다.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>잘라내기 및 줄 바꿈

레이블을 설정할 수 있는 의해 노출 되는 여러 가지 방법 중 하나에서 한 줄에 맞지 않는 텍스트를 처리 합니다 `LineBreakMode` 속성입니다. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 다음 옵션 중 열거형이 같습니다.

- **HeadTruncation** &ndash; 끝 표시 텍스트의 머리를 자릅니다.
- **CharacterWrap** &ndash; 문자 경계에서 새 줄에 텍스트를 배치 합니다.
- **MiddleTruncation** &ndash; 줄임표로 중간 replace 사용 하 여 텍스트의 시작과 끝을 표시 합니다.
- **NoWrap** &ndash; 만 표시할 텍스트를 래핑하지 않으며 수 만큼 텍스트 한 줄에 적합 합니다.
- **TailTruncation** &ndash; 끝 잘라내기 텍스트의 시작 부분을 보여줍니다.
- **WordWrap** &ndash; 단어 경계에서 텍스트를 배치 합니다.

## <a name="font"></a>글꼴

참조 [글꼴을 사용 하 여 작업](~/xamarin-forms/user-interface/text/fonts.md) 자세한 내용은 합니다.

## <a name="color"></a>색

`Label`s는 바인딩 가능한를 통해 사용자 지정 텍스트 색을 사용 하도록 설정할 수 있습니다 `TextColor` 속성입니다.

특별 한 주의 색 각 플랫폼에서 사용할 수 있는지 확인 하는 데 필요한 경우 각 플랫폼에 텍스트 및 배경색에 대 한 다른 기본값 때문에 각에서 작동 하는 기본값을 선택 하도록 주의 해야 합니다.

레이블의 텍스트 색을 설정 하려면 다음 코드를 사용 합니다.

코드

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "레이블 TextColor 예")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>서식 있는 텍스트

레이블 표시를 `FormattedText` 같은 보기에서 색 및 여러 글꼴을 사용 하 여 텍스트를 제공할 수 있도록 하는 속성입니다.

합니다 `FormattedText` 형식의 속성은 [ `FormattedString` ](xref:Xamarin.Forms.FormattedString)합니다. 서식이 지정 된 문자열은 하나 이상의 구성 `Span`s, 각각 다음 속성을 사용 합니다.

- **BackgroundColor** &ndash; highlighter 효과 달성 하기 위해 예제에 대 한 배경색을 설정에 사용할 수 있습니다.
- **FontAttributes** &ndash; 집합 굵게, 기울임꼴 또는 둘 다를 수 있습니다.
- **FontFamily** &ndash; 사용할 글꼴을 설정 합니다.
- **FontSize** &ndash; 텍스트의 크기를 설정 합니다.
- **ForegroundColor** &ndash; 텍스트의 색을 설정 합니다.
- **텍스트** &ndash; 텍스트를 표시 합니다.

다음 C# 코드에는 여기서 굵게 표시 된 첫 번째 단어 하 고 마지막 단어는 빨간색 레이블을 보여 줍니다.

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "레이블 FormattedText 예")


## <a name="related-links"></a>관련 링크

- [3 장 Xamarin.Forms를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [레이블 API](xref:Xamarin.Forms.Label)
