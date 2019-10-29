---
title: 도구 모음 호환성
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 809dc8ec8fd1106b8ad8631c0c506067abdf0d97
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029065"
---
# <a name="toolbar-compatibility"></a>도구 모음 호환성

## <a name="overview"></a>개요

이 섹션에서는 Android 5.0 롤리팝 이전 버전의 Android에서 `Toolbar`를 사용 하는 방법을 설명 합니다. 앱이 Android 5.0 이전 버전의 Android를 지원 하지 않는 경우이 섹션을 건너뛸 수 있습니다. 

`Toolbar`은 Android v7 지원 라이브러리의 일부 이므로 Android 2.1 (API 수준 7) 이상을 실행 하는 장치에서 사용할 수 있습니다. 그러나 [Android 지원 라이브러리 V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet을 설치 하 고이 라이브러리에 제공 된 `Toolbar` 구현을 사용 하도록 코드를 수정 해야 합니다. 이 섹션에서는이 NuGet을 설치 하 고 **, 응용 프로그램** [도구 모음을 추가](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) 하는 방법에 대해 설명 합니다 .이 도구 모음은 롤리팝 5.0 이전 버전의 Android에서 실행 되도록 합니다.

응용 프로그램을 수정 하 여 도구 모음의 AppCompat 버전을 사용 하려면 다음을 수행 합니다. 

1. 앱에 대 한 최소 및 대상 Android 버전을 설정 합니다.

2. AppCompat NuGet 패키지를 설치 합니다.

3. 기본 제공 Android 테마 대신 AppCompat 테마를 사용 합니다.

4. `MainActivity`를 수정 하 여 `Activity`아닌 하위 클래스가 `AppCompatActivity` 되도록 합니다. 

이러한 각 단계는 다음 섹션에 자세히 설명 되어 있습니다.

## <a name="set-the-minimum-and-target-android-version"></a>최소 및 대상 Android 버전 설정

앱의 대상 프레임 워크를 API 수준 21 이상으로 설정 해야 합니다. 그렇지 않으면 앱이 제대로 배포 되지 않습니다. 앱을 배포 하는 동안 **' android ' 패키지에서 ' tileModeX ' 특성에 대 한 리소스 식별자가 없는** 오류가 표시 되 면 대상 프레임 워크가 **ANDROID 5.0 (API 수준 21-롤리팝)** 이상으로 설정 되지 않았기 때문입니다. 

대상 프레임 워크 수준을 API 수준 21 이상으로 설정 하 고 Android API 수준 프로젝트 설정을 앱에서 지원할 최소 Android 버전으로 설정 합니다. Android API 수준을 설정 하는 방법에 대 한 자세한 내용은 [ANDROID Api 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요. `ToolbarFun` 예제에서 최소 Android 버전은 KitKat (API 수준 4.4)로 설정 됩니다. 

## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet 패키지 설치

그런 다음, [Android 지원 라이브러리 V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 패키지를 프로젝트에 추가 합니다. Visual Studio에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 합니다. **찾아보기** 를 클릭 하 고 **Android 지원 라이브러리 v7 AppCompat**을 검색 합니다. **V7** 를 선택 하 고 **설치**를 클릭 합니다. 

[NuGet 패키지 관리에서 선택한 V7 Appcompat 패키지의![스크린샷](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

이 NuGet이 설치 될 때 다른 여러 NuGet 패키지 (예: **xamarin.ios. 지원** **되지 않는 경우)도 설치 됩니다 (예: xamarin.ios. Xamarin.ios**.를 그릴 수 있습니다. NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 [연습: 프로젝트에 Nuget 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)을 참조 하세요. 

## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat 테마 및 도구 모음 사용

AppCompat 라이브러리는 AppCompat 라이브러리에서 지원 되는 모든 버전의 Android에서 사용할 수 있는 몇 가지 `Theme.AppCompat` 테마와 함께 제공 됩니다. `ToolbarFun` 예제 앱 테마는 `Theme.Material.Light.DarkActionBar`에서 파생 되며,이는 롤리팝 이전 버전의 Android에서 사용할 수 없습니다. 따라서이 테마에 해당 하는 AppCompat을 사용 하도록 `ToolbarFun`를 `Theme.AppCompat.Light.DarkActionBar`합니다. 또한 `Toolbar`는 롤리팝 이전 버전의 Android에서 사용할 수 없기 때문에 `Toolbar`AppCompat 버전을 사용 해야 합니다. 따라서 레이아웃은 `Toolbar`대신 `android.support.v7.widget.Toolbar`를 사용 해야 합니다. 

### <a name="update-layouts"></a>레이아웃 업데이트

**리소스/레이아웃/기본. axml** 을 편집 하 고 `Toolbar` 요소를 다음 XML로 바꿉니다. 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

**리소스/레이아웃/도구 모음 .xml** 을 편집 하 고 내용을 다음 xml로 바꿉니다. 

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

`?attr` 값에는 더 이상 `android:` 접두사가 붙습니다. `?` 표기법이 현재 테마의 리소스를 참조 한다는 것을 기억 하세요. 여기에서 `?android:attr` 계속 사용 되는 경우 Android는 AppCompat 라이브러리가 아닌 현재 실행 중인 플랫폼에서 특성 값을 참조 합니다. 이 예제에서는 AppCompat 라이브러리에 정의 된 `actionBarSize`를 사용 하기 때문에 `android:` 접두사가 삭제 됩니다. 마찬가지로 `@android:style`은 `@style`으로 변경 되므로 `android:theme` 특성이 AppCompat 라이브러리의 테마로 설정 &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` 테마가 `ThemeOverlay.Material.Dark.ActionBar`대신 사용 됩니다. 

### <a name="update-the-style"></a>스타일 업데이트

**리소스/값/스타일 .xml** 을 편집 하 고 내용을 다음 xml로 바꿉니다. 

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

AppCompat 라이브러리를 사용 하 고 있으므로이 예제의 항목 이름 및 부모 테마에는 더 이상 `android:` 접두사가 붙습니다. 또한 부모 테마가 `Light.DarkActionBar`의 AppCompat 버전으로 변경 됩니다. 

### <a name="update-menus"></a>업데이트 메뉴

이전 버전의 Android를 지원 하기 위해 AppCompat 라이브러리는 `android:` 네임 스페이스의 특성을 미러링 하는 사용자 지정 특성을 사용 합니다. 그러나 일부 특성 (예: `<menu>` 태그에 사용 되는 `showAsAction` 특성)은 android API 11에서 도입 되었지만 android API 7에는 제공 되지 않지만 이전 장치의 Android framework에는 없습니다 &ndash; `showAsAction`. 이러한 이유로 지원 라이브러리에서 정의한 모든 특성에 접두사를 지정 하려면 사용자 지정 네임 스페이스를 사용 해야 합니다. 메뉴 리소스 파일에서 `local` 라는 네임 스페이스는 `showAsAction` 특성에 접두사를 지정 하기 위해 정의 됩니다. 

**리소스/메뉴/top_menus** 를 편집 하 고 내용을 다음 xml로 바꿉니다.

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

`local` 네임 스페이스가 다음 줄로 추가 됩니다.

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction` 특성 앞에는이 `local:` 네임 스페이스가 아닌이 `android:` 

```csharp
local:showAsAction="ifRoom"
```

마찬가지로 **Resources/menu/edit_menus** 를 편집 하 고 내용을 다음 xml로 바꿉니다.

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

이 네임 스페이스 스위치는 API 수준 11 이전의 Android 버전에 `showAsAction` 특성에 대 한 지원을 제공 합니다. 사용자 지정 특성 `showAsAction` 및 모든 가능한 값은 AppCompat NuGet이 설치 될 때 앱에 포함 됩니다. 

## <a name="subclass-appcompatactivity"></a>서브 클래스 AppCompatActivity

변환의 마지막 단계는 `AppCompactActivity`의 서브 클래스를 위해 `MainActivity`를 수정 하는 것입니다. **MainActivity.cs** 를 편집 하 고 다음 `using` 문을 추가 합니다. 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

이 `Toolbar`은 `Toolbar`의 AppCompat 버전이 되도록 선언 됩니다. 다음으로 `MainActivity`의 클래스 정의를 변경 합니다. 

```csharp
public class MainActivity : AppCompatActivity
```

작업 모음을 `Toolbar`의 AppCompat 버전으로 설정 하려면 `SetActionBar` 호출을 `SetSupportActionBar`로 대체 합니다. 이 예제에서는 `Toolbar`의 AppCompat 버전이 사용 중임을 나타내기 위해 제목도 변경 되었습니다.

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

마지막으로, 최소 Android 수준을 지원 될 미리 롤리팝 값 (예: API 19)으로 변경 합니다. 

앱을 빌드하고 미리 롤리팝 장치 또는 Android 에뮬레이터에서 실행 합니다. 다음 스크린샷에서는 KitKat (API 19)를 실행 하는 Nexus **4의 AppCompat** 버전을 보여 줍니다. 

[KitKat 장치에서 실행 되는 앱의 전체 스크린샷![두 도구 모음이 모두 표시 됩니다.](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

AppCompat 라이브러리를 사용 하는 경우, AppCompat 라이브러리 &ndash; Android 버전에 따라 테마를 전환할 필요가 없습니다. 지원 되는 모든 Android 버전에서 일관 된 사용자 환경을 제공할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
