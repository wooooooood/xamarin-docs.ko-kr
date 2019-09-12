---
title: 효과 만들기
description: 효과는 컨트롤의 사용자 지정을 간소화합니다. 이 문서에서는 컨트롤에 포커스가 있을 때 Entry 컨트롤의 배경 색을 변경하는 효과를 만드는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: c07848b808d023439c88117924e69c336984630b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771504"
---
# <a name="creating-an-effect"></a>효과 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)

_효과는 컨트롤 사용자 지정을 간소화합니다. 이 문서에서는 컨트롤에 포커스가 있을 때 Entry 컨트롤의 배경 색을 변경하는 효과를 만드는 방법을 보여줍니다._

각 플랫폼별 프로젝트에서 효과를 만들기 위한 프로세스는 다음과 같습니다.

1. `PlatformEffect` 클래스의 서브클래스를 만듭니다.
1. `OnAttached` 메서드를 재정의하고 컨트롤을 사용자 지정하기 위한 논리를 작성합니다.
1. `OnDetached` 메서드를 재정의하고 필요한 경우 컨트롤 사용자 지정을 정리하기 위한 논리를 작성합니다.
1. 효과 클래스에 [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 특성을 추가합니다. 이 특성은 효과에 대한 회사 전체 네임스페이스를 설정하여 동일한 이름의 다른 효과와의 충돌을 방지합니다. 이 특성은 프로젝트당 한 번만 적용할 수 있습니다.
1. 효과 클래스에 [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 특성을 추가합니다. 이 특성은 컨트롤에 적용하기 전에 효과를 찾기 위해 그룹 이름과 함께 Xamarin.Forms에서 사용되는 고유 ID로 효과를 등록합니다. 특성은 컨트롤에 적용하기 전에 효과를 찾는 데 사용되는 두 개의 매개 변수, 효과의 형식 이름 및 고유한 문자열을 사용합니다.

그런 다음, 적절한 컨트롤에 연결하여 효과를 사용할 수 있습니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 효과를 제공하는 것은 선택 사항입니다. 하나가 등록되지 않은 경우 효과를 사용하려는 시도는 아무 작업도 수행하지 않는 null이 아닌 값을 반환합니다.

샘플 애플리케이션은 포커스가 있는 경우 컨트롤의 배경색을 변경하는 `FocusEffect`를 보여줍니다. 다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](creating-images/focus-effect.png "포커스 효과 프로젝트 책임")

`HomePage`의 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤은 각 플랫폼별 프로젝트에서 `FocusEffect` 클래스로 사용자 지정됩니다. 각 `FocusEffect` 클래스는 각 플랫폼에 대한 `PlatformEffect` 클래스에서 파생됩니다. 그러면 다음 스크린샷과 같이 컨트롤에 포커스가 있는 경우 변경되는 `Entry` 컨트롤이 플랫폼별 배경색으로 렌더링됩니다.

![](creating-images/screenshots-1.png "각 플랫폼의 포커스 효과")
![](creating-images/screenshots-2.png "각 플랫폼의 포커스 효과")

## <a name="creating-the-effect-on-each-platform"></a>각 플랫폼의 효과 만들기

다음 섹션에서는 `FocusEffect` 클래스의 플랫폼별 구현을 설명합니다.

## <a name="ios-project"></a>iOS 프로젝트

다음 코드 예제는 iOS 프로젝트에 대한 `FocusEffect` 구현을 보여줍니다.

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(EffectsDemo.iOS.FocusEffect), nameof(EffectsDemo.iOS.FocusEffect))]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` 메서드는 `UIColor.FromRGB` 메서드를 사용하여 컨트롤의 `BackgroundColor` 속성을 연한 자주색으로 설정하고, 필드에서 이 색을 저장합니다. 이 기능은 효과가 연결된 컨트롤에 `BackgroundColor` 속성이 없는 경우 `try`/`catch` 블록에서 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

`OnElementPropertyChanged` 재정의는 Xamarin.Forms 컨트롤의 바인딩 가능한 속성 변경에 응답합니다. [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) 속성이 변경되는 경우 컨트롤의 `BackgroundColor` 속성은 컨트롤에 포커스가 있으면 흰색으로 변경되고, 그렇지 않으면 연한 자주색으로 변경됩니다. 이 기능은 효과가 연결된 컨트롤에 `BackgroundColor` 속성이 없는 경우 `try`/`catch` 블록에서 래핑됩니다.

## <a name="android-project"></a>Android 프로젝트

다음 코드 예제는 Android 프로젝트에 대한 `FocusEffect` 구현을 보여줍니다.

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.Droid.FocusEffect), nameof(EffectsDemo.Droid.FocusEffect))]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color originalBackgroundColor = new Android.Graphics.Color(0, 0, 0, 0);
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached()
        {
            try
            {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor(backgroundColor);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);
            try
            {
                if (args.PropertyName == "IsFocused")
                {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor)
                    {
                        Control.SetBackgroundColor(originalBackgroundColor);
                    }
                    else
                    {
                        Control.SetBackgroundColor(backgroundColor);
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` 메서드는 `SetBackgroundColor` 메서드를 호출하여 컨트롤의 배경색을 연한 녹색으로 설정하고, 필드에서 이 색을 저장합니다. 이 기능은 효과가 연결된 컨트롤에 `SetBackgroundColor` 속성이 없는 경우 `try`/`catch` 블록에서 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

