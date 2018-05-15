---
title: 시작 화면
description: Android 앱 다소 시간이 걸릴 수 특히 앱을 처음 시작할 때 장치를 시작 합니다. 시작 화면이 시작 표시 될 수 있습니다를 사용자에 게 또는 브랜드를 나타내는 진행률입니다.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/11/2018
ms.openlocfilehash: 431cc359f4191ab2b247b3cacf0f54c3ba44cd57
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="splash-screen"></a>시작 화면

_Android 앱 다소 시간이 걸릴 수 특히 앱을 처음 시작할 때 장치를 시작 합니다. 시작 화면이 시작 표시 될 수 있습니다를 사용자에 게 또는 브랜드를 나타내는 진행률입니다._


## <a name="overview"></a>개요

Android 앱에 시작 될 때까지 다소 시간이 걸리는, 특히 처음으로 하는 동안 앱이 장치에서 실행 됩니다 (이 라고도 _콜드 시작_). 시작 화면이 표시 될 수 있습니다는 사용자에 게 진행률을 시작 또는 식별 하 고 응용 프로그램을 승격 브랜드 정보를 표시할 수 있습니다.

이 가이드에서는 Android 응용 프로그램의 시작 화면을 구현 하는 기술에 설명 합니다. 다음 단계에 설명 합니다.

1.  시작 화면에 그릴 수 있는 리소스를 만듭니다.

2.  그릴 수 있는 리소스를 표시 하는 새 테마를 정의 합니다.

3.  이전 단계에서 만든 테마에 정의 된 시작 화면으로 사용 될 응용 프로그램에 새 활동을 추가 합니다.

