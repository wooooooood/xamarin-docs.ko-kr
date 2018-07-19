---
title: 효과 만들기
description: 효과 컨트롤의 사용자 지정을 간소화합니다. 이 문서에서는 컨트롤이 포커스를 얻으면 항목 컨트롤의 배경색을 변경 하는 효과 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: b29d83999724a35293882f7b9efc0158171c4fd2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998162"
---
# <a name="creating-an-effect"></a>효과 만들기

_효과 컨트롤의 사용자 지정을 간소화합니다. 이 문서에서는 컨트롤이 포커스를 얻으면 항목 컨트롤의 배경색을 변경 하는 효과 만드는 방법을 보여 줍니다._

각 플랫폼 특정 프로젝트에서 효과 만들기 위한 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든를 `PlatformEffect` 클래스입니다.
1. 재정의 `OnAttached` 메서드와 쓰기 논리 컨트롤을 사용자 지정할 수 있습니다.
1. 재정의 `OnDetached` 메서드와 쓰기 논리가 필요한 경우 컨트롤 사용자 지정 정리 합니다.
1. 추가 된 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 특성을 적용 클래스입니다. 이 특성에 대 한 효과 동일한 이름의 다른 효과 사용 하 여 충돌을 방지 회사 와이드 네임 스페이스를 설정 합니다. 이 특성만 적용할 수 있음을 번 프로젝트당 참고 합니다.
1. 추가 된 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) 특성을 적용 클래스입니다. 이 특성에서 Xamarin.Forms 컨트롤에 적용 하기 전에 결과 찾으려는 그룹 이름과 함께 사용 되는 고유 ID를 사용 하 여 효과 등록 합니다. 특성 매개 변수 두 개 – 효과 컨트롤에 적용 하기 전에 결과 찾는 데 사용할 됩니다 하는 고유 문자열을 형식 이름입니다.

그런 다음 해당 컨트롤에 연결 하 여 영향을 사용할 수 있습니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 효과 제공 하는 선택 사항입니다. 하나이 등록 되지 않은 경우 효과 사용 하려고 아무 작업도 수행 하지는 null이 아닌 값을 반환 합니다.

샘플 응용 프로그램을 보여 줍니다는 `FocusEffect` 포커스를 획득 하는 경우 컨트롤의 배경색을 변경 하는 합니다. 다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](creating-images/focus-effect.png "포커스 효과 프로젝트 책임")

[ `Entry` ](xref:Xamarin.Forms.Entry) 대 한 control 권한 합니다 `HomePage` 를 사용자 지정을 `FocusEffect` 각 플랫폼별 프로젝트에 클래스. 각 `FocusEffect` 클래스에서 파생 되는 `PlatformEffect` 각 플랫폼에 대 한 클래스입니다. 이 인해는 `Entry` 다음 스크린샷과에서 같이 컨트롤에 포커스를 얻으면를 변경 하는 플랫폼별 배경 색을 사용 하 여 렌더링 되 고 제어 합니다.

![](creating-images/screenshots-1.png "각 플랫폼에는 영향을 집중")
![](creating-images/screenshots-2.png "집중 각 플랫폼에 미치는 영향")

## <a name="creating-the-effect-on-each-platform"></a>각 플랫폼에 대 한 효과 만들기

다음 섹션에서는 플랫폼 전용 구현에 설명 된 `FocusEffect` 클래스입니다.

## <a name="ios-project"></a>iOS 프로젝트

다음 코드 예제는 `FocusEffect` iOS 프로젝트에 대 한 구현 합니다.

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

`OnAttached` 메서드 집합을 `BackgroundColor` 연한 자주 사용 하 여 컨트롤의 속성을 `UIColor.FromRGB` 메서드를도이 색이 필드에 저장 합니다. 이 기능에 래핑됩니다를 `try` / `catch` 결과에 연결 된 컨트롤에 없는 경우 블록을 `BackgroundColor` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

`OnElementPropertyChanged` 재정의 Xamarin.Forms 컨트롤에 바인딩 가능한 속성 변경에 응답 합니다. 경우는 [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) 속성 변경의 `BackgroundColor` 컨트롤에 포커스가 있으면 컨트롤의 속성을 흰색으로 변경 되, 그렇지 않으면 밝은 보라색으로 변경 됩니다. 이 기능에 래핑됩니다를 `try` / `catch` 결과에 연결 된 컨트롤에 없는 경우 블록을 `BackgroundColor` 속성입니다.

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

합니다 `OnAttached` 메서드 호출을 `SetBackgroundColor` 명확 하 게 컨트롤의 배경색을 설정 하는 방법, 녹색 및도이 색이 필드에 저장 합니다. 이 기능에 래핑됩니다를 `try` / `catch` 결과에 연결 된 컨트롤에 없는 경우 블록을 `SetBackgroundColor` 속성입니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