`OnElementPropertyChanged` 재정의는 Xamarin.Forms 컨트롤의 바인딩 가능한 속성 변경에 응답합니다. [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) 속성이 변경되는 경우 컨트롤의 배경색은 컨트롤에 포커스가 있으면 흰색으로 변경되고, 그렇지 않으면 연한 녹색으로 변경됩니다. 이 기능은 효과가 연결된 컨트롤에 `BackgroundColor` 속성이 없는 경우 `try`/`catch` 블록에서 래핑됩니다.

## <a name="universal-windows-platform-projects"></a>유니버설 Windows 플랫폼 프로젝트

다음 코드 예제는 UWP(유니버설 Windows 플랫폼) 프로젝트에 대한 `FocusEffect` 구현을 보여줍니다.

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.UWP.FocusEffect), nameof(EffectsDemo.UWP.FocusEffect))]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

`OnAttached` 메서드는 컨트롤의 `Background` 속성을 녹청색으로 설정하고, `BackgroundFocusBrush` 속성을 흰색으로 설정합니다. 이 기능은 효과가 연결된 컨트롤에 이러한 속성이 없는 경우 `try`/`catch` 블록에서 래핑됩니다. 정리가 필요하지 않으므로 `OnDetached` 메서드에 의한 구현은 제공되지 않습니다.

## <a name="consuming-the-effect"></a>효과 사용

Xamarin.Forms .NET 표준 라이브러리 또는 공유 라이브러리 프로젝트에서 효과를 사용하기 위한 프로세스는 다음과 같습니다.

1. 효과에 의해 사용자 지정되는 컨트롤을 선언합니다.
1. 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가하여 컨트롤에 효과를 연결합니다.

> [!NOTE]
> 효과 인스턴스는 단일 컨트롤에만 연결할 수 있습니다. 따라서 두 개의 컨트롤에서 사용하려면 효과를 두 번 확인해야 합니다.

## <a name="consuming-the-effect-in-xaml"></a>XAML에서 효과 사용

다음 XAML 코드 예제는 `FocusEffect`가 연결된 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤을 보여줍니다.

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

.NET 표준 라이브러리의 `FocusEffect` 클래스는 XAML에서 효과 사용을 지원하며, 다음 코드 예제에 표시됩니다.

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ($"MyCompany.{nameof(FocusEffect)}")
    {
    }
}
```

`FocusEffect` 클래스는 [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 클래스를 서브클래스하며, 일반적으로 플랫폼에 따라 내부 효과를 래핑하는 플랫폼 독립적인 효과를 나타냅니다. `FocusEffect` 클래스는 기본 클래스 생성자를 호출하여 해상도 그룹 이름(효과 클래스의 [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 특성을 사용하여 지정됨)의 연결과 효과 클래스의 [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 특성을 사용하여 지정된 고유 ID로 이루어진 매개 변수를 전달합니다. 따라서 [`Entry`](xref:Xamarin.Forms.Entry)가 런타임 시 초기화되는 경우 `MyCompany.FocusEffect`의 새 인스턴스는 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가됩니다.

동작을 사용하거나 연결된 속성을 사용하여 효과를 컨트롤에 연결할 수도 있습니다. 동작을 사용하여 효과를 컨트롤에 연결에 대한 자세한 내용은 [재사용 가능한 EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md)를 참조하세요. 연결된 속성을 사용하여 효과를 컨트롤에 연결에 대한 자세한 내용은 [효과에 매개 변수 전달](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md)을 참조하세요.

## <a name="consuming-the-effect-in-cnum"></a>C&num;에서 효과 사용

C#의 해당하는 [`Entry`](xref:Xamarin.Forms.Entry)가 다음 코드 예제에 나와 있습니다.

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect`는 다음 코드 예제에서 설명한 것처럼 효과를 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가하여 `Entry` 인스턴스에 연결됩니다.

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ($"MyCompany.{nameof(FocusEffect)}"));
  ...
}
```

[`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String))는 지정된 이름에 대한 [`Effect`](xref:Xamarin.Forms.Effect)를 반환하며, 이는 해상도 그룹 이름(효과 클래스의 [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 특성을 사용하여 지정됨)의 연결과 효과 클래스의 [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 특성을 사용하여 지정된 고유 ID입니다. 플랫폼에서 효과를 제공하지 않으면 `Effect.Resolve` 메서드에서 `null`이 아닌 값을 반환합니다.

## <a name="summary"></a>요약

이 문서에서는 컨트롤에 포커스가 있을 때 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤의 배경색을 변경하는 효과를 만드는 방법을 설명했습니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [배경색 효과(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-backgroundcoloreffect)
- [포커스 효과(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)
