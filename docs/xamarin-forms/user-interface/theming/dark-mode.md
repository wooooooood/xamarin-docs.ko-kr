---
title: Xamarin.Forms 응용 프로그램에서 다크 모드 감지
description: 다크 모드는 리소스 사전, DynamicResources 및 플랫폼 지식의 조합을 사용하여 모든 Xamarin.Forms 응용 프로그램에서 지원될 수 있습니다.
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 7fe1a98e6a497a5791f26df2fc96d41781b71ef6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628306"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Xamarin.Forms 응용 프로그램에서 다크 모드 감지

Xamarin.Forms 응용 프로그램은 [테마를](theming.md)지원할 수 있는 동일한 전략을 사용하여 운영 체제 테마 변경에 응답할 수 있습니다. 주요 차이점은 테마 변경이 트리거되는 방식입니다. 다크 모드는 운영 체제 수준에서 설정할 수 있고 어떤 응용 프로그램이 즉시 존중하고 응답해야 하는 광범위한 모양 환경 설정을 말합니다.

Xamarin.Forms 응용 프로그램에서 다크 모드를 사용하기 위한 최소 요구 사항은 다음과 같습니다.

- iOS 13 이상.
- 안드로이드 9 이상 (안드로이드 9는 쿼리 할 수있는 모양 모드를 지원합니다).
- 안드로이드 10 이상 (안드로이드 10 모드 변경의 응용 프로그램을 통보).
- UWP v10.0.10240.0 이상.
- Xamarin.Forms (모든 버전).

밝고 어두운 모양을 포함한 모양 모드를 지원하는 프로세스는 다음과 같습니다.

1. 에서 전체 응용 프로그램에서 공유하는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)리소스를 정의합니다.
2. 변경하려는 각 스타일에 특정한 재정의를 제공하는 각 모양 모드에 대한 리소스 사전 테마를 정의합니다.
3. 응용 프로그램의 **App.xaml** 파일에서 기본 모양 모드 테마를 설정합니다.
4. 모양 모드가 `DynamicResource` 변경될 때 스타일이 반응할 태그 확장을 사용하여 응용 프로그램의 스타일을 지정합니다.
5. 코드를 추가하여 OS 테마 변경 내용을 수신 하고 응용 프로그램의 해당 테마를 로드합니다.

다음 스크린샷은 밝은 모드와 어두운 모드의 테마 페이지를 보여 준다.

[![테마 앱의 메인 페이지 스크린샷, iOS 및 Android](theming-images/main-page-both-themes.png "테마 앱의 메인 페이지")](theming-images/main-page-both-themes-large.png#lightbox "테마 앱의 메인 페이지")
[![앱의 세부 정보 페이지의 iOS 및 Android 스크린샷, iOS 및 Android](theming-images/detail-page-both-themes.png "테마 앱의 세부 정보 페이지")](theming-images/detail-page-both-themes-large.png#lightbox "테마 앱의 세부 정보 페이지")

## <a name="define-themes"></a>테마 정의

어둡고 밝은 테마를 만드는 것에 대한 단계별 세부 정보로 [테마 가이드를](theming.md) 따르십시오.

플랫폼 코드의 적절한 위치에서 응용 프로그램에 대한 새 테마를 쉽게 설정할 수 있습니다. 새 테마를 로드하려면 응용 프로그램의 현재 리소스 사전을 선택한 테마 리소스 사전으로 바꾸기만 하면 됩니다.

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>테마 감지 및 적용

현재 실행 중인 테마를 감지하는 것은 [`RequestedTheme`](~/essentials/app-theme.md) [Xamarin.Essentials(버전](~/essentials/index.md) 1.4.0 이상)의 기능을 사용하여 달성할 수 있습니다. 그런 다음 새 클래스 또는 클래스에서 도우미 `App` 메서드를 만들어 테마를 구성할 수 있습니다.

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // Change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
        else
        {
            // Change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
    }
}
```

## <a name="react-to-appearance-mode-changes"></a>외형 모드 변경에 반응

장치의 모양 모드는 사용자가 모드를 명시적으로 선택하거나 하루 중 시간 또는 저조도와 같은 환경 요인을 포함하여 기본 설정을 구성한 방법에 따라 다양한 이유로 변경될 수 있습니다. 응용 프로그램이 이러한 변경 사항에 대응할 수 있도록 플랫폼 코드를 추가해야 하며 다음 섹션에서는 이에 대해 자세히 설명합니다.

### <a name="android"></a>Android

앱에서 다크 모드를 지원하려면 `Resources/values/styles.xml`에서 찾을 수 있는 앱의 테마를 `DayNight` 업데이트하여 테마에서 상속해야 합니다.

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

AndroidX의 [재질 구성](https://www.nuget.org/packages/Xamarin.Google.Android.Material/) 요소(1.1.0-rc2)로 업그레이드했거나 최신으로 업그레이드한 경우 다음을 사용할 수 있습니다.

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

응용 프로그램의 **MainActivity.cs** 파일에서 `ConfigChanges.UiMode` `ConfigurationChanges` `Activity` 속성의 속성에 필드를 추가하여 앱에 UI 모드 변경 사항에 대한 알림을 받게 됩니다.

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

동일한 **MainActivity.cs** 파일에서 메서드를 `OnConfigurationChanged` 재정의합니다.

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### <a name="ios"></a>iOS

iOS에서는 UiViewController에 모양 모드 변경 내용이 알림이 전송되며, Xamarin.Forms는 `ContentPage`. 해당 메서드를 재정의하려면 iOS 프로젝트에서 다음과 같은 `PageRenderer.cs`사용자 지정 렌더러를 만듭니다.

```csharp
using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourApp.iOS.Renderers.PageRenderer))]
namespace YourApp.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                App.ApplyTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            App.ApplyTheme();
        }
    }
}
```

메서드를 `TraitCollectionDidChange` 재정의하면 변경 에 `UserInterfaceStyle` 대해 조치를 취할 수 있습니다.

### <a name="uwp"></a>UWP

UWP에서 응용 프로그램의 **MainPage.xaml.cs** 파일에 다음 코드를 추가합니다.

```csharp
public sealed partial class MainPage
{
    UISettings uiSettings;

    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new YourApp.App());

        uiSettings = new UISettings();
        uiSettings.ColorValuesChanged += ColorValuesChanged;
    }

    private void ColorValuesChanged(UISettings sender, object args)
    {
        Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
        {
            App.ApplyTheme();
        });
    }
}
```

## <a name="related-links"></a>관련 링크

- [테마(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [자마린의 동적 스타일.양식](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