[![응용 프로그램 화면에서 다음 예제에서는 Xamarin 로고 시작 화면](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>요구 사항

이 가이드에서는 응용 프로그램이 Android API 수준 15 (Android 4.0.3) 대상으로 지정 함을 가정 이상. 응용 프로그램이 있어야는 **Xamarin.Android.Support.v4** 및 **Xamarin.Android.Support.v7.AppCompat** NuGet 패키지를 프로젝트에 추가 합니다.

모든 코드 및이 가이드에는 XML에서 찾을 수 있습니다는 [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen) 이 가이드에 대 한 샘플 프로젝트입니다.


## <a name="implementing-a-splash-screen"></a>시작 화면을 구현합니다.

렌더링 하 고 시작 화면이 표시 가장 빠른 방법은 사용자 지정 테마를 만들어 시작 화면을 수행 하는 활동에 적용 하는 것입니다. 활동을 렌더링할 때 테마를 로드 하 고 활동의 배경을 그릴 수 있는 리소스 (테마에서 참조 됨) 적용 됩니다. 이 방법은 레이아웃 파일을 만들기 위한 필요가 없습니다.

시작 화면의 브랜드가 지정 된 표시 하는 활동으로 구현 됩니다을 그릴 수 있는 모든 초기화를 수행 하 고 작업을 시작 합니다. 응용 프로그램 부트스트랩가 되 면 활동의 시작 화면 기본 활동을 시작 하 고 응용 프로그램 백 스택에 자체 제거 합니다.


### <a name="creating-a-drawable-for-the-splash-screen"></a>시작 화면에 대 한를 그릴 수 있는 만들기

시작 화면이 시작 화면 활동의 배경을 그릴 수 있는 XML에 표시 됩니다. 표시 하려면 이미지에 대 한 비트맵 이미지 (예: JPG 또는 PNG)을 사용 해야 하는 합니다.

이 가이드를 사용 하 여는 [레이어 목록](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) 응용 프로그램의 시작 화면 이미지를 가운데에 있습니다. 다음 코드 조각은의 한 예로 `drawable` 리소스를 사용 하는 `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

이 `layer-list` 시작 화면 이미지 화면 가운데 **splash.png** 로 지정 된 배경는 `@color/splash_background` 리소스입니다.
이 파일에 배치 된 **리소스/그릴 수 있는** 폴더 (예를 들어 **Resources/drawable/splash_screen.xml**).

그릴 수 있는 시작 화면을 만든 후에 다음 단계에서는 시작 화면에 대 한 테마를 만드는 것입니다.


### <a name="implementing-a-theme"></a>테마를 구현합니다.

시작 화면 활동에 대 한 사용자 지정 테마를 만들려면 편집 (또는 추가) 파일 **values/styles.xml** 을 새로 만듭니다 `style` 시작 화면에 대 한 요소입니다. 샘플 **values/style.xml** 와 파일은 다음과 같습니다는 `style` 라는 **MyTheme.Splash**:

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
  </style>
</resources>
```

**MyTheme.Splash** 은 매우 스파르타식 &ndash; 것 선언 창 배경, 명시적으로 창에서 제목 표시줄을 제거 하 고 전체 화면 임을 선언 합니다. 활동의 첫 번째 레이아웃을 확장 하기 전에 응용 프로그램의 UI를 에뮬레이트하는 시작 화면을 만들려는 경우 사용할 수 있습니다 `windowContentOverlay` 대신 `windowBackground` 스타일 정의에 있습니다. 이 경우 수정 해야는 **splash_screen.xml** 그릴 UI의 에뮬레이션 표시 되도록 합니다.


### <a name="create-a-splash-activity"></a>시작 작업 만들기

지금 시작 하려면 시작 이미지 우리의 있으며 모든 시작 작업을 수행 하는 Android 용 새 활동이 필요 합니다. 다음 코드는 전체 시작 화면 구현의 예:

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

`SplashActivity` 명시적으로 재정의 기본 테마 응용 프로그램의 이전 섹션에서 작성 된 테마를 사용 합니다.
레이아웃에 로드할 필요가 없습니다 `OnCreate` 테마는 배경으로 그릴 선언으로 합니다.

설정 해야는 `NoHistory=true` 특성 활동 백 스택에에서 제거 됩니다. 뒤로 단추 시작 프로세스를 취소 하지 않도록 하려면 재정의할 수도 있습니다 `OnBackPressed` 아무 작업도 수행 하지 있습니다.

```csharp
public override void OnBackPressed() { }
```

시작 작업에 비동기적으로 수행 `OnResume`합니다. 이것이 필요한 시작 작업 속도가 저하 하지 않거나 시작 화면의 모양을 지연 시킬 수 있도록 합니다. 작업이 완료 되 면 `SplashActivity` 시작 `MainActivity` 사용자 응용 프로그램을 시작 하 고 있습니다.

이 새로운 `SplashActivity` 설정 하 여 응용 프로그램에 대 한 시작 관리자 활동으로 설정 됩니다는 `MainLauncher` 특성을 `true`합니다. 때문에 `SplashActivity` 편집 해야 시작 관리자 활동이 이제 `MainActivity.cs`를 제거 하 고는 `MainLauncher` 에서 특성 `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>가로 모드

이전 단계에서 구현 되는 시작 화면 가로 세로 모드에서 제대로 표시 됩니다. 그러나 일부 경우에 필요는 경우이 없으면 가로 세로 모드에 대 한 별도 시작 화면 (예를 들어 시작 이미지는 전체 화면).

가로 모드에 대 한 시작 화면을 추가 하려면 다음 단계를 사용 합니다.

1. 에 **리소스/그릴** 폴더를 사용 하려면 시작 화면 이미지의 가로 버전을 추가 합니다. 이 예제에서는 **splash_logo_land.png** (검정 문자 대신 사용 파랑) 위 예제에서 사용 된 로고의 가로 버전입니다.

2. 에 **리소스/그릴** 폴더를 가로 버전을 만듭니다는 `layer-list` 그릴는 이전에 정의 된 (예를 들어 **splash_screen_land.xml**). 이 파일의 시작 화면 이미지의 가로 버전에 비트맵 경로 설정 합니다. 다음 예에서 **splash_screen_land.xml** 사용 하 여 **splash_logo_land.png**:

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

3.  만들기는 **리소스/값-토지** 폴더 존재 하지 않는 경우.

4.  파일 추가 **colors.xml** 및 **style.xml** 를 **값 토지** (복사 및 기존 수정할 수 있습니다 이러한 **values/colors.xml**및 **values/style.xml** 파일).

5.  수정 **값-토지/style.xml** 가로 버전에 대해 그릴의 구성 된 `windowBackground`합니다. 이 예제에서는 **splash_screen_land.xml** 사용 됩니다.

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

6.  수정 **값-토지/colors.xml** 시작 화면의 가로 버전에 대 한 사용 하려는 색을 구성 하려면. 이 예제에서는 시작 화면 배경 색상은 가로 모드에 대 한 노랑으로 변경 됩니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
      <color name="primary">#2196F3</color>
      <color name="primaryDark">#1976D2</color>
      <color name="accent">#FFC107</color>
      <color name="window_background">#F5F5F5</color>
      <color name="splash_background">#FFFF00</color>
    </resources>
    ```

7.  빌드하고 앱을 다시 실행 합니다. 가로 모드로 시작 화면이 계속 표시 되는 동안 장치를 회전 합니다. 시작 화면 가로 버전 변경 내용:

    [![가로 모드로 시작 화면의 회전](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)


참고 가로 모드 시작 화면을 사용 하는 항상 원활한 환경을 제공 하 고 하지 않습니다. 기본적으로 Android 세로 모드에서 앱을 시작 하 고 것도 장치에에서 이미 있으면 가로 모드 가로 모드로 전환 합니다. 결과적으로, 앱을 장치 가로 모드에 있는 동안 실행을 하는 경우 장치 세로 시작 화면을 간략하게 표시 고 회전에서 세로에서 가로 시작 화면을 다음 애니메이션입니다. 이 초기 세로-가로 전환 수행 하지만 경우에 `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` 시작 활동의 플래그에 지정 합니다. 이 제한을 해결 하는 가장 좋은 방법은 가로 세로 모드에서 제대로 렌더링 하는 단일 시작 화면 이미지를 만드는 것입니다.


## <a name="summary"></a>요약

이 가이드는 Xamarin.Android 응용 프로그램의 시작 화면을 구현 하는 방법을 설명 즉, 시작 작업에는 사용자 지정 테마를 적용합니다.


## <a name="related-links"></a>관련 링크

- [시작 화면 (샘플)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [레이어 목록 Drawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ 자재 디자인 패턴-시작 화면](https://www.google.com/design/spec/patterns/launch-screens.html)
