---
title: 연결 된 속성으로 결과 매개 변수 전달
description: 연결 된 속성은 런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 정의에 사용할 수 있습니다. 이 문서에서는 연결 된를 적용 하 고 런타임에 매개 변수를 변경 하려면 매개 변수를 전달 하는 속성을 사용 하 여 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 9483e424a74a88ce3f0eb49624bb5315551f2062
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996453"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>연결 된 속성으로 결과 매개 변수 전달

_연결 된 속성은 런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 정의에 사용할 수 있습니다. 이 문서에서는 연결 된를 적용 하 고 런타임에 매개 변수를 변경 하려면 매개 변수를 전달 하는 속성을 사용 하 여 하는 방법을 보여 줍니다._

런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 만드는 프로세스는 다음과 같습니다.

1. 만들기는 `static` 효과에 전달할 각 매개 변수에 대 한 연결된 된 속성을 포함 하는 클래스입니다.
1. 클래스에 추가 또는 제거에 연결할 클래스는 컨트롤에 효과 제어 하는 데 사용할 연결된 된 추가 속성을 추가 합니다. 속성 등록 연결이 있는지 확인 하십시오는 `propertyChanged` 속성의 값이 변경 될 때 실행 될 대리자입니다.
1. 만들 `static` getter 및 setter 각각에 대해 연결 된 속성입니다.
1. 논리를 구현 합니다 `propertyChanged` 대리자를 추가 하 고 효과 제거 합니다.
1. 내에 중첩 된 클래스를 구현 합니다 `static` 효과 서브 클래스의 이름을 딴 클래스는 `RoutingEffect` 클래스입니다. 생성자에 대 한 확인 그룹 이름 및 각 플랫폼별 결과 클래스에 지정 된 고유 ID 문자열이 전달 하 여 기본 클래스 생성자를 호출 합니다.

매개 변수가 해당 컨트롤에 연결 된 속성 및 속성 값을 추가 하 여 결과를 전달할 수 있습니다. 또한 매개 변수는 새 연결 된 속성 값을 지정 하 여 런타임 시 변경할 수 있습니다.

> [!NOTE]
> 연결된 된 속성에 속성 이름과 마침표로 구분 된 클래스를 포함 하는 특성으로 다른 개체에 연결 되 고 XAML에서 인식할 수 있지만 하나의 클래스에 정의 된 바인딩 가능한 속성의 특수 형식입니다. 자세한 내용은 [연결 속성](~/xamarin-forms/xaml/attached-properties.md)합니다.

샘플 응용 프로그램을 보여 줍니다를 `ShadowEffect` 하 여 표시 되는 텍스트에 그림자를 추가 하는 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤. 또한 런타임 시 그림자의 색을 변경할 수 있습니다. 다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](attached-properties-images/shadow-effect.png "그림자 효과 프로젝트 책임")

A [ `Label` ](xref:Xamarin.Forms.Label) 대 한 control 권한 합니다 `HomePage` 를 사용자 지정는 `LabelShadowEffect` 각 플랫폼별 프로젝트에. 매개 변수는 각 전달 `LabelShadowEffect` 를 통해 연결 된 속성에는 `ShadowEffect` 클래스입니다. 각 `LabelShadowEffect` 클래스에서 파생 되는 `PlatformEffect` 각 플랫폼에 대 한 클래스입니다. 그러면 표시 되는 텍스트에 추가할 그림자를 `Label` 다음 스크린샷과에서 같이 제어:

![](attached-properties-images/screenshots.png "각 플랫폼에 그림자 효과")

## <a name="creating-effect-parameters"></a>결과 매개 변수 만들기

`static` 다음 코드 예제에서 설명한 것 처럼 결과 매개 변수를 나타내는 클래스를 만들 수 해야 합니다.

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

