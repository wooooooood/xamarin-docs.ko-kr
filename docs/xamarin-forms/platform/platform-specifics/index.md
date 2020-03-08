---
title: 플랫폼 사양
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 플랫폼 세부 정보를 사용 하 고 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: f6190b9c0d29d57d6d509bdff25e2ce3572e3a3c
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78910547"
---
# <a name="platform-specifics"></a>플랫폼 사양

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다._

플랫폼별 API는 fluent 코드 또는 XAML을 통해 사용 하기 위한 프로세스는 다음과 같습니다.

1. [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) 네임 스페이스에 대 한 `xmlns` 선언 또는 `using` 지시문을 추가 합니다.
1. 플랫폼별 기능을 포함 하는 네임 스페이스에 대 한 `xmlns` 선언 또는 `using` 지시문을 추가 합니다.
    1. IOS에서 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스입니다.
    1. Android에서 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스입니다. Android AppCompat의 경우 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 네임 스페이스입니다.
    1. 유니버설 Windows 플랫폼에서 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스입니다.
1. XAML에서 또는 `On<T>` 흐름 API를 사용 하 여 코드에서 플랫폼별를 적용 합니다. `T` 값은 [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) 네임 스페이스의 [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS), [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android)또는 [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) 형식일 수 있습니다.

> [!NOTE]
> 참고를 사용할 수 없는 플랫폼에서 플랫폼별을 사용 하는 동안 발생 하지 않을 것임 오류가 발생 합니다. 대신 코드 적용 되 고 플랫폼별 없이 실행 됩니다.

`On<T>` 흐름 코드 API를 통해 사용 되는 플랫폼 특성은 [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) 개체를 반환 합니다. 이렇게 하면 메서드 연계 된 동일한 개체에서 호출할 여러 플랫폼별 수 있습니다.

Xamarin.ios에서 제공 하는 플랫폼 세부 정보에 대 한 자세한 내용은 [IOS 플랫폼](~/xamarin-forms/platform/ios/index.md)관련, [Android 플랫폼 세부](~/xamarin-forms/platform/android/index.md)정보 및 [Windows 플랫폼 세부](~/xamarin-forms/platform/windows/index.md)정보를 참조 하세요.

## <a name="creating-platform-specifics"></a>플랫폼 세부 정보 만들기

공급 업체는 효과를 사용 하 여 고유한 플랫폼 정보를 만들 수 있습니다. 효과가는 플랫폼별을 통해 노출 되는 특정 기능을 제공 합니다. 결과 코드를 fluent API 및 XAML을 통해 보다 쉽게 사용할 수 있는 효과입니다.

플랫폼 전용을 만들기 위한 프로세스는 다음과 같습니다.

