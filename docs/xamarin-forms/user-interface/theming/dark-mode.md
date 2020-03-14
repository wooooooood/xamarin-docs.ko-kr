---
title: Xamarin Forms 응용 프로그램에서 짙은 모드 검색
description: ResourceDictionaries, DynamicResources 및 platform knowledge의 조합을 사용 하 여 모든 Xamarin.ios 응용 프로그램에서 어두운 모드를 지원할 수 있습니다.
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 104237155797ca90c52ad385e8349480f9666c4c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303504"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Xamarin Forms 응용 프로그램에서 짙은 모드 검색

Xamarin Forms 응용 프로그램은 [테마](theming.md)를 지원할 수 있도록 하는 동일한 전략을 사용 하 여 운영 체제 테마 변경에 응답할 수 있습니다. 주요 차이점은 테마 변경을 트리거하는 방법입니다. 어두운 모드는 운영 체제 수준에서 설정할 수 있는 다양 한 모양 기본 설정 집합을 의미 하 고, 즉시 적용 되 고 응답 해야 하는 응용 프로그램입니다.

Xamarin Forms 응용 프로그램에서 짙은 모드를 사용 하기 위한 최소 요구 사항은 다음과 같습니다.

- iOS 13 이상
- Android 9 이상 (Android 9에서 쿼리할 수 있는 모양 모드 지원)
- Android 10 이상 (Android 10에서 앱 모드 변경을 알립니다.)
- UWP v 10.0.10240.0 이상
- Xamarin.ios (모든 버전).

밝은 및 어두운을 비롯 하 여 모양 모드를 지 원하는 프로세스는 다음과 같습니다.

1. [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 전체 응용 프로그램에서 공유 하는 리소스를 정의 합니다.
2. 변경 하려는 각 스타일에 대 한 재정의를 제공 하는 각 모양 모드에 대해 리소스 사전 테마를 정의 합니다.
3. 응용 프로그램의 **app.xaml** 파일에서 기본 모양 모드 테마를 설정 합니다.
4. 모양 모드가 변경 될 때 스타일을 반응할 `DynamicResource` 태그 확장을 사용 하 여 응용 프로그램의 스타일을 지정할 수 있습니다.
5. OS 테마 변경을 수신 대기 하 고 응용 프로그램의 해당 테마를 로드 하는 코드를 추가 합니다.

다음 스크린샷에서는 밝은 모드와 어두운 모드의 테마 페이지를 보여 줍니다.

[![](theming-images/main-page-both-themes.png "테마가 적용 된 앱의 기본 페이지")](theming-images/main-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 기본 페이지") Ios 및 android에서 테마가 적용 된 앱의 [ ![세부 정보 페이지](theming-images/detail-page-both-themes.png "테마가 적용 된 앱의 세부 정보 페이지") 에 있는 ios 및 Android
의 기본 페이지 스크린샷](theming-images/detail-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 세부 정보 페이지")

## <a name="define-themes"></a>테마 정의

어두운 색 및 밝은 테마를 만드는 방법에 대 한 단계별 세부 정보를 보려면 [테마 가이드](theming.md) 를 따르세요. 

플랫폼 코드의 적절 한 위치에서 응용 프로그램에 대 한 새 테마를 쉽게 설정할 수 있습니다. 새 테마를 로드 하려면 응용 프로그램의 현재 리소스 사전을 선택한 테마가 적용 된 리소스 사전으로 바꿉니다.

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>테마 검색 및 적용

[Xamarin.ios](~/essentials/index.md) (version 1.4.0 이상)의 [`RequestedTheme`](~/essentials/app-theme.md) 기능을 사용 하 여 현재 실행 중인 테마를 검색할 수 있습니다. 그런 다음 새 클래스 또는 `App` 클래스에서 도우미 메서드를 만들어 테마를 구성할 수 있습니다.

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
        else
        {
            // change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
    }
}
```

## <a name="react-to-appearance-mode-changes"></a>모양 모드 변경에 반응

장치에서의 모양 모드는 사용자가 명시적으로 모드를 선택 하는 방법, 시간 또는 낮은 조명 등의 환경적 요인을 포함 하 여 기본 설정을 구성한 방법에 따라 다양 한 이유로 변경 될 수 있습니다. 응용 프로그램이 이러한 변경 내용에 반응할 수 있도록 플랫폼 코드를 추가 해야 하며, 다음 섹션에서이에 대해 자세히 설명 합니다.

### <a name="android"></a>Android

앱에서 어두운 모드를 지원 하려면 앱의 테마를 업데이트 해야 합니다 .이 테마는 `Resources/values/styles.xml`에서 검색 하 여 `DayNight` 테마에서 상속할 수 있습니다.

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

AndroidX의 [재질 구성 요소](https://www.nuget.org/packages/Xamarin.Google.Android.Material/) (1.1.0-rc2) 이상으로 업그레이드 한 경우 다음을 사용할 수 있습니다.

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

응용 프로그램의 **MainActivity.cs** 파일에서 `Activity` 특성의 `ConfigurationChanges` 속성에 `ConfigChanges.UiMode` 필드를 추가 하 여 UI 모드 변경 내용에 대 한 알림이 앱에 표시 되도록 합니다.

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

동일한 **MainActivity.cs** 파일에서 `OnConfigurationChanged` 메서드를 재정의 합니다.

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### <a name="ios"></a>iOS

IOS에서는 UIViewController에서 모양 모드 변경 내용에 대 한 알림이 표시 됩니다 .이는 Xamarin. Forms는 `ContentPage`입니다. 해당 메서드를 재정의 하기 위해 iOS 프로젝트에서 `PageRenderer.cs`이라는 사용자 지정 렌더러를 만듭니다.

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

`TraitCollectionDidChange` 메서드를 재정의 하면 `UserInterfaceStyle` 변경 작업을 수행할 수 있습니다.

### <a name="uwp"></a>UWP

UWP에서 응용 프로그램의 **MainPage.xaml.cs** 파일에 다음 코드를 추가 합니다.

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

- [테마 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin.ios의 동적 스타일](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