합니다 `ShadowEffect` 5 개의 연결 된 속성이 포함 되어 `static` getter 및 setter 각각에 대해 연결 된 속성입니다. 각 플랫폼별에 전달할 매개 변수를 나타내는 이러한 속성 중 4 개 `LabelShadowEffect`합니다. `ShadowEffect` 클래스도 정의 합니다.는 `HasShadow` 연결 된 속성 추가 또는 제거를 제어 하는 효과 제어 하는 데 사용 되는는 `ShadowEffect` 클래스에 연결 됩니다. 이 연결 속성 레지스터는 `OnHasShadowChanged` 속성의 값이 변경 될 때 실행 되는 메서드. 이 메서드를 추가 하거나의 값을 기반으로 하는 효과 제거 합니다 `HasShadow` 연결 된 속성입니다.

중첩 `LabelShadowEffect` 클래스를 서브클래싱하는 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) 클래스를 지 원하는 효과 추가 및 제거 합니다. `RoutingEffect` 클래스 내부 효과 일반적으로 플랫폼별을 래핑하는 플랫폼 독립적인 효과 나타냅니다. 이 플랫폼별 효과 대 한 형식 정보에 액세스할 수 없는 컴파일 시간 이후 효과 제거 프로세스를 간소화 합니다. `LabelShadowEffect` 확인 그룹 이름 및 각 플랫폼별 결과 클래스에 지정 된 고유 ID를 연결으로 구성 된 매개 변수 전달, 기본 클래스 생성자를 호출 하는 생성자입니다. 이렇게 하면 효과 추가 및 제거는 `OnHasShadowChanged` 같이 메서드:

- **추가 효과** –의 새 인스턴스를 `LabelShadowEffect` 컨트롤에 추가 됩니다 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션입니다. 사용 하 여이 대체 합니다 [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) 효과 추가 하는 방법입니다.
- **제거를 적용** – 첫 번째 인스턴스의 합니다 `LabelShadowEffect` 컨트롤의 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션을 검색 하 고 제거 합니다.

## <a name="consuming-the-effect"></a>효과 사용합니다.

각 플랫폼별 `LabelShadowEffect` 연결 된 속성을 추가 하 여 사용할 수는 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤을 다음 XAML 코드 예제에서 설명한 것 처럼:

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

설정를 `ShadowEffect.HasShadow` 연결 된 속성을 `true` 실행를 `ShadowEffect.OnHasShadowChanged` 추가 하거나 제거 하는 메서드를 `LabelShadowEffect` 에 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤입니다. 두 코드 예제에서는 `ShadowEffect.Color` 플랫폼별 색 값을 제공 하는 연결 된 속성입니다. 자세한 내용은 [장치 클래스](~/xamarin-forms/platform/device.md)합니다.

또한 한 [ `Button` ](xref:Xamarin.Forms.Button) 그림자 색을 런타임 시 변경할 수 있습니다. 경우는 `Button` 를 클릭 하면 그림자 색을 설정 하 여 코드 변경의 `ShadowEffect.Color` 연결 된 속성:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>스타일을 사용 하 여 효과 사용합니다.

컨트롤에 연결 된 속성을 추가 하 여 사용할 수 있는 효과 스타일에 의해 사용 될 수도 있습니다. 다음 XAML 코드 예제는 *명시적* 스타일을 적용할 수 있는 그림자 효과 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

합니다 [ `Style` ](xref:Xamarin.Forms.Style) 에 적용할 수는 [ `Label` ](xref:Xamarin.Forms.Label) 설정 하 여 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성을는 `Style` 를사용하여인스턴스`StaticResource`다음 코드 예제 에서처럼 태그 확장:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

