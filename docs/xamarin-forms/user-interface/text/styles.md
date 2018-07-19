---
title: Xamarin.Forms 텍스트 스타일
description: 이 문서에서는 설명 Xamarin.Forms 응용 프로그램에서 텍스트 스타일 지정 하는 방법입니다. 스타일을 한 번 정의 여러 뷰를 사용 하 고 있지만 한 가지 유형의 보기를 사용 하 여 스타일만 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 73aa3115e92d1e3954f5ae3eb8dcb84abf9d9efb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998770"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.Forms 텍스트 스타일

_Xamarin.Forms에서 텍스트 스타일 지정_

스타일은 레이블, 항목 및 편집기의 모양에 맞게 사용할 수 있습니다. 스타일을 한 번 정의 여러 뷰를 사용 하 고 있지만 한 가지 유형의 보기를 사용 하 여 스타일만 사용할 수 있습니다.
스타일을 지정할 수는 `Key` 선택적으로 특정 컨트롤을 사용 하 여 적용 하 고 `Style` 속성입니다.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>기본 제공 스타일

Xamarin.Forms에 몇 [기본 제공](xref:Xamarin.Forms.Device.Styles) 일반적인 시나리오에 대 한 스타일:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

기본 제공 스타일 중 하나를 적용 하려면 사용는 `DynamicResource` 태그 확장 스타일을 지정 하려면:

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

스타일 setter 이루어진 setter 속성으로 구성 하 고 속성 값으로 설정 됩니다.

C#에서는 30 크기의 빨간색 텍스트를 사용 하 여 레이블에 대 한 사용자 지정 스타일을 다음과 같이 정의 됩니다.

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

XAML:

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

리소스 (스타일을 모두 포함) 내에서 정의 되어 `ContentPage.Resources`에 더 친숙 한의 형제인 `ContentPage.Content` 요소입니다.

![](styles-images/customstyle.png "사용자 지정 스타일 예제")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>스타일 적용

일치 하는 모든 보기에 적용할 수는 스타일을 만든 후 해당 `TargetType`합니다.

XAML에서 사용자 지정 스타일에 적용 되 뷰를 제공 하 여 해당 `Style` 속성을 `StaticResource` 원하는 스타일을 참조 하는 태그 확장:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C#에서 스타일 중 하나 수는 뷰에 직접 적용 또는 추가할 메시지를 검색할 수는 페이지에서 `ResourceDictionary`합니다. 직접 추가 합니다.

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

추가 하 고 페이지의 검색 `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

기본 제공 스타일은 내게 필요한 옵션 설정에 응답 해야 하기 때문에 다르게 적용 됩니다. XAML의 기본 제공 스타일을 적용 하는 `DynamicResource` 태그 확장을 사용:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C#에서 기본 제공 스타일에서 선택 `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>액세스 가능성

기본 제공 스타일 쉽게 내게 필요한 옵션 기본 설정을 적용 하기 위해 존재 합니다. 기본 제공 스타일 중 하나를 사용 하면 글꼴 크기를 사용자가 내게 필요한 옵션 기본 설정을 적절 하 게 설정 하는 경우 자동으로 증가 합니다.

내게 필요한 옵션 설정을 사용 하거나 사용 하지 않도록 설정으로 기본 스타일으로 스타일이 지정 된 보기의 동일한 페이지의 다음 예제를 살펴보세요.

사용 안 함:

![](styles-images/pre-access.png "내게 필요한 옵션 사용 안 함을 사용 하 여 장치 스타일")

사용:

![](styles-images/post-access.png "사용 하도록 설정 하는 내게 필요한 옵션을 사용 하 여 장치 스타일")

접근성을 위해 앱 내에서 모든 텍스트 관련 스타일에 대 한 기준으로 기본 제공 스타일을 사용 하 고 스타일을 일관 되 게 사용 중인지를 확인 합니다. 참조 [스타일](~/xamarin-forms/user-interface/styles/index.md) 확장 하 고 스타일을 사용 하 여 일반적인 작동에 대 한 자세한 내용은 합니다.


## <a name="related-links"></a>관련 링크

- [12 장 Xamarin.Forms를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [스타일](xref:Xamarin.Forms.Style)