`OnElementPropertyChanged` 재정의 Xamarin.Forms 컨트롤에 바인딩 가능한 속성 변경에 응답 합니다. 경우는 [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) 속성 변경 내용을 컨트롤에 포커스가 있으면 컨트롤의 배경색을 흰색으로 변경 되, 그렇지 않으면 연한 녹색으로 변경 됩니다. 이 기능에 래핑됩니다를 `try` / `catch` 결과에 연결 된 컨트롤에 없는 경우 블록을 `BackgroundColor` 속성입니다.

## <a name="universal-windows-platform-projects"></a>유니버설 Windows 플랫폼 프로젝트

다음 코드 예제는 `FocusEffect` 유니버설 Windows 플랫폼 (UWP) 프로젝트에 대 한 구현:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
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

`OnAttached` 메서드 집합을 `Background` 녹청 집합과 컨트롤의 속성을 `BackgroundFocusBrush` 속성을 흰색입니다. 이 기능은 래핑됩니다를 `try` / `catch` 경우 효과에 연결 된 컨트롤이 이러한 속성을 차단 합니다. 구현 되는 `OnDetached` 메서드 정리가 필요 없으므로 합니다.

## <a name="consuming-the-effect"></a>효과 사용합니다.

Xamarin.Forms.NET Standard 라이브러리 또는 라이브러리 공유 프로젝트에서 효과 사용 하기 위한 프로세스는 다음과 같습니다.

1. 효과 의해 사용자 지정 해야 하는 컨트롤을 선언 합니다.
1. 컨트롤에 추가 하 여 결과 컨트롤에 연결 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션입니다.

> [!NOTE]
> 효과 인스턴스, 단일 컨트롤에만 연결할 수 있습니다. 따라서 효과 두 가지 컨트롤에서 사용 하려면 두 번 확인 되어야 합니다.

## <a name="consuming-the-effect-in-xaml"></a>XAML에서 효과 사용합니다.

다음 XAML 코드 예제와 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤에는 `FocusEffect` 연결:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` .NET 표준 라이브러리의 클래스를 XAML 효과 사용을 지원 하며 다음 코드 예제에 표시 됩니다.

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

합니다 `FocusEffect` 서브 클래스를 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) 클래스 내부 효과 일반적으로 플랫폼별을 래핑하는 플랫폼 독립적인 효과 나타냅니다. 합니다 `FocusEffect` 해상도 그룹 이름의 연결으로 구성 된 매개 변수 전달, 기본 클래스 생성자를 호출 하는 클래스 (사용 하 여 지정 합니다 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 효과 클래스의 특성), 및 고유 ID 사용 하 여 지정 된 된 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) 효과 클래스의 특성입니다. 따라서 경우는 [ `Entry` ](xref:Xamarin.Forms.Entry) 의 새 인스턴스를 런타임에 초기화 되는 `MyCompany.FocusEffect` 컨트롤에 추가 됩니다 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션입니다.

결과 동작을 사용 하 여 컨트롤에 연결할 수 있습니다 또는 연결 된 속성을 사용 하 여 합니다. 동작을 사용 하 여 컨트롤에 영향을 연결 하는 방법에 대 한 자세한 내용은 참조 하세요. [재사용 가능한 EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md)합니다. 연결 된 속성을 사용 하 여 컨트롤에 영향을 연결 하는 방법에 대 한 자세한 내용은 참조 하세요. [효과로 매개 변수 전달](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md)합니다.

## <a name="consuming-the-effect-in-cnum"></a>C에서 효과 사용합니다.&num;

해당 [ `Entry` ](xref:Xamarin.Forms.Entry) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` 에 연결할 때 합니다 `Entry` 컨트롤의 효과 추가 하 여 인스턴스 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션에 다음 코드 예제에서 설명한 것 처럼:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) 반환를 [ `Effect` ](xref:Xamarin.Forms.Effect) 해상도 그룹 이름의 연결은 지정 된 이름에 대 한 (사용 하 여 지정 합니다 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 효과 클래스의 특성), 및를 사용 하 여 지정 된 고유 ID를 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) 효과 클래스의 특성입니다. 플랫폼 효과 제공 하지 않는 경우는 `Effect.Resolve` 메서드는 반환 된 비-`null` 값입니다.

## <a name="summary"></a>요약

이 문서에서는 설명의 배경색을 변경 하는 효과 만드는 방법 합니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤이 포커스를 획득 하는 경우를 제어 합니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [효과](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [배경 색 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [포커스 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
