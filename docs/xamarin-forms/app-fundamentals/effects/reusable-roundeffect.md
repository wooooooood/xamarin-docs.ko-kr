---
title: Xamarin.Forms 재사용 가능 able RoundEffect
description: RoundEffect는 VisualElement에서 파생된 모든 컨트롤에 적용하여 컨트롤을 원으로 렌더링할 수 있는 재사용 가능한 효과입니다.
ms.prod: xamarin
ms.assetid: B5DE7507-B565-4EE5-9897-27E5733FD173
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/25/2019
ms.openlocfilehash: 851ed7a2ad1c416b4d03d583b9d0aeb7f7774eea
ms.sourcegitcommit: aa7e9d2c370ba9cbc830f10b94b4cd4221fc5467
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73055941"
---
# <a name="xamarinforms-reusable-roundeffect"></a>Xamarin.Forms 재사용 가능 able RoundEffect

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)

RoundEffect는 VisualElement에서 파생된 모든 컨트롤을 원으로 렌더링하는 작업을 간소화합니다. 이 효과는 원형 이미지, 단추 및 기타 컨트롤을 만드는 데 사용할 수 있습니다.

[![iOS 및 Android의 RoundEffect 스크린샷](example-roundeffect-images/round-effect-cropped.png)](example-roundeffect-images/round-effect.png#lightbox)

## <a name="create-a-shared-routingeffect"></a>공유 RoutingEffect 만들기

플랫폼 간 효과를 만들려면 공유 프로젝트에 효과 클래스를 만들어야 합니다. 샘플 애플리케이션은 `RoutingEffect` 클래스에서 파생된 빈 `RoundEffect` 클래스를 만듭니다.

```csharp
public class RoundEffect : RoutingEffect
{
    public RoundEffect() : base($"Xamarin.{nameof(RoundEffect)}")
    {
    }
}
```

이 클래스는 공유 프로젝트가 코드 또는 XAML에 있는 효과에 대한 참조를 확인할 수 있도록 하지만 기능은 제공하지 않습니다. 효과에는 각 플랫폼별 구현이 있어야 합니다.

## <a name="implement-the-android-effect"></a>Android 효과 구현

Android 플랫폼 프로젝트는 `PlatformEffect`에서 파생된 `RoundEffect` 클래스를 정의합니다. 이 클래스는 Xamarin.Forms가 효과 클래스를 확인할 수 있도록 해 주는 `assembly` 특성으로 태그가 지정되었습니다.

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.Droid.RoundEffect), nameof(RoundEffectDemo.Droid.RoundEffect))]
namespace RoundEffectDemo.Droid
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
}
```

Android 플랫폼은 `OutlineProvider` 개념을 사용하여 컨트롤의 가장자리를 정의합니다. 샘플 프로젝트에는 `ViewOutlineProvider` 클래스에서 파생된 `CornerRadiusProvider` 클래스가 포함됩니다.

```csharp
class CornerRadiusOutlineProvider : ViewOutlineProvider
{
    Element element;

    public CornerRadiusOutlineProvider(Element formsElement)
    {
        element = formsElement;
    }

    public override void GetOutline(Android.Views.View view, Outline outline)
    {
        float scale = view.Resources.DisplayMetrics.Density;
        double width = (double)element.GetValue(VisualElement.WidthProperty) * scale;
        double height = (double)element.GetValue(VisualElement.HeightProperty) * scale;
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;
        Rect rect = new Rect(0, 0, (int)width, (int)height);
        outline.SetRoundRect(rect, radius);
    }
}
```

이 클래스는 Xamarin.Forms `Element` 인스턴스의 `Width` 속성과 `Height` 속성을 사용하여 가장 짧은 길이의 절반인 반경을 계산합니다.

윤곽선 공급자가 정의되면 `RoundEffect` 클래스가 이를 사용하여 효과를 구현할 수 있습니다.

```csharp
public class RoundEffect : PlatformEffect
{
    ViewOutlineProvider originalProvider;
    Android.Views.View effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalProvider = effectTarget.OutlineProvider;
            effectTarget.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            effectTarget.ClipToOutline = true;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if(effectTarget != null)
        {
            effectTarget.OutlineProvider = originalProvider;
            effectTarget.ClipToOutline = false;
        }
    }
}
```

효과가 요소에 연결되면 `OnAttached` 메서드가 호출됩니다. 기존 `OutlineProvider` 개체는 효과가 연결 해제될 경우 복원될 수 있도록 저장됩니다. `CornerRadiusOutlineProvider`의 새로운 인스턴스가 `OutlineProvider`로 사용되고, 오버플로 요소를 윤곽 테두리에 맞게 자르기 위해 `ClipToOutline`이 true로 설정됩니다.

`OnDetatched` 메서드는 요소에서 효과가 제거될 경우 호출되어 원래 `OutlineProvider` 값을 복원합니다.

> [!NOTE]
> 요소 유형에 따라 `Control` 속성은 Null이거나 Null이 아닐 수 있습니다. `Control` 속성이 Null이 아니면 컨트롤에 직접 동근 모퉁이를 적용할 수 있습니다. 그러나 이 속성이 Null이면 둥근 모퉁이를 `Container` 개체에 적용해야 합니다. `effectTarget` 필드를 사용하여 적절한 개체에 효과를 적용할 수 있습니다.

## <a name="implement-the-ios-effect"></a>iOS 효과 구현

iOS 플랫폼 프로젝트는 `PlatformEffect`에서 파생된 `RoundEffect` 클래스를 정의합니다. 이 클래스는 Xamarin.Forms가 효과 클래스를 확인할 수 있도록 해 주는 `assembly` 특성으로 태그가 지정되었습니다.

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.iOS.RoundEffect), nameof(RoundEffectDemo.iOS.RoundEffect))]
namespace RoundEffectDemo.iOS
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
```

