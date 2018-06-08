---
title: 연결 된 속성으로 효과 매개 변수 전달
description: 연결 된 속성은 런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 정의 데 사용할 수 있습니다. 이 문서에서는 연결 된 효과 하 고 런타임에 매개 변수를 변경 하려면 매개 변수를 전달 하는 속성을 사용 하 여 보여줍니다.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 2ad27289fb7a4d34b9a951c8132f0147577dfc55
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847919"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>연결 된 속성으로 효과 매개 변수 전달

_연결 된 속성은 런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 정의 데 사용할 수 있습니다. 이 문서에서는 연결 된 효과 하 고 런타임에 매개 변수를 변경 하려면 매개 변수를 전달 하는 속성을 사용 하 여 보여줍니다._

런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 만드는 프로세스는 다음과 같습니다.

1. 만들기는 `static` 효과를 전달 하도록 각 매개 변수에 대해 연결된 된 속성을 포함 하는 클래스입니다.
1. 연결된 된 추가 속성을 추가 또는 제거 하는 클래스에 연결 될 수 있는 컨트롤에 영향을 제어 하는 클래스에 추가 합니다. 이 연결 속성 레지스터 확인는 `propertyChanged` 속성의 값이 변경 될 때 실행 될 대리자입니다.
1. 만들 `static` getter 및 setter를 각각에 대해 연결 된 속성입니다.
1. 논리를 구현는 `propertyChanged` 대리자를 추가 하 고 효과 제거 합니다.
1. 구현 내부에 중첩된 클래스는 `static` 서브클래싱하 효과 이름을 딴 클래스는 `RoutingEffect` 클래스입니다. 생성자에 대 한 해상도 그룹 이름 및 각 플랫폼 특정 효과 클래스에 지정 된 고유 ID의 연결에 전달 하 여 기본 클래스 생성자를 호출 합니다.

매개 변수가 해당 컨트롤에 연결 된 속성 및 속성 값을 추가 하 여 효과에 전달할 수 있습니다. 또한 매개 변수는 새 연결 된 속성 값을 지정 하 여 런타임 시 변경할 수 있습니다.

> [!NOTE]
> 연결된 된 속성은 특수 한 유형의 바인딩 가능한 속성을 다른 개체에 연결 되 고 XAML에서 인식할 수 있지만 하나의 클래스에서 속성 이름과 마침표로 구분 되는 클래스를 포함 하는 특성으로 정의 합니다. 자세한 내용은 참조 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md)합니다.

샘플 응용 프로그램을 보여 줍니다.는 `ShadowEffect` 하 여 표시 되는 텍스트에 그림자를 추가 하는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 제어 합니다. 또한 런타임 시 그림자의 색을 변경할 수 있습니다. 다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](attached-properties-images/shadow-effect.png "그림자 효과 프로젝트 책임")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤에 `HomePage` 를 사용자 지정의 `LabelShadowEffect` 각 플랫폼별 프로젝트에 있습니다. 매개 변수는 각 전달 `LabelShadowEffect` 를 통해 연결 된 속성에는 `ShadowEffect` 클래스입니다. 각 `LabelShadowEffect` 클래스에서 파생 되는 `PlatformEffect` 각 플랫폼에 대 한 클래스입니다. 그림자 하 여 표시 되는 텍스트에 추가 되 고 그 결과 `Label` 다음 스크린샷에서 같이 제어:

![](attached-properties-images/screenshots.png "각 플랫폼에 그림자 효과")

## <a name="creating-effect-parameters"></a>효과 매개 변수 만들기

A `static` 다음 코드 예제에서와 같이 효과 매개 변수를 나타내는 클래스를 만들 수 해야 합니다.

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

`ShadowEffect` 포함 하는 다섯 개의 연결 된 속성 `static` getter 및 setter를 각각에 대해 연결 된 속성입니다. 각 플랫폼 특정에 전달할 매개 변수를 나타내는 이러한 속성 중 4 개 `LabelShadowEffect`합니다. `ShadowEffect` 클래스도 정의 `HasShadow` 연결 된 속성 추가 또는 제거 하 고 효과를 컨트롤을 제어 하는 데 사용 되는 하는 `ShadowEffect` 클래스에 연결 되어 있습니다. 이 연결 속성 레지스터는 `OnHasShadowChanged` 속성의 값이 변경 될 때 실행 되는 메서드. 이 메서드를 추가 하거나 제거의 값에 따라 효과 `HasShadow` 연결 된 속성입니다.

