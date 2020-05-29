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
ms.openlocfilehash: 8478db85bd9904ee6c5cfeab9b2af390e7d3096d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139505"
---
# <a name="platform-specifics"></a>플랫폼 사양

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다._

XAML을 통해 또는 흐름 코드 API를 통해 플랫폼별를 사용 하는 프로세스는 다음과 같습니다.

1. `xmlns` `using` 네임 스페이스에 대 한 선언 또는 지시문을 추가 [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) 합니다.
1. `xmlns` `using` 플랫폼별 기능을 포함 하는 네임 스페이스에 대 한 선언 또는 지시문을 추가 합니다.
    1. IOS에서 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스입니다.
    1. Android에서 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스입니다. Android AppCompat의 경우 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 네임 스페이스입니다.
    1. 유니버설 Windows 플랫폼에서이는 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스입니다.
1. XAML에서 또는 흐름 API를 사용 하 여 코드에서 플랫폼별를 적용 `On<T>` 합니다. 값은 `T` [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS) [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android) 네임 스페이스의, 또는 형식일 수 있습니다 [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) .

> [!NOTE]
> 사용할 수 없는 플랫폼에서 플랫폼별를 사용 하려고 하면 오류가 발생 하지 않습니다. 대신, 플랫폼 특정 적용을 하지 않고 코드가 실행 됩니다.

흐름 code API 반환 개체를 통해 사용 되는 플랫폼 세부 정보 `On<T>` [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) 입니다. 이를 통해 메서드 연계를 사용 하는 동일한 개체에서 여러 플랫폼 특성을 호출할 수 있습니다.

에서 제공 하는 플랫폼 세부 정보에 대 한 자세한 내용은 Xamarin.Forms [iOS 플랫폼](~/xamarin-forms/platform/ios/index.md)관련, [Android 플랫폼 세부](~/xamarin-forms/platform/android/index.md)정보 및 [Windows 플랫폼 세부](~/xamarin-forms/platform/windows/index.md)정보를 참조 하세요.

## <a name="creating-platform-specifics"></a>플랫폼 세부 정보 만들기

공급 업체는 효과를 사용 하 여 고유한 플랫폼 정보를 만들 수 있습니다. 효과는 특정 기능을 제공 하며,이 기능은 플랫폼별를 통해 노출 됩니다. 결과는 XAML을 통해 보다 쉽게 사용 하 고 흐름 코드 API를 통해 보다 쉽게 사용할 수 있는 효과입니다.

플랫폼별를 만드는 프로세스는 다음과 같습니다.

