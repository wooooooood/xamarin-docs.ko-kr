---
title: 연결된 속성으로 효과 매개 변수 전달
description: 연결된 속성은 런타임 속성 변경 내용에 응답하는 효과 매개 변수를 정의하는 데 사용할 수 있습니다. 이 문서에서는 연결된 속성을 사용하여 효과에 매개 변수를 전달하고 런타임 시 매개 변수를 변경하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: cc33bc7eef68e5d6cb0a1b717da5e9d9f49338b2
ms.sourcegitcommit: 05ba8ffb8b34ec881b89e442323f3edd8de18f2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/18/2020
ms.locfileid: "83546026"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>연결된 속성으로 효과 매개 변수 전달

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)

_연결된 속성은 런타임 속성 변경 내용에 응답하는 효과 매개 변수를 정의하는 데 사용할 수 있습니다. 이 문서에서는 연결된 속성을 사용하여 효과에 매개 변수를 전달하고 런타임 시 매개 변수를 변경하는 방법을 설명합니다._

런타임 속성 변경 내용에 응답하는 효과 매개 변수를 만드는 프로세스는 다음과 같습니다.

1. 효과에 전달할 각 매개 변수에 대한 연결된 속성을 포함하는 `static` 클래스를 만듭니다.
1. 클래스가 연결될 컨트롤에 효과의 추가 또는 제거를 제어하는 데 사용할 클래스에 추가적인 연결된 속성을 추가합니다. 이 연결된 속성이 속성 값이 변경될 때 실행할 `propertyChanged` 대리자를 등록하는지 확인하십시오.
1. 각 연결된 속성에 대해 `static` getter 및 setter를 만듭니다.
1. `propertyChanged` 대리자에서 논리를 구현하여 효과를 추가 및 제거합니다.
1. `RoutingEffect` 클래스를 서브클래스하는 효과의 이름을 따서 명명된 `static` 클래스 내에 중첩된 클래스를 구현합니다. 생성자의 경우 기본 클래스 생성자를 호출하여 해상도 그룹 이름의 연결과 각 플랫폼별 효과 클래스에 지정된 고유 ID를 전달합니다.

그런 다음, 적절한 컨트롤에 연결된 속성 및 속성 값을 추가하면 매개 변수를 효과로 전달할 수 있습니다. 또한 새 연결된 속성 값을 지정하여 런타임 시 매개 변수를 변경할 수 있습니다.

> [!NOTE]
> 연결된 속성은 한 클래스에서 정의되지만 다른 개체에 연결되어 있는 특수한 형식의 바인딩 가능 속성이며, XAML에서 마침표로 구분된 클래스 및 속성 이름을 포함하는 특성으로 인식할 수 있습니다. 자세한 내용은 [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)을 참조하세요.

샘플 애플리케이션은 [`Label`](xref:Xamarin.Forms.Label) 컨트롤에서 표시되는 텍스트에 그림자를 추가하는 `ShadowEffect`를 설명합니다. 또한 런타임 시 그림자의 색을 변경할 수 있습니다. 다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](attached-properties-images/shadow-effect.png "Shadow Effect Project Responsibilities")

`HomePage`의 [`Label`](xref:Xamarin.Forms.Label) 컨트롤은 각 플랫폼별 프로젝트에서 `LabelShadowEffect`로 사용자 지정됩니다. 매개 변수는 `ShadowEffect` 클래스의 연결된 속성을 통해 각 `LabelShadowEffect`에 전달됩니다. 각 `LabelShadowEffect` 클래스는 각 플랫폼에 대한 `PlatformEffect` 클래스에서 파생됩니다. 그러면 다음 스크린샷과 같이 `Label` 컨트롤에 표시되는 텍스트에 그림자가 추가됩니다.

![](attached-properties-images/screenshots.png "Shadow Effect on each Platform")

## <a name="creating-effect-parameters"></a>효과 매개 변수 만들기

다음 코드 예제에서 설명한 것처럼 효과 매개 변수를 표현하도록 `static` 클래스를 만들어야 합니다.

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

