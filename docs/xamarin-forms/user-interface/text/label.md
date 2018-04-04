---
title: 레이블
description: Xamarin.Forms에 텍스트를 표시 합니다.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: c1056626b336dd9b6ce265ab693ceed2a24eae0f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="label"></a>레이블

_Xamarin.Forms에 텍스트를 표시 합니다._

`Label` 보기는 단일 및 여러 줄 텍스트를 표시 하기 위해 사용 됩니다. 레이블 (패밀리, 크기 및 옵션) 사용자 지정 글꼴 및 색이 지정 된 텍스트를 가질 수 있습니다. 이 문서에서는 다음 항목을 다룹니다.

- **[잘림 및 줄 바꿈](#Truncation_and_Wrapping)**  &ndash; 잘림 및 줄 바꿈 텍스트 한 줄에 표시할 수 없는 위치는 상황을 처리 하기 위한 옵션입니다.
- **[글꼴](#Font)**  &ndash; 글꼴 옵션입니다.
- **[색](#Color)**  &ndash; 레이블 텍스트 색 옵션입니다.
- **[서식 있는 텍스트](#Formatted_Text)**  &ndash; 여러 형식/스타일 인라인 된 텍스트를 표시 하기 위한 옵션입니다.

## <a name="styling-label"></a>레이블 스타일 지정

다음 섹션에서는 설명의 속성을 설정 `Label` 수동으로 인스턴스 당 기준입니다. 하나 이상의 보기에 일관 되 게 적용 되는 하나의 스타일 속성의 집합 그룹화 할 수 있습니다. 코드의 가독성을 늘리고 디자인 변경 내용을 더 쉽게 구현할 수 있습니다이 있습니다. 참조 [스타일](~/xamarin-forms/user-interface/text/styles.md) 자세한 정보에 대 한 합니다.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>잘림 및 줄 바꿈

에 의해 노출 되는 여러 가지 방법 중 하나에서 한 줄에 표시할 수 없는 텍스트 처리 레이블을 설정할 수 있습니다는 `LineBreakMode` 속성입니다. [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) 다음 옵션 중 열거형이입니다.

- **HeadTruncation** &ndash; 끝 표시 텍스트의 머리를 자릅니다.
- **CharacterWrap** &ndash; 문자 경계에서 새 행에 텍스트를 배치 합니다.
- **MiddleTruncation** &ndash; 줄임표로 중간 바꿀 텍스트의 시작과 끝을 표시 합니다.
- **NoWrap** &ndash; 만 표시 텍스트를 래핑하지 않습니다 한 줄에 들어가지 많은 텍스트를 수 있습니다.
- **TailTruncation** &ndash; 끝 잘라내기 텍스트의 시작 부분을 보여 줍니다.
- **자동 줄 바꿈** &ndash; 단어 경계에서 텍스트를 배치 합니다.

## <a name="font"></a>글꼴

참조 [글꼴 사용](~/xamarin-forms/user-interface/text/fonts.md) 자세한 정보에 대 한 합니다.

## <a name="color"></a>색

`Label`s는 바인딩 가능한를 통해 사용자 지정 텍스트 색을 사용 하도록 설정할 수 있습니다 `TextColor` 속성입니다.

특별 한 주의 되 색 각 플랫폼에서 사용할 수 있는지 확인 해야 합니다. 각 플랫폼에 대 한 텍스트 색과 배경색은 다른 기본값을 사용 하므로 주의 각 작동 하는 기본값을 선택 해야 합니다.

다음 코드를 사용 하 여 레이블의 텍스트 색을 설정 하려면:

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

Xaml의 경우:

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

![](label-images/textcolor.png "레이블 TextColor 예제")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>서식 있는 텍스트

레이블 노출 한 `FormattedText` 여러 글꼴에 텍스트를 제공할 수 있으며 동일한 보기에 색을 지정 하는 속성입니다.

`FormattedText` 형식의 속성은 [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/)합니다. 하나 이상의으로 구성 된 서식이 지정 된 문자열은 `Span`다음 속성이 설정 된 각 중 하나:

- **BackgroundColor** &ndash; 는 highlighter 효과 달성 하기 위해 예제에 대 한 배경색을 설정 하는 데 사용할 수 있습니다.
- **FontAttributes** &ndash; 집합을 굵게, 기울임꼴 또는 둘 다를 수 있습니다.
- **FontFamily** &ndash; 글꼴 사용을 설정 합니다.
- **FontSize** &ndash; 텍스트의 크기를 설정 합니다.
- **ForegroundColor** &ndash; 텍스트의 색을 설정 합니다.
- **텍스트** &ndash; 텍스트를 표시 해야 합니다.

다음 C# 코드에는 여기서 첫 번째 단어는 굵게 표시 하 고 마지막 단어는 빨간색 레이블을 보여 줍니다.

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

Xaml의 경우:

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

![](label-images/formattedtext.png "레이블 FormattedText 예제")


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms에 Chapter 3을 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [레이블 API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