중첩 된 `LabelShadowEffect` 서브클래싱하 클래스는 [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) 클래스를 지 원하는 효과 추가 및 제거 합니다. `RoutingEffect` 클래스를 래핑하는 일반적으로 플랫폼별으로 설정 하는 내부 효과 플랫폼 독립적인 효과 나타냅니다. 플랫폼별 효과 대 한 형식 정보에 액세스할 수 없습니다 컴파일 타임 하므로 효과 제거 프로세스를 보다 간단 합니다. `LabelShadowEffect` 생성자 해상도 그룹 이름 및 각 플랫폼 특정 효과 클래스에 지정 된 고유 ID의 연결으로 구성 된 매개 변수를 전달 하 고 기본 클래스 생성자가 호출 됩니다. 이렇게 하면 효과 추가 및 제거에는 `OnHasShadowChanged` 다음과 같이 메서드:

- **또한 영향** –의 새 인스턴스는 `LabelShadowEffect` 컨트롤의에 추가 됩니다 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션입니다. 이 사용 하 여 대체는 [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) 효과 추가 하려면 메서드.
- **영향을 제거** –의 첫 번째 인스턴스는 `LabelShadowEffect` 컨트롤의 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션 검색 되 고 제거 합니다.

## <a name="consuming-the-effect"></a>효과 사용합니다.

각 플랫폼 특정 `LabelShadowEffect` 연결 된 속성을 추가 하 여 사용할 수는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 다음 XAML 코드 예제에서와 같이 제어:

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

에 해당 하는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) C#에서 다음 코드 예제에 표시 됩니다.

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

설정의 `ShadowEffect.HasShadow` 연결 된 속성을 `true` 실행는 `ShadowEffect.OnHasShadowChanged` 메서드를 추가 하거나 제거 하는 `LabelShadowEffect` 에 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 제어 합니다. 두 코드 예제에서는 `ShadowEffect.Color` 플랫폼 특정 색 값을 제공 하는 연결 된 속성입니다. 자세한 내용은 참조 [장치 클래스](~/xamarin-forms/platform/device.md)합니다.

또한 한 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 그림자 색을 런타임에 변경할 수 있습니다. 경우는 `Button` 를 클릭 하면 그림자 색을 설정 하 여 다음 코드 변경의 `ShadowEffect.Color` 연결 된 속성:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>스타일으로 효과 사용합니다.

효과 컨트롤에 연결 된 속성을 추가 하 여 사용할 수 있는 스타일으로도 사용할 수 있습니다. 다음 XAML 코드 예제는 *명시적* 에 적용할 수 있는 그림자 효과 대 한 스타일 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤:

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

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 에 적용할 수는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 설정 하 여 해당 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성을는 `Style` 는 를사용하여인스턴스`StaticResource`태그 확장, 다음 코드 예제에서와 같이:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