iOS에서 컨트롤은 `Layer` 속성을 가지며, 여기에는 `CornerRadius` 속성이 있습니다. iOS의 `RoundEffect` 클래스 구현은 적절한 모퉁이 반경을 계산하여 계층의 `CornerRadius` 속성을 업데이트합니다.

```csharp
public class RoundEffect : PlatformEffect
{
    nfloat originalRadius;
    UIKit.UIView effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalRadius = effectTarget.Layer.CornerRadius;
            effectTarget.ClipsToBounds = true;
            effectTarget.Layer.CornerRadius = CalculateRadius();
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if (effectTarget != null)
        {
            effectTarget.ClipsToBounds = false;
            if (effectTarget.Layer != null)
            {
                effectTarget.Layer.CornerRadius = originalRadius;
            }
        }
    }

    float CalculateRadius()
    {
        double width = (double)Element.GetValue(VisualElement.WidthRequestProperty);
        double height = (double)Element.GetValue(VisualElement.HeightRequestProperty);
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;

        return radius;
    }
}
```

`CalculateRadius` 메서드는 Xamarin.Forms `Element`의 최소 길이를 바탕으로 반경을 계산합니다. 효과가 요소에 연결되면 `OnAttached` 메서드가 호출되어 계층의 `CornerRadius` 속성을 업데이트합니다. 오버플로 요소가 컨트롤의 테두리에 맞게 잘리도록 `ClipToBounds` 속성을 `true`로 설정합니다. 효과가 컨트롤에서 제거되면 `OnDetatched` 메서드가 호출되어 변경 사항을 되돌리고 원래 모퉁이 반경을 복원합니다.

> [!NOTE]
> 요소 유형에 따라 `Control` 속성은 Null이거나 Null이 아닐 수 있습니다. `Control` 속성이 Null이 아니면 컨트롤에 직접 동근 모퉁이를 적용할 수 있습니다. 그러나 이 속성이 Null이면 둥근 모퉁이를 `Container` 개체에 적용해야 합니다. `effectTarget` 필드를 사용하여 적절한 개체에 효과를 적용할 수 있습니다.

## <a name="consume-the-effect"></a>효과 사용

효과를 여러 플랫폼에서 구현한 후에는 Xamarin.Forms 컨트롤에서 사용할 수 있습니다. `RoundEffect`의 일반적인 적용 사례는 `Image` 개체를 원형으로 만드는 것입니다. 다음 XAML에서는 효과가 `Image` 인스턴스에 적용되는 것을 보여 줍니다.

```xaml
<Image Source=outdoors"
       HeightRequest="100"
       WidthRequest="100">
    <Image.Effects>
        <local:RoundEffect />
    </Image.Effects>
</Image>
```

코드 안에서 효과를 적용할 수도 있습니다.

```csharp
var image = new Image
{
    Source = ImageSource.FromFile("outdoors"),
    HeightRequest = 100,
    WidthRequest = 100
};
image.Effects.Add(new RoundEffect());
```

`RoundEffect` 클래스는 `VisualElement`에서 파생된 모든 컨트롤에 적용할 수 있습니다.

> [!NOTE]
> 효과가 올바른 반경을 계산할 수 있으려면 효과가 적용된 컨트롤이 명시적 크기를 가져야 합니다. 따라서 `HeightRequest` 속성과 `WidthRequest` 속성이 정의되어 있어야 합니다. 영향을 받는 컨트롤이 `StackLayout`에 나타나는 경우, `HorizontalOptions` 속성은 `LayoutOptions.CenterAndExpand`와 같은 **Expand** 값에 사용되어서는 안 됩니다. 그렇지 않으면 정확한 크기를 갖지 않게 됩니다.

## <a name="related-links"></a>관련 링크

- [RoundEffect 샘플 애플리케이션](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)
- [효과 소개](~/xamarin-forms/app-fundamentals/effects/introduction.md)
- [효과 만들기](~/xamarin-forms/app-fundamentals/effects/creating.md)