1. 효과로 특정 기능을 구현 합니다. 자세한 내용은 [효과 만들기](~/xamarin-forms/app-fundamentals/effects/creating.md)를 참조 하세요.
1. 효과를 노출 하는 플랫폼별 클래스를 만듭니다. 자세한 내용은 [플랫폼별 클래스 만들기](#creating-a-platform-specific-class)를 참조 하세요.
1. 플랫폼별 클래스에서 XAML을 통해 플랫폼별를 사용 하도록 연결 된 속성을 구현 합니다. 자세한 내용은 [연결 된 속성 추가](#adding-an-attached-property)를 참조 하세요.
1. 플랫폼별 클래스에서 흐름 코드 API를 통해 플랫폼별를 사용 하는 확장 메서드를 구현 합니다. 자세한 내용은 [확장 메서드 추가](#adding-extension-methods)를 참조 하세요.
1. 효과가 적용 되는 것과 동일한 플랫폼에서 플랫폼별가 호출 된 경우에만 효과가 적용 되도록 효과 구현을 수정 합니다. 자세한 내용은 [효과 만들기](#creating-the-effect)를 참조 하세요.

효과를 플랫폼별로 노출 하는 결과는 XAML을 통해 흐름 코드 API를 통해 보다 쉽게 사용할 수 있습니다.

> [!NOTE]
> 사용자의 편리한 사용을 위해 공급 업체는이 기술을 사용 하 여 고유한 플랫폼 정보를 만드는 것이 예상. 사용자가 고유한 플랫폼 정보를 만들도록 선택할 수 있지만 효과를 만들고 소비 하는 것 보다 더 많은 코드가 필요 하다는 점에 유의 해야 합니다.

[샘플 응용 프로그램](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) 은 `Shadow` 컨트롤에 의해 표시 되는 텍스트에 그림자를 추가 하는 플랫폼별를 보여 줍니다 [`Label`](xref:Xamarin.Forms.Label) .

![](images/screenshots.png "Shadow Platform-Specific")

이 [샘플 응용 프로그램](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) 은 `Shadow` 이해를 용이 하 게 하기 위해 플랫폼 별로 각 플랫폼을 구현 합니다. 그러나 각 플랫폼별 효과 구현 외에는 각 플랫폼에 대해 섀도 클래스 구현이 거의 동일 합니다. 따라서이 가이드는 섀도 클래스의 구현에 대 한 집중적으로 살펴봅니다 단일 플랫폼에 미치는 영향을 설명 합니다.

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

다음 섹션에서는 `Shadow` 플랫폼별 효과 및 관련 효과의 구현에 대해 설명 합니다.

#### <a name="adding-an-attached-property"></a>연결 된 속성 추가

XAML을 통해 사용 하려면 연결 된 속성을 플랫폼별로 추가 해야 합니다 `Shadow` .

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

`IsShadowed`연결 된 속성은 `MyCompany.LabelShadowEffect` 클래스가 연결 된 컨트롤에 효과를 추가 하 고이를 제거 하는 데 사용 됩니다 `Shadow` . 이 연결된 속성은 속성 값이 변경될 때 실행할 `OnIsShadowedPropertyChanged` 메서드를 등록합니다. 그러면이 메서드는 `AttachEffect` 또는 메서드를 호출 `DetachEffect` 하 여 연결 된 속성의 값을 기반으로 효과를 추가 하거나 제거 `IsShadowed` 합니다. 이 효과는 컨트롤의 컬렉션을 수정 하 여 컨트롤에서 추가 되거나 제거 됩니다 [`Effects`](xref:Xamarin.Forms.Element.Effects) .

> [!NOTE]
> 효과는 해상도 그룹 이름과 효과 구현에 지정 된 고유 식별자를 연결 하는 값을 지정 하 여 해결 됩니다. 자세한 내용은 [효과 만들기](~/xamarin-forms/app-fundamentals/effects/creating.md)를 참조 하세요.

연결된 속성에 대한 자세한 내용은 [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)를 참조하세요.

#### <a name="adding-extension-methods"></a>확장 메서드 추가

`Shadow`흐름 코드 API를 통해 사용 하려면 확장 메서드를 플랫폼별로 추가 해야 합니다.

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

`IsShadowed`및 `SetIsShadowed` 확장 메서드는 각각 연결 된 속성에 대 한 get 및 set 접근자를 호출 합니다 `IsShadowed` . 각 확장 메서드는 형식에 대해 작동 `IPlatformElementConfiguration<iOS, FormsElement>` 하며,이는 플랫폼 특정이 iOS의 인스턴스에서 호출 될 수 있도록 지정 합니다 [`Label`](xref:Xamarin.Forms.Label) .

#### <a name="creating-the-effect"></a>효과 만들기

플랫폼별는를에 `Shadow` 추가 하 `MyCompany.LabelShadowEffect` [`Label`](xref:Xamarin.Forms.Label) 고 제거 합니다. 다음 코드 예제는 iOS 프로젝트에 대한 `LabelShadowEffect` 구현을 보여줍니다.

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

`UpdateShadow`메서드는 `Control.Layer` 연결 된 `IsShadowed` 속성이로 설정 되어 `true` 있고 효과가 구현 된 것 `Shadow` 과 동일한 플랫폼에서 플랫폼별가 호출 된 경우 속성을 설정 하 여 그림자를 만듭니다. 이 확인은 메서드를 사용 하 여 수행 됩니다 `OnThisPlatform` .

`Shadow.IsShadowed`런타임에 연결 된 속성 값이 변경 되 면 효과는 그림자를 제거 하 여 응답 해야 합니다. 따라서 메서드의 재정의 된 버전 `OnElementPropertyChanged` 은 메서드를 호출 하 여 바인딩 가능한 속성 변경에 응답 하는 데 사용 됩니다 `UpdateShadow` .

효과를 만드는 방법에 대 한 자세한 내용은 효과 [만들기](~/xamarin-forms/app-fundamentals/effects/creating.md) 및 [효과 매개 변수를 연결 된 속성으로 전달](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md)을 참조 하세요.

### <a name="consuming-the-platform-specific"></a>플랫폼별 사용

`Shadow`플랫폼 특정은 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `Shadow.IsShadowed` `boolean` .

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

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