스타일에 대 한 자세한 내용은 참조 [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

## <a name="creating-the-effect-on-each-platform"></a>만드는 각 플랫폼에 대 한 영향

다음 섹션에서는 설명의 플랫폼별 구현을 `LabelShadowEffect` 클래스입니다.

### <a name="ios-project"></a>iOS 프로젝트

다음 코드 예제는 `LabelShadowEffect` iOS 프로젝트에 대 한 구현:

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

`OnAttached` 메서드를 사용 하 여 연결 된 속성 값을 검색 하는 메서드 호출에서 `ShadowEffect` getter, 및 설정 `Control.Layer` 속성 그림자를 만드는 데 속성 값을 합니다. 이 기능은 요소에 래핑되는 `try` / `catch` 차단 효과에 연결 된 컨트롤에 없는 경우에 `Control.Layer` 속성. 구현 되는 `OnDetached` 메서드 정리 작업은 필요 없으므로 합니다.

#### <a name="responding-to-property-changes"></a>속성 변경 내용에 응답

경우는 `ShadowEffect` 적용 해야 변경 내용을 표시 하 여 응답 런타임에 속성 값이 변경 연결 합니다. 재정의 된 버전의 `OnElementPropertyChanged` 다음 코드 예제에서와 같이 플랫폼 특정 효과 클래스에서 메서드는 바인딩 가능한 속성 변경에 대응할 수 있는 곳:

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

`OnElementPropertyChanged` radius, 색 또는 그림자 오프셋 메서드를 업데이트 하는 적절 한 제공 `ShadowEffect` 연결 된 속성 값이 변경 합니다. 변경 된 속성에 대 한 확인을 항상을 설정 해야으로이 재정의 여러 번 호출할 수 있습니다.

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

`OnAttached` 메서드를 사용 하 여 연결 된 속성 값을 검색 하는 메서드를 호출는 `ShadowEffect` getter를 호출 하는 메서드를 호출 하 고는 [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) 속성 값을 사용 하 여 그림자를 만드는 방법. 이 기능은 요소에 래핑되는 `try` / `catch` 차단 효과에 연결 된 컨트롤에 없는 경우에 `Control.Layer` 속성. 구현 되는 `OnDetached` 메서드 정리 작업은 필요 없으므로 합니다.

#### <a name="responding-to-property-changes"></a>속성 변경 내용에 응답

경우는 `ShadowEffect` 적용 해야 변경 내용을 표시 하 여 응답 런타임에 속성 값이 변경 연결 합니다. 재정의 된 버전의 `OnElementPropertyChanged` 다음 코드 예제에서와 같이 플랫폼 특정 효과 클래스에서 메서드는 바인딩 가능한 속성 변경에 대응할 수 있는 곳:

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

`OnElementPropertyChanged` radius, 색 또는 그림자 오프셋 메서드를 업데이트 하는 적절 한 제공 `ShadowEffect` 연결 된 속성 값이 변경 합니다. 변경 된 속성에 대 한 확인을 항상을 설정 해야으로이 재정의 여러 번 호출할 수 있습니다.

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

유니버설 Windows 플랫폼에 그림자 효과 제공 하지 않습니다는 `LabelShadowEffect` 두 플랫폼 모두에서 구현 된 두 번째 오프셋을 추가 하 여 시뮬레이션 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 주 뒤 `Label`합니다. `OnAttached` 메서드는 새 만듭니다 `Label` 일부 레이아웃 속성을 설정 하 고는 `Label`합니다. 그런 다음 사용 하 여 연결 된 속성 값을 검색 하는 메서드를 호출는 `ShadowEffect` getter를 설정 하 여 그림자를 생성 하 고는 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), 및 [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) 색과의 위치를 제어 하는 속성은 `Label`합니다. `shadowLabel` 다음 삽입은 주 뒤 오프셋 `Label`합니다. 이 기능은 요소에 래핑되는 `try` / `catch` 차단 효과에 연결 된 컨트롤에 없는 경우에 `Control.Layer` 속성. 구현 되는 `OnDetached` 메서드 정리 작업은 필요 없으므로 합니다.

#### <a name="responding-to-property-changes"></a>속성 변경 내용에 응답

경우는 `ShadowEffect` 적용 해야 변경 내용을 표시 하 여 응답 런타임에 속성 값이 변경 연결 합니다. 재정의 된 버전의 `OnElementPropertyChanged` 다음 코드 예제에서와 같이 플랫폼 특정 효과 클래스에서 메서드는 바인딩 가능한 속성 변경에 대응할 수 있는 곳:

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

`OnElementPropertyChanged` 색 또는 그림자 오프셋 메서드를 업데이트 하는 적절 한 제공 `ShadowEffect` 연결 된 속성 값이 변경 합니다. 변경 된 속성에 대 한 확인을 항상을 설정 해야으로이 재정의 여러 번 호출할 수 있습니다.

## <a name="summary"></a>요약

이 문서에는 연결 된 효과 하 고 런타임에 매개 변수를 변경 하려면 매개 변수를 전달 하는 속성을 사용 하 여 제시 합니다. 연결 된 속성은 런타임 속성 변경 내용에 응답 하는 효과 매개 변수를 정의 데 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [그림자 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
