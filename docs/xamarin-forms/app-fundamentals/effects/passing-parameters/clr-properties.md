---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 37d870509e034f4c23afba60fa055965ed9df4de
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138868"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>공용 언어 런타임 속성으로 효과 매개 변수 전달

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)

_CLR(공용 언어 런타임) 속성은 런타임 속성 변경 내용에 응답하지 않는 효과 매개 변수를 정의하는 데 사용할 수 있습니다. 이 문서에서는 CLR 속성을 사용하여 매개 변수를 효과에 전달하는 방법을 설명합니다._

런타임 속성 변경 내용에 응답하지 않는 효과 매개 변수를 만드는 프로세스는 다음과 같습니다.

1. [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 클래스를 서브클래스하는 `public` 클래스를 만듭니다. `RoutingEffect` 클래스는 일반적으로 플랫폼에 따라 내부 효과를 래핑하는 플랫폼 독립적인 효과를 나타냅니다.
1. 기본 클래스 생성자를 호출하여 해상도 그룹 이름의 연결과 각 플랫폼별 효과 클래스에 지정된 고유 ID를 전달하는 생성자를 만듭니다.
1. 효과에 전달될 각 매개 변수에 대한 클래스에 속성을 추가합니다.

그런 다음, 효과를 인스턴스화할 때 각 속성에 대한 값을 지정하여 매개 변수를 효과에 전달할 수 있습니다.

샘플 애플리케이션은 [`Label`](xref:Xamarin.Forms.Label) 컨트롤에서 표시되는 텍스트에 그림자를 추가하는 `ShadowEffect`를 설명합니다. 다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](clr-properties-images/shadow-effect.png "Shadow Effect Project Responsibilities")

`HomePage`의 [`Label`](xref:Xamarin.Forms.Label) 컨트롤은 각 플랫폼별 프로젝트에서 `LabelShadowEffect`로 사용자 지정됩니다. 매개 변수는 `ShadowEffect` 클래스의 속성을 통해 각 `LabelShadowEffect`에 전달됩니다. 각 `LabelShadowEffect` 클래스는 각 플랫폼에 대한 `PlatformEffect` 클래스에서 파생됩니다. 그러면 다음 스크린샷과 같이 `Label` 컨트롤에 표시되는 텍스트에 그림자가 추가됩니다.

![](clr-properties-images/screenshots.png "Shadow Effect on each Platform")

## <a name="creating-effect-parameters"></a>효과 매개 변수 만들기

다음 코드 예제에서 설명한 것처럼 효과 매개 변수를 표현하도록 [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 클래스를 서브클래스하는 `public` 클래스를 만들어야 합니다.

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {            
  }
}
```

`ShadowEffect`에는 각 플랫폼별 `LabelShadowEffect`에 전달할 매개 변수를 나타내는 네 가지 속성이 포함되어 있습니다. 클래스 생성자는 기본 클래스 생성자를 호출하여 해상도 그룹 이름의 연결과 각 플랫폼별 효과 클래스에 지정된 고유 ID로 이루어진 매개 변수를 전달합니다. 따라서 `ShadowEffect`가 인스턴스화될 때 `MyCompany.LabelShadowEffect`의 새 인스턴스가 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가됩니다.

## <a name="consuming-the-effect"></a>효과 사용

다음 XAML 코드 예제는 `ShadowEffect`가 연결된 [`Label`](xref:Xamarin.Forms.Label) 컨트롤을 보여줍니다.

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
</Label>
```

C#의 해당하는 [`Label`](xref:Xamarin.Forms.Label)가 다음 코드 예제에 나와 있습니다.

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

두 코드 예제에서 `ShadowEffect` 클래스의 인스턴스는 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가되기 전에 각 속성에 대해 지정되는 값을 사용하여 인스턴스화됩니다. `ShadowEffect.Color` 속성은 플랫폼별 색 값을 사용합니다. 자세한 내용은 [디바이스 클래스](~/xamarin-forms/platform/device.md)를 참조하세요.

## <a name="creating-the-effect-on-each-platform"></a>각 플랫폼의 효과 만들기

다음 섹션에서는 `LabelShadowEffect` 클래스의 플랫폼별 구현을 설명합니다.

### <a name="ios-project"></a>iOS 프로젝트

다음 코드 예제는 iOS 프로젝트에 대한 `LabelShadowEffect` 구현을 보여줍니다.

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.ShadowRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` 메서드는 `ShadowEffect` 인스턴스를 검색하고, `Control.Layer` 속성을 그림자를 만들도록 지정된 속성 값으로 설정합니다. 이 기능은 효과가 연결된 컨트롤에 `Control.Layer` 속성이 없는 경우 `try`/`catch` 블록에 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

### <a name="android-project"></a>Android 프로젝트

다음 코드 예제는 Android 프로젝트에 대한 `LabelShadowEffect` 구현을 보여줍니다.

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` 메서드는 `ShadowEffect` 인스턴스를 검색하고, 지정된 속성 값을 사용하여 그림자를 만들도록 [`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*) 메서드를 호출합니다. 이 기능은 효과가 연결된 컨트롤에 `Control.Layer` 속성이 없는 경우 `try`/`catch` 블록에 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

### <a name="universal-windows-platform-project"></a>유니버설 Windows 플랫폼 프로젝트

다음 코드 예제는 UWP(유니버설 Windows 플랫폼) 프로젝트에 대한 `LabelShadowEffect` 구현을 보여줍니다.

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

유니버설 Windows 플랫폼은 그림자 효과를 제공하지 않으므로 두 가지 플랫폼의 `LabelShadowEffect` 구현은 기본 `Label` 뒤에 두 번째 오프셋 [`Label`](xref:Xamarin.Forms.Label)을 추가하여 시뮬레이션합니다. `OnAttached` 메서드는 `ShadowEffect` 인스턴스를 검색하고, 새 `Label`을 만든 후, 일부 레이아웃 속성을 `Label`로 설정합니다. 그런 다음, [`TextColor`](xref:Xamarin.Forms.Label.TextColor), [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성을 설정하여 그림자를 생성하여 `Label`의 위치와 색을 제어합니다. 그런 다음, `shadowLabel`이 기본 `Label` 뒤에 삽입됩니다. 이 기능은 효과가 연결된 컨트롤에 `Control.Layer` 속성이 없는 경우 `try`/`catch` 블록에 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

## <a name="summary"></a>요약

이 문서에서는 CLR 속성을 사용하여 매개 변수를 효과에 전달하는 방법을 설명했습니다. CLR 속성은 런타임 속성 변경 내용에 응답하지 않는 효과 매개 변수를 정의하는 데 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [그림자 효과(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
