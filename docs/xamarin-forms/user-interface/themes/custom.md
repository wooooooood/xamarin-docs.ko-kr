---
title: 사용자 지정 Xamarin.Forms 테마 만들기
description: 이 문서에서는 앱에서 참조 하기 위해 Xamarin.Forms 사용자 지정 테마를 만드는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 4FE08ADC-093F-47FA-B33C-20CF08B5D7E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0897afeacffc89e30b28474e4530a83d05d33cd2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61187290"
---
# <a name="creating-a-custom-xamarinforms-theme"></a>사용자 지정 Xamarin.Forms 테마 만들기

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

Nuget 패키지에서 테마를 추가 하는 것 외에도 (같은 합니다 [Light](~/xamarin-forms/user-interface/themes/light.md) 및 [어두운](~/xamarin-forms/user-interface/themes/dark.md) 테마), 앱에서 사용자 고유의 리소스 참조할 수 있는 사전 테마를 만들 수 있습니다.

## <a name="example"></a>예제

세 가지 `BoxView`s에 표시 합니다 [테마 페이지](~/xamarin-forms/user-interface/themes/index.md) 는 두 개의 다운로드 가능한 테마에 정의 된 세 가지 클래스에 따라 스타일이 지정 합니다.

작동 방식을 이해, 다음 태그에 직접 추가할 수는 해당 하는 스타일을 만듭니다 하 **App.xaml**합니다.

참고 합니다 `Class` 에 대 한 특성 `Style` (아닌 사이트별로 [`x:Key`](~/xamarin-forms/user-interface/styles/inheritance.md)
특성 Xamarin.Forms의 이전 버전에서 사용할 수 있습니다).

```xml
<ResourceDictionary>
  <!-- DEFINE ANY CONSTANTS -->
  <Color x:Key="SeparatorLineColor">#CCCCCC</Color>
  <Color x:Key="iOSDefaultTintColor">#007aff</Color>
  <Color x:Key="AndroidDefaultAccentColorColor">#1FAECE</Color>
  <OnPlatform x:TypeArguments="Color" x:Key="AccentColor">
    <On Platform="iOS" Value="{StaticResource iOSDefaultTintColor}" />
    <On Platform="Android" Value="{StaticResource AndroidDefaultAccentColorColor}" />
  </OnPlatform>
  <!--  BOXVIEW CLASSES -->
  <Style TargetType="BoxView" Class="HorizontalRule">
    <Setter Property="BackgroundColor" Value="{ StaticResource SeparatorLineColor }" />
    <Setter Property="HeightRequest" Value="1" />
  </Style>

  <Style TargetType="BoxView" Class="Circle">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="WidthRequest" Value="34"/>
    <Setter Property="HeightRequest" Value="34"/>
    <Setter Property="HorizontalOptions" Value="Start" />

    <Setter Property="local:ThemeEffects.Circle" Value="True" />
  </Style>

  <Style TargetType="BoxView" Class="Rounded">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />

    <Setter Property="local:ThemeEffects.CornerRadius" Value="4" />
  </Style>
</ResourceDictionary>
```

있다는 사실을 알 수는 `Rounded` 클래스는 사용자 지정 효과을 참조 `CornerRadius`합니다.
이 효과 대 한 코드는 아래에 나와-참조 올바르게 사용자 지정 `xmlns` 에 추가 해야 합니다 **App.xaml**의 루트 요소:

```csharp
xmlns:local="clr-namespace:ThemesDemo;assembly=ThemesDemo"
```

### <a name="c-code-in-the-net-standard-library-project-or-shared-project"></a>.NET Standard 라이브러리 프로젝트 또는 공유 프로젝트에서 C# 코드

