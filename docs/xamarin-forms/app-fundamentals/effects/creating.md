---
title: "효과 만들기"
description: "효과 컨트롤의 사용자 지정을 간소화합니다. 이 문서에 컨트롤에 포커스가 때 항목 컨트롤의 배경색을 변경 하는 효과 만드는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: 3a66ec9f935159e4854a12584a6c9f70ab805abb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="creating-an-effect"></a>효과 만들기

_효과 컨트롤의 사용자 지정을 간소화합니다. 이 문서에 컨트롤에 포커스가 때 항목 컨트롤의 배경색을 변경 하는 효과 만드는 방법을 보여 줍니다._

각 플랫폼 관련 프로젝트의 효과 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든는 `PlatformEffect` 클래스입니다.
1. 재정의 `OnAttached` 메서드 및 쓰기 논리 컨트롤을 사용자 지정할 수 있습니다.
1. 재정의 `OnDetached` 메서드 및 쓰기 논리가 필요한 경우 컨트롤 사용자 지정 정리 합니다.
1. 추가 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) 특성을 적용 클래스입니다. 이 특성 효과 같은 이름의 다른 효과로 인해 충돌을 방지에 대 한 회사 넓은 네임 스페이스를 설정 합니다. 이 특성만 적용할 수 있음을 한 번 프로젝트 마다 참고 합니다.
1. 추가 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) 특성을 적용 클래스입니다. 이 특성 효과 Xamarin.Forms에 의해 컨트롤에 적용 하기 전에 효과를 찾습니다 그룹 이름과 함께 사용 되는 고유 ID에 등록 합니다. 속성에는 두 개의 매개 변수-효과 및 효과 컨트롤에 적용 하기 전에 찾습니다는 하는 고유 문자열의 형식 이름을 사용 합니다.

그러면 해당 컨트롤에 연결 하 여 효과 사용할 수 있습니다.

> [!NOTE]
> 각 플랫폼 프로젝트의 효과 제공 하려면 선택적입니다. 하나이 등록 되지 않은 경우 효과 사용 하려고 하면 아무 작업도 수행 하는 null이 아닌 값을 반환 됩니다.

샘플 응용 프로그램을 보여 줍니다.는 `FocusEffect` 포커스를 획득 하는 경우 컨트롤의 배경색을 변경 하는 합니다. 다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](creating-images/focus-effect.png "포커스 효과 프로젝트 책임")

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤에 `HomePage` 를 사용자 지정는 `FocusEffect` 각 플랫폼별 프로젝트에 클래스입니다. 각 `FocusEffect` 클래스에서 파생 되는 `PlatformEffect` 각 플랫폼에 대 한 클래스입니다. 이 인해는 `Entry` 다음 스크린샷에 표시 된 대로 컨트롤에 포커스를 얻으면를 변경 하는 플랫폼 특정 배경 색으로 렌더링 되 고 제어 합니다.

![](creating-images/screenshots-1.png "각 플랫폼에는 영향을 집중")
![](creating-images/screenshots-2.png "집중 각 플랫폼에 미치는 영향")

## <a name="creating-the-effect-on-each-platform"></a>만드는 각 플랫폼에 대 한 영향

다음 섹션에서는 설명의 플랫폼별 구현을 `FocusEffect` 클래스입니다.

## <a name="ios-project"></a>iOS 프로젝트

다음 코드 예제는 `FocusEffect` iOS 프로젝트에 대 한 구현:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
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

`OnAttached` 메서드 설정은 `BackgroundColor` 와 밝은 자주색으로 컨트롤의 속성은 `UIColor.FromRGB` 메서드를 또한 필드에이 색으로 저장 하 고 합니다. 이 기능은 요소에 래핑되는 `try` / `catch` 차단 효과에 연결 된 컨트롤에 없는 경우에 `BackgroundColor` 속성. 구현 되는 `OnDetached` 메서드 정리 작업은 필요 없으므로 합니다.

`OnElementPropertyChanged` 재정의 Xamarin.Forms 컨트롤에 바인딩 가능한 속성 변경에 응답 합니다. 경우는 [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) 속성이 변경 된 `BackgroundColor` 컨트롤에 포커스가 있으면 흰색으로 변경 되는 컨트롤의 속성, 그렇지 않으면 밝은 보라색으로 변경 됩니다. 이 기능은 요소에 래핑되는 `try` / `catch` 차단 효과에 연결 된 컨트롤에 없는 경우에 `BackgroundColor` 속성.

## <a name="android-project"></a>Android 프로젝트

