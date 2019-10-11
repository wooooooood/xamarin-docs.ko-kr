---
title: 시작 화면
description: 특히 앱이 장치에서 처음 시작 될 때 Android 앱을 시작 하는 데 시간이 오래 걸립니다. 시작 화면(Splash screen)은 사용자에게 시작 진행률을 표시하거나 브랜드를 보여주기 위해 표시할 수 있습니다.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/02/2019
ms.openlocfilehash: 4633811b2c0b001ab220f5fedaf116b1b269344a
ms.sourcegitcommit: 5110d1279809a2af58d3d66cd14c78113bb51436
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72032558"
---
# <a name="splash-screen"></a>시작 화면

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)

@no__t-특히 앱이 장치에서 처음 시작 될 때 Android 앱을 시작 하는 데 시간이 오래 걸립니다. 시작 화면(Splash screen)은 사용자에게 시작 진행률을 표시하거나 브랜드를 보여주기 위해 표시할 수 있습니다._

## <a name="overview"></a>개요

Android 앱은 시작 하는 데 약간의 시간이 걸립니다. 특히 앱이 처음으로 장치에서 실행 되는 경우 ( _콜드 시작_이라고 함) 시작 화면은 사용자에게 시작 진행률을 표시하거나 응용 프로그램 식별 및 홍보를 위한 브랜드 정보를 표시할 수 있습니다.

이 가이드에서는 Android 응용 프로그램의 시작 화면을 구현하는 한 가지 기술을 설명합니다. 다음 단계를 포함합니다.

1. 시작 화면에 그림 리소스를 만듭니다.

2. 그림 리소스를 표시하는 새 테마를 정의합니다.

3. 이전 단계에서 만든 테마에서 정의한 시작 화면으로 사용할 새 작업을 응용 프로그램에 추가 합니다.