둥근 모퉁이 만들기 위한 코드 `BoxView` 사용 하 여 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)합니다.
모퉁이 반경을 사용 하 여 적용 되는 `BindableProperty` 적용 하 여 구현 됩니다는 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)합니다. 효과에 플랫폼별 코드가 필요 합니다 [iOS](#ios) 하 고 [Android](#android) 프로젝트 (아래 참조).

```csharp
namespace ThemesDemo
{
  public static class ThemeEffects
  {
  public static readonly BindableProperty CornerRadiusProperty =
    BindableProperty.CreateAttached("CornerRadius", typeof(double), typeof(ThemeEffects), 0.0, propertyChanged: OnChanged<CornerRadiusEffect, double>);
    private static void OnChanged<TEffect, TProp>(BindableObject bindable, object oldValue, object newValue)
              where TEffect : Effect, new()
    {
        if (!(bindable is View view))
        {
            return;
        }

        if (EqualityComparer<TProp>.Equals(newValue, default(TProp)))
        {
            var toRemove = view.Effects.FirstOrDefault(e => e is TEffect);
            if (toRemove != null)
            {
                view.Effects.Remove(toRemove);
            }
        }
        else
        {
            view.Effects.Add(new TEffect());
        }

    }
    public static void SetCornerRadius(BindableObject view, double radius)
    {
        view.SetValue(CornerRadiusProperty, radius);
    }

    public static double GetCornerRadius(BindableObject view)
    {
        return (double)view.GetValue(CornerRadiusProperty);
    }

    private class CornerRadiusEffect : RoutingEffect
    {
        public CornerRadiusEffect()
            : base("Xamarin.CornerRadiusEffect")
        {
        }
    }
  }
}
```

<a name="ios" />

### <a name="c-code-in-the-ios-project"></a>IOS 프로젝트에서 C# 코드

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using CoreGraphics;
using Foundation;
using XFThemes;

namespace ThemesDemo.iOS
{
    public class CornerRadiusEffect : PlatformEffect
    {
        private nfloat _originalRadius;

        protected override void OnAttached()
        {
            if (Container != null)
            {
                _originalRadius = Container.Layer.CornerRadius;
                Container.ClipsToBounds = true;

                UpdateCorner();
            }
        }

        protected override void OnDetached()
        {
            if (Container != null)
            {
                Container.Layer.CornerRadius = _originalRadius;
                Container.ClipsToBounds = false;
            }
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                UpdateCorner();
            }
        }

        private void UpdateCorner()
        {
            Container.Layer.CornerRadius = (nfloat)ThemeEffects.GetCornerRadius(Element);
        }
    }
}
```

<a name="android" />

### <a name="c-code-in-the-android-project"></a>Android 프로젝트에서 C# 코드

```csharp
using System;
using Xamarin.Forms.Platform;
using Xamarin.Forms.Platform.Android;
using Android.Views;
using Android.Graphics;

namespace ThemesDemo.Droid
{
    public class CornerRadiusEffect : BaseEffect
    {
        private ViewOutlineProvider _originalProvider;

        protected override bool CanBeApplied()
        {
            return Container != null && Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop;
        }

        protected override void OnAttachedInternal()
        {
            _originalProvider = Container.OutlineProvider;
            Container.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            Container.ClipToOutline = true;
        }

        protected override void OnDetachedInternal()
        {
            Container.OutlineProvider = _originalProvider;
            Container.ClipToOutline = false;
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (!Attached)
            {
                return;
            }

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                Container.Invalidate();
            }
        }

        private class CornerRadiusOutlineProvider : ViewOutlineProvider
        {
            private Xamarin.Forms.Element _element;

            public CornerRadiusOutlineProvider(Xamarin.Forms.Element element)
            {
                _element = element;
            }

            public override void GetOutline(Android.Views.View view, Outline outline)
            {
                var pixels =
                    (float)ThemeEffects.GetCornerRadius(_element) *
                    view.Resources.DisplayMetrics.Density;

                outline.SetRoundRect(new Rect(0, 0, view.Width, view.Height), (int)pixels);
            }
        }
    }
}
```


## <a name="summary"></a>요약

사용자 지정 모양을 필요한 각 컨트롤에 대 한 스타일을 정의 하 여 사용자 지정 테마를 만들 수 있습니다. 다른 컨트롤에 대 한 여러 스타일을 구별할 수 해야 `Class` 리소스 사전에 특성을 설정 하 여 다음 적용을 `StyleClass` 컨트롤에는 특성입니다.

스타일을 활용할 수도 [효과](~/xamarin-forms/app-fundamentals/effects/index.md) 추가 컨트롤의 모양을 사용자 지정할 수 있습니다.

[암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md) (포함 하지 않고는 `x:Key` 또는 `Style` 특성) 계속 일치 하는 모든 컨트롤에 적용 됩니다는 `TargetType`합니다.
