---
title: 시작 화면
description: 특히 앱이 장치에서 처음 시작 될 때 Android 앱을 시작 하는 데 시간이 오래 걸립니다. 시작 화면에 사용자에 대 한 시작 진행률 또는 브랜딩 표시를 표시할 수 있습니다.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/02/2019
ms.openlocfilehash: 8f225df47b299ae4748c3a3fea586f277e14213d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028726"
---
# <a name="splash-screen"></a>시작 화면

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)

_특히 앱이 장치에서 처음 시작 될 때 Android 앱을 시작 하는 데 시간이 오래 걸립니다. 시작 화면에 사용자에 대 한 시작 진행률 또는 브랜딩 표시를 표시할 수 있습니다._

## <a name="overview"></a>개요

Android 앱은 시작 하는 데 약간의 시간이 걸립니다. 특히 앱이 처음으로 장치에서 실행 되는 경우 ( _콜드 시작_이라고 함) 시작 화면에 사용자에 대 한 시작 진행률이 표시 되거나 응용 프로그램을 식별 하 고 승격할 수 있는 브랜딩 정보를 표시할 수 있습니다.

이 가이드에서는 Android 응용 프로그램에서 시작 화면을 구현 하는 한 가지 방법을 설명 합니다. 다음 단계를 다룹니다.

1. 시작 화면에 대해 그릴 때 사용할 리소스를 만듭니다.

2. 그릴 수 있는 리소스를 표시 하는 새 테마를 정의 합니다.

3. 이전 단계에서 만든 테마에서 정의한 시작 화면으로 사용할 새 작업을 응용 프로그램에 추가 합니다.