[![Example 로고 시작 화면, 앱 화면에 대 한 예제](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)

## <a name="requirements"></a>요구 사항

이 가이드에서는 응용 프로그램이 Android API level 21 이상을 대상으로 한다고 가정 합니다. 또한 응용 프로그램은 프로젝트에 추가된 **Xamarin.Android.Support.v4** 및 **Xamarin.Android.Support.v7.AppCompat** NuGet 패키지가 있어야 합니다.

이 가이드에 있는 모든 코드 및 XML은 이 가이드에 대한 샘플 프로젝트인 [SplashScreen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)에 포함되어 있습니다.

## <a name="implementing-a-splash-screen"></a>시작 화면 구현

시작 화면을 렌더링하고 표시하는 가장 빠른 방법은 사용자 정의 테마를 생성하고 시작 화면을 표시하는 액티비티에 그것을 적용하는 것입니다. 액티비티는 렌더링될 때 테마를 로드하고 액티비티의 배경에 그림 리소스(테마에 의해 참조 됨)를 적용합니다. 이 방법은 레이아웃 파일을 만들 필요가 없습니다.

시작 화면은 브랜드 그림 표시, 모든 초기화 수행 및 모든 작업 시작 액티비티로 구현됩니다. 앱이 부트스트랩되면 시작 화면 액티비티는 메인 액티비티를 시작하고 응용 프로그램 백 스택에서 자신을 제거합니다.

### <a name="creating-a-drawable-for-the-splash-screen"></a>시작 화면용 그림 생성

시작 화면은 시작 화면 액티비티의 배경에 XML 그림을 표시합니다. 그렇게 하려면 이미지 표시를 위해 비트맵 이미지(예: JPG 또는 PNG)를 사용해야 합니다.

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

이 `layer-list`은 시작 이미지를 @no__t 1 리소스로 지정 된 배경색으로 가운데 맞춤 합니다. 샘플 응용 프로그램은 **Resources/values/color .xml** 파일에서이 색을 정의 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  ...
  <color name="splash_background">#FFFFFF</color>
</resources>
```

@No__t-0 개체에 대 한 자세한 내용은 [Android에서 사용할 Google 설명서](https://developer.android.com/reference/android/graphics/drawable/Drawable)를 참조 하세요.

### <a name="implementing-a-theme"></a>테마 구현

시작 화면 액티비티에 대한 사용자 정의 테마를 만들려면 **values/styles.xml** 파일을 편집(또는 추가)하고 시작 화면에 대한 새 `style` 요소를 만듭니다. 샘플 **values/style.xml** 파일은 다음과 같이 **MyTheme.Splash** 이름을 가진 `style`을 가지고 있습니다.

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

**MyTheme.Splash**은 엄격한 스파르타식입니다. 창 배경을 선언, 명시적으로 창에서 제목 표시줄을 제거하고 전체 화면임을 선언합니다. 액티비티의 첫 번째 레이아웃을 올리기 전에 응용 프로그램의 UI를 에뮬레이트하는 시작 화면을 만들려는 경우, 자신의 스타일 정의에 `windowContentOverlay` 대신 `windowBackground`를 사용할 수 있습니다. 이 경우 UI 에뮬레이션이 표시되도록 **splash_screen.xml** 그리기를 수정해야 합니다.

### <a name="create-a-splash-activity"></a>시작 액티비티 생성

이제 시작 이미지 및 모든 시작 작업 수행이 포함된 Android용 새 액티비티가 필요합니다. 다음 코드는 완전한 시작 화면 구현의 예제입니다.

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

`SplashActivity`은 이전 섹션에서 만든 테마를 명시적으로 사용 하 여 응용 프로그램의 기본 테마를 재정의 합니다.
테마가 그릴 수 있는를 배경으로 선언 하므로 `OnCreate`의 레이아웃을 로드할 필요가 없습니다.

백 스택에서 액티비티를 제거하기 위한 `NoHistory=true` 특성을 설정하는 것이 중요합니다. 시작 프로세스를 취소시키는 뒤로 가기 버튼을 막기 위해서 `OnBackPressed` 역시 재정의하고 아무 작업도 수행하지 않습니다.

```csharp
public override void OnBackPressed() { }
```

시작 작업은 `OnResume`에서 비동기적으로 수행됩니다. 이는 작업이 시작 화면 표시를 느리게 하거나 연기하지 않도록 하기 위해 필요합니다. 작업이 완료되면 `SplashActivity`는 `MainActivity`를 시작하고 사용자와 앱 간의 상호작용이 시작될 것입니다.

이 새로운 `SplashActivity`는 `MainLauncher` 특성을 `true`로 설정하여 응용 프로그램에 대한 시작 관리자 액티비티로 설정됩니다. 이제 `SplashActivity`가 시작 관리자 액티비티이므로 `MainActivity.cs`를 편집하고, `MainActivity`에서 `MainLauncher` 특성을 제거해야 합니다.

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>가로 모드

이전 단계에서 구현한 시작 화면은 가로 및 세로 모드 모두 제대로 표시됩니다. 그러나 일부 경우에 가로 및 세로 모드용 시작 화면을 분리할 필요가 있습니다(예를 들어 시작 이미지가 전체 화면인 경우).

가로 모드용 시작 화면을 추가하려면 다음 단계를 사용합니다.

1. **Resources/drawable** 폴더에 사용하려는 시작 화면 이미지의 가로 버전을 추가합니다. 이 예제에서 **splash_logo_land.png**는 위 예제에서 사용된 로고의 가로 버전입니다(파란색 대신 흰색 문자 사용).

2. **Resources/drawable** 폴더에 이전에 정의했던 `layer-list` 그림의 가로 버전을 만듭니다(예를 들어 **splash_screen_land.xml**). 이 파일에 시작 화면 이미지의 가로 버전에 대한 비트맵 경로를 설정합니다. 다음 예제에서 **splash_screen_land.xml**은 **splash_logo_land.png**를 사용합니다.

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

5. `windowBackground`에 대한 그림의 가로 버전을 사용하도록 **values-land/style.xml**을 수정합니다. 이 예제에서는 **splash_screen_land.xml**을 사용하였습니다.

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

6. 시작 화면의 가로 버전용으로 사용하려는 색을 구성하려면 **values-land/colors.xml**을 수정합니다. 이 예제에서 가로 모드용 시작 배경 색은 파란색으로 변경되었습니다.

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

7. 빌드하고 앱을 다시 실행합니다. 시작 화면이 계속 표시되는 동안 가로 모드로 장치를 회전합니다. 다음과 같이 시작 화면이 가로 버전으로 변경됩니다.

    [![ 시작 화면을 가로 모드로 회전](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)

참고로 가로 모드 시작 화면을 사용하면 항상 매끄럽지 못한 환경을 제공합니다. 기본적으로 Android 앱은 장치가 이미 가로 모드일지라도 세로 모드로 시작하고 그것을 가로 모드로 전환합니다. 결과적으로, 장치가 가로 모드에 있는 동안 앱을 실행하는 경우, 장치는 세로 시작 화면을 간략하게 표시한 다음 세로에서 가로 시작 화면으로 애니메이션하면서 회전합니다. 불행하게도, 이 초기 세로-가로 전환은 `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape`이 시작 액티비티의 플래그에 지정되어 있을지라도 일어납니다. 이 제한을 해결하는 가장 좋은 방법은 가로 세로 모드 모두 제대로 렌더링 가능한 단일 시작 화면 이미지를 만드는 것입니다.

## <a name="summary"></a>요약

이 가이드는 Xamarin.Android 응용 프로그램의 시작 화면을 구현하는 한 가지 방법, 다시 말해 시작 액티비티에 사용자 정의 테마 적용하기를 설명하였습니다.

## <a name="related-links"></a>관련 링크

- [SplashScreen (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)
- [계층 목록 (그릴 때)](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [재질 디자인 패턴-시작 화면](https://material.io/design/communication/launch-screen.html#usage)
