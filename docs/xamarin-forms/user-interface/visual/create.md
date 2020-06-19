---
title: Xamarin.Forms시각적 렌더러 만들기
description: Xamarin.Forms뷰를 서브 클래스 하지 않고도 VisualElement 개체에 선택적으로 적용 되는 시각적 개체를 만듭니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 23edbb007e912d13858686d1c5ec574c9e3349c7
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127143"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Xamarin.Forms시각적 렌더러 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

Xamarin.Forms시각적 개체를 사용 하면 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 뷰를 서브 클래스 하지 않고도 개체에 렌더러를 만들고 선택적으로 적용할 수 있습니다 Xamarin.Forms . 의 일부로 형식을 지정 하는 렌더러는 `IVisual` `ExportRendererAttribute` 기본 렌더러 대신 옵트인 (opt in) 뷰를 렌더링 하는 데 사용 됩니다. 렌더러 선택 시간에 `Visual` 뷰의 속성이 검사 되 고 렌더러 선택 프로세스에 포함 됩니다.

> [!IMPORTANT]
> 현재 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) 뷰를 렌더링 한 후에는 속성을 변경할 수 없지만 이후 릴리스에서 변경 될 수 있습니다.

시각적 렌더러를 만들고 사용 하는 프로세스는 Xamarin.Forms 다음과 같습니다.

1. 필요한 보기에 대 한 플랫폼 렌더러를 만듭니다. 자세한 내용은 [렌더러 만들기](#create-platform-renderers)를 참조 하세요.
1. 에서 파생 되는 형식을 만듭니다 `IVisual` . 자세한 내용은 [IVisual 개체 형식 만들기](#create-an-ivisual-type)를 참조 하세요.
1. 렌더러를 `IVisual` 데코레이팅하는의 일부로 형식을 등록 합니다 `ExportRendererAttribute` . 자세한 내용은 [IVisual 개체 형식 등록](#register-the-ivisual-type)을 참조 하세요.
1. 뷰의 속성을 이름으로 설정 하 여 시각적 렌더러를 사용 합니다 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `IVisual` . 자세한 내용은 [시각적 렌더러 사용](#consume-the-visual-renderer)을 참조 하세요.
1. 필드 형식에 대 한 이름을 등록 `IVisual` 합니다. 자세한 내용은 [IVisual 개체 형식에 대 한 이름 등록](#register-a-name-for-the-ivisual-type)을 참조 하세요.

## <a name="create-platform-renderers"></a>플랫폼 렌더러 만들기

렌더러 클래스를 만드는 방법에 대 한 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 참조 하세요. 그러나 Xamarin.Forms 뷰를 서브 클래스 하지 않고도 뷰에 시각적 렌더러가 적용 됩니다.

여기에 설명 된 렌더러 클래스는 [`Button`](xref:Xamarin.Forms.Button) 그림자를 사용 하 여 해당 텍스트를 표시 하는 사용자 지정을 구현 합니다.

### <a name="ios"></a>iOS

다음 코드 예제에서는 iOS에 대 한 단추 렌더러를 보여 줍니다.

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### <a name="android"></a>Android

다음 코드 예제에서는 Android의 단추 렌더러를 보여 줍니다.

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## <a name="create-an-ivisual-type"></a>IVisual 개체 형식 만들기

플랫폼 간 라이브러리에서에서 파생 되는 형식을 만듭니다 `IVisual` .

```csharp
public class CustomVisual : IVisual
{
}
```

`CustomVisual`그런 다음 렌더러 클래스에 대해 형식을 등록 하 여 [`Button`](xref:Xamarin.Forms.Button) 개체가 렌더러를 사용 하 여 옵트인 하도록 허용할 수 있습니다.

## <a name="register-the-ivisual-type"></a>IVisual 개체 형식 등록

플랫폼 프로젝트에서 `ExportRendererAttribute` 어셈블리 수준에를 추가 합니다.

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
namespace VisualDemos.iOS
{
    public class CustomButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            // ...
        }
    }
}
```

IOS 플랫폼 프로젝트에 대 한이 예제에서는 `ExportRendererAttribute` 클래스를 사용 하 여 `CustomButtonRenderer` [`Button`](xref:Xamarin.Forms.Button) `IVisual` 세 번째 인수로 등록 된 형식을 사용 하는 개체를 렌더링 하도록 지정 합니다. 의 일부로 형식을 지정 하는 렌더러는 `IVisual` `ExportRendererAttribute` 기본 렌더러 대신 옵트인 (opt in) 뷰를 렌더링 하는 데 사용 됩니다.

## <a name="consume-the-visual-renderer"></a>시각적 렌더러 사용

[`Button`](xref:Xamarin.Forms.Button)개체는 속성을로 설정 하 여 렌더러 클래스를 사용 하도록 선택할 수 있습니다 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `Custom` .

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> XAML에서 형식 변환기는 속성 값에 "시각적 개체" 접미사를 포함 하지 않아도 됩니다 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) . 그러나 전체 형식 이름을 지정할 수도 있습니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

렌더러 선택 시간에 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) 의 속성을 [`Button`](xref:Xamarin.Forms.Button) 검사 하 여 렌더러 선택 프로세스에 포함 합니다. 렌더러가 없으면 Xamarin.Forms 기본 렌더러가 사용 됩니다.

다음 스크린샷은 [`Button`](xref:Xamarin.Forms.Button) 그림자를 사용 하 여 해당 텍스트를 표시 하는 렌더링 된를 보여 줍니다.

[![IOS 및 Android에서 섀도 텍스트를 사용 하는 사용자 지정 단추의 스크린샷](material-visual-images/custom-button.png "그림자 텍스트가 있는 단추")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>IVisual 형식의 이름 등록

를 [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) 사용 하 여 형식에 대해 다른 이름을 선택적으로 등록할 수 있습니다 `IVisual` . 이 방법은 서로 다른 시각적 라이브러리 간의 이름 충돌을 해결 하는 데 사용 하거나 형식 이름과 다른 이름을 사용 하 여 시각적 개체를 참조 하려는 경우에 사용할 수 있습니다.

는 [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) 플랫폼 간 라이브러리 또는 플랫폼 프로젝트의 어셈블리 수준에서 정의 해야 합니다.

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

`IVisual`그런 다음 등록 된 이름을 통해 형식을 사용할 수 있습니다.

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> 등록 된 이름을 통해 시각적 개체를 사용 하는 경우 모든 "시각적" 접미사가 포함 되어야 합니다.

## <a name="related-links"></a>관련 링크

- [재질 시각적 개체 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin.Forms 재질 시각적 개체](material-visual.md)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
