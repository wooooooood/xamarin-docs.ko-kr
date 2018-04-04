---
title: 스타일
description: Xamarin.Forms에 텍스트 스타일
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: f38e4bc9ecd66c1dc33e53fa5c9046ff363802e6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="styles"></a>스타일

_Xamarin.Forms에 텍스트 스타일_


스타일은 레이블, 항목 및 편집기의 모양을 조정할 데 사용할 수 있습니다. 스타일을 두 번 정의 하 고 많은 보기에서 사용 하는 수는 있지만 한 종류의 뷰로 구성 된 스타일 에서만 사용할 수 있습니다.
스타일을 지정할 수는 `Key` 특정 컨트롤을 사용 하 여 선택적으로 적용 하 고 `Style` 속성입니다.

이 문서에서는 다음 항목을 다룹니다.

- **[기본 제공 스타일](#Built-In_Styles)**  &ndash; 응용 프로그램 전체에서 텍스트 기반 보기 스타일에 기본 제공 스타일을 사용 합니다.
- **[사용자 지정 스타일](#Custom_Styles)**  &ndash; 기본 제공 옵션 충분 하지 때 사용자 지정 스타일을 정의 합니다.
- **[스타일 적용](#Applying_Styles)**  &ndash; 보기에는 사용자 정의 함수와 기본 제공 스타일을 적용 합니다.
- **[내게 필요한 옵션](#Accessibility)**  &ndash; 텍스트 내게 필요한 옵션 설정을 고려 하는지 확인 합니다.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>기본 제공 스타일

Xamarin.Forms에 몇 [기본 제공](http://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) 일반적인 시나리오에 대 한 스타일:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

기본 제공 스타일 중 하나를 적용 하려면 사용 된 `DynamicResource` 태그 확장 스타일을 지정 하려면:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C#에서 기본 제공 스타일에서 선택 `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "장치 스타일 예제")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>사용자 지정 스타일

스타일 및 구성 됩니다 setter 및 setter를 속성으로 구성 속성 값으로 설정 됩니다.

C#에서는 30 크기의 빨간색 텍스트 레이블 스타일을 사용자를 다음과 같이 정의 합니다.

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Xaml의 경우:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

리소스 (모든 스타일 포함) 내에서 정의 되어 `ContentPage.Resources`의 더 친숙 한 형제 변수인 `ContentPage.Content` 요소입니다.

![](styles-images/customstyle.png "사용자 지정 스타일 예제")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>스타일 적용

일치 하는 모든 보기에 적용할 수는 스타일을 만든 후 해당 `TargetType`합니다.

XAML에서 사용자 지정 스타일에 적용 됩니다 뷰를 제공 하 여 자신의 `Style` 속성으로는 `StaticResource` 원하는 스타일을 참조 하는 태그 확장:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C# 스타일 중 하나 수는 뷰에 직접 적용 또는에 추가 메시지를 검색할 수는 페이지에서 `ResourceDictionary`합니다. 직접 추가 합니다.

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

추가 하 고 페이지의 검색 `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

내게 필요한 옵션 설정에 응답 필요 하기 때문에 기본 제공 스타일 다르게 적용 됩니다. Xaml에서는 기본 제공 스타일을 적용 하는 `DynamicResource` 태그 확장을 사용 합니다.

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C#에서 기본 제공 스타일에서 선택 `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>액세스 가능성

내게 필요한 옵션 기본 설정을 준수 하도록 쉽게 수행할 수 있도록 기본 제공 스타일 존재 합니다. 기본 제공 스타일 중 하나를 사용할 때 사용자 설정의 내게 필요한 옵션 기본 설정을 적절 하 게 하는 경우 글꼴 크기 자동으로 증가 합니다.

기본 스타일으로 설정 및 해제 된 내게 필요한 옵션 설정으로 스타일이 지정 된 뷰는 동일한 페이지의 다음 예제를 참조 하세요.

사용 안 함:

![](styles-images/pre-access.png "내게 필요한 옵션 Disabled로 장치 스타일")

사용:

![](styles-images/post-access.png "사용 하도록 설정 하는 내게 필요한 옵션 장치 스타일")

내게 필요한 옵션을 보장 하려면 응용 프로그램 내에서 한 텍스트와 관련 된 스타일에 대 한 기반으로 기본 제공 스타일을 사용 하 고 스타일 일관 되 게 사용 되어 있는지를 확인 합니다. 참조 [스타일](~/xamarin-forms/user-interface/styles/index.md) 에 대 한 자세한 내용은 확장 하 고 일반적 스타일을 사용 합니다.


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms에 장 12를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