다음 코드 예제는 `FocusEffect` Android 프로젝트에 대 한 구현 합니다.

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` 메서드 호출의 `SetBackgroundColor` 를 컨트롤의 배경색을 설정 하는 방법은 녹색, 고도이 색이 필드에 저장 합니다. 이 기능은 요소에 래핑되는 `try` / `catch` 차단 효과에 연결 된 컨트롤에 없는 경우에 `SetBackgroundColor` 속성. 구현 되는 `OnDetached` 메서드 정리 작업은 필요 없으므로 합니다.

`OnElementPropertyChanged` 재정의 Xamarin.Forms 컨트롤에 바인딩 가능한 속성 변경에 응답 합니다. 경우는 [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) 속성 변경 내용을 컨트롤에 포커스가 있으면 컨트롤의 배경색을 흰색 변경 되, 그렇지 않으면 연한 녹색으로 변경 됩니다. 이 기능은 요소에 래핑되는 `try` / `catch` 차단 효과에 연결 된 컨트롤에 없는 경우에 `BackgroundColor` 속성.

## <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone 및 유니버설 Windows 플랫폼 프로젝트

다음 코드 예제는 `FocusEffect` Windows Phone 및 유니버설 Windows 플랫폼 (UWP) 프로젝트에 대 한 구현:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinRT;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.WinPhone81
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

`OnAttached` 메서드 설정은 `Background` 녹청 및 설정 하려면 컨트롤의 속성은 `BackgroundFocusBrush` 속성을 흰색입니다. 이 기능은 요소에 래핑되는 `try` / `catch` 경우 효과에 연결 된 컨트롤에 게 없는 경우 이러한 속성을 차단 합니다. 구현 되는 `OnDetached` 메서드 정리 작업은 필요 없으므로 합니다.

## <a name="consuming-the-effect"></a>효과 사용합니다.

Xamarin.Forms PCL 이식 가능한 클래스 라이브러리 () 또는 라이브러리 공유 프로젝트에서 효과 사용 하기 위한 프로세스는 다음과 같습니다.

1. 효과 의해 사용자 지정 될 컨트롤을 선언 합니다.
1. 컨트롤의에 추가 하 여 결과 컨트롤에 연결 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션입니다.

> [!NOTE]
> 효과 인스턴스 단일 컨트롤에만 연결할 수 있습니다. 따라서 두 컨트롤에서 사용 하려면 두 번 효과 해결 해야 합니다.

## <a name="consuming-the-effect-in-xaml"></a>XAML의 효과 사용합니다.

다음 XAML 코드 예제는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤에는 `FocusEffect` 연결:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` PCL에 클래스 XAML에서 효과 소비를 지원 하며 다음 코드 예제에 표시 됩니다.

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` 서브 클래스는 [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) 클래스를 래핑하는 일반적으로 플랫폼별으로 설정 하는 내부 효과 플랫폼 독립적인 효과 나타냅니다. `FocusEffect` 연결한 해상도 그룹 이름으로 구성 된 매개 변수를 전달 하는 기본 클래스 생성자를 호출 하는 클래스 (사용 하 여 지정 된 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) 효과 클래스의 특성)는 고유 ID는 및 사용 하 여 지정 된 된 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) 효과 클래스의 특성입니다. 따라서,는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 런타임에:의 새 인스턴스를 초기화는 `MyCompany.FocusEffect` 를 컨트롤의 추가 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션입니다.

효과 동작을 사용 하 여 컨트롤에 연결할 수도 있습니다 또는 연결 된 속성을 사용 하 여 합니다. 동작을 사용 하 여 효과 컨트롤에 연결 하는 방법에 대 한 자세한 내용은 참조 [다시 사용할 수 있는 EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md)합니다. 연결 된 속성을 사용 하 여 효과 컨트롤에 연결 하는 방법에 대 한 자세한 내용은 참조 [효과 매개 변수 전달](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md)합니다.

## <a name="consuming-the-effect-in-cnum"></a>C에서 효과 사용합니다.&num;

에 해당 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` 에 연결 된 `Entry` 를 컨트롤의 효과 추가 하 여 인스턴스 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션, 다음 코드 예제에서와 같이:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) 반환는 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) , 지정된 된 이름의 하는 해상도 그룹 이름의 연결 (사용 하 여 지정 된 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) 효과 클래스의 특성), 및를 사용 하 여 지정 된 고유 ID는 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) 효과 클래스의 특성입니다. 플랫폼의 효과 제공 하지 않는 경우는 `Effect.Resolve` 메서드는 반환 이외`null` 값입니다.

## <a name="summary"></a>요약

배경색을 변경 하는 효과를 만드는 방법을이 문서에 명시 된 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤이 포커스를 획득 하는 시기를 제어 합니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [배경 색 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [포커스 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
