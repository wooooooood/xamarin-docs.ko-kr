---
title: 공용 언어 런타임 속성으로 결과 매개 변수 전달
description: 공용 언어 런타임 (CLR) 속성은 런타임 속성 변경 내용에 응답 하지 않는 효과 매개 변수 정의를 사용할 수 있습니다. 이 문서에서는 CLR 속성을 사용 하 여 매개 변수는 효과를 전달 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 1bb357b256a7cc6d52d1d92613f38cbf48400c4c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995770"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>공용 언어 런타임 속성으로 결과 매개 변수 전달

_공용 언어 런타임 (CLR) 속성은 런타임 속성 변경 내용에 응답 하지 않는 효과 매개 변수 정의를 사용할 수 있습니다. 이 문서에서는 CLR 속성을 사용 하 여 매개 변수는 효과를 전달 하는 방법을 보여 줍니다._

런타임 속성 변경 내용에 응답 하지 않는 효과 매개 변수를 만드는 프로세스는 다음과 같습니다.

1. 만들기는 `public` 해당 서브 클래스를 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) 클래스입니다. `RoutingEffect` 클래스 내부 효과 일반적으로 플랫폼별을 래핑하는 플랫폼 독립적인 효과 나타냅니다.
1. 해상도 그룹 이름 및 각 플랫폼별 결과 클래스에 지정 된 고유 ID 문자열이 전달 하 여 기본 클래스 생성자를 호출 하는 생성자를 만듭니다.
1. 각 매개 변수에 결과를 전달할 수에 대 한 클래스 속성을 추가 합니다.

매개 변수 효과 인스턴스화할 때 각 속성에 대 한 값을 지정 하 여 결과를 전달할 수 있습니다.

샘플 응용 프로그램을 보여 줍니다를 `ShadowEffect` 하 여 표시 되는 텍스트에 그림자를 추가 하는 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤. 다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](clr-properties-images/shadow-effect.png "그림자 효과 프로젝트 책임")

A [ `Label` ](xref:Xamarin.Forms.Label) 대 한 control 권한 합니다 `HomePage` 를 사용자 지정는 `LabelShadowEffect` 각 플랫폼별 프로젝트에. 매개 변수는 각 전달 `LabelShadowEffect` 속성을 통해는 `ShadowEffect` 클래스입니다. 각 `LabelShadowEffect` 클래스에서 파생 되는 `PlatformEffect` 각 플랫폼에 대 한 클래스입니다. 그러면 표시 되는 텍스트에 추가할 그림자를 `Label` 다음 스크린샷과에서 같이 제어:

![](clr-properties-images/screenshots.png "각 플랫폼에 그림자 효과")

## <a name="creating-effect-parameters"></a>결과 매개 변수 만들기

`public` 해당 서브 클래스를 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) 다음 코드 예제에서 설명한 것 처럼 결과 매개 변수를 나타내는 클래스를 만들 수 해야 합니다.

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

합니다 `ShadowEffect` 각 플랫폼별에 전달할 매개 변수를 나타내는 네 가지 속성을 포함 `LabelShadowEffect`합니다. 클래스 생성자는 해상도 그룹 이름 및 각 플랫폼별 결과 클래스에 지정 된 고유 ID를 연결으로 구성 된 매개 변수에서 전달 기본 클래스 생성자를 호출 합니다. 따라서의 새 인스턴스를 `MyCompany.LabelShadowEffect` 컨트롤을 추가할 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션 때를 `ShadowEffect` 인스턴스화됩니다.

## <a name="consuming-the-effect"></a>효과 사용합니다.

다음 XAML 코드 예제와 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤에는 `ShadowEffect` 연결:

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

해당 [ `Label` ](xref:Xamarin.Forms.Label) C#에서 다음 코드 예제에 표시 됩니다.

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

두 코드 예제에서는 인스턴스를 `ShadowEffect` 컨트롤에 추가 되기 전에 각 속성에 대해 지정 되는 값을 사용 하 여 클래스가 인스턴스화되고 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션입니다. `ShadowEffect.Color` 속성 플랫폼별 색 값을 사용 합니다. 자세한 내용은 [장치 클래스](~/xamarin-forms/platform/device.md)합니다.

## <a name="creating-the-effect-on-each-platform"></a>각 플랫폼에 대 한 효과 만들기

다음 섹션에서는 플랫폼 전용 구현에 설명 된 `LabelShadowEffect` 클래스입니다.

### <a name="ios-project"></a>iOS 프로젝트

다음 코드 예제는 `LabelShadowEffect` iOS 프로젝트에 대 한 구현 합니다.

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
                    Control.Layer.CornerRadius = effect.Radius;
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

`OnAttached` 메서드에서 검색 합니다 `ShadowEffect` 인스턴스 및 집합 `Control.Layer` 그림자를 만들 지정 된 속성 값에 대 한 속성. 이 기능에 래핑됩니다를 `try` / `catch` 효과에 연결 된 컨트롤에 없는 경우 차단 된 `Control.Layer` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

### <a name="android-project"></a>Android 프로젝트

다음 코드 예제는 `LabelShadowEffect` Android 프로젝트에 대 한 구현 합니다.

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

`OnAttached` 메서드 검색 합니다 `ShadowEffect` 인스턴스 및 호출 합니다 [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) 지정 된 속성 값을 사용 하 여 그림자를 만드는 방법. 이 기능에 래핑됩니다를 `try` / `catch` 효과에 연결 된 컨트롤에 없는 경우 차단 된 `Control.Layer` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

### <a name="universal-windows-platform-project"></a>유니버설 Windows 플랫폼 프로젝트

다음 코드 예제는 `LabelShadowEffect` 유니버설 Windows 플랫폼 (UWP) 프로젝트에 대 한 구현:

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

유니버설 Windows 플랫폼에 그림자 효과 제공 하지 않으므로 `LabelShadowEffect` 구현은 두 가지 플랫폼에서 두 번째 오프셋을 추가 하 여 시뮬레이션 [ `Label` ](xref:Xamarin.Forms.Label) 주 뒤 `Label`합니다. `OnAttached` 메서드에서 검색 합니다 `ShadowEffect` 인스턴스를 만들고 새 `Label`, 일부 레이아웃 속성을 설정 하 고는 `Label`합니다. 다음 설정 하 여 그림자를 생성 합니다 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)를 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), 및 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) 합니다 의위치와색을제어하는속성`Label`. `shadowLabel` 그런 다음 삽입은 주 뒤 오프셋 `Label`합니다. 이 기능에 래핑됩니다를 `try` / `catch` 효과에 연결 된 컨트롤에 없는 경우 차단 된 `Control.Layer` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

## <a name="summary"></a>요약

이 문서에서는 CLR 속성을 사용 하 여 영향을 줄에 매개 변수를 전달할 살펴보았습니다. CLR 속성은 런타임 속성 변경 내용에 응답 하지 않는 효과 매개 변수 정의를 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [그림자 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
