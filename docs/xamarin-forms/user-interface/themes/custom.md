---
title: "사용자 지정 테마 만들기"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4FE08ADC-093F-47FA-B33C-20CF08B5D7E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: b981e6a076c8ab35d9c8be43ffd5cdc68b4a59bd
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="creating-a-custom-theme"></a>사용자 지정 테마 만들기

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

Nuget 패키지에서 테마를 추가할 뿐만 아니라 (같은 [Light](~/xamarin-forms/user-interface/themes/light.md) 및 [어두운](~/xamarin-forms/user-interface/themes/dark.md) 테마), 응용 프로그램에서 사용자 고유의 리소스 참조 될 수 있는 사전 테마를 만들 수 있습니다.

## <a name="example"></a>예

세 개의 `BoxView`s에 표시 되는 [테마 페이지](~/xamarin-forms/user-interface/themes/index.md) 하 여 두 개의 다운로드할 수 있는 테마에 정의 된 세 개의 클래스에 따라 구현 합니다.

이러한 작업을 이해 하려면 다음 태그 만듭니다는 해당 하는 스타일에 직접 추가할 수 있는 프로그램 **App.xaml**합니다.

참고는 `Class` 특성 `Style` (반대인는 [ `x:Key` ](~/xamarin-forms/user-interface/styles/inheritance.md) Xamarin.Forms의 이전 버전에서 사용할 수 있는 특성).

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

함을 알 수 있습니다는 `Rounded` 클래스 효과를 참조 하는 사용자 지정 `CornerRadius`합니다.
이 효과 대 한 코드는 참조할 수 올바르게 사용자 지정-아래 제공 된 `xmlns` 에 추가 되어야 합니다는 **App.xaml**의 루트 요소:

```csharp
xmlns:local="clr-namespace:ThemesDemo;assembly=ThemesDemo"
```

### <a name="c-code-in-the-pcl-or-shared-project"></a>C# 또는에서 코드를 PCL 프로젝트 공유

둥근 모퉁이 만들기 위한 코드 `BoxView` 사용 하 여 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)합니다.
모서리 반지름을 사용 하 여 적용 되는 `BindableProperty` 적용 하 여 구현 됩니다는 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)합니다. 효과에 플랫폼 관련 코드가 필요는 [iOS](#ios) 및 [Android](#android) 프로젝트 (아래 표시).

```csharp
namemspace ThemesDemo
{
  public static class ThemeEffects
  {
  public static readonly BindableProperty CornerRadiusProperty =
    BindableProperty.CreateAttached("CornerRadius", typeof(double), typeof(ThemeEffects), 0.0, propertyChanged: OnChanged<CornerRadiusEffect, double>);
    private static void OnChanged<TEffect, TProp>(BindableObject bindable, object oldValue, object newValue)
              where TEffect : Effect, new()
    {
        var view = bindable as View;
        if (view == null)
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

### <a name="c-code-in-the-ios-project"></a>IOS 프로젝트에 C# 코드

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
            return Container != null && (int)Android.OS.Build.VERSION.SdkInt >= 21;
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
                var pixles =
                    (float)ThemeEffects.GetCornerRadius(_element) *
                    view.Resources.DisplayMetrics.Density;

                outline.SetRoundRect(new Rect(0, 0, view.Width, view.Height), (int)pixles);
            }
        }
    }
}
```


## <a name="summary"></a>요약

사용자 지정 모양을 필요로 하는 각 컨트롤에 대 한 스타일을 정의 하 여 사용자 지정 테마를 만들 수 있습니다. 여러 스타일을 컨트롤에 대 한 다른으로 구별할 수 해야 `Class` 리소스 사전에 특성을 설정 하 여 다음 적용는 `StyleClass` 컨트롤에는 특성입니다.

스타일을 사용할 수 있습니다 [효과](~/xamarin-forms/app-fundamentals/effects/index.md) 추가 컨트롤의 모양을 사용자 지정할 수 있습니다.

[암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md) (포함 하지 않고는 `x:Key` 또는 `Style` 특성) 계속 일치 하는 모든 컨트롤에 적용 된 `TargetType`합니다.
