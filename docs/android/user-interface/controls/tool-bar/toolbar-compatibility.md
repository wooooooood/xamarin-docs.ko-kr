---
title: "도구 모음 호환성"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: d4d6e93bf3a755d9b48c9e096de87b4c89f2831f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="toolbar-compatibility"></a>도구 모음 호환성

<a name="overview" />

## <a name="overview"></a>개요

이 섹션에서는 사용 하는 방법을 설명 `Toolbar` Android 5.0 롤리팝 보다 이전 버전의 Android에 있습니다. 앱이 Android 5.0 이전 버전의 Android 지원 하지 않는 경우에이 섹션을 건너뛸 수 있습니다. 

때문에 `Toolbar` 일부인 Android v7 지원 라이브러리의 Android 2.1 (API 수준 7)을 실행 하는 장치에서 사용할 수 있습니다 이상. 그러나는 [지원 라이브러리 Android v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet을 설치 해야 하 고 코드를 사용 하도록 수정 된 `Toolbar` 이 라이브러리에서 제공 되는 구현 합니다. 이 섹션에서는이 NuGet을 설치 하 고 수정 하는 방법에 설명는 **ToolbarFun** 응용 프로그램에서 [두 번째 도구 추가](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) 롤리팝 5.0 이전 버전의 Android에서 실행 되도록 합니다.

도구 모음의 AppCompat 버전을 사용 하는 응용 프로그램을 수정 합니다. 

1.  응용 프로그램에 대 한 최소 및 대상 Android 버전을 설정 합니다.

2.  AppCompat NuGet 패키지를 설치 합니다.

3.  기본 Android 테마 대신 AppCompat 테마를 사용 합니다.

4.  수정 `MainActivity` 있도록 것 하위 클래스 `AppCompatActivity` 대신 `Activity`합니다. 

이러한 각 단계는 다음 섹션에서 자세히 설명 되어 있습니다.


<a name="android_version" />

## <a name="set-the-minimum-and-target-android-version"></a>최소 및 대상 Android 버전 설정

응용 프로그램의 대상 프레임 워크 API 수준 21 이상 설정 해야 합니다 또는 응용 프로그램을 제대로 배포 되지 않습니다. 와 같은 오류가 **패키지 'android'에서 'tileModeX' 특성에 대해 찾을 수 없는 리소스 식별자** 표시 대상 프레임 워크 설정 하지 않으면 때문에 이것이 응용 프로그램을 배포 하는 동안 **Android 5.0 (API 수준 21-롤리팝)**  큽니다. 

대상 프레임 워크 API 수준 21로 수준 이상으로 설정 하 고 하는 최소 Android 버전을 지원 하기 위해 앱이 Android API 수준 프로젝트 설정을 설정 합니다. Android API 수준 설정에 대 한 자세한 내용은 참조 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다. 에 `ToolbarFun` 예제에서는 최소 Android 버전이 KitKat (API 수준 4.4)으로 설정 됩니다. 

<a name="install_nuget" />

## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet 패키지 설치

다음으로 추가 된 [지원 라이브러리 Android v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 프로젝트에 패키지 합니다. Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조** 선택 **NuGet 패키지 관리...** . 클릭 **찾아보기** 검색 한 **지원 라이브러리 Android v7 AppCompat**합니다. 선택 **Xamarin.Android.Support.v7.AppCompat** 클릭 **설치**: 

[![NuGet 패키지 관리에서 선택한 V7 Appcompat 스크린샷 패키지](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png)

이 NuGet이 설치 되 면 다른 여러 NuGet 패키지가 설치 됩니다 아직 없는 경우 (예: **Xamarin.Android.Support.Animated.Vector.Drawable**, **Xamarin.Android.Support.v4**, 및 **Xamarin.Android.Support.Vector.Drawable**). NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 [연습: 프로젝트에 포함 하는 NuGet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다. 

<a name="appcompat_theme" />

## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat 테마 및 도구 모음을 사용 하 여

AppCompat 라이브러리 여러와 함께 제공 `Theme.AppCompat` AppCompat 라이브러리에서 지 원하는 Android 버전에서 사용할 수 있는 테마입니다. `ToolbarFun` 예제 응용 프로그램 테마에서 파생 된 `Theme.Material.Light.DarkActionBar`, 롤리팝 보다 이전 Android 버전에서 사용할 수 없으면입니다. 따라서 `ToolbarFun` 이 테마에 대 한 해당 AppCompat 버전을 사용할 수 있도록 적용할 해야 `Theme.AppCompat.Light.DarkActionBar`합니다. 또한 때문에 `Toolbar` 는 Android 버전에서 사용할 수 없음, 롤리팝 이전의에서는 버전을 사용 해야 AppCompat의 `Toolbar`합니다. 따라서 레이아웃을 사용 해야 `android.support.v7.widget.Toolbar` 대신 `Toolbar`합니다. 

<a name="update_layouts" />

### <a name="update-layouts"></a>레이아웃을 업데이트 합니다.

편집 **Resources/layout/Main.axml** 바꾸고는 `Toolbar` 요소는 다음 XML로: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

편집 **Resources/layout/toolbar.xml** 해당 내용을 다음 XML로 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

`?attr` 값은 더 이상 앞 `android:` (이전에 설명한 대로 `?` 표기법은 현재 테마의 리소스 참조). 경우 `?android:attr` 여전히 사용 되는 특성 값 보다 AppCompat 라이브러리는 현재 실행 중인 플랫폼에서 Android은 여기에서 참조 합니다. 이 예에서는 사용 하므로 `actionBarSize` AppCompat 라이브러리에 정의 된는 `android:` 접두사 삭제 됩니다. 마찬가지로, `@android:style` 로 변경 `@style` 있도록는 `android:theme` 특성이 AppCompat 라이브러리에서 테마로 설정 된 &ndash; 는 `ThemeOverlay.AppCompat.Dark.ActionBar` 테마는 여기에 대신 `ThemeOverlay.Material.Dark.ActionBar`합니다. 

<a name="update_style" />

### <a name="update-the-style"></a>스타일을 업데이트

편집 **Resources/values/styles.xml** 해당 내용을 다음 XML로 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

항목 이름 및이 예제에서 부모 테마 더 이상 접두사로 `android:` AppCompat 라이브러리를 사용 중 이므로 합니다. 부모 테마의 AppCompat 버전으로 변경 되는 또한 `Light.DarkActionBar`합니다. 


<a name="update_menus" />

### <a name="update-menus"></a>메뉴 업데이트

AppCompat 라이브러리를 지원 하기 위해 이전 버전의 Android 사용자 지정 특성의 특성을 반영 하는 사용 된 `android:` 네임 스페이스입니다. 하지만 일부 특성 (같은 `showAsAction` 에 사용 된 특성의 `<menu>` 태그) 오래 된 장치에서 Android 프레임 워크에 존재 하지 않는 &ndash; `showAsAction` Android API 11에 도입 된 하지만 Android API 7에서 사용할 수 없습니다. 이러한 이유로 지원 라이브러리에 정의 된 특성의 모든 접두사를 지정 하는 사용자 지정 네임 스페이스를 사용 해야 합니다. 메뉴 리소스 파일에서 네임 스페이스 라는 `local` 접두사로 사용에 대 한 정의 `showAsAction` 특성입니다. 

편집 **Resources/menu/top_menus.xml** 해당 내용을 다음 XML로 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local` 네임 스페이스는 줄이 추가 됩니다.

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction` 특성 앞이 `local:` 네임 스페이스 대신 `android:` 

```csharp
local:showAsAction="ifRoom"
```

마찬가지로, 편집 **Resources/menu/edit_menus.xml** 해당 내용을 다음 XML로 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

이 네임 스페이스 스위치에 대 한 지원을 제공 되는 방식에 `showAsAction` API 수준 11 이전 Android 버전에는 특성? 사용자 지정 특성 `showAsAction` AppCompat NuGet 설치 될 때 응용 프로그램에 포함 된 모든 값을 사용할 수 있습니다. 

<a name="subclass" />

## <a name="subclass-appcompatactivity"></a>하위 클래스 AppCompatActivity

변환에서 마지막 단계를 수정 하는 `MainActivity` 의 서브 클래스는 않도록 `AppCompactActivity`합니다. 편집 **MainActivity.cs** 다음 추가 `using` 문: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

이 선언 `Toolbar` 의 AppCompat 버전 `Toolbar`합니다. 다음으로의 클래스 정의 변경할 `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

AppCompat 버전의 작업 모음을 설정 하려면 `Toolbar`, 대체에 대 한 호출 `SetActionBar` 와 `SetSupportActionBar`합니다. 이 예제에서는 제목이 변경 임을 나타내는 AppCompat 버전의 `Toolbar` 사용 중입니다.

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

최소 Android 수준을 사전 롤리팝 값입니다. (예: API 19) 지원 되는 데 마지막으로 변경 합니다. 

앱을 빌드하고 사전 롤리팝 장치 또는 Android 에뮬레이터에서 실행 합니다. 다음 스크린샷은 AppCompat 버전의 **ToolbarFun** KitKat (API 19)를 실행 하는 Nexus 4: 

[![두 도구 모음에 표시 된 KitKat 장치에서 실행 중인 앱의 전체 스크린 샷](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png)

AppCompat 라이브러리를 사용 하면 테마를 필요가 없으며 전환할 수 Android 버전에 따라 &ndash; AppCompat 라이브러리를 사용 하면 모든 지원 되는 Android 버전에서 일관 된 사용자 경험을 제공할 수 있습니다. 




## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