스타일에 대 한 자세한 내용은 참조 하세요. [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

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
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

합니다 `OnAttached` 메서드를 사용 하 여 연결 된 속성 값을 검색 하는 메서드를 호출 합니다 `ShadowEffect` getter, 및 설정 `Control.Layer` 속성을 만드는 그림자 속성 값을 합니다. 이 기능에 래핑됩니다를 `try` / `catch` 효과에 연결 된 컨트롤에 없는 경우 차단 된 `Control.Layer` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

#### <a name="responding-to-property-changes"></a>속성 변경에 응답

하나라는 `ShadowEffect` 속성 값 변경 런타임에 적용 해야 변경 내용을 표시 하 여 응답을 연결 합니다. 재정의 된 버전을 `OnElementPropertyChanged` 플랫폼별 결과 클래스에서 메서드를 다음 코드 예제에서 설명한 것 처럼 바인딩 가능 속성 변경 내용에 응답할 수 있는 곳은:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

합니다 `OnElementPropertyChanged` radius, 색 또는 그림자 오프셋 메서드를 업데이트 하는 적절 한 제공 `ShadowEffect` 연결 된 속성 값이 변경 합니다. 변경 되는 속성에 대 한 검사를 항상 수 있어야를 따라이 재정의 여러 번 호출할 수 있습니다.

### <a name="android-project"></a>Android 프로젝트

다음 코드 예제는 `LabelShadowEffect` Android 프로젝트에 대 한 구현 합니다.

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

`OnAttached` 메서드를 사용 하 여 연결 된 속성 값을 검색 하는 메서드를 호출 합니다 `ShadowEffect` getter를 호출 하는 메서드를 호출 하 고는 [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) 속성 값을 사용 하 여 그림자를 만드는 방법. 이 기능에 래핑됩니다를 `try` / `catch` 효과에 연결 된 컨트롤에 없는 경우 차단 된 `Control.Layer` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

#### <a name="responding-to-property-changes"></a>속성 변경에 응답

하나라는 `ShadowEffect` 속성 값 변경 런타임에 적용 해야 변경 내용을 표시 하 여 응답을 연결 합니다. 재정의 된 버전을 `OnElementPropertyChanged` 플랫폼별 결과 클래스에서 메서드를 다음 코드 예제에서 설명한 것 처럼 바인딩 가능 속성 변경 내용에 응답할 수 있는 곳은:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

합니다 `OnElementPropertyChanged` radius, 색 또는 그림자 오프셋 메서드를 업데이트 하는 적절 한 제공 `ShadowEffect` 연결 된 속성 값이 변경 합니다. 변경 되는 속성에 대 한 검사를 항상 수 있어야를 따라이 재정의 여러 번 호출할 수 있습니다.

### <a name="universal-windows-platform-project"></a>유니버설 Windows 플랫폼 프로젝트

다음 코드 예제는 `LabelShadowEffect` 유니버설 Windows 플랫폼 (UWP) 프로젝트에 대 한 구현:

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

유니버설 Windows 플랫폼에 그림자 효과 제공 하지 않으므로 `LabelShadowEffect` 구현은 두 가지 플랫폼에서 두 번째 오프셋을 추가 하 여 시뮬레이션 [ `Label` ](xref:Xamarin.Forms.Label) 주 뒤 `Label`합니다. 합니다 `OnAttached` 메서드를 만듭니다 `Label` 일부 레이아웃 속성을 설정 하 고는 `Label`합니다. 그런 다음 사용 하 여 연결 된 속성 값을 검색 하는 메서드를 호출 합니다 `ShadowEffect` getter를 설정 하 여 그림자를 생성 하 고는 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), 및 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) 색상 및 위치를 제어 하는 속성을 `Label`입니다. `shadowLabel` 그런 다음 삽입은 주 뒤 오프셋 `Label`합니다. 이 기능에 래핑됩니다를 `try` / `catch` 효과에 연결 된 컨트롤에 없는 경우 차단 된 `Control.Layer` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

#### <a name="responding-to-property-changes"></a>속성 변경에 응답

하나라는 `ShadowEffect` 속성 값 변경 런타임에 적용 해야 변경 내용을 표시 하 여 응답을 연결 합니다. 재정의 된 버전을 `OnElementPropertyChanged` 플랫폼별 결과 클래스에서 메서드를 다음 코드 예제에서 설명한 것 처럼 바인딩 가능 속성 변경 내용에 응답할 수 있는 곳은:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

합니다 `OnElementPropertyChanged` 색 또는 그림자 오프셋 메서드를 업데이트 하는 적절 한 제공 `ShadowEffect` 연결 된 속성 값이 변경 합니다. 변경 되는 속성에 대 한 검사를 항상 수 있어야를 따라이 재정의 여러 번 호출할 수 있습니다.

## <a name="summary"></a>요약

이 문서에 연결 된를 적용 하 고 런타임에 매개 변수를 변경 하려면 매개 변수를 전달 하는 속성을 사용 하 여 보여 줍니다. 연결 된 속성은 런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 정의에 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [그림자 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
