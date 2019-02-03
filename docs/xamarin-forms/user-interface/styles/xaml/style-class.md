---
title: Xamarin.Forms 스타일 클래스
description: Xamarin.Forms 스타일 클래스 스타일 상속 방식을 사용 하지 않고 컨트롤에 적용할 스타일을 여러 개를 사용 합니다.
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: dd749a4a78adbab5317f1ae5ca6334caa009b9b3
ms.sourcegitcommit: 9dcb7377dc92ad921285fbb857b0be13030bbea3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668552"
---
# <a name="xamarinforms-style-classes"></a>Xamarin.Forms 스타일 클래스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Xamarin.Forms 스타일 클래스 스타일 상속 방식을 사용 하지 않고 컨트롤에 적용할 스타일을 여러 개를 사용 합니다._

## <a name="create-style-classes"></a>스타일 클래스 만들기

스타일 클래스를 설정 하 여 만들 수 있습니다는 [ `Class` ](xref:Xamarin.Forms.Style.Class) 속성을 [ `Style` ](xref:Xamarin.Forms.Style) 에 `string` 클래스 이름을 나타내는입니다. 장점은이 비해를 사용 하 여 명시적 스타일 정의 `x:Key` 특성에 여러 스타일 클래스에 적용할 수는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.

> [!IMPORTANT]
> 다른 형식을 대상 제공 여러 스타일 클래스 이름이 공유할 수 있습니다. 그러면 동일 하 게 명명 하는 다양 한 대상 유형에 여러 스타일 클래스.

다음 예제에서는 세 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 스타일 클래스와 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스 스타일:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="BoxView"
               Class="Separator">
            <Setter Property="BackgroundColor"
                    Value="#CCCCCC" />
            <Setter Property="HeightRequest"
                    Value="1" />
        </Style>

        <Style TargetType="BoxView"
               Class="Rounded">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="10" />
        </Style>    

        <Style TargetType="BoxView"
               Class="Circle">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="WidthRequest"
                    Value="100" />
            <Setter Property="HeightRequest"
                    Value="100" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="50" />
        </Style>

        <Style TargetType="VisualElement"
               Class="Rotated"
               ApplyToDerivedTypes="true">
            <Setter Property="Rotation"
                    Value="45" />
        </Style>        
    </ContentPage.Resources>
</ContentPage>
```

합니다 `Separator`, `Rounded`, 및 `Circle` 스타일 클래스 각 집합 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 속성을 특정 값입니다.

합니다 `Rotated` 스타일 클래스에는 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 의 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), 즉만 적용할 수 있습니다 `VisualElement` 인스턴스. 그러나 해당 [ `ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) 속성이로 설정 되어 `true`에서 파생 되는 모든 컨트롤에 적용할 수 있는지 확인 하는 `VisualElement`와 같은 [ `BoxView` ](xref:Xamarin.Forms.BoxView)합니다. 파생된 형식으로 스타일을 적용 하는 방법에 대 한 자세한 내용은 참조 하세요. [파생된 형식에 스타일을 적용할](implicit.md#apply-a-style-to-derived-types)합니다.

해당 하는 C# 코드가입니다.

```csharp
var separatorBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Separator",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#CCCCCC")
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 1
        }
    }
};

var roundedBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Rounded",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 10
        }
    }
};

var circleBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Circle",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = VisualElement.WidthRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 50
        }
    }
};

var rotatedVisualElementStyle = new Style(typeof(VisualElement))
{
    Class = "Rotated",
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.RotationProperty,
            Value = 45
        }
    }
};

Resources = new ResourceDictionary
{
    separatorBoxViewStyle,
    roundedBoxViewStyle,
    circleBoxViewStyle,
    rotatedVisualElementStyle
};
```

## <a name="consume-style-classes"></a>스타일 클래스를 사용 합니다.

스타일 클래스를 설정 하 여 사용할 수는 [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) 유형인 컨트롤의 속성 `IList<string>`, 스타일 클래스 이름 목록에 있습니다. 스타일 클래스를 적용할 컨트롤 형식이 일치 하는 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 스타일 클래스입니다.

다음 예제에서는 세 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 인스턴스를 각각 설정 다른 스타일 클래스:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <BoxView StyleClass="Separator" />       
        <BoxView WidthRequest="100"
                 HeightRequest="100"
                 HorizontalOptions="Center"
                 StyleClass="Rounded, Rotated" />
        <BoxView HorizontalOptions="Center"
                 StyleClass="Circle" />
    </StackLayout>
</ContentPage>    
```

이 예제에서는 첫 번째 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 의 세 번째 하는 동안 줄 구분 기호, 되도록 스타일 `BoxView` 순환 됩니다. 두 번째 `BoxView` 에 두 개의 스타일 클래스를 적용 하는 it 둥근 모퉁이 지정 하 고 45도 회전 합니다.

![](style-class-images/boxviews.png "BoxViews는 스타일 클래스를 사용 하 여 스타일")

> [!IMPORTANT]
> 여러 스타일 클래스에 적용할 수를 제어 하기 때문에 합니다 [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) 형식의 속성이 `IList<string>`합니다. 이 경우 스타일 클래스 목록을 오름차순 적용 됩니다. 따라서 여러 스타일 클래스와 동일한 속성을 설정 하는 경우 가장 높은 목록 위치에 있는 스타일 클래스에서 속성 우선을 적용 됩니다.

해당 하는 C# 코드가입니다.

```csharp
...
Content = new StackLayout
{
    Children =
    {
        new BoxView { StyleClass = new [] { "Separator" } },
        new BoxView { WidthRequest = 100, HeightRequest = 100, HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Rounded", "Rotated" } },
        new BoxView { HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Circle" } }
    }
};
```

## <a name="related-links"></a>관련 링크

- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