[![예제 Xamarin 로고 시작 화면, 앱 화면](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)

## <a name="requirements"></a>요구 사항

이 가이드에서는 응용 프로그램이 Android API level 21 이상을 대상으로 한다고 가정 합니다. 또한 응용 프로그램에는 프로젝트에 **추가 된** **v7 및 xamarin.ios** 패키지를 포함 해야 합니다.

이 가이드의 모든 코드와 XML은이 가이드의 [SplashScreen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen) 샘플 프로젝트에서 찾을 수 있습니다.

## <a name="implementing-a-splash-screen"></a>시작 화면 구현

시작 화면을 렌더링 하 고 표시 하는 가장 빠른 방법은 사용자 지정 테마를 만들어 시작 화면을 표시 하는 활동에 적용 하는 것입니다. 활동은 렌더링 될 때 테마를 로드 하 고 테마에 의해 참조 되는 그릴 수 있는 리소스를 활동의 배경에 적용 합니다. 이 방법을 사용 하면 레이아웃 파일을 만들 필요가 없습니다.

시작 화면은 그릴 수 있는 브랜드를 표시 하 고, 모든 초기화를 수행 하 고, 모든 작업을 시작 하는 활동으로 구현 됩니다. 앱이 부트스트랩 되 면 시작 화면 작업에서 주 활동을 시작 하 고 응용 프로그램 백오프 스택에서 해당 활동을 제거 합니다.

### <a name="creating-a-drawable-for-the-splash-screen"></a>시작 화면에 그릴 그릴 만들기

시작 화면에 시작 화면 작업의 배경에 그릴 수 있는 XML이 표시 됩니다. 이미지를 표시 하려면 비트맵 이미지 (예: PNG 또는 JPG)를 사용 해야 합니다.

샘플 응용 프로그램은 **splash_screen**이라는 그릴을 정의 합니다. 이 예에서는 다음 xml에 표시 된 것 처럼 [계층 목록을](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) 사용 하 여 응용 프로그램의 시작 화면 이미지를 가운데에 맞춥니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash_logo"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

이 `layer-list`는 시작 이미지를 `@color/splash_background` 리소스로 지정 된 배경색으로 가운데 맞춤 합니다. 샘플 응용 프로그램은 **Resources/values/color .xml** 파일에서이 색을 정의 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  ...
  <color name="splash_background">#FFFFFF</color>
</resources>
```

`Drawable` 개체에 대 한 자세한 내용은 [Android에서 사용할 Google 설명서](https://developer.android.com/reference/android/graphics/drawable/Drawable)를 참조 하세요.

### <a name="implementing-a-theme"></a>테마 구현

시작 화면 작업에 대 한 사용자 지정 테마를 만들려면 파일 **값/스타일 .xml** 을 편집 하거나 추가 하 고 시작 화면에 대 한 새 `style` 요소를 만듭니다. 샘플 **값/스타일 .xml** 파일은 다음과 같이 이름이 mytheme 인 `style` 표시 됩니다 **. 시작**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

    <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>  
    <item name="android:windowFullscreen">true</item>  
    <item name="android:windowContentOverlay">@null</item>  
    <item name="android:windowActionBar">true</item>  
  </style>
</resources>
```

**Mytheme.** 부흥 시킴는 창 배경을 선언 하 고 창에서 제목 표시줄을 명시적으로 제거 하 고 전체 화면 임을 선언 하는 &ndash; 매우 유용 합니다. 활동이 첫 번째 레이아웃을 늘어납니다 하기 전에 앱의 UI를 에뮬레이트하는 시작 화면을 만들려면 스타일 정의에서 `windowBackground` 대신 `windowContentOverlay`를 사용할 수 있습니다. 이 경우 UI의 에뮬레이션을 표시 하도록 **splash_screen** 를 수정 해야 합니다.

### <a name="create-a-splash-activity"></a>시작 활동 만들기

이제 시작 이미지를 포함 하 고 시작 작업을 수행 하는 Android 용 새 활동이 필요 합니다. 다음 코드는 전체 시작 화면 구현의 예입니다.

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity`는 이전 섹션에서 만든 테마를 명시적으로 사용 하 여 응용 프로그램의 기본 테마를 재정의 합니다.
테마가 그릴 수 있는를 배경으로 선언 하므로 `OnCreate` 레이아웃을 로드할 필요가 없습니다.

작업이 뒤로 스택에서 제거 되도록 `NoHistory=true` 특성을 설정 하는 것이 중요 합니다. 뒤로 단추가 시작 프로세스를 취소 하는 것을 방지 하기 위해 `OnBackPressed`를 재정의 하 고 아무 작업도 수행 하지 않을 수도 있습니다.

```csharp
public override void OnBackPressed() { }
```

시작 작업은 `OnResume`에서 비동기적으로 수행 됩니다. 이는 시작 작업으로 인해 시작 화면의 속도가 느려지거나 지연 되지 않도록 하기 위해 필요 합니다. 작업이 완료 되 면 `SplashActivity` `MainActivity` 시작 되 고 사용자가 앱과 상호 작용을 시작할 수 있습니다.

이 새 `SplashActivity`는 `MainLauncher` 특성을 `true`로 설정 하 여 응용 프로그램에 대 한 시작 관리자 활동으로 설정 됩니다. 이제 `SplashActivity` 시작 관리자 작업 이므로 `MainActivity.cs`를 편집 하 고 `MainActivity`에서 `MainLauncher` 특성을 제거 해야 합니다.

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>가로 모드

이전 단계에서 구현 된 시작 화면은 세로 및 가로 모드에서 올바르게 표시 됩니다. 그러나 경우에 따라 세로 및 가로 모드의 경우 별도의 시작 화면이 있어야 합니다 (예: 시작 이미지가 전체 화면에 있는 경우).

가로 모드의 시작 화면을 추가 하려면 다음 단계를 사용 합니다.

1. **리소스/그릴** 수 있는 폴더에 사용 하려는 시작 화면 이미지의 가로 버전을 추가 합니다. 이 예제에서 **splash_logo_land** 는 위의 예제에서 사용 된 로고의 가로 버전입니다 (파란색 대신 흰색 문자를 사용).

2. **리소스/그릴** 폴더에서 이전에 정의 된 `layer-list` (예: **splash_screen_land**)의 가로 버전을 만듭니다. 이 파일에서 비트맵 경로를 시작 화면 이미지의 가로 버전으로 설정 합니다. 다음 예제에서 **splash_screen_land** 는 splash_logo_land를 사용 **합니다**.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item>
        <color android:color="@color/splash_background"/>
      </item>
      <item>
        <bitmap
            android:src="@drawable/splash_logo_land"
            android:tileMode="disabled"
            android:gravity="center"/>
      </item>
    </layer-list>
    ```

3. **리소스/값-육지** 폴더가 아직 없는 경우 만듭니다.

4. 파일 **색 .xml** 및 **스타일 .xml** 을 **값** 에 추가 합니다. 이러한 파일은 기존 **값/colors** 및 **값/스타일 .xml** 파일에서 복사 하 여 수정할 수 있습니다.

5. `windowBackground`를 위해 그릴 수 있는의 가로 버전을 사용 하도록 **values-land/style .xml** 을 수정 합니다. 이 예제에서는 splash_screen_land를 사용 **합니다** .

    ```xml
    <resources>
      <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
      </style>
        <style name="MyTheme" parent="MyTheme.Base">
      </style>
      <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
        <item name="android:windowBackground">@drawable/splash_screen_land</item>
        <item name="android:windowNoTitle">true</item>  
        <item name="android:windowFullscreen">true</item>  
        <item name="android:windowContentOverlay">@null</item>  
        <item name="android:windowActionBar">true</item>  
      </style>
    </resources>
    ```

6. **Values-land/colors** 를 수정 하 여 시작 화면의 가로 버전에 사용 하려는 색을 구성 합니다. 이 예제에서는 가로 모드의 경우 시작 배경색이 blue로 변경 됩니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
      <color name="primary">#2196F3</color>
      <color name="primaryDark">#1976D2</color>
      <color name="accent">#FFC107</color>
      <color name="window_background">#F5F5F5</color>
      <color name="splash_background">#3498DB</color>
    </resources>
    ```

7. 앱을 다시 빌드하고 실행 합니다. 시작 화면이 계속 표시 되어 있는 동안 장치를 가로 모드로 회전 합니다. 시작 화면이 가로 버전으로 변경 됩니다.

    [시작 화면을 가로 모드로 회전![](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)

가로 모드 시작 화면을 사용 하는 경우에는 항상 원활한 환경을 제공 하지는 않습니다. 기본적으로 Android는 앱을 세로 모드로 시작 하 고 장치가 이미 가로 모드에 있는 경우에도 가로 모드로 전환 합니다. 따라서 장치가 가로 모드에 있는 동안 앱이 시작 되 면, 장치는 세로로 세로 시작 화면을 표시 한 다음 세로에서 가로 시작 화면으로 회전에 애니메이션 효과를 적용 합니다. 아쉽게도이 초기 세로-가로 전환은 시작 작업의 플래그에 `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` 지정 된 경우에도 적용 됩니다. 이 제한을 해결 하는 가장 좋은 방법은 세로 및 가로 모드에서 올바르게 렌더링 되는 단일 시작 화면 이미지를 만드는 것입니다.

## <a name="summary"></a>요약

이 가이드에서는 Xamarin Android 응용 프로그램에서 시작 화면을 구현 하는 한 가지 방법에 대해 설명 했습니다. 즉, 시작 활동에 사용자 지정 테마를 적용 합니다.

## <a name="related-links"></a>관련 링크

- [SplashScreen (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)
- [계층 목록 (그릴 때)](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [재질 디자인 패턴-시작 화면](https://material.io/design/communication/launch-screen.html#usage)