`ShadowEffect`에는 각 연결된 속성에 대한 `static` getter 및 setter와 함께 5개의 연결된 속성이 포함되어 있습니다. 이러한 네 가지 속성은 각 플랫폼별 `LabelShadowEffect`에 전달할 매개 변수를 나타냅니다. 또한 `ShadowEffect` 클래스는 `ShadowEffect` 클래스가 연결될 컨트롤에 효과의 추가 또는 제거를 제어하는 데 사용되는 `HasShadow` 연결된 속성을 정의합니다. 이 연결된 속성은 속성 값이 변경될 때 실행할 `OnHasShadowChanged` 메서드를 등록합니다. 이 메서드는 `HasShadow` 연결된 속성의 값을 기반으로 효과를 추가 또는 제거합니다.

[`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 클래스를 서브클래스하는 중첩된 `LabelShadowEffect` 클래스는 효과 추가 및 제거를 지원합니다. `RoutingEffect` 클래스는 일반적으로 플랫폼에 따라 내부 효과를 래핑하는 플랫폼 독립적인 효과를 나타냅니다. 이는 플랫폼별 효과에 대한 형식 정보에 컴파일 시간 액세스가 없으므로 효과 제거 프로세스를 간소화합니다. `LabelShadowEffect` 생성자는 기본 클래스 생성자를 호출하여 해상도 그룹 이름의 연결과 각 플랫폼별 효과 클래스에 지정된 고유 ID로 이루어진 매개 변수를 전달합니다. 이를 통해 다음과 같이 `OnHasShadowChanged` 메서드에서 효과 추가 및 제거를 수행할 수 있습니다.

- **효과 추가** – `LabelShadowEffect`의 새 인스턴스가 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가됩니다. 이는 효과를 추가하는 [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) 메서드 사용을 대체합니다.
- **효과 제거** – 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에서 `LabelShadowEffect`의 첫 번째 인스턴스가 검색되어 제거됩니다.

## <a name="consuming-the-effect"></a>효과 사용

다음과 같은 XAML 코드 예제에 설명된 대로 연결된 속성을 [`Label`](xref:Xamarin.Forms.Label) 컨트롤에 추가하면 각 플랫폼별 `LabelShadowEffect`를 사용할 수 있습니다.

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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

`ShadowEffect.HasShadow` 연결된 속성을 `true`로 설정하면 `LabelShadowEffect`를 [`Label`](xref:Xamarin.Forms.Label) 컨트롤에 추가하거나 제거하는 `ShadowEffect.OnHasShadowChanged` 메서드를 실행합니다. 두 코드 예제에서 `ShadowEffect.Color` 연결된 속성은 플랫폼별 색 값을 제공합니다. 자세한 내용은 [디바이스 클래스](~/xamarin-forms/platform/device.md)를 참조하세요.

또한 [`Button`](xref:Xamarin.Forms.Button)으로 런타임 시 그림자 색을 변경할 수 있습니다. `ShadowEffect.Color` 연결된 속성을 설정하면 `Button`을 클릭할 때 다음 코드가 그림자 색을 변경합니다.

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>스타일을 통한 효과 사용

컨트롤에 연결된 속성을 추가하여 사용할 수 있는 효과는 스타일로도 사용할 수 있습니다. 다음 XAML 코드 예제는 [`Label`](xref:Xamarin.Forms.Label) 컨트롤에 적용할 수 있는 그림자 효과에 대한 *명시적* 스타일을 보여줍니다.

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

다음 코드 예제에 설명된 대로 `StaticResource` 태그 확장을 사용하여 해당 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성을 `Style` 인스턴스로 설정하면 [`Style`](xref:Xamarin.Forms.Style)을 [`Label`](xref:Xamarin.Forms.Label)에 적용할 수 있습니다.

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

스타일에 대한 자세한 내용은 [스타일](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.

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
            Control.Layer.ShadowRadius = (nfloat)ShadowEffect.GetRadius (Element);
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

`OnAttached` 메서드는 `ShadowEffect` getter를 사용하여 연결된 속성 값을 검색하고 그림자를 만드는 속성 값으로 `Control.Layer` 속성을 설정하는 메서드를 호출합니다. 이 기능은 효과가 연결된 컨트롤에 `Control.Layer` 속성이 없는 경우 `try`/`catch` 블록에 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

#### <a name="responding-to-property-changes"></a>속성 변경 내용에 응답

`ShadowEffect` 연결된 속성 값이 런타임 시 변경되는 경우 효과는 변경 내용을 표시하여 응답해야 합니다. 플랫폼별 효과 클래스에서 `OnElementPropertyChanged` 메서드의 재정의된 버전은 다음 코드 예제에 설명된 대로 바인딩 가능한 속성 변경 내용에 응답하는 위치입니다.

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

`OnElementPropertyChanged` 메서드는 적절한 `ShadowEffect` 연결된 속성 값이 변경된 경우 그림자의 반경, 색 또는 오프셋을 업데이트합니다. 이 재정의는 여러 번 호출될 수 있으므로 변경된 속성에 대한 검사는 항상 수행되어야 합니다.

### <a name="android-project"></a>Android 프로젝트

다음 코드 예제는 Android 프로젝트에 대한 `LabelShadowEffect` 구현을 보여줍니다.

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

`OnAttached` 메서드는 `ShadowEffect` getter를 사용하여 연결된 속성 값을 검색하는 메서드를 호출하며, 속성 값을 사용하여 그림자를 만드는 [`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*) 메서드를 호출하는 메서드를 호출합니다. 이 기능은 효과가 연결된 컨트롤에 `Control.Layer` 속성이 없는 경우 `try`/`catch` 블록에 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