1. 효과로 특정 기능을 구현 합니다. 자세한 내용은 [효과 만들기](~/xamarin-forms/app-fundamentals/effects/creating.md)를 참조 하세요.
1. 효과 노출 하는 플랫폼 특정 클래스를 만듭니다. 자세한 내용은 [플랫폼별 클래스 만들기](#creating-a-platform-specific-class)를 참조 하세요.
1. 플랫폼별 클래스에서 플랫폼별 XAML을 통해 사용할 수 있도록 연결된 된 속성을 구현 합니다. 자세한 내용은 [연결 된 속성 추가](#adding-an-attached-property)를 참조 하세요.
1. 플랫폼 관련 클래스에 플랫폼별 코드를 fluent API 통해 사용할 수 있도록 확장 메서드를 구현 합니다. 자세한 내용은 [확장 메서드 추가](#adding-extension-methods)를 참조 하세요.
1. 효과 플랫폼 특정 효과와 동일한 플랫폼에서 호출 된 경우에 적용 됩니다 있도록 효과 구현을 수정 합니다. 자세한 내용은 [효과 만들기](#creating-the-effect)를 참조 하세요.

플랫폼 특정 효과 노출 합니다. 결과 XAML 및 fluent 코드 API 통해 효과 보다 쉽게 사용할 수 있습니다.

> [!NOTE]
> 공급 업체 명의 사용 편의성을 위해 자체 플랫폼별을 만들려면이 기술은 사용할지는 예상는 것입니다. 사용자가 자신의 플랫폼별을 만들도록 선택할 수, 하는 동안 유의 만들고 효과 사용 보다 더 많은 코드가 필요 하다는 것입니다.

[샘플 응용 프로그램](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) 은 [`Label`](xref:Xamarin.Forms.Label) 컨트롤에 의해 표시 되는 텍스트에 그림자를 추가 하는 `Shadow` 플랫폼별를 보여 줍니다.

![](images/screenshots.png "Shadow Platform-Specific")

이 [샘플 응용 프로그램](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) 은 이해를 용이 하 게 하기 위해 각 플랫폼에 대 한 `Shadow` 플랫폼 관련 기능을 구현 합니다. 그러나 각 플랫폼별 효과 구현 외에도 섀도 클래스의 구현은 각 플랫폼에 대해 거의 동일 합니다. 따라서이 가이드 섀도 클래스와 연결 된 미치는 단일 플랫폼의 구현에 중점을 둡니다.

효과에 대 한 자세한 내용은 [효과를 사용 하 여 컨트롤 사용자 지정](~/xamarin-forms/app-fundamentals/effects/index.md)을 참조 하세요.

### <a name="creating-a-platform-specific-class"></a>플랫폼별 클래스 만들기

플랫폼별는 `public static` 클래스로 만들어집니다.

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

다음 섹션에서는 플랫폼별 효과와 관련 된 효과 `Shadow`의 구현에 대해 설명 합니다.

#### <a name="adding-an-attached-property"></a>연결 된 속성 추가

XAML을 통해 사용 하려면 연결 된 속성을 플랫폼에 맞게 `Shadow`에 추가 해야 합니다.

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

연결 된 `IsShadowed` 속성은 `Shadow` 클래스가 연결 된 컨트롤에서 `MyCompany.LabelShadowEffect` 효과를 추가 하 고 제거 하는 데 사용 됩니다. 이 연결된 속성은 속성 값이 변경될 때 실행할 `OnIsShadowedPropertyChanged` 메서드를 등록합니다. 그러면이 메서드는 `AttachEffect` 또는 `DetachEffect` 메서드를 호출 하 여 `IsShadowed` 연결 된 속성의 값을 기준으로 효과를 추가 하거나 제거 합니다. 이 효과는 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션을 수정 하 여 컨트롤에서 추가 되거나 제거 됩니다.

> [!NOTE]
> 그룹 이름 확인 및 적용 구현에 지정 된 고유 식별자를 연결한 값을 지정 하 여 효과 해결는 note 합니다. 자세한 내용은 [효과 만들기](~/xamarin-forms/app-fundamentals/effects/creating.md)를 참조 하세요.

연결된 속성에 대한 자세한 내용은 [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)를 참조하세요.

#### <a name="adding-extension-methods"></a>확장 메서드를 추가합니다.

흐름 코드 API를 통해 사용 하려면 확장 메서드를 `Shadow` 플랫폼별에 추가 해야 합니다.

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

`IsShadowed` 및 `SetIsShadowed` 확장 메서드는 각각 `IsShadowed` 연결 된 속성에 대 한 get 및 set 접근자를 호출 합니다. 각 확장 메서드는 `IPlatformElementConfiguration<iOS, FormsElement>` 형식에서 작동 하며, iOS의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에서 플랫폼별를 호출할 수 있도록 지정 합니다.

#### <a name="creating-the-effect"></a>효과 만들기

`Shadow` 플랫폼별는 [`Label`](xref:Xamarin.Forms.Label)에 `MyCompany.LabelShadowEffect`를 추가 하 고 제거 합니다. 다음 코드 예제는 iOS 프로젝트에 대한 `LabelShadowEffect` 구현을 보여줍니다.

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
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

`UpdateShadow` 메서드는 `Control.Layer` 속성을 설정 하 여 `IsShadowed` 연결 된 속성이 `true`로 설정 되 고 `Shadow` 플랫폼별가 효과가 구현 된 동일한 플랫폼에서 호출 된 경우 그림자를 만듭니다. 이 확인은 `OnThisPlatform` 메서드를 사용 하 여 수행 됩니다.

런타임에 `Shadow.IsShadowed` 연결 된 속성 값이 변경 되 면 그림자를 제거 하 여 효과를 응답 해야 합니다. 따라서 `OnElementPropertyChanged` 메서드의 재정의 된 버전은 `UpdateShadow` 메서드를 호출 하 여 바인딩 가능한 속성 변경에 응답 하는 데 사용 됩니다.

효과를 만드는 방법에 대 한 자세한 내용은 효과 [만들기](~/xamarin-forms/app-fundamentals/effects/creating.md) 및 [효과 매개 변수를 연결 된 속성으로 전달](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md)을 참조 하세요.

### <a name="consuming-the-platform-specific"></a>플랫폼별 사용

`Shadow` 플랫폼별는 `Shadow.IsShadowed` 연결 된 속성을 `boolean` 값으로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [ShadowPlatformSpecific (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)
- [iOS 플랫폼-세부 정보](~/xamarin-forms/platform/ios/index.md)
- [Android 플랫폼-세부 정보](~/xamarin-forms/platform/android/index.md)
- [Windows 플랫폼-세부 정보](~/xamarin-forms/platform/windows/index.md)
- [효과를 사용 하 여 컨트롤 사용자 지정](~/xamarin-forms/app-fundamentals/effects/index.md)
- [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)
- [PlatformConfiguration API](xref:Xamarin.Forms.PlatformConfiguration)