#### <a name="responding-to-property-changes"></a>속성 변경 내용에 응답

`ShadowEffect` 연결된 속성 값이 런타임 시 변경되는 경우 효과는 변경 내용을 표시하여 응답해야 합니다. 플랫폼별 효과 클래스에서 `OnElementPropertyChanged` 메서드의 재정의된 버전은 다음 코드 예제에 설명된 대로 바인딩 가능한 속성 변경 내용에 응답하는 위치입니다.

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

`OnElementPropertyChanged` 메서드는 적절한 `ShadowEffect` 연결된 속성 값이 변경된 경우 그림자의 반경, 색 또는 오프셋을 업데이트합니다. 이 재정의는 여러 번 호출될 수 있으므로 변경된 속성에 대한 검사는 항상 수행되어야 합니다.

### <a name="universal-windows-platform-project"></a>유니버설 Windows 플랫폼 프로젝트

다음 코드 예제는 UWP(유니버설 Windows 플랫폼) 프로젝트에 대한 `LabelShadowEffect` 구현을 보여줍니다.

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

유니버설 Windows 플랫폼은 그림자 효과를 제공하지 않으므로 두 가지 플랫폼의 `LabelShadowEffect` 구현은 기본 `Label` 뒤에 두 번째 오프셋 [`Label`](xref:Xamarin.Forms.Label)을 추가하여 시뮬레이션합니다. `OnAttached` 메서드는 새 `Label`을 만들고 `Label`에서 일부 레이아웃 속성을 설정합니다. 그런 다음, `ShadowEffect` getter를 사용하여 연결된 속성 값을 검색하는 메서드를 호출하고, `Label`의 색 및 위치를 제어하는 [`TextColor`](xref:Xamarin.Forms.Label.TextColor), [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성을 설정하여 그림자를 만듭니다. 그런 다음, `shadowLabel`이 기본 `Label` 뒤에 삽입됩니다. 이 기능은 효과가 연결된 컨트롤에 `Control.Layer` 속성이 없는 경우 `try`/`catch` 블록에 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

#### <a name="responding-to-property-changes"></a>속성 변경 내용에 응답

`ShadowEffect` 연결된 속성 값이 런타임 시 변경되는 경우 효과는 변경 내용을 표시하여 응답해야 합니다. 플랫폼별 효과 클래스에서 `OnElementPropertyChanged` 메서드의 재정의된 버전은 다음 코드 예제에 설명된 대로 바인딩 가능한 속성 변경 내용에 응답하는 위치입니다.

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

`OnElementPropertyChanged` 메서드는 적절한 `ShadowEffect` 연결된 속성 값이 변경된 경우 그림자의 색 또는 오프셋을 업데이트합니다. 이 재정의는 여러 번 호출될 수 있으므로 변경된 속성에 대한 검사는 항상 수행되어야 합니다.

## <a name="summary"></a>요약

이 문서에서는 연결된 속성을 사용하여 효과에 매개 변수를 전달하고 런타임 시 매개 변수를 변경하는 방법을 설명했습니다. 연결된 속성은 런타임 속성 변경 내용에 응답하는 효과 매개 변수를 정의하는 데 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [그림자 효과(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)
